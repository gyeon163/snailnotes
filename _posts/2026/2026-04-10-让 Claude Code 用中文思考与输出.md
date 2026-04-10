---
layout: post
title: "让 Claude Code 用中文思考与输出"
author: "gyeon"
date: "2026-04-10 15:30:00"
header-img: "img/post-bg-default.jpg"
header-mask: 0.4
tags:
  - AI
  - Claude
  - 效率工具
---

## 背景

在使用 Claude Code 进行代码开发时，默认情况下 AI 会以英文进行思考和输出。对于中文开发者来说，这可能会造成一定的理解成本。本文将介绍如何配置 Claude Code，使其使用中文进行思考和输出。

---

## 方法一：在 CLAUDE.md 中配置（推荐）

### 1. 创建配置文件

在项目根目录或用户主目录创建 `CLAUDE.md` 文件：

**项目级别**（仅对当前项目生效）:
```
你的项目根目录/CLAUDE.md
```

**用户级别**（对所有项目生效）:
```
# Windows
C:\Users\你的用户名\.claude\CLAUDE.md

# macOS/Linux
~/.claude/CLAUDE.md
```

### 2. 配置内容

```markdown
# 语言设置

请始终使用**简体中文**进行回复和解释。

## 技术内容保持原样

以下内容保持英文，不要翻译：
- 代码块（所有编程语言）
- 命令行指令和输出
- 文件路径
- 错误信息和日志
- JSON/YAML/XML 的键名
- 变量名、函数名、类名

## 回复风格

- 简洁直接，避免冗长
- 技术解释清晰明了
- 代码示例完整可运行
```

### 配置说明

| 配置项 | 说明 |
|--------|------|
| `请始终使用简体中文` | 强制 AI 使用中文回复 |
| `技术内容保持原样` | 代码、命令等不被翻译 |

---

## 方法二：使用 .cursorrules 文件（Cursor 用户）

如果你使用 Cursor 编辑器集成 Claude：

### 1. 创建规则文件

在项目根目录创建 `.cursorrules` 文件：

```markdown
# Cursor Rules for Claude

## 语言偏好
- 始终使用简体中文回复
- 代码和技术术语保持英文

## 代码风格
- 提供完整可运行的代码
- 添加必要的注释
```

---

## 方法三：在对话中直接指定

在每次会话开始时，明确告诉 Claude 使用中文：

```
请用中文回答我的问题。接下来的所有对话都请使用中文。
```

或者创建一个快捷指令/别名：

**Windows PowerShell** (`$PROFILE`):
```powershell
function claude-cn { 
    Write-Host "请用中文回答我的问题。" -ForegroundColor Green
    claude
}
```

**macOS/Linux** (`~/.bashrc` 或 `~/.zshrc`):
```bash
alias claude-cn='echo "请用中文回答我的问题。" && claude'
```

---

## 方法四：使用系统提示词（高级用户）

如果你使用的是 Claude API 或可定制的系统提示词：

```
你是一位中文技术助手。请遵循以下规则：

1. **语言**: 始终使用简体中文进行回复
2. **技术内容**: 代码、命令、路径、错误信息等保持英文原样
3. **风格**: 简洁、专业、直接
4. **代码质量**: 提供完整、可运行、有注释的代码示例
```

---

## 实际效果对比

### 配置前（英文输出）

```
I'll help you refactor this function. Let me first analyze the current 
implementation to identify potential improvements...

The main issues I see are:
1. No error handling
2. Tight coupling between modules
...
```

### 配置后（中文输出）

```
我来帮你重构这个函数。首先让我分析当前的实现，找出可以改进的地方...

我发现的主要问题有：
1. 没有错误处理
2. 模块之间耦合过紧
...
```

---

## 注意事项

### 保持技术内容原样

配置中文输出后，以下内容**不会**被翻译：

- ✅ 代码块（JavaScript、Python、Shell 等）
- ✅ 命令行指令
- ✅ 文件路径
- ✅ 错误日志和堆栈跟踪
- ✅ JSON/YAML 的键名
- ✅ 变量名和函数名

示例：

```bash
# 命令保持英文
npm install --save-dev typescript

# 错误信息保持英文
Error: Cannot find module 'react'
```

### 混合场景处理

当遇到系统输出时，AI 会保留原始英文内容，并添加中文解释：

```
> npm run build
Error: Build failed.

构建失败，请检查上述错误信息。
```

---

## 验证配置

创建配置后，可以通过简单的问题验证是否生效：

```
你好，请介绍一下你自己。
```

如果 AI 使用中文回复，说明配置成功。

---

## 常见问题

### Q: 为什么配置后还是英文回复？

**A**: 可能的原因：
1. `CLAUDE.md` 文件位置不正确
2. 文件名大小写错误（必须是 `CLAUDE.md`）
3. 需要重启 Claude 会话

### Q: 如何让 Claude 同时输出中英文？

**A**: 在配置文件中添加：
```markdown
重要内容请同时提供中英文对照。
```

### Q: 配置文件不生效怎么办？

**A**: 尝试：
1. 检查文件路径是否正确
2. 确认文件编码为 UTF-8
3. 在对话开头再次强调语言要求

---

## 总结

| 方法 | 适用场景 | 优先级 |
|------|----------|--------|
| `CLAUDE.md` | 官方推荐，持久化配置 | 高 |
| `.cursorrules` | Cursor 编辑器用户 | 高 |
| 对话中指定 | 临时需求 | 中 |
| 系统提示词 | API 用户/高级定制 | 中 |

通过以上配置，你可以让 Claude Code 在保持技术内容准确性的同时，使用中文进行思考和输出，提升开发效率。

---

## 参考资料

- [Claude Code 官方文档](https://docs.anthropic.com/claude-code)
- [Anthropic Developer Console](https://console.anthropic.com/)
