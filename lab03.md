---
layout: default
title: "lab 03"
nav_order: 3
---

# 教程 3

植被指数计算（NDVI/EVI）

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
