---
layout: default
title: "lab 03"
nav_order: 3
---

# 教程 3

植被指数计算（NDVI/EVI）


> **目标**：像植物医生一样，用卫星数据给地球上的植被做“健康检查”。  
> **核心技能**：计算 NDVI 和 EVI，看懂植被长势，还能画一张“绿度变化曲线”。

```javascript

// 1. 加载 Landsat 9 影像，用 aoi 筛选（不用写坐标！）
var collection = ee.ImageCollection('LANDSAT/LC09/C02/T1_TOA')
  .filterBounds(aoi)               // 👈 直接用你画的框！这一步秒杀了所有经纬度
  .filterDate('2025-06-01', '2025-08-01')
  .sort('CLOUD_COVER');

var myimage = collection.first();

// 2. 地图自动跳到这个框的中心（不用写坐标！）
Map.centerObject(aoi, 13);        // 👈 自动算中心，10是缩放级别

// 3. 显示真彩色
Map.addLayer(myimage, {bands: ['B4', 'B3', 'B2'], min:0, max:0.3}, '我的AOI');

// ========== 第 5 步：计算并显示 NDVI（植被健康指数） ==========

// 计算 NDVI（用归一化差值）
var ndvi = myimage.normalizedDifference(['B5', 'B4']).rename('NDVI');

// 设置一个好看的配色（棕色 -> 黄 -> 绿 -> 深绿）
var ndviPalette = {
  min: -0.2,
  max: 0.8,
  palette: [
    '#d73027', // 红棕色（无植被/水体）
    '#f46d43', 
    '#fdae61',
    '#fee08b', // 黄绿色
    '#d9ef8b',
    '#a6d96a',
    '#66bd63',
    '#1a9850', // 绿色（健康植被）
    '#006837'  // 深绿（茂密森林）
  ]
};

// 把 NDVI 结果作为新图层加到地图上（默认打开）
Map.addLayer(ndvi, ndviPalette, '🌳 NDVI 植被健康度');

// ========== 第 6 步（进阶）：画个时间序列图 ==========

// 把整个夏天的影像集合拿出来，每一张都算 NDVI
var ndviCollection = collection.map(function(img) {
  return img.normalizedDifference(['B5', 'B4']).rename('NDVI')
      .copyProperties(img, ['system:time_start']); // 保留时间信息
});

// 计算每一天、整个 AOI 区域内的平均 NDVI
var chart = ui.Chart.image.series({
  imageCollection: ndviCollection,
  region: aoi,
  reducer: ee.Reducer.mean(),
  scale: 30,        // Landsat 分辨率是 30米
  xProperty: 'system:time_start'
})
.setChartType('LineChart')
.setOptions({
  title: 'AOI区域夏季平均 NDVI 变化趋势',
  hAxis: {title: '日期'},
  vAxis: {title: '平均 NDVI', min: 0, max: 0.8},
  lineWidth: 2,
  colors: ['#1a9850']
});

// 打印折线图到 Console（控制台）面板
print(chart);

```


颜色不可能记得住也不需要记的解决方案 -> [sup03](https://ptsch1979.github.io/gee/sup03.html)

---

## 👩‍🔬 第一步：打开“上帝视角” —— 加载并挑选你的卫星影像

打开 Google Earth Engine 编辑器，先画一个你感兴趣的区域（**AOI**，就是 Area of Interest）。  
点击地图左上角的 **几何图形绘制工具**（那个小方块），画一个矩形或多边形——比如你家乡的一片农田、学校操场、或者城市公园。画完后，图层里会出现一个 `geometry`，它会自动成为我们代码里的 `aoi`。

> 💡 **为什么不用写经纬度？**  
> 因为你画了框，GEE 就自动记住了它的边界。`Map.centerObject(aoi, 13)` 会智能地把地图中心对准你的框，缩放级别 13 正好能看清细节。

```javascript
// 你的“画框”就是 aoi，下面这行代码会自动识别（如果 geometry 名字叫 aoi）
var aoi = geometry;   // 如果你画的框默认叫 geometry，就这样赋值
```

接下来，我们召唤 **Landsat 9** 卫星（美国宇航局的“老大哥”，30米分辨率，每 16 天拍一次全球）。

```javascript
var collection = ee.ImageCollection('LANDSAT/LC09/C02/T1_TOA')
  .filterBounds(aoi)               // 👈 只拿你框内的照片，省时省力
  .filterDate('2025-06-01', '2025-08-01') // 夏天，植物最活跃的时候
  .sort('CLOUD_COVER');            // 云少的优先，谁都不想看一堆棉花糖

var myimage = collection.first();  // 挑最清爽的一张当“主照片”
```

> 🤔 **思考**：为什么选 TOA（表观反射率）而不是 SR（地表反射率）？  
> 因为 TOA 计算简单，适合新手入门；如果要做严谨分析，可以换成 SR 产品（但需要额外大气校正，以后再说）。

---

## 🎨 第二步：看一眼真彩色 —— 你的“实地考察”

在你开始算指数之前，先用肉眼看看这张图长啥样。我们组合 **红（B4）、绿（B3）、蓝（B2）** 三个波段，调好亮度（min/max），得到一张“谷歌地图风格”的影像。

```javascript
Map.addLayer(myimage, {bands: ['B4', 'B3', 'B2'], min: 0, max: 0.3}, '🌈 真彩色');
```

现在你应该能看到绿色植被、蓝色水体、灰色建筑。有没有觉得很亲切？这就是我们卫星眼中的世界。

---

## 🌿 第三步：NDVI —— 植物的“健康手环”

NDVI（归一化差值植被指数）就像给植物戴了个智能手环，测它的“绿色活力”。  
**公式**：`NDVI = (近红外 - 红光) / (近红外 + 红光)`  
- 近红外（Landsat 9 的 B5）：植物反射强烈，因为细胞结构会散射近红外光。  
- 红光（B4）：植物叶绿素会大量吸收红光进行光合作用。

**比值越大**（接近 1）→ 植被越茂盛、健康；  
**比值越小**（接近 0 或负值）→ 裸土、水体或不毛之地。

```javascript
var ndvi = myimage.normalizedDifference(['B5', 'B4']).rename('NDVI');
```

> 🧪 **手动验算**：假设某像素在近红外波段值为 0.6，红光为 0.1，则  
> NDVI = (0.6 - 0.1) / (0.6 + 0.1) = 0.5 / 0.7 ≈ 0.71 → 很健康的植被！

---

## 🎨 第四步：给 NDVI 上色 —— 像调色板一样直观

灰度图不容易看，我们设计一个**从红到绿的渐变色盘**，让结果一目了然：

- 红棕色（-0.2 ~ 0）→ 水或裸土  
- 黄色 → 稀疏植被  
- 亮绿色（0.3 ~ 0.8）→ 茂密森林  

```javascript
var ndviPalette = {
  min: -0.2,
  max: 0.8,
  palette: [
    '#d73027', '#f46d43', '#fdae61', 
    '#fee08b', '#d9ef8b', '#a6d96a',
    '#66bd63', '#1a9850', '#006837'
  ]
};
Map.addLayer(ndvi, ndviPalette, '🌳 NDVI 植被健康度');
```

现在地图上应该出现一片色彩斑斓的热力图，绿色越深，植物越“壮实”。

---

## 📈 第五步（进阶）：画一条“绿度时间曲线”

单张图只能看一天，但植物是动态的——6 月到 8 月，庄稼从播种到收割，绿度会有起伏。  
我们拿出整个夏季的影像集合，对**每一天**都算一个平均 NDVI（覆盖你的整个 AOI），然后画成折线图。

```javascript
var ndviCollection = collection.map(function(img) {
  return img.normalizedDifference(['B5', 'B4']).rename('NDVI')
      .copyProperties(img, ['system:time_start']);
});

var chart = ui.Chart.image.series({
  imageCollection: ndviCollection,
  region: aoi,
  reducer: ee.Reducer.mean(),
  scale: 30,
  xProperty: 'system:time_start'
})
.setChartType('LineChart')
.setOptions({
  title: 'AOI 夏季平均 NDVI 变化趋势',
  hAxis: {title: '日期'},
  vAxis: {title: '平均 NDVI', min: 0, max: 0.8},
  lineWidth: 2,
  colors: ['#1a9850']
});

print(chart);
```

运行后，控制台会弹出一张曲线图。如果发现 7 月中旬有个峰，那就是植物最旺盛的时候；如果 8 月底掉下来，可能开始收割或落叶了。

---

## 🧪 额外彩蛋：EVI —— 增强型植被指数（更抗干扰）

NDVI 在植被浓密或大气有雾时容易“饱和”或受干扰，于是科学家设计了 **EVI**（增强型植被指数），它加入了**蓝光波段**来校正大气散射，并且对土壤背景更鲁棒。

**EVI 公式**（Landsat 版本）：
```
EVI = 2.5 * (NIR - Red) / (NIR + 6*Red - 7.5*Blue + 1)
```
其中 NIR = B5, Red = B4, Blue = B2。

在 GEE 里可以这样算（手工写表达式，因为没有内置函数）：

```javascript
var evi = myimage.expression(
  '2.5 * ((NIR - RED) / (NIR + 6 * RED - 7.5 * BLUE + 1))',
  {
    'NIR': myimage.select('B5'),
    'RED': myimage.select('B4'),
    'BLUE': myimage.select('B2')
  }
).rename('EVI');

// 显示（用同样的调色板）
Map.addLayer(evi, {min: -0.2, max: 0.8, palette: ndviPalette.palette}, '🌿 EVI 增强植被');
```

> 💡 **对比 NDVI 和 EVI**：在高植被区域，EVI 不易饱和，能更好区分不同密度的森林；在干旱区，EVI 也更稳定。你可以两张图叠加比较，看看哪张更符合你的直觉。

---

## 📝 小结：你学会了什么？

| 步骤 | 操作 | 记忆口诀 |
|------|------|----------|
| 1️⃣ | 画 AOI、筛选影像 | “先画框，再选片，云少清晰是关键” |
| 2️⃣ | 显示真彩色 | “红绿蓝，三原色，肉眼先看一眼” |
| 3️⃣ | 计算 NDVI | “近红减红除以和，绿度高低全靠它” |
| 4️⃣ | 配色显示 | “红黄绿，色阶走，健康指数一目了然” |
| 5️⃣ | 时间序列图 | “多张影像算平均，变化趋势画曲线” |
| 6️⃣（彩蛋）| 计算 EVI | “加个蓝，抗干扰，增强版更靠谱” |

---

## 🧠 挑战任务

1. **改日期**：把 `filterDate` 改为你出生那年的 6-8 月，看看当年的植被状况。  
2. **比较不同地区**：在城市和森林分别画两个 AOI，分别算 NDVI，对比它们的平均数值。  
3. **添加云掩膜**：从 TOA 产品中，你能用 `QA_PIXEL` 波段去掉云和云阴影吗？（提示：查 GEE 文档）

---

## 🏁 最终完整代码（可直接复制运行）

```javascript
// ================= 0. 定义 AOI（用你画的多边形） =================
var aoi = geometry; // 如果你画的不是 geometry，请改成对应的图层名

// ================= 1. 加载和筛选影像 =================
var collection = ee.ImageCollection('LANDSAT/LC09/C02/T1_TOA')
  .filterBounds(aoi)
  .filterDate('2025-06-01', '2025-08-01')
  .sort('CLOUD_COVER');

var myimage = collection.first();

Map.centerObject(aoi, 13);

// ================= 2. 真彩色显示 =================
Map.addLayer(myimage, {bands: ['B4', 'B3', 'B2'], min: 0, max: 0.3}, '🌈 真彩色');

// ================= 3. NDVI =================
var ndvi = myimage.normalizedDifference(['B5', 'B4']).rename('NDVI');

var ndviPalette = {
  min: -0.2,
  max: 0.8,
  palette: ['#d73027','#f46d43','#fdae61','#fee08b','#d9ef8b','#a6d96a','#66bd63','#1a9850','#006837']
};
Map.addLayer(ndvi, ndviPalette, '🌳 NDVI 植被健康度');

// ================= 4. 时间序列 =================
var ndviCollection = collection.map(function(img) {
  return img.normalizedDifference(['B5', 'B4']).rename('NDVI')
      .copyProperties(img, ['system:time_start']);
});

var chart = ui.Chart.image.series({
  imageCollection: ndviCollection,
  region: aoi,
  reducer: ee.Reducer.mean(),
  scale: 30,
  xProperty: 'system:time_start'
})
.setChartType('LineChart')
.setOptions({
  title: 'AOI 夏季平均 NDVI 变化趋势',
  hAxis: {title: '日期'},
  vAxis: {title: '平均 NDVI', min: 0, max: 0.8},
  lineWidth: 2,
  colors: ['#1a9850']
});
print(chart);

// ================= 5. EVI（增强型，可选） =================
var evi = myimage.expression(
  '2.5 * ((NIR - RED) / (NIR + 6 * RED - 7.5 * BLUE + 1))',
  {
    'NIR': myimage.select('B5'),
    'RED': myimage.select('B4'),
    'BLUE': myimage.select('B2')
  }
).rename('EVI');
Map.addLayer(evi, {min: -0.2, max: 0.8, palette: ndviPalette.palette}, '🌿 EVI 增强植被');
```
颜色不可能记得住也不需要记的解决方案 -> [sup03](https://ptsch1979.github.io/gee/sup03.html)

