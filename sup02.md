---
layout: default
title: "sup 02"
nav_order: 100
---

# 补充 2

# Landsat 9 与 Sentinel-2 波段详细对比

虽然 Landsat 9 （兰萨特/美国陆地卫星）和 Sentinel-2 （哨兵/欧盟哥白尼哨兵）都是非常优秀的光学陆地观测卫星，但它们的波段数量、设置和侧重点并不完全一致。Landsat 9 共有 **11 个波段**，而 Sentinel-2 共有 **13 个波段**。它们在 **7 个核心波段** 上高度对齐，可以互相兼容使用，但各自也拥有独特的专属波段。

## 详细波段对比表

| 波段类型 (用途) | Landsat 9 (OLI-2/TIRS-2) | Sentinel-2 (MSI) | 主要区别与说明 |
| :--- | :--- | :--- | :--- |
| **沿海 / 气溶胶** | **Band 1** (0.43–0.45 μm) <br> 分辨率: 30 m | **Band 1** (0.443 μm) <br> 分辨率: 60 m | 两者都有，主要用于大气校正和水体监测。Landsat 9 分辨率更高。 |
| **蓝色 (Blue)** | **Band 2** (0.45–0.51 μm) <br> 分辨率: 30 m | **Band 2** (0.490 μm) <br> 分辨率: 10 m | **核心对齐波段**。用于水深探测和地物分类，Sentinel-2 分辨率更精细。 |
| **绿色 (Green)** | **Band 3** (0.53–0.59 μm) <br> 分辨率: 30 m | **Band 3** (0.560 μm) <br> 分辨率: 10 m | **核心对齐波段**。用于植被健康和农业评估，Sentinel-2 分辨率更精细。 |
| **红色 (Red)** | **Band 4** (0.64–0.67 μm) <br> 分辨率: 30 m | **Band 4** (0.665 μm) <br> 分辨率: 10 m | **核心对齐波段**。用于植被分类和叶绿素吸收，Sentinel-2 分辨率更精细。 |
| **近红外 (NIR)** | **Band 5** (0.85–0.88 μm) <br> 分辨率: 30 m | **Band 8** (0.842 μm) <br> 分辨率: 10 m | **核心对齐波段**。用于植被生物量估算，Sentinel-2 分辨率更精细。 |
| **短波红外 1 (SWIR-1)** | **Band 6** (1.57–1.65 μm) <br> 分辨率: 30 m | **Band 11** (1.610 μm) <br> 分辨率: 20 m | **核心对齐波段**。用于土壤湿度和矿物识别，Sentinel-2 分辨率更精细。 |
| **短波红外 2 (SWIR-2)** | **Band 7** (2.11–2.29 μm) <br> 分辨率: 30 m | **Band 12** (2.190 μm) <br> 分辨率: 20 m | **核心对齐波段**。用于地质制图和土壤含水量评估，Sentinel-2 分辨率更精细。 |
| **卷云 (Cirrus)** | **Band 9** (1.36–1.38 μm) <br> 分辨率: 30 m | **Band 10** (1.375 μm) <br> 分辨率: 60 m | 两者都有，用于探测高空薄卷云，辅助大气校正。 |
| **全色 (Pan)** | **Band 8** (0.50–0.68 μm) <br> 分辨率: **15 m** | **无** | **Landsat 9 独有**。用于影像锐化，将多光谱波段融合至 15 米以提高清晰度。 |
| **热红外 1 (TIRS-1)** | **Band 10** (10.6–11.19 μm) <br> 分辨率: 100 m → 30 m | **无** | **Landsat 9 独有**。用于地表温度反演、城市热岛效应研究。 |
| **热红外 2 (TIRS-2)** | **Band 11** (11.50–12.51 μm) <br> 分辨率: 100 m → 30 m | **无** | **Landsat 9 独有**。用于地表温度反演和热辐射监测。 |
| **红边 1 (Red Edge 1)** | **无** | **Band 5** (0.705 μm) <br> 分辨率: 20 m | **Sentinel-2 独有**。对监测植被健康、作物胁迫非常关键。 |
| **红边 2 (Red Edge 2)** | **无** | **Band 6** (0.740 μm) <br> 分辨率: 20 m | **Sentinel-2 独有**。用于分析植被叶绿素含量和生理状态。 |
| **红边 3 (Red Edge 3)** | **无** | **Band 7** (0.783 μm) <br> 分辨率: 20 m | **Sentinel-2 独有**。用于分析植被冠层结构和生物量。 |
| **窄近红外 (NIR narrow)** | **无** | **Band 8A** (0.865 μm) <br> 分辨率: 20 m | **Sentinel-2 独有**。用于植被含水量分析和大气校正。 |
| **水蒸气 (Water vapour)** | **无** | **Band 9** (0.940 μm) <br> 分辨率: 60 m | **Sentinel-2 独有**。用于大气水汽含量反演和大气校正。 |

---

## 核心差异总结

两者在设计上并非相互替代，而是天然互补，形成“1+1>2”的合作关系。主要区别体现在以下四点：

1.  **波段数量不同**：Landsat 9 拥有 11 个波段，而 Sentinel-2 拥有 13 个波段。
2.  **核心功能互补**：
    - **Landsat 9 的独家优势**：拥有 **全色波段 (Band 8)** 和 **两个热红外波段 (Band 10, 11)**。这使得它在影像锐化和地表温度、热辐射研究方面具有不可替代的地位。
    - **Sentinel-2 的独家优势**：拥有 **三个红边波段 (Band 5, 6, 7)**、**窄带近红外 (Band 8A)** 和 **水蒸气波段 (Band 9)**。这使得它在精细农业、植被胁迫监测和大气校正方面更加专业。
3.  **空间分辨率有别**：在共有的 7 个核心波段上，Sentinel-2 的分辨率（可见光/近红外为 10 m，短波红外为 20 m）通常优于 Landsat 9（统一为 30 m）。
4.  **数据连续性**：两者的轨道设计相互协调，且核心波段的光谱响应函数高度兼容，目的是方便科研人员将两套数据联合使用，大幅提高时间分辨率（重访周期可缩短至 2-3 天）。

## 网站阅读

笔记是静态的。最新动态可以参考官方权威网站：
### 🛰️ Landsat 9（美国陆地卫星9号）

Landsat 9 由美国国家航空航天局（NASA）和美国地质调查局（USGS）联合管理。

#### 官方任务主页

| 机构 | 网站 | 说明 |
| :--- | :--- | :--- |
| **NASA** | [landsat.gsfc.nasa.gov](https://landsat.gsfc.nasa.gov/) | NASA Landsat 科学主页，提供任务概述、最新动态、科学应用和图片展廊。 |
| **USGS** | [www.usgs.gov/landsat](https://www.usgs.gov/landsat) | USGS Landsat 任务网站，侧重于数据、技术文档和项目管理的官方信息源。 |

#### 权威技术参数与文档

| 网站 | 说明 |
| :--- | :--- |
| [USGS 波段设计说明](https://www.usgs.gov/faqs/what-are-band-designations-landsat-satellites) | 关于 Landsat 各卫星波段设计的官方 FAQ 说明页面。 |
| [Landsat 9 数据用户手册](https://pubs.usgs.gov/fs/2019/3008/fs20193008.pdf) | USGS 官方发布的数据手册，包含仪器、波段等核心参数（PDF）。 |
| [ESA Earth Online - Landsat 9](https://earth.esa.int/eogateway/missions/landsat-9) | 欧洲航天局（ESA）维护的页面，提供轨道、重访周期等任务详细信息。 |

#### 数据访问平台

| 网站 | 说明 |
| :--- | :--- |
| [USGS EarthExplorer](https://earthexplorer.usgs.gov/) | USGS 官方数据门户，可搜索、下载 Landsat 等遥感数据。 |
| [Google Earth Engine](https://developers.google.com/earth-engine) | 强大的云端地理分析平台，集成了 Landsat 等海量遥感数据集供在线分析。 |

---

### 🛰️ Sentinel-2（欧盟哨兵2号）

Sentinel-2 是欧盟哥白尼计划（Copernicus Programme）的一部分，由欧洲航天局（ESA）负责研发和运营。

#### 官方任务主页

| 机构 | 网站 | 说明 |
| :--- | :--- | :--- |
| **ESA** | [www.esa.int/Applications/Observing_the_Earth/Copernicus/Sentinel-2](https://www.esa.int/Applications/Observing_the_Earth/Copernicus/Sentinel-2) | ESA 官网关于 Sentinel-2 任务的介绍页面。 |
| **Copernicus** | [sentinels.copernicus.eu](https://sentinels.copernicus.eu/) | 哥白尼计划的 Sentinel 卫星专门信息网站。 |
| **Copernicus 数据空间** | [dataspace.copernicus.eu](https://dataspace.copernicus.eu/) | 哥白尼计划官方数据平台，提供数据访问、文档和工具。 |

#### 权威技术文档与 Wiki

| 网站 | 说明 |
| :--- | :--- |
| [Sentinel-2 任务文档 Wiki](https://sentiwiki.copernicus.eu/) | 哥白尼计划的官方 Wiki，包含任务、产品、算法等详细技术文档。 |
| [ESA Earth Online - Sentinel-2](https://earth.esa.int/eogateway/missions/sentinel-2) | 提供 Sentinel-2 的任务概览、参数和新闻。 |

#### 数据访问平台

| 网站 | 说明 |
| :--- | :--- |
| [Copernicus 数据空间生态系统](https://dataspace.copernicus.eu/) | 官方主要数据访问入口，提供免费数据浏览和下载。 |
| [Google Earth Engine](https://developers.google.com/earth-engine) | 同样集成了完整的 Sentinel-2 数据集，支持在线分析。 |

---

### 💡 快速查阅建议

- **查询权威技术参数**：首选 USGS（Landsat）和 ESA / Copernicus（Sentinel-2）的官方文档。
- **下载原始数据**：推荐 USGS EarthExplorer 或 Copernicus 数据空间。
- **快速浏览或在线分析**：推荐 Google Earth Engine。
