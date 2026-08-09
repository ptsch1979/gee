---
layout: default
title: "lab 01"
nav_order: 1
---

# 教程 1

GEE基础语法入门

**前言**：千里之行始于足下！本笔记不追求大而全的语法手册，采用“最小可行知识”原则，聚焦于GEE最核心的客户端语法。

## 1. GEE 的原则

在 Google Earth Engine 的 Code Editor 中编程，本质上不是写一段简单顺序执行的脚本，而是构建一个发送到云端并行执行的计算图。

（1）禁止无脑 for/while/if else
- 大部分地理空间数据都是服务器端对象，这些对象只是云端的“代理”，必须通过 Earth Engine 提供的服务器端函数。

（2）使用函数式编程 (map/filter/reduce)
- GEE禁止写客户端循环，会引发阻塞卡死。处理集合的唯一高效途径就是向集合本身描述“对每个元素做什么”，而不是自己逐一迭代。

（3）先可视化交互，再严谨导出
- GEE是惰性求值。代码只是构建计算图，真正执行只在 Map.addLayer（瓦片请求）、print、Export 和 getInfo 时发生。
- 交互浏览时，用预合成的轻量图层保证流畅；最终交付时，用导出任务承受高精度、大计算量。


## 2. 变量与数据类型（够用版）

| 类型 | 示例 | 说明 |
|------|------|------|
| 数字 | `var year = 2020;` | 常规数字 |
| 字符串 | `var name = 'Forest';` | 用引号 |
| 列表 | `var bands = ['B2','B3','B4'];` | 0索引，用`get(0)`获取 |
| 字典 | `var param = {start: '2020-01-01', end: '2020-12-31'};` | 键值对，用`get('start')`获取 |
| **ee.Number** | `var num = ee.Number(10);` | **GEE地球引擎数字**（用于计算） |
| **ee.String** | `var str = ee.String('plot');` | **GEE字符串** |
| **ee.List** | `var list = ee.List([1,2,3]);` | **GEE列表** |
| **ee.Dictionary**| `var dict = ee.Dictionary({a:1, b:2});` | **GEE字典** |

**⚠️ 重要**：GEE服务器端的对象（带`ee.`前缀）和客户端JavaScript对象（不带前缀）**不能混用计算**！

```javascript
// ❌ 错误写法
var num1 = 10;
var num2 = ee.Number(20);
var result = num1 + num2;  // 报错！

// ✅ 正确写法
var num1 = ee.Number(10);
var num2 = ee.Number(20);
var result = num1.add(num2);  // 使用ee.Number的方法
print('结果', result);
```

## 3. 地理空间核心对象（三巨头）
### 3.1 ee.Image —— 单时相影像

```javascript
// 加载一幅Landsat 8影像
var image = ee.Image('LANDSAT/LC08/C02/T1_TOA/LC08_123032_20200101');

// 查看波段信息
print('波段列表', image.bandNames());

// 选择特定波段（森林工程常用）
var rgb = image.select(['B4', 'B3', 'B2']);  // 红绿蓝
var nir = image.select('B5');  // 近红外
```

### 3.2 ee.ImageCollection —— 影像集合（时间序列基础）

```javascript
// 加载2020年所有Landsat 8影像（研究区过滤在后面）
var collection = ee.ImageCollection('LANDSAT/LC08/C02/T1_TOA')
  .filterDate('2020-01-01', '2020-12-31');

// 查看有多少景影像
print('影像数量', collection.size());

// 合成一张中值影像（去云常用）
var medianImage = collection.median();
```

### 3.3 ee.Geometry 与 ee.Feature —— 形状和属性：空间范围

```javascript
// 点（森林样地）
var point = ee.Geometry.Point([116.4, 39.9]);

// 矩形（研究区边界）
var roi = ee.Geometry.Rectangle([116.3, 39.8, 116.5, 40.0]);

// 导入外部矢量（面）
var forestArea = ee.FeatureCollection('users/yourname/forest_boundary');
```

## 4. 最常用操作
### 4.1 研究区裁剪（clip）
```javascript
// 将影像裁剪到你的研究区
var clipped = image.clip(roi);
```

### 4.2 波段计算（NDVI示例）
```javascript
// 方法一：使用表达式（推荐，可读性强）
var ndvi = image.expression(
  '(NIR - RED) / (NIR + RED)',
  {
    'NIR': image.select('B5'),
    'RED': image.select('B4')
  }
);

// 方法二：使用normalizedDifference（更简洁，Landsat专用）
var ndvi2 = image.normalizedDifference(['B5', 'B4']);
```

### 4.3 影像集合的映射操作（批量处理）
```javascript
// 给集合中的每一景影像计算NDVI
var collectionWithNDVI = collection.map(function(img) {
  var ndvi = img.normalizedDifference(['B5', 'B4']);
  return img.addBands(ndvi.rename('NDVI'));  // 将NDVI作为新波段添加
});
```

### 4.4 影像集合的筛选与排序
```javascript
// 筛选云量小于20%的影像
var clean = collection.filter(ee.Filter.lt('CLOUD_COVER', 20));

// 按云量排序，取最清晰的一景
var best = collection.sort('CLOUD_COVER').first();
```

## 5. 可视化
```javascript
// 定义可视化参数（Landsat 8标准假彩色）
var visParams = {
  bands: ['B5', 'B4', 'B3'],  // 近红外、红、绿 -> 植被呈红色
  min: 0,
  max: 0.3,
  gamma: 1.2
};

// 添加到地图
Map.centerObject(roi, 10);  // 缩放到研究区，级别10
Map.addLayer(clipped, visParams, 'Landsat 8 假彩色');

// 显示NDVI（绿色为高植被）
var ndviVis = {min: -1, max: 1, palette: ['blue', 'white', 'green']};
Map.addLayer(ndvi, ndviVis, 'NDVI');
```

## 6. 练习
加载2020年6月-9月（生长季）的Landsat 8影像集合，筛选云量<10%的影像，计算中值合成影像，并裁剪到你的研究区；

基于合成的影像，分别计算NDVI和EVI；

用Map.addLayer()分别显示假彩色合成影像、NDVI和EVI，设置合适的配色。

参考代码框架：
```javascript
// 1. 定义研究区（替换为你的矢量或坐标）
var roi = ______;  

// 2. 加载并筛选Landsat 8集合
var collection = ee.ImageCollection('LANDSAT/LC08/C02/T1_TOA')
  .filterDate('______', '______')
  .filter(ee.Filter.lt('CLOUD_COVER', ____));

// 3. 中值合成
var composite = ______;

// 4. 裁剪
var compositeClip = ______;

// 5. 计算NDVI
var ndvi = ______;

// 6. 计算EVI（提示：需要用到B2蓝光波段）
var evi = compositeClip.expression(
  '2.5 * (NIR - RED) / (NIR + 6*RED - 7.5*BLUE + 1)',
  {
    'NIR': ______,
    'RED': ______,
    'BLUE': ______
  }
);

// 7. 可视化（填空调色板）
Map.addLayer(______, ______, 'Composite');
Map.addLayer(______, {min: -1, max: 1, palette: ['______']}, 'NDVI');
Map.addLayer(______, {min: -1, max: 1, palette: ['______']}, 'EVI');
```

填充参考：
```javascript
// 1. 研究区（先在地图上画一个多边形，导入为 table，或直接画 roi）
var roi = table.geometry(); // 或者 ee.Geometry.Rectangle([...])

// 2. 日期和云量
var collection = ee.ImageCollection('LANDSAT/LC08/C02/T1_TOA')
  .filterDate('2020-06-01', '2020-09-30')  // 生长季
  .filter(ee.Filter.lt('CLOUD_COVER', 10)); // 小于10%

// 3. 中值合成 
var composite = collection.median();

// 4. 裁剪研究区
var compositeClip = composite.clip(roi); 

// 5. 计算NDVI（基于裁剪后的影像）
var ndvi = compositeClip.normalizedDifference(['B5', 'B4']);

// 6. 计算EVI（显式传入波段）
var evi = compositeClip.expression(
  '2.5 * (NIR - RED) / (NIR + 6*RED - 7.5*BLUE + 1)',
  {
    'NIR': compositeClip.select('B5'),
    'RED': compositeClip.select('B4'),
    'BLUE': compositeClip.select('B2')
  }
);

// 7. 可视化（别忘了先把地图中心对准研究区）
Map.centerObject(roi, 10); 
Map.addLayer(compositeClip, {bands: ['B5', 'B4', 'B3'], min: 0, max: 0.3}, 'Composite');
Map.addLayer(ndvi, {min: -1, max: 1, palette: ['blue', 'white', 'green']}, 'NDVI');
Map.addLayer(evi, {min: -1, max: 1, palette: ['brown', 'yellow', 'darkgreen']}, 'EVI');
```
