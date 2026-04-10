# Snail Notes Blog - 项目上下文

## 项目概述

**Snail Notes Blog** 是一个基于 **Jekyll** 的静态博客网站，使用 Hux Blog 主题进行定制。该项目托管在 GitHub Pages 上，支持响应式设计、PWA（渐进式 Web 应用）、离线访问等功能。

### 核心技术栈

| 类别 | 技术 |
|------|------|
| **静态网站生成器** | Jekyll 4.x |
| **模板引擎** | Liquid |
| **样式** | Less (编译为 CSS) |
| **构建工具** | Grunt, npm scripts |
| **包管理** | Bundler (Ruby), npm (Node.js) |
| **Markdown** | kramdown (GFM) |
| **代码高亮** | Rouge |
| **部署** | GitHub Pages |

### 项目结构

```
snailnotes/
├── _config.yml          # Jekyll 主配置文件
├── _includes/           # 可重用的 Liquid 模板组件
├── _layouts/            # 页面布局模板
├── _posts/              # 博客文章 (Markdown 格式)
├── _doc/                # 项目文档
├── css/                 # 样式文件 (Less 源文件在 less/)
├── js/                  # JavaScript 文件
├── img/                 # 图片资源
├── fonts/               # 字体文件
├── pwa/                 # PWA 相关配置
├── Gruntfile.js         # Grunt 构建配置
├── package.json         # Node.js 依赖和脚本
├── Gemfile              # Ruby 依赖
└── Rakefile             # Ruby Rake 任务
```

## 构建与运行

### 环境准备

1. **安装 Ruby 和 Bundler**
   ```sh
   # 确保已安装 Ruby (推荐 2.7+)
   ruby -v
   
   # 安装 Bundler
   gem install bundler
   ```

2. **安装 Node.js 依赖** (可选，用于 Grunt 构建)
   ```sh
   npm install
   ```

### 安装依赖

```sh
# 安装 Ruby 依赖
bundle install
```

### 启动本地开发服务器

```sh
# 方式 1: 使用 Bundler (推荐)
bundle exec jekyll serve

# 方式 2: 使用 npm 脚本
npm start

# 访问地址：http://localhost:4000/snailnotes
```

### 开发模式 (自动监听)

```sh
# 同时启动 Grunt 监听和 Jekyll 服务器
npm run dev
```

### 构建任务

```sh
# Grunt 默认任务：压缩 JS、编译 Less、添加许可证头
grunt

# 单独任务
grunt uglify    # 压缩 JavaScript
grunt less      # 编译 Less 到 CSS
grunt watch     # 监听文件变化
```

### 创建新文章

```sh
# 使用 Rake 任务创建新文章
rake post title="文章标题" subtitle="文章副标题"

# 文章将生成在 _posts/ 目录下，格式为：YYYY-MM-DD-标题.md
```

### 部署

```sh
# 推送到主分支
npm run push

# 推送到 boilerplate 分支
npm run boil
```

## 开发规范

### 文章格式 (Front Matter)

每篇文章的开头需要包含 YAML 格式的前置元数据：

```yaml
---
layout:     post
title:      "文章标题"
subtitle:   "文章副标题"
date:       2024-01-01 12:00:00
author:     "作者名"
header-img: "img/post-bg-default.jpg"
catalog: true
tags:
    - 标签 1
    - 标签 2
---
```

**可选配置：**
- `header-style: text` - 使用文本风格的头部
- `mathjax: true` - 启用 LaTeX 数学公式支持
- `header-mask: 0.3` - 头部图片遮罩透明度

### 目录结构约定

| 目录 | 用途 |
|------|------|
| `_includes/` | 可复用的模板片段 (导航栏、页脚、侧边栏等) |
| `_layouts/` | 页面布局模板 (default, post, page, keynote) |
| `_posts/` | 博客文章，按年份组织 |
| `less/` | Less 样式源文件 |
| `css/` | 编译后的 CSS 文件 |
| `js/` | JavaScript 文件 |
| `img/` | 图片资源 |

### 样式开发

- 样式源文件使用 **Less** 编写，位于 `less/` 目录
- 通过 Grunt 的 `less` 任务编译为 CSS
- 代码高亮主题通过修改 `highlight.less` 实现
- 支持响应式设计，基于 Bootstrap Grid System

### Keynote 布局

项目支持嵌入 HTML 演示文稿（如 Reveal.js）：

```yaml
---
layout: keynote
iframe: "https://your-presentation-url.com"
---
```

## 配置说明

### 主要配置项 (`_config.yml`)

```yaml
# 站点设置
title: Snail Notes Blog
SEOTitle: SEO 标题
description: "站点描述"
keyword: "关键词列表"
url: "https://gyeon163.github.io"
baseurl: "/snailnotes"

# SNS 设置
github_username: gyeon163
disqus_username: hux

# 构建设置
highlighter: rouge
markdown: kramdown
paginate: 10

# 侧边栏设置
sidebar: true
sidebar-about-description: "个人描述"
sidebar-avatar: "头像 URL"

# 功能开关
featured-tags: true
service-worker: true  # PWA 支持
```

### 友链配置

```yaml
friends:
  - title: "友链名称"
    href: "https://example.com"
```

## 功能特性

- ✅ 响应式设计（移动端优先）
- ✅ PWA / Service Worker 支持
- ✅ 离线页面
- ✅ 侧边栏导航
- ✅ 特色标签展示
- ✅ 友情链接
- ✅ Disqus 评论集成
- ✅ Google Analytics / 百度统计
- ✅ 搜索功能
- ✅ 归档页面
- ✅ MathJax 数学公式支持
- ✅ 代码语法高亮（Rouge）

## 常见问题

### cannot load such file -- jekyll-paginate

确保已通过 Bundler 安装依赖：
```sh
bundle install
```

### 时区问题（中国）

在 `_config.yml` 中添加：
```yaml
timezone: CN
```

## 相关资源

- [Jekyll 官方文档](https://jekyllrb.com/)
- [Hux Blog 主题](https://github.com/Huxpro/huxpro.github.io)
- [Liquid 模板语法](https://github.com/Shopify/liquid/wiki)
- [kramdown Markdown 解析器](https://kramdown.gettalong.org/)

## 许可证

Apache License 2.0
