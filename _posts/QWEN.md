# Snail Notes Blog - 博客文档编写规范

## 一、文件命名规范

### 目录结构
文章按年份组织，存放在对应的年份子目录中：

```
_posts/
├── 2010/
│   └── 2010-10-03-团队建设.md
├── 2011/
│   └── 2011-03-10-DOS 命令.md
└── 2024/
    └── 2024-01-15-文章标题.md
```

### 文件名格式
```
YYYY-MM-DD-文章标题.md
```

- **日期格式**：`YYYY-MM-DD`（年 - 月 - 日）
- **标题部分**：使用中文或英文，多个单词之间用 `-` 连接
- **文件扩展名**：`.md`（Markdown 格式）

---

## 二、Front Matter 格式

每篇文章开头必须包含 YAML 格式的前置元数据，用 `---` 包裹：

```yaml
---
layout: post
title: "文章标题"
author: "作者名"
date: "YYYY-MM-DD HH:MM:SS"
header-img: "img/图片路径"
header-mask: 0.4
tags:
  - 标签 1
  - 标签 2
---
```

### 必填字段

| 字段 | 说明 | 示例 |
|------|------|------|
| `layout` | 固定为 `post` | `layout: post` |
| `title` | 文章标题（建议加引号） | `title: "团队建设"` |
| `author` | 作者名 | `author: "gyeon"` |
| `date` | 发布时间（精确到秒） | `date: "2024-01-15 12:00:00"` |
| `header-img` | 头部背景图片路径 | `header-img: "img/home-bg-o.jpg"` |
| `header-mask` | 头部图片遮罩透明度（0-1） | `header-mask: 0.4` |
| `tags` | 文章标签列表 | `tags: [标签 1, 标签 2]` |

### 可选字段

| 字段 | 说明 | 示例 |
|------|------|------|
| `subtitle` | 文章副标题 | `subtitle: "副标题内容"` |
| `header-style` | 头部样式，`text` 表示文本风格 | `header-style: text` |
| `catalog` | 是否显示目录 | `catalog: true` |
| `mathjax` | 是否启用数学公式支持 | `mathjax: true` |

### 常用头部图片

```
img/home-bg-o.jpg          # 默认首页背景
img/post-bg-halting.jpg    #  Halt 主题背景
img/post-bg-default.jpg    # 默认文章背景
```

---

## 三、文章内容格式

### 标题层级

使用 Markdown 标准语法，最多支持 6 级标题：

```markdown
# 一级标题（文章主标题，通常不使用）

## 二级标题（主要章节）

### 三级标题（子章节）

#### 四级标题（更细粒度）
```

### 代码块

使用三个反引号包裹，并指定语言以获得语法高亮：

````markdown
```bash
# Shell 脚本示例
echo "Hello World"
```

```javascript
// JavaScript 示例
console.log("Hello");
```

```python
# Python 示例
print("Hello")
```
````

### 列表

```markdown
## 有序列表
1. 第一项
2. 第二项
3. 第三项

## 无序列表
- 项目一
- 项目二
- 项目三

## 嵌套列表
1. 主项目
   a) 子项目 1
   b) 子项目 2
2. 主项目二
```

### 引用

```markdown
> 这是一段引用文字
> 可以多行
```

### 链接与图片

```markdown
[链接文字](https://example.com)

![图片描述](/img/path/to/image.png)
```

---

## 四、标签使用规范

### 标签命名原则

- 使用简洁明了的名称
- 同一主题使用统一标签
- 避免过多标签（建议 1-5 个）

### 常见标签示例

| 分类 | 标签 |
|------|------|
| 技术 | `Shell`, `JavaScript`, `Python`, `Java`, `CSS`, `HTML` |
| 管理 | `领导`, `团队建设`, `项目管理` |
| 生活 | `随笔`, `读书`, `旅行` |

---

## 五、快速创建文章

### 使用 Rake 任务（推荐）

在项目根目录执行：

```sh
rake post title="文章标题" subtitle="文章副标题"
```

文章将自动生成在 `_posts/年份/` 目录下。

### 手动创建

1. 在 `_posts/` 下创建对应年份的文件夹（如不存在）
2. 创建文件：`YYYY-MM-DD-文章标题.md`
3. 填写 Front Matter 和文章内容

---

## 六、发布前检查清单

- [ ] Front Matter 格式正确（`---` 包裹，字段完整）
- [ ] 文件名符合 `YYYY-MM-DD-标题.md` 格式
- [ ] 文章存放在正确的年份目录
- [ ] 代码块指定了语言
- [ ] 标签使用恰当
- [ ] 本地预览正常（`bundle exec jekyll serve`）

---

## 七、示例模板

```yaml
---
layout: post
title: "文章标题"
author: "gyeon"
date: "2024-01-15 12:00:00"
header-img: "img/post-bg-default.jpg"
header-mask: 0.4
tags:
  - 标签 1
  - 标签 2
---

## 引言

这里是文章引言...

## 主要内容

### 子章节 1

内容...

### 子章节 2

内容...

## 总结

这里是总结...
```

---

## 参考资料

- [Jekyll 官方文档](https://jekyllrb.com/)
- [Markdown 语法指南](https://daringfireball.net/projects/markdown/)
- [项目根目录 QWEN.md](../QWEN.md)
