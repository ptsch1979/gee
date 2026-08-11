---
layout: default
title: "sup 03"
nav_order: 101
---

# 补充 3

关于配色不可能也不需要记的解决方案


## 方案一
Google Earth Engine 的 `palette` 参数**直接支持英文单词**，完全不用写那串难记的代码。

```javascript
// 示例：NDVI 配色（谁都能看懂）
var ndviPalette = {
  min: -0.2,
  max: 0.8,
  palette: [
    'brown',      // 裸地/水体
    'yellow',     // 过渡区
    'limegreen',  // 一般植被
    'darkgreen'   // 茂密森林
  ]
};
```

## 常用颜色词

| 英文  | 中文 | 英文      | 中文 |
|-------|------|-----------|------|
| red   | 红   | green     | 绿   |
| blue  | 蓝   | yellow    | 黄   |
| brown | 棕   | cyan      | 青   |
| orange| 橙   | purple    | 紫   |
| pink  | 粉   | limegreen | 亮绿 |
| darkgreen | 深绿 | lightgray | 浅灰 |

## 方案二
把长串颜色码扔在代码最顶部定义为变量，后面直接调用变量名即可。

```javascript
// 在代码顶部定义一次（直接找网站复制粘贴，不用背）
var ndviColors = [
    '#d73027', '#f46d43', '#fdae61', '#fee08b', 
    '#d9ef8b', '#a6d96a', '#66bd63', '#1a9850', '#006837'
];

// 使用时只需调用变量名
Map.addLayer(ndvi, {min: -0.2, max: 0.8, palette: ndviColors}, 'NDVI');
```
网站 `https://colorbrewer2.org/`

## 方案三
只用 3 个颜色，电脑自动渐变
```javascript
// 只需记住 3 个关键词：棕色 → 黄色 → 深绿
palette: ['brown', 'yellow', 'darkgreen']
```

## 方案四
使用 ColorBrewer 专业配色（推荐）
```javascript
// RdYlGn = Red-Yellow-Green（红-黄-绿），NDVI 最常用方案
var palette = ['#d73027', '#fc8d59', '#fee08b', '#d9ef8b', '#91cf60', '#1a9850'];
```

## 实际建议

1. 创建个人配色库：在 GEE 脚本中专门建一个文件（或放在代码最顶部），存放你常用的调色板变量。

2. 随用随搜：在 Google 搜索 "NDVI color palette hex"，直接复制粘贴。

3. 记住 3 个关键色：棕色（差）、黄色（中）、绿色（好），其他交给电脑自动渐变。
