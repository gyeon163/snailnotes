---
layout: post
title: "Matplotlib中文乱码问题解决"
author: "gyeon"
date: "2024-10-29 14:11:16"
header-img: "img/post-bg-ai.jpg"
header-mask: 0.4
tags:
  - Python
  - Machine Learning
  - Matplotlib
---

# 全局修改
1. 下载中文字体 `SimHei`
2. 安装字体`SimHei.ttf`文件
  a. `Linux`下，拷贝字体文件`SimHei.ttf`到目录`/usr/share/fonts`下
```shell
cp SimHei.ttf /usr/share/fonts/SimHei.ttf
```
  b. `Window`下，双击安装字体
3. 删除`.matplotlib`的缓存文件
  a. 查看`.matplotlib`的缓存目录
```python
'''查询matplotlib缓存目录'''
import matplotlib
print('cache dir:', matplotlib.get_cachedir())
```
```shell
# 删除matplotlib缓存文件文件
rm -r <<上面脚本打印的目录>>
```
4. 修改配置文件matplotlibrc文件
```python
import matplotlib
print('rc file:', matplotlib.matplotlib_fname())
```
```shell
vi <<上面脚本打印的文件>>

#以下文件内容
font.family:  sans-serif
font.sans-serif: SimHei
axes.unicode_minus: False 
```