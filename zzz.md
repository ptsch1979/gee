石先生，你这个背景很适合用 1 个月把 GEE 学起来：  
**会一点 Python/C + 用过 ArcGIS Pro/ENVI + 有明确植被监测方向**，这比纯零基础快很多。你的关键突破点不是“从零学遥感”，而是：

> **把已有 ArcGIS/ENVI 经验，迁移到 GEE 的云端遥感计算流程中，并围绕荒漠草原/荒漠灌木林建立一套可复用的植被监测项目。**

下面我给你一版 **定制化 1 个月计划**，主线是：  
**GEE 基础 → 植被指数与时序 → 干旱区植被监测 → 职业项目交付**。

---

# 一、你的背景判断与学习策略

## 你的优势

### 1. 有 Python/C 基础

这意味着你可以较快理解：

- 函数
- 变量
- 条件判断
- 循环思想
- API 调用
- 数据结构

GEE 的 JavaScript/Python API 对你不会太难。

### 2. 用过 ArcGIS Pro

你已经具备：

- 图层概念
- 矢量/栅格概念
- 制图表达
- 研究区思维
- 属性表/空间数据感觉

这对后续导出结果、制图、写项目报告很有帮助。

### 3. 用过 ENVI 框 ROI、做波段预处理

这说明你理解：

- ROI
- 波段
- 影像显示
- 简单预处理
- 地物样本

这些可以直接迁移到 GEE 的：

- `ee.FeatureCollection`
- 训练样本
- 分区统计
- 分类
- 指数计算

---

## 你需要补的短板

### 1. GEE 的服务端计算思维

GEE 不是本地 ENVI/ArcGIS，它是云端大规模地理计算。  
你不能总是用本地变量思维处理影像集合，而要学会：

```text
ImageCollection.map()
Reducer
Filter
Expression
Server-side object
```

### 2. JavaScript/Python API 熟练度

你不需要成为程序员，但必须能写：

- 数据筛选
- 函数封装
- 指数计算
- 时序统计
- 导出
- 简单分类

### 3. 干旱区植被遥感的专业细节

荒漠草原/荒漠灌木林和森林、农田不同，难点很明显：

- 植被覆盖度低
- 土壤背景强
- NDVI 容易受裸土影响
- 灌木/草本混合像元严重
- 降水后短期返青明显
- 物候窗口短
- 年份间波动大
- 分类精度容易偏低

所以你后面不能只学 NDVI，还要学：

- SAVI
- MSAVI
- EVI
- NDVI anomaly
- 生长季合成
- 降水滞后响应
- 覆盖度估计
- 退化/恢复趋势分析

---

# 二、技术路线建议：先 JS，后 Python

## 推荐策略

### 前 2–3 周：主要用 GEE Code Editor JavaScript

原因：

1. 上手快
2. 可视化方便
3. 官方示例最多
4. 适合快速建立项目原型
5. 你未来汇报、复现论文、调试方法时最方便

### 第 3–4 周开始：加入 Python API

原因：

1. 职业项目需要自动化
2. 便于批量处理
3. 便于和 pandas/matplotlib/geopandas 结合
4. 便于形成工程化流程

### 最终目标

你最好能同时使用：

```text
GEE Code Editor：探索、可视化、快速验证
GEE Python API：批处理、自动化、项目工程化
ArcGIS Pro/QGIS：最终制图、矢量编辑、成果表达
```

---

# 三、1 个月定制计划总览

目标：  
**能够围绕荒漠草原/荒漠灌木林，独立完成 GEE 植被生长监测项目，并具备迁移到其他方向的能力。**

| 周次 | 主题 | 目标 | 关键产出 |
|---|---|---|---|
| 第1周 | GEE 基础 + 植被指数 | 会用 Code Editor，能计算 NDVI/SAVI/EVI | 单期植被指数图 + 区域统计 |
| 第2周 | 时序数据 + 云掩膜 + 气候数据 | 会做生长季合成和时序曲线 | 年度/多年 NDVI/SAVI 时序 |
| 第3周 | 植被监测专题 + 干旱区方法 | 会做长势异常、降水响应、初步分类 | 距平图/相关性图/植被分类初版 |
| 第4周 | 完整项目实战 + 职业交付 | 能完成一个可展示项目 | 代码仓库 + 图件 + 报告 + README |

---

# 四、第1周：GEE 基础与植被指数

目标：从会打开 GEE，到能独立完成一个植被指数计算与统计。

---

## Day 1：GEE 平台入门

### 任务

1. 登录 GEE
2. 打开 Code Editor
3. 加载 Sentinel-2
4. 显示真彩色/假彩色
5. 画一个研究区 ROI
6. 保存脚本

### 你要形成习惯

以后所有项目都按这个结构写：

```javascript
// 1. 研究区
// 2. 数据加载
// 3. 预处理
// 4. 指数计算
// 5. 统计/可视化
// 6. 导出
```

### 产出

- 一张研究区影像图
- 一个自己的 ROI
- 一个保存的 GEE 脚本

---

## Day 2：GEE 数据模型

重点理解：

| 对象 | 含义 |
|---|---|
| `ee.Image` | 单景影像 |
| `ee.ImageCollection` | 影像集合 |
| `ee.Geometry` | 点线面 |
| `ee.Feature` | 带属性的矢量 |
| `ee.FeatureCollection` | 矢量集合 |
| `ee.List` | 列表 |
| `ee.Dictionary` | 字典 |
| `ee.Reducer` | 统计归约器 |

### 关键概念

GEE 中很多对象是服务端对象，不能直接当普通数字/变量处理。

例如：

```javascript
var x = ee.Number(10);
```

你不能简单把它当 JavaScript 数字乱用。  
需要时可以用：

```javascript
print(x.getInfo());
```

但不要大量滥用 `.getInfo()`。

### 产出

- 能创建点、面
- 能加载矢量
- 能理解 Image 和 ImageCollection 的区别

---

## Day 3：Sentinel-2 基础处理

### 重点数据集

```text
COPERNICUS/S2_SR_HARMONIZED
```

这是 Sentinel-2 地表反射率常用数据集。

### 必会操作

```javascript
var roi = ee.Geometry.Point([经度, 纬度]);

var s2 = ee.ImageCollection('COPERNICUS/S2_SR_HARMONIZED')
          .filterBounds(roi)
          .filterDate('2023-06-01', '2023-09-01')
          .median();

Map.centerObject(roi, 9);

Map.addLayer(s2, {bands: ['B4','B3','B2'], min: 0, max: 3000}, 'True color');
Map.addLayer(s2, {bands: ['B8','B4','B3'], min: 0, max: 4000}, 'False color');
```

### 你要理解

| 波段 | 含义 |
|---|---|
| B2 | Blue |
| B3 | Green |
| B4 | Red |
| B8 | NIR |
| B11 | SWIR1 |
| B12 | SWIR2 |

对于植被监测：

- Red：B4
- NIR：B8
- SWIR：B11/B12 常用于水分、裸土、植被状态

### 产出

- 能显示 Sentinel-2 真彩色/假彩色
- 能解释常用波段

---

## Day 4：NDVI 与荒漠草原适用指数

荒漠草原不能只学 NDVI。你要从一开始就接触：

| 指数 | 作用 | 适合场景 |
|---|---|---|
| NDVI | 基础植被绿度 | 常规植被监测 |
| EVI | 减少大气/背景影响 | 中低植被、干旱区可用 |
| SAVI | 土壤调节 | 低覆盖度、裸土多 |
| MSAVI | 改进土壤调节 | 荒漠草原/灌木林较有用 |
| NDWI | 水分 | 水体、植被水分状态辅助 |
| NBR | 燃烧/植被水分胁迫 | 火灾、干旱胁迫辅助 |

### 基础 NDVI

```javascript
var ndvi = s2.normalizedDifference(['B8', 'B4']).rename('NDVI');

Map.addLayer(ndvi, {
  min: 0,
  max: 0.6,
  palette: ['brown', 'yellowgreen', 'green']
}, 'NDVI');
```

荒漠草原 NDVI 通常不会像森林那么高，很多区域可能只有 0.1–0.3，不要按森林标准设色带。

### 产出

- 能计算 NDVI
- 能理解为什么荒漠草原需要土壤调节指数

---

## Day 5：SAVI/EVI 计算

### SAVI 示例

```javascript
var savi = s2.expression(
  '((NIR - RED) / (NIR + RED + 0.5)) * 1.5',
  {
    NIR: s2.select('B8'),
    RED: s2.select('B4')
  }
).rename('SAVI');

Map.addLayer(savi, {
  min: 0,
  max: 0.5,
  palette: ['brown', 'yellowgreen', 'green']
}, 'SAVI');
```

### EVI 示例

```javascript
var evi = s2.expression(
  '2.5 * ((NIR - RED) / (NIR + 6 * RED - 7.5 * BLUE + 1))',
  {
    NIR: s2.select('B8'),
    RED: s2.select('B4'),
    BLUE: s2.select('B2')
  }
).rename('EVI');

Map.addLayer(evi, {
  min: 0,
  max: 0.5,
  palette: ['brown', 'yellowgreen', 'green']
}, 'EVI');
```

### 产出

- 能计算 NDVI、SAVI、EVI
- 能对比三者在裸土背景下的差异

---

## Day 6：分区统计与导出

你之前用 ArcGIS Pro，后面成果很可能要导回 ArcGIS/QGIS。  
所以必须掌握导出。

### 区域均值统计

```javascript
var stats = ndvi.reduceRegion({
  reducer: ee.Reducer.mean(),
  geometry: roi,
  scale: 10,
  maxPixels: 1e13
});

print(stats);
```

### 导出 GeoTIFF 到 Drive

```javascript
Export.image.toDrive({
  image: ndvi,
  description: 'NDVI_export',
  scale: 10,
  region: roi,
  fileFormat: 'GeoTIFF'
});
```

### 导出 CSV 统计表

```javascript
Export.table.toDrive({
  collection: featureCollection,
  description: 'table_export',
  fileFormat: 'CSV'
});
```

### 产出

- 能把 NDVI/SAVI 导出 GeoTIFF
- 能用 ArcGIS Pro 打开结果
- 能统计 ROI 均值

---

## Day 7：第1周小项目

### 项目：单期荒漠植被指数制图

任务：

1. 选择你的荒漠草原/灌木林研究区
2. 加载 Sentinel-2
3. 计算 NDVI、SAVI、EVI
4. 对比三者差异
5. 统计 ROI 内均值
6. 导出 GeoTIFF
7. 写 1 页笔记

### 验收标准

- 能正确计算指数
- 能解释 NDVI 在裸土背景下的问题
- 能导出结果并在 ArcGIS Pro 中打开
- 能说明为什么 SAVI/MSAVI 可能更适合干旱区

---

# 五、第2周：时序合成、云掩膜与气候数据

目标：从单期影像，升级为时间序列植被监测。

---

## Day 8：云掩膜

虽然荒漠草原云通常比湿润区少，但仍然必须会云掩膜。  
尤其是 Sentinel-2 做 10 m 精细监测时，云和云阴影影响很明显。

### Sentinel-2 QA60 云掩膜

```javascript
function maskS2clouds(image) {
  var qa = image.select('QA60');

  var cloudBitMask = 1 << 10;
  var cirrusBitMask = 1 << 11;

  var mask = qa.bitwiseAnd(cloudBitMask).eq(0)
              .and(qa.bitwiseAnd(cirrusBitMask).eq(0));

  return image.updateMask(mask);
}
```

### 应用

```javascript
var s2 = ee.ImageCollection('COPERNICUS/S2_SR_HARMONIZED')
          .filterBounds(roi)
          .filterDate('2023-05-01', '2023-09-30')
          .filter(ee.Filter.lt('CLOUDY_PIXEL_PERCENTAGE', 20))
          .map(maskS2clouds);
```

### 产出

- 能做基本云掩膜
- 能解释 QA60 中 cloud/cirrus bit mask

---

## Day 9：生长季合成

荒漠草原植被监测必须关注生长季。  
不同区域生长季不同，你需要自己设定。

例如北方干旱半干旱区常见生长季：

```text
5月—9月
```

或：

```text
6月—8月
```

具体要根据你的研究区降水和物候调整。

### 生长季中值合成

```javascript
var composite = s2.median().clip(roi);
```

### 生长季最大值合成

对于植被指数，有时用最大值更合理：

```javascript
var ndviMax = s2.map(function(image){
  return image.normalizedDifference(['B8','B4']).rename('NDVI');
}).max();
```

### 产出

- 能生成生长季合成影像
- 能解释 median 和 max 合成的差异

---

## Day 10：多年时间序列构建

你要开始从“一年”扩展到“多年”。

### 目标

生成：

```text
2018 NDVI
2019 NDVI
2020 NDVI
2021 NDVI
2022 NDVI
2023 NDVI
2024 NDVI
```

或者：

```text
2018 SAVI
2019 SAVI
...
```

### 思路

每年分别筛选、掩膜、计算指数、合成。

### 产出

- 能生成多年生长季 NDVI/SAVI 合成产品
- 能把年份写入属性

---

## Day 11：MODIS 长时间序列

对于荒漠草原/灌木林，如果你要做 2000 年以来趋势，MODIS 很重要。

### 常用数据

```text
MODIS/061/MOD13Q1
MODIS/061/MOD13A2
```

如果数据 ID 有更新，以 GEE Data Catalog 为准。

### 特点

| 数据 | 分辨率 | 优势 |
|---|---:|---|
| MOD13Q1 | 250 m | 16 天 NDVI/EVI |
| MOD13A2 | 1 km | 16 天 NDVI/EVI |

### 适合任务

- 多年植被趋势
- 干旱响应
- 距平分析
- 区域尺度监测

### 不适合任务

- 精细灌木林斑块
- 小面积样地
- 精细地类边界

### 产出

- 能加载 MODIS NDVI
- 能理解 MODIS 和 Sentinel-2 的尺度差异

---

## Day 12：降水与气候数据

荒漠草原植被和降水关系极强。  
你后面做植被监测，几乎一定要加入降水、温度、土壤水分或干旱指数。

### 常用数据

| 数据 | 用途 |
|---|---|
| CHIRPS | 降水 |
| ERA5-Land | 温度、土壤水分、蒸散发相关 |
| TerraClimate | 水分亏缺、PET、干旱相关 |
| MODIS LST | 地表温度 |
| MODIS ET | 蒸散发 |
| SPEI/SC-PDSI 等 | 干旱指数，部分需外部计算 |

### 常用 GEE 数据集示例

```text
UCSB-CHG/CHIRPS/PENTAD
ECMWF/ERA5_LAND/DAILY_AGGR
IDAHO_EPSCOR/TERRACLIMATE
```

### 产出

- 能加载 CHIRPS 或 ERA5-Land
- 能计算生长季降水总量
- 能理解降水对荒漠植被的滞后影响

---

## Day 13：时序曲线提取

你要能提取某个样地/ROI 的 NDVI 时间曲线。

### 目标

例如：

```text
2018—2025 年每年生长季 NDVI 均值
```

或者：

```text
2023 年 5—9 月 NDVI 变化曲线
```

### 意义

- 判断返青期
- 判断峰值期
- 判断枯黄期
- 分析降水响应
- 比较不同年份长势

### 产出

- 能输出 NDVI 时间序列 CSV
- 能用 Excel/Python/ArcGIS 画图

---

## Day 14：第2周综合项目

### 项目：生长季植被指数时序产品

任务：

1. 选择研究区
2. 生成 2023 年生长季 Sentinel-2 NDVI/SAVI 合成
3. 生成 2018—2024 年年度 NDVI/SAVI 合成
4. 提取 ROI 时序均值
5. 导出 CSV
6. 画时序曲线

### 验收标准

- 能说明生长季选择依据
- 能说明云掩膜策略
- 能输出多年时序曲线
- 能解释年份差异可能与降水的关系

---

# 六、第3周：干旱区植被监测专题

目标：从“会算指数”升级到“能做科研/业务分析”。

---

## Day 15：荒漠草原植被监测核心问题

你需要明确几个关键问题：

### 1. 低覆盖度问题

荒漠草原植被覆盖度低，土壤背景影响强。  
NDVI 可能受土壤亮度干扰。

建议：

- SAVI
- MSAVI
- EVI
- 结合高分辨率影像判读

### 2. 草本与灌木混合

荒漠灌木林和草本经常混合。  
单纯 NDVI 很难区分。

可用信息：

- 多时相物候差异
- 纹理
- 高度/冠层结构间接信息
- SAR 后向散射
- 高分辨率影像
- 样地调查数据

### 3. 降水滞后效应

降雨后植被响应可能滞后：

```text
1周、2周、4周，甚至更久
```

所以不能只分析同期降水。

### 4. 年际波动大

干旱区植被受降水影响大，不能轻易把一年低值解释为退化。  
需要多年基线。

---

## Day 16：NDVI/SAVI 距平分析

距平是植被长势监测的核心方法之一。

### 基本思想

```text
当年植被指数 - 多年平均植被指数
```

即：

```text
Anomaly = NDVI_year - NDVI_baseline_mean
```

### 标准化距平

```text
Z = (NDVI_year - mean) / stdDev
```

### 意义

| Z 值 | 含义 |
|---:|---|
| 明显大于 0 | 长势偏好 |
| 接近 0 | 接近常年 |
| 明显小于 0 | 长势偏差 |

### 产出

- 能计算某一年相对于多年均值的距平
- 能制作长势偏好/偏差图

---

## Day 17：VCI 或相对状态指数

### VCI 思路

VCI，Vegetation Condition Index，常用于植被状态监测：

```text
VCI = (NDVI - NDVI_min) / (NDVI_max - NDVI_min)
```

其中 NDVI_min 和 NDVI_max 可以用多年同期最小/最大值。

### 适用

- 干旱监测
- 植被状态相对评价
- 长势分级

### 注意

在荒漠区，NDVI_min 可能受裸土影响很大，需要谨慎。  
可以改用：

```text
SAVI
EVI
```

或者结合降水背景解释。

### 产出

- 能计算 VCI 或类似状态指数
- 能解释其局限性

---

## Day 18：降水—植被关系

这是荒漠草原项目里非常重要的一环。

### 可做的分析

1. 年降水 vs 年 NDVI/SAVI  
2. 生长季降水 vs 生长季 NDVI/SAVI  
3. 滞后降水 vs NDVI  
4. 降水距平 vs 植被距平  
5. 不同地类的降水响应差异

### 简单思路

```text
X = 生长季累计降水
Y = 生长季 NDVI/SAVI 均值
```

然后做：

- 散点图
- 相关系数
- 线性回归
- 滞后相关

### 产出

- 能提取研究区多年降水与植被指数表
- 能初步分析降水与植被关系

---

## Day 19：趋势分析

如果你想做退化/恢复，需要趋势分析。

### 常见方法

1. 线性趋势  
2. Theil-Sen slope  
3. Mann-Kendall 检验  
4. 距平趋势  
5. 残差趋势

### 简单线性趋势

GEE 中可用：

```javascript
ee.Reducer.linearFit()
```

或者参考社区 Theil-Sen 脚本。

### 注意

干旱区趋势分析不能只看 NDVI 下降就说退化。  
因为降水年际波动可能导致短期下降。

更稳妥的是：

```text
NDVI ~ 降水模型残差
```

如果降水解释后仍下降，才更可能指向人为退化或生态变化。

### 产出

- 能生成 2000—2025 年 NDVI 趋势斜率图
- 能解释趋势分析中的降水干扰问题

---

## Day 20：植被/地物分类入门

即使你的方向是监测，也要会基本分类。  
因为职业项目经常需要：

```text
灌木林
草地
裸地
水体
农田
沙地
```

### 分类目标

第一版不要追求太复杂，建议先做：

```text
1. 植被
2. 裸地/沙地
3. 水体
4. 农田/居民地
```

之后再细化：

```text
灌木林
高覆盖草地
低覆盖草地
裸土
盐碱地
水体
```

### 特征建议

| 特征 | 作用 |
|---|---|
| Sentinel-2 波段 | 光谱基础 |
| NDVI/SAVI/EVI | 植被绿度 |
| NDWI | 水分 |
| DEM/slope | 地形分异 |
| 年度最大 NDVI | 植被强度 |
| 生长季均值 | 稳定性 |
| 纹理 | 灌木斑块识别辅助 |

### 产出

- 能做一个简单土地覆盖分类
- 能理解荒漠区分类难点

---

## Day 21：第3周综合项目

### 项目：荒漠草原植被长势异常监测

任务：

1. 使用 MODIS NDVI 或 Sentinel-2  
2. 建立 2015—2020 基线  
3. 计算 2023 或 2024 生长季 NDVI/SAVI  
4. 生成距平图  
5. 提取研究区距平统计  
6. 加入 CHIRPS 降水数据  
7. 输出长势分析结论

### 验收标准

- 能说明基线年份选择
- 能计算距平
- 能解释长势偏好/偏差
- 能结合降水说明原因
- 能导出结果图

---

# 七、第4周：完整职业项目实战

目标：做出一个能写进简历、能用于科研/工作项目的完整成果。

---

## 项目主题推荐

结合你的方向，我建议首选：

# **项目A：荒漠草原/荒漠灌木林植被生长监测与干旱响应分析**

这是最贴合你当前方向的。

---

## 项目目标

围绕一个研究区，完成：

1. 2018—2024 年生长季植被指数合成  
2. NDVI/SAVI/EVI 对比  
3. 植被长势距平分析  
4. 降水距平分析  
5. 植被—降水相关性  
6. 结果制图  
7. 方法报告  
8. 可复现代码

---

## 技术路线

```text
研究区确定
↓
Sentinel-2 / MODIS 数据获取
↓
云掩膜
↓
生长季合成
↓
NDVI / SAVI / EVI 计算
↓
多年基线构建
↓
距平/标准化距平计算
↓
CHIRPS/ERA5 降水数据提取
↓
植被—降水相关性分析
↓
结果导出与制图
↓
不确定性说明
```

---

## 成果清单

| 成果 | 说明 |
|---|---|
| 研究区矢量 | GeoJSON/Shapefile |
| 年度植被指数图 | GeoTIFF/PNG |
| 距平图 | GeoTIFF/PNG |
| 时序曲线 | CSV/图 |
| 降水统计表 | CSV |
| 相关性结果 | 表格/图 |
| 代码 | GEE JS 或 Python |
| README | 项目说明 |
| 方法报告 | 1–2页 |

---

## 可写进简历的描述

> 基于 Google Earth Engine 平台，利用 Sentinel-2/MODIS 多源遥感数据，构建某荒漠草原/荒漠灌木林研究区 2018—2024 年生长季植被指数时序产品。通过云掩膜、生长季合成、SAVI/NDVI 距平和降水相关性分析，评估植被长势年际变化及其对降水的响应。项目形成可复现代码、专题图件、统计表格和方法报告，可支撑退化评估与生态监测。

---

# 八、第4周备选项目

如果你以后想拓展方向，可以选一个作为副项目。

---

## 项目B：灌木林/草地/裸地初步分类

### 目标

利用 Sentinel-2 年度合成、指数、DEM 和样本点，做简单土地覆盖分类。

### 类别建议

第一版：

```text
植被
裸地/沙地
水体
农田/人工地表
```

第二版：

```text
灌木林
高覆盖草地
低覆盖草地
裸土
盐碱地
水体
```

### 难点

- 灌木和草地光谱相似
- 低覆盖草地容易与裸土混淆
- 样本质量决定分类上限

### 产出

- 分类图
- 混淆矩阵
- 面积统计
- 误差说明

---

## 项目C：植被退化/恢复趋势分析

### 目标

基于 MODIS NDVI 2000—2025，分析趋势与异常。

### 方法

```text
多年 NDVI 趋势
↓
降水校正/残差分析
↓
退化风险区识别
```

### 产出

- 趋势斜率图
- 残差趋势图
- 可能退化区
- 不确定性说明

---

# 九、可直接使用的 GEE 代码模板

下面给你一套入门代码模板。你可以先把 `roi` 换成自己的研究区。

---

## 模板1：Sentinel-2 NDVI/SAVI/EVI

```javascript
// =========================
// 荒漠草原植被指数基础模板
// =========================

// 1. 研究区
// 可以用自己画的 roi，也可以加载 Asset
var roi = ee.Geometry.Point([110.0, 40.0]); // 替换成你的研究区中心点

// 2. 云掩膜函数
function maskS2clouds(image) {
  var qa = image.select('QA60');

  var cloudBitMask = 1 << 10;
  var cirrusBitMask = 1 << 11;

  var mask = qa.bitwiseAnd(cloudBitMask).eq(0)
              .and(qa.bitwiseAnd(cirrusBitMask).eq(0));

  return image.updateMask(mask);
}

// 3. 加载 Sentinel-2
var s2 = ee.ImageCollection('COPERNICUS/S2_SR_HARMONIZED')
          .filterBounds(roi)
          .filterDate('2023-05-01', '2023-09-30')
          .filter(ee.Filter.lt('CLOUDY_PIXEL_PERCENTAGE', 20))
          .map(maskS2clouds);

// 4. 计算指数
function addVegetationIndices(image) {
  var ndvi = image.normalizedDifference(['B8', 'B4']).rename('NDVI');

  var savi = image.expression(
    '((NIR - RED) / (NIR + RED + 0.5)) * 1.5',
    {
      NIR: image.select('B8'),
      RED: image.select('B4')
    }
  ).rename('SAVI');

  var evi = image.expression(
    '2.5 * ((NIR - RED) / (NIR + 6 * RED - 7.5 * BLUE + 1))',
    {
      NIR: image.select('B8'),
      RED: image.select('B4'),
      BLUE: image.select('B2')
    }
  ).rename('EVI');

  return image.addBands([ndvi, savi, evi]);
}

var s2_idx = s2.map(addVegetationIndices);

// 5. 生长季合成
var composite = s2_idx.median().clip(roi);

// 6. 可视化
Map.centerObject(roi, 8);

Map.addLayer(composite.select('NDVI'), {
  min: 0,
  max: 0.5,
  palette: ['brown', 'yellowgreen', 'darkgreen']
}, 'NDVI');

Map.addLayer(composite.select('SAVI'), {
  min: 0,
  max: 0.5,
  palette: ['brown', 'yellowgreen', 'darkgreen']
}, 'SAVI');

Map.addLayer(composite.select('EVI'), {
  min: 0,
  max: 0.5,
  palette: ['brown', 'yellowgreen', 'darkgreen']
}, 'EVI');

// 7. 区域统计
var stats = composite.select(['NDVI', 'SAVI', 'EVI']).reduceRegion({
  reducer: ee.Reducer.mean(),
  geometry: roi,
  scale: 30,
  maxPixels: 1e13
});

print('Mean indices:', stats);
```

---

## 模板2：MODIS NDVI 多年距平

```javascript
// =========================
// MODIS NDVI 距平模板
// =========================

// 研究区
var roi = ee.Geometry.Point([110.0, 40.0]); // 替换

// 加载 MODIS NDVI
// 如果 ID 有变化，请在 GEE Data Catalog 中搜索 MOD13Q1
var modis = ee.ImageCollection('MODIS/061/MOD13Q1')
              .select('NDVI')
              .map(function(image){
                return image.multiply(0.0001)
                            .copyProperties(image, ['system:time_start']);
              });

// 年份列表
var years = ee.List.sequence(2015, 2025);

// 生成每年生长季均值 NDVI
function annualNDVI(year) {
  var start = ee.Date.fromYMD(year, 5, 1);
  var end = ee.Date.fromYMD(year, 9, 30).advance(1, 'day');

  var img = modis.filterDate(start, end)
                 .mean()
                 .rename('NDVI')
                 .set('year', year)
                 .set('system:time_start', start.millis());

  return img;
}

var annual = ee.ImageCollection(years.map(annualNDVI));

// 基线：2015—2020
var baseline = annual.filterDate('2015-01-01', '2021-01-01');

var mean = baseline.mean().rename('NDVI_mean');

var std = baseline.reduce(ee.Reducer.stdDev()).select('NDVI_stdDev');

// 计算标准化距平
function anomaly(image) {
  var z = image.subtract(mean).divide(std).rename('NDVI_z');
  return z.set('year', image.get('year'));
}

var anomalyCollection = annual.map(anomaly);

// 显示某一年
var yearTarget = 2023;

var zYear = anomalyCollection
              .filter(ee.Filter.eq('year', yearTarget))
              .first()
              .clip(roi);

Map.centerObject(roi, 7);

Map.addLayer(zYear, {
  min: -2,
  max: 2,
  palette: ['red', 'white', 'green']
}, 'NDVI anomaly ' + yearTarget);
```

---

## 模板3：CHIRPS 生长季降水

```javascript
// =========================
// CHIRPS 生长季降水模板
// =========================

var roi = ee.Geometry.Point([110.0, 40.0]); // 替换

var chirps = ee.ImageCollection('UCSB-CHG/CHIRPS/PENTAD')
              .filterBounds(roi)
              .filterDate('2023-05-01', '2023-09-30');

var rain = chirps.sum().rename('rain_sum');

Map.addLayer(rain, {
  min: 100,
  max: 500,
  palette: ['white', 'blue']
}, 'Growing season rainfall');

var stats = rain.reduceRegion({
  reducer: ee.Reducer.mean(),
  geometry: roi,
  scale: 5000,
  maxPixels: 1e13
});

print('Rainfall mean:', stats);
```

---

# 十、你应该重点掌握的数据集

## 1. 光学精细数据

### Sentinel-2

```text
COPERNICUS/S2_SR_HARMONIZED
```

适合：

- 荒漠草原斑块监测
- 灌木林精细结构初步分析
- 10 m 尺度制图

### Landsat

```text
LANDSAT/LC08/C02/T1_L2
LANDSAT/LC09/C02/T1_L2
LANDSAT/LE07/C02/T1_L2
LANDSAT/LT05/C02/T1_L2
```

适合：

- 1984 以来长时序
- 30 m 尺度变化检测
- 退化趋势分析

---

## 2. 长时间序列数据

### MODIS

```text
MODIS/061/MOD13Q1
MODIS/061/MOD13A2
```

适合：

- 区域尺度植被趋势
- 干旱响应
- 多年基线

### VIIRS

搜索：

```text
VIIRS NDVI Earth Engine
```

适合：

- MODIS 之后延续时序
- 2012 以后监测

---

## 3. 降水与干旱

### CHIRPS

```text
UCSB-CHG/CHIRPS/PENTAD
```

适合：

- 降水趋势
- 降水距平
- 干旱区植被响应

### ERA5-Land

```text
ECMWF/ERA5_LAND/DAILY_AGGR
```

适合：

- 温度
- 土壤水分
- 蒸散发相关变量

### TerraClimate

```text
IDAHO_EPSCOR/TERRACLIMATE
```

适合：

- PET
- 水分亏缺
- 干旱相关指标

---

## 4. 地形与辅助数据

### DEM

```text
USGS/SRTMGL1_003
COPERNICUS/DEM/GLO30
```

适合：

- 高程
- 坡度
- 坡向
- 地形对植被分异解释

### ESA WorldCover

```text
ESA/WorldCover/v200
```

适合：

- 土地覆盖参考
- 辅助分类
- 验证参考

### JRC Global Surface Water

```text
JRC/GSW1_4/GlobalSurfaceWater
```

适合：

- 水体识别
- 季节性水体

---

# 十一、荒漠草原/荒漠灌木林方向的关键科研能力

如果你想到研究生中等偏上水平，下面这些能力非常重要。

---

## 1. 能解释 NDVI 的局限性

你要能说出：

- NDVI 在低覆盖区受土壤背景影响
- NDVI 对稀疏植被敏感度有限
- NDVI 容易受大气和云影响
- NDVI 不能直接区分灌木和草本
- NDVI 高不一定代表生态健康，可能是短期降雨后草本返青

---

## 2. 能说明 SAVI/MSAVI 的价值

干旱区裸土多，土壤调节指数通常更稳。  
你需要能解释：

```text
SAVI 通过 L 参数降低土壤背景影响
MSAVI 进一步减少土壤噪声
```

---

## 3. 能设计生长季

不同区域生长季不同。  
你需要说明：

- 为什么选 5—9 月？
- 为什么不用全年？
- 是否考虑雨季？
- 是否考虑物候峰值？

---

## 4. 能处理降水滞后

例如：

```text
当年 NDVI 可能与前一年秋季降水有关
可能与当月前 2—4 周降水有关
```

这是干旱区植被研究的关键点。

---

## 5. 能谨慎解释“退化”

不能看到 NDVI 下降就说退化。  
必须考虑：

- 年际降水差异
- 季节差异
- 物候差异
- 传感器变化
- 云影响
- 样本代表性

---

# 十二、职业项目交付标准

你以后做项目，不能只交一张图。  
建议按以下标准交付。

---

## 1. 项目文档

包含：

```text
项目背景
研究区
数据源
时间范围
方法流程
关键参数
结果解释
精度/不确定性说明
```

---

## 2. 代码规范

每个脚本最好有：

```javascript
// 作者：
// 日期：
// 目标：
// 输入：
// 输出：
// 说明：
```

---

## 3. 成果文件

建议文件夹：

```text
project/
│
├── code/
│   ├── 01_data.js
│   ├── 02_indices.js
│   ├── 03_anomaly.js
│   └── 04_export.js
│
├── data/
│   ├── roi.geojson
│   └── samples/
│
├── outputs/
│   ├── ndvi_2023.tif
│   ├── savi_2023.tif
│   ├── anomaly_2023.tif
│   └── stats.csv
│
├── figures/
│   ├── map1.png
│   └── time_series.png
│
└── README.md
```

---

## 4. README 模板

```markdown
# 荒漠草原植被生长监测项目

## 研究区
某某区域。

## 数据源
Sentinel-2 SR、MODIS NDVI、CHIRPS。

## 时间范围
2018—2024 年生长季。

## 方法
1. Sentinel-2 云掩膜
2. 生长季 median 合成
3. NDVI/SAVI/EVI 计算
4. 多年基线构建
5. NDVI 距平计算
6. CHIRPS 降水统计
7. 导出结果

## 输出
- ndvi_2023.tif
- anomaly_2023.tif
- stats.csv

## 局限性
- 缺少地面样地验证
- 降水滞后效应未完全建模
- 灌木/草地尚未精细区分
```

---

# 十三、学习资源推荐

---

## 1. 官方必读

### Google Earth Engine Developer Guide

搜索：

```text
Google Earth Engine Developer Guide
```

重点看：

- Image
- ImageCollection
- Reducers
- Classification
- Time Series
- Export

### Earth Engine Data Catalog

搜索：

```text
Google Earth Engine Data Catalog
```

重点查：

- Sentinel-2
- Landsat
- MODIS
- CHIRPS
- ERA5-Land
- TerraClimate
- SRTM
- ESA WorldCover

---

## 2. geemap

适合 Python 学习和可视化。

搜索：

```text
geemap Earth Engine
```

GitHub 关键词：

```text
gee-community geemap
```

---

## 3. Awesome GEE

搜索：

```text
Awesome Google Earth Engine
```

适合找案例脚本。

---

## 4. 干旱区植被遥感学习关键词

你后续查论文、教程时可以搜：

```text
dryland vegetation remote sensing
rangeland monitoring NDVI
shrubland mapping remote sensing
SAVI MSAVI dryland
precipitation vegetation response
NDVI anomaly drought
vegetation trend analysis dryland
residual NDVI degradation
dryland phenology remote sensing
```

---

## 5. 建议阅读的方法主题

### 指数类

```text
NDVI limitations soil background
SAVI soil adjusted vegetation index
MSAVI modified soil adjusted vegetation index
EVI enhanced vegetation index
```

### 干旱监测类

```text
vegetation condition index
NDVI anomaly drought monitoring
precipitation use efficiency
dryland vegetation response to rainfall
```

### 趋势与退化类

```text
NDVI trend analysis
Theil-Sen trend remote sensing
Mann-Kendall trend test
residual NDVI land degradation
```

### 精度验证类

```text
land cover classification accuracy assessment
confusion matrix overall accuracy kappa
Olofsson good practices accuracy assessment
```

---

# 十四、你的 30 天里程碑检查表

---

## 第7天应该能做到

- 会加载 Sentinel-2
- 会计算 NDVI/SAVI/EVI
- 会做区域统计
- 会导出 GeoTIFF
- 能解释 NDVI 在荒漠草原的局限

---

## 第14天应该能做到

- 会云掩膜
- 会生长季合成
- 会多年 NDVI/SAVI 合成
- 会提取时序 CSV
- 会加载 CHIRPS/ERA5 数据

---

## 第21天应该能做到

- 会计算 NDVI/SAVI 距平
- 会做 VCI 或标准化距平
- 会分析降水与植被关系
- 会做简单分类
- 能解释干旱区趋势分析风险

---

## 第30天应该能做到

- 独立完成一个植被监测项目
- 有代码、图、表、README
- 能说明方法、误差、局限性
- 能把结果导入 ArcGIS Pro 制图
- 能把项目写成简历/科研材料

---

# 十五、我建议你今天立刻做的三件事

## 1. 建一个 GitHub 仓库

名字可以叫：

```text
dryland-vegetation-gee
```

或者：

```text
gee-vegetation-monitoring
```

---

## 2. 在 GEE Code Editor 里跑通模板1

先用我上面给的：

```text
模板1：Sentinel-2 NDVI/SAVI/EVI
```

把 `roi` 改成你的研究区。

---

## 3. 整理你的研究区矢量

如果你已经在 ArcGIS Pro/ENVI 里有 ROI：

1. 导出为 Shapefile 或 GeoJSON  
2. 上传到 GEE Assets  
3. 在 GEE 中加载  
4. 替代代码里的 `roi`

这样你马上就能进入自己的真实项目。

---

# 十六、后续你可以随时问我

你可以直接把下面这些问题发给我：

## 代码类

```text
这段 GEE 代码报错：xxx
我想实现 xxx，怎么改？
如何把这段 JS 改成 Python？
```

## 方法类

```text
荒漠草原 NDVI 和 SAVI 哪个更合适？
我的研究区生长季应该怎么定？
如何用降水滞后分析植被响应？
NDVI 下降能否解释为退化？
```

## 项目类

```text
帮我设计一个荒漠草原植被监测项目。
我想做一个简历项目，如何包装？
如何把这个 GEE 项目写成论文方法？
```

## 数据类

```text
Sentinel-2 和 MODIS 应该怎么选？
我的研究区面积小，是否适合 MODIS？
Landsat 和 Sentinel-2 哪个更适合长时序？
```

---

# 十七、下一步我需要你给我的信息

为了进一步给你定制代码和计划，你可以回复我以下任一项：

1. **你的研究区大概位置**  
   例如：某盆地、某旗县、某保护区、经纬度范围。

2. **你关心的核心问题**  
   例如：
   - 植被长势年际变化
   - 退化/恢复评估
   - 降水响应
   - 灌木林/草地分类
   - 覆盖度估算
   - 物候变化

3. **你现有数据**  
   例如：
   - 已有 ROI
   - 已有样地
   - 已有 ArcGIS 矢量
   - 只有研究区想法

4. **你每天可学习时间**  
   例如：
   - 2小时/天
   - 4小时/天
   - 6小时/天

你回复后，我可以继续给你：  
**“你的研究区专属 GEE 代码模板 + 每日任务拆解 + 项目成果清单”。**
