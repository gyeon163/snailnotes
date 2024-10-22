---
layout: post
title: "运行Jupyter服务"
subtitle: "在Linux服务器上后台运行Jupyter服务"
author: "gyeon"
date: 2023-01-05 13:15:00
header-style: text
header-mask: 0.4
tags:
  - Python
  - Jupyter
---

# 在Linux服务器上后台运行Jupyter服务

在Linux服务器上后台运行Jupyter服务，可以通过以下步骤实现：

## 1. 安装Jupyter Notebook

如果你还没有安装Jupyter Notebook，可以使用以下命令进行安装：

```shell
pip install jupyter
```

## 2. 生成Jupyter配置文件
运行以下命令生成配置文件：

```shell
jupyter notebook --generate-config
```

默认情况下，配置文件会生成在`~/.jupyter/jupyter_notebook_config.py`。

## 3. 设置Jupyter Notebook的密码
使用以下命令设置密码：

```shell
jupyter notebook password
```

这将提示你输入并确认密码。密码将被哈希化并存储在配置文件中。

## 4. 后台运行Jupyter Notebook
要在后台运行Jupyter Notebook，可以使用nohup命令或者tmux/screen会话。

### 4.1使用tmux或screen(十分建议)
也可以使用tmux或screen会话管理器，这样可以在会话断开后仍然保持Jupyter运行。

启动tmux或screen会话：

```shell
tmux new -s jupyter
```

或者

```shell
screen -S jupyter
```

启动Jupyter Notebook：

```shell
jupyter notebook --ip=0.0.0.0 --port=8888 --no-browser
```

使用Ctrl+B然后D（对于tmux）或Ctrl+A然后D（对于screen）分离会话。你可以随时重新连接：

```shell
tmux attach -t jupyter
```

或者

```shell
screen -r jupyter
```

### 4.2 使用nohup
你可以使用nohup命令将Jupyter Notebook放到后台运行，并将输出重定向到一个文件：

```shell
nohup jupyter notebook --ip=0.0.0.0 --port=8888 --no-browser > jupyter.log 2>&1 &
```

解释：
```shell
--ip=0.0.0.0：允许外部访问。
--port=8888：指定端口号（可以更改为你需要的端口）。
--no-browser：不在启动时打开浏览器。
jupyter.log 2>&1 &：将输出重定向到jupyter.log文件并在后台运行。
```

## 5. 配置防火墙（如有需要）
确保你的服务器防火墙允许外部访问Jupyter Notebook使用的端口（如8888）。

## 6. 访问Jupyter Notebook
使用浏览器访问http://your-server-ip:8888，并使用之前设置的密码进行登录。

## 7. 终止Jupyter Notebook服务
tmux停止jupyter服务
重新连接tmux ，然后直接ctrl + c,再把tmux窗口关了

nohup
要终止Jupyter Notebook服务，可以找到进程并终止：

```shell
ps aux | grep jupyter

kill <process_id>
```

这样，你就可以在Linux服务器上后台运行Jupyter服务了！

### ps aux | grep jupyter
ps aux | grep jupyter 是一个在 Linux 系统上常用的命令，用来查找与 Jupyter Notebook 相关的正在运行的进程。让我们逐步解释这个命令的含义：

```shell
ps aux:

ps：这是一个用于显示当前正在运行的进程的命令。
a：显示所有用户的所有进程。
u：以用户为中心的格式显示进程信息，包括用户、CPU 和内存使用情况等。
x：显示没有控制终端的进程（如后台进程）。
因此，ps aux 将列出系统上所有正在运行的进程及其详细信息。

|（管道符）:

管道符 | 用于将前一个命令的输出作为输入传递给下一个命令。在这个例子中，ps aux 的输出被传递给 grep jupyter。
grep jupyter:

grep 是一个用于搜索文本的命令。
jupyter 是 grep 的搜索模式，它将在 ps aux 的输出中查找包含 “jupyter” 的行。
总结
ps aux | grep jupyter 的作用是：
```

显示系统上所有正在运行的进程。
过滤并显示与 jupyter 相关的进程。
通过这个命令，你可以快速找到与 Jupyter Notebook 相关的进程的进程 ID（PID）以及其他信息，如启动时间、CPU 和内存使用等。找到 PID 后，你可以使用 kill 命令来终止相应的进程。