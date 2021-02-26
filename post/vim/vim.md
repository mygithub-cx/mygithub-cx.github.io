---
title: "Vim"
description: "文本编辑器常用快捷键提示"
date: 2021-01-22T18:15:16+08:00
draft: false
image: /post/vim/vim.png
categories: 
    - tool
tags:
    - text edit
    - vim
---

# Vim

## 三种模式 

1. 控制模式
    1. i
        - 文本编辑模式
    1. 跳转
    1. w
        - 跳到下一个单词
    1. b
        - 向左移动光标，以单词为单位
    1. 光标的移动
        - j 向下
        - k 向上
        - h 向左
        - l 向右
    1. ^
        - 跳转到该行的首个字符
    1. 0
        - 跳转到该行的行首
    1. $
        - 跳转到该行的行尾
    1. a
        - 在行尾添加文字
    1. o
        - 新建并跳转到下一行进行输入
    1. H
        - 跳转到该页的头部
    1. M
        - 跳转到该页的中间
    1. L
        - 跳转到该页的底部
    1. m[a-z]
        - 添加标记,标记为[a-z]
    1. '[a-z]
        - 跳回标记处
    1. { / }
        - 在段落之间跳转
    1. g
        - [number]gg 跳转到指定行
        - G 跳到文档最后
    1. x
        - 剪切字符
        - [number]x 删除number个字符
    1. d
        - 删除
            - dd 删除一行
            - d[number]j 向下删除number行和本行
            - d[number]j 向上删除number行和本行
    1. D
        - 删除到行尾
    1. y
        - 复制
            - yy 复制本行
            - y[number]y 向下复制number行
    1. u
        - 撤销操作
    1. ctrl + r
        - 恢复撤销的操作
    1. p
        - 粘贴
            - [number]p 粘贴number遍
    1. /
        - 搜索
    1. 替换
        - 格式
            - %s/旧内容/新内容 所有内容
            - s/旧内容/新内容 选中的部分
            - 后面添加 /c 会询问是否替换，可以一个一个确认
    1. :
        - sp 横向分屏
        - vsp 纵向分屏
        - w 写入文件
        - q 退出
        - x 保存并退出

2. 插入模式
    - 直接输入信息
3. 可视模式
    - v 可视模式，自由选择
    - V 可视行模式，选择行
    - ctrl + v 可视块模式，选择块

1. esc
    - 返回控制模式