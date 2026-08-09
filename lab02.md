---
layout: default
title: "教程 2：测试笔记"
nav_order: 2
---

# 教程 2
    ## 代码示例

    这是一个简单的 JavaScript 代码，用于加载并显示一幅 Landsat 影像：

    ```javascript
    var image = ee.Image('LANDSAT/LC08/C02/T1_TOA/LC08_123032_20140515');
    Map.centerObject(image, 9);
    Map.addLayer(image, {bands: ['B4', 'B3', 'B2'], max: 0.3}, 'Landsat 8');

