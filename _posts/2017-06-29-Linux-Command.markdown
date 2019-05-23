---
layout:     post
title:      "Linux Command"
subtitle:   "Linux常用命令"
# date:       2015-01-29 12:00:00
author:     "Noir"
tags:
    - linux
---

记录一些常用命令备忘

```
检索进程：ps -ef
查看磁盘空间：df -hl
查看当前目录下一级子文件和子目录占用的磁盘容量: du -lh --max-depth=1
查找文件：find -name {name}
查看md5：md5sum {filePath}
检索目标及后n行：grep -A {n} {name}
检索目标及前n行：grep -B {n} {name}
检索目标及前后n行：grep -C {n} {name}
查看正在监听的端口和进程：lsof -nP | grep TCP | grep LISTEN
```
