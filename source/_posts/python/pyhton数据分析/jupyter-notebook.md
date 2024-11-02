---
title: jupyter notebook
date: 2023-02-09 10:25:07
description: jupyter notebook学习（部分）
tags:
- jupyter notebook
categories:
- python
music:
 server: netease   # netease, tencent, kugou, xiami, baidu
 type: song        # song, playlist, album, search, artist
 id: 16846091      # song id / playlist id / album id / search keyword
author: 林宇轩
---

## 单元格有两种状态

1. 编辑状态 - **写代码或者文本**（ENTER or 鼠标点击单元格内部）
2. 选中状态 - **单元格进行编辑**（ESC or 鼠标点击左边空白处）

## 单元格操作

**要先处于选中状态**   

1. 新增单元格:
   B 在当前选中的单元格下方新增
   A 在当前选中的单元格上方新增
2. 删除单元格：
   DD 删除选中单元格
3. 复制，粘贴，剪切单元格：
   C 复制
   V 粘贴
   X 剪切
4. 撤销：
   Z

## 单元格两种模式

1. Code - **选中状态下 Y**
2. Markdown  - **选中状态下M**
3. **Mardown一般用来整理思路写分析思路**

## 运行

**所有的单元格运行都是独立的**
Cltrl + Enter

> Markdown进入预览模式 Code进入运行模式

## 命令

- 快速查看所有变量与函数名称
  `Code`%who 
- 返回变量列表
  `Code`%who_ls
- 运行外部文件（默认为当前目录）
  `Code` %run *.py
- 运行计时
  `Code` %time statement(代码名称)   **必须写在同一行
  
  **`Code` %timeit statement(代码名称)  (去多次时间平均)
  `Code`%%timeit (测试多组) 

## 输入输出历史

- In 输入
- out 输出
- “_”表示上一个输出
- “_2 表示Out[2]
- 

