---
layout: default
title: "lab 02"
nav_order: 2
---

# 教程 2
Landsat/Sentinel数据调用
定义研究区（AOI）并加载影像
    
## 原则
活人研究，我们不是机器：先画AOI，再写代码，不用记经纬度！

## 1. 第一步：手动绘制并定义研究区（AOI）

1. 点击左侧工具栏的 **“几何图形（Geometry）”** 倒三角。
2. 选择 **`Rectangle`（矩形）** 或 **`Polygon`（多边形）**。
3. 在地图上按住鼠标左键，拖拽框出你想研究的区域（例如 Syracuse）。
4. 在代码编辑器顶部的 **“Imports”** 区域，点击默认名称（如 `geometry`），将其重命名为 **`aoi`**（Area Of Interest 缩写）。

## 2. 最终核心代码

将以下代码复制到 GEE 代码编辑器中运行：

```javascript
// 1. 加载 Landsat 9 影像集合（用 aoi 筛选，不用写坐标！）
var collection = ee.ImageCollection('LANDSAT/LC09/C02/T1_TOA')
  .filterBounds(aoi)               // 筛选出覆盖你画的那个框的影像
  .filterDate('2025-06-01', '2025-08-01') // 筛选日期范围
  .sort('CLOUD_COVER');            // 按云量从小到大排序（最清晰的排最前）

// 2. 取出云量最少的第一张影像
var myimage = collection.first();

// 3. 地图自动跳转到你画的框的中心（不用写坐标！）
Map.centerObject(aoi, 10);        // 数字 10 是缩放级别（可调，越大越近）

// 4. 将真彩色影像添加到地图上
Map.addLayer(myimage, {
  bands: ['B4', 'B3', 'B2'],      // Landsat 9 的 红、绿、蓝 波段
  min: 0,
  max: 0.3
}, '我的真彩色影像');
