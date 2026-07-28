---
title: "教程 1：测试笔记"
nav_order: 1
---

# 教程 1：初识 GEE 编辑器

欢迎来到我的 Google Earth Engine 学习笔记！

## 核心概念

GEE 的核心是 **Image** (影像) 和 **ImageCollection** (影像集合)。

## 代码示例

这是一个简单的 JavaScript 代码，用于加载并显示一幅 Landsat 影像：

```javascript
var image = ee.Image('LANDSAT/LC08/C02/T1_TOA/LC08_123032_20140515');
Map.centerObject(image, 9);
Map.addLayer(image, {bands: ['B4', 'B3', 'B2'], max: 0.3}, 'Landsat 8');

文本文本123123*123123*~123123~
