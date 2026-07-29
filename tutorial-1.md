---
layout: default
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

我们来一步步复刻一个和你截图中一模一样的笔记网站。整个过程不需要你写代码，全程在浏览器里操作，很简单。

这套方案的核心是使用一个叫做 **Just the Docs Template** 的模板，它已经把所有复杂的技术配置都做好了。

### 第一步：创建属于你的网站仓库

1.  **找到模板**：在浏览器中打开 Just the Docs 的官方模板页面：[https://just-the-docs.github.io/just-the-docs-template/](https://just-the-docs.github.io/just-the-docs-template/)。
2.  **使用模板**：点击页面右上角绿色的 **`Use this template`** 按钮，然后在下拉菜单中选择 **`Create a new repository`**。

    *   **这一步是魔法所在**：它不会复制代码，而是会创建一个全新的仓库，但内容完全基于这个模板，相当于你“继承”了所有设置。

3.  **填写仓库信息**：你会被带到 GitHub 的新建仓库页面。
    *   **Repository name (仓库名称)**：这里比较关键。如果你希望网站的地址是 `你的用户名.github.io`，那仓库名就必须**一字不差**地写成 `你的用户名.github.io`。这是 GitHub Pages 的默认规则。
    *   如果你写的是其他名字，比如 `my-notes`，那么网站地址就会是 `你的用户名.github.io/my-notes`。可以按你的喜好来。
    *   **Description (描述)**：可以给你的笔记网站写一句简短的介绍，也可以不填。
    *   **Public / Private**：**一定要选择 `Public`**。因为 GitHub Pages 服务只能公开访问私有仓库需要付费。
4.  **创建仓库**：点击绿色的 **`Create repository`** 按钮，你的网站仓库就建好了。

### 第二步：让网站上线

新仓库创建好后，我们只需要告诉 GitHub 自动帮我们把网站建好并发布出去。

1.  **进入设置**：在你新仓库的页面，点击顶部的 **`Settings`** (设置) 标签页。
2.  **找到 Pages 设置**：在左侧的菜单栏里，找到 **`Pages`** 选项并点击。
3.  **选择构建方式**：在 “Build and deployment” (构建和部署) 区域，会有一个 **`Source`** (源) 的下拉菜单。点击它，选择 **`GitHub Actions`**。
4.  **完成**：选择之后，设置会自动保存。现在，GitHub 的服务器已经在后台开始为你构建网站了。

### 第三步：修改一点配置

为了让网站的标题、描述等显示为你自己的内容，需要修改一个配置文件。

1.  **回到仓库首页**：点击顶部的 **`Code`** (代码) 标签页，回到仓库的文件列表。
2.  **找到配置文件**：在文件列表里，找到并点击 **`_config.yml`** 这个文件。
3.  **开始编辑**：点击文件内容右上角的 **铅笔图标 (Edit this file)**，进入编辑模式。
4.  **修改关键信息**：你只需要关注文件开头的这几行：

    ```yaml
    # 网站的标题，会显示在浏览器标签栏
    title: "我的 GEE 学习笔记"
    # 网站的描述
    description: "记录 Google Earth Engine 的学习心得"
    # 网站的网址，非常重要！格式必须正确
    url: "https://你的用户名.github.io" 
    ```

    *   **`url` 字段**：**必须修改！** 如果你的仓库名是 `用户名.github.io`，这里的值就写 `https://你的用户名.github.io`。如果你的仓库名是 `my-notes`，这里就写 `https://你的用户名.github.io/my-notes`。
5.  **保存修改**：修改完后，滚动到页面最下方，在 “Commit changes” (提交更改) 区域，可以简单写个备注，比如“更新网站标题和网址”。然后点击绿色的 **`Commit changes`** 按钮。

### 第四步：开始写你的第一篇笔记

现在，激动人心的时刻到了！我们来创建第一个教程页面。

1.  **回到仓库首页**，点击 **`Add file`** (添加文件) 下拉菜单，选择 **`Create new file`** (创建新文件)。
2.  **命名文件**：在页面顶部的文件名输入框里，输入 `tutorial-1.md`。`.md` 是 Markdown 文件的后缀，你的所有教程都将以这种格式编写。
3.  **添加“前页”信息**：在内容输入区，粘贴以下内容。这是 Jekyll 需要的“前页”(Front Matter)，用来定义这个页面的标题和它在目录里的顺序。

    ```
    ---
    title: "教程 1：初识 GEE 编辑器"
    nav_order: 1
    ---
    ```

4.  **写你的正文**：紧接着上面的内容，用 Markdown 语法开始写你的笔记。比如：

    ```markdown
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
    ```
    ```

5.  **保存你的笔记**：滚动到页面下方，和上一步一样，写好备注后，点击绿色的 **`Commit changes`** 按钮。

### 见证奇迹的时刻

保存好文件后，耐心等待一两分钟。然后，在浏览器里输入你的网站地址（比如 `你的用户名.github.io` 或 `你的用户名.github.io/my-notes`），刷新一下，你就能看到属于自己的、和截图里一样的笔记网站了！

之后，每当你需要添加新教程，就重复第四步的操作，在仓库里创建一个新的 `.md` 文件即可。

---

### 如果你想让本地预览更流畅（可选）

直接在网页上编辑很方便，但如果能写完马上在本地预览效果，体验会更好。这需要一点点命令行操作。

1.  **在电脑上安装 Jekyll**：这需要先安装 Ruby，具体步骤可以搜索 “Jekyll 安装教程”。
2.  **将你的仓库克隆到电脑**：使用 `git clone` 命令。
3.  **安装依赖并启动**：在项目文件夹下，依次运行 `bundle install` 和 `bundle exec jekyll serve`。之后在浏览器打开 `http://localhost:4000` 就能看到实时预览了。

这个“本地预览”的配置是一次性的，但对后续写作体验提升巨大。如果过程中遇到问题，可以随时搜索相关报错信息，或者查阅 Just the Docs 的官方文档来定制更多功能。
