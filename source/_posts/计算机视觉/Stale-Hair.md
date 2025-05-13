---
title: Stable-Hair
date: 2024-11-10
description: Stable-Hair实现了人物换发型
tags: 
- Github项目
- opencv 
- 数字人
categories: 
- 计算机视觉
music:
  server: netease
  type: playlist
  id: 8109052701
author: 林宇轩

---

## 基本信息
Name: Stable-Hair: Real-World Hair Transfer via Diffusion Model
Github: https://xiaojiu-z.github.io/Stable-Hair.github.io/
## 摘要&框架
它将广泛的现实世界发型稳健地转移到用户提供的面部上，以进行虚拟头发试穿。为了实现这一目标，我们的 Stable - 头发框架被设计为一个两阶段的管道。在第一阶段，我们在稳定扩散的同时训练了一个秃头转换器，以从用户提供的面部图像中去除头发，从而产生秃头图像。在第二阶段，我们特别设计了三个模块：毛发提取器、潜在识别网和头发交叉注意层，以将具有高度细节和高保真度的目标发型转移到秃头图像中。具体来说，毛发提取器经过训练，可以对具有所需发型的参考图像进行编码。为了保持源图像和传输结果之间身份内容和背景的一致性，我们使用潜在身份网络对源图像进行编码。借助我们在 U-Net 中的头发交叉注意层，我们可以准确、精确地将高度细节和高保真度的发型转移到秃头图像上。广泛的实验表明，我们的方法在现有的头发转移方法中提供了最先进的（SOTA）结果。

![](https://cdn.jsdmirror.com/gh/LinYuXuan168/imgs@main/tmp/20241110191904.png)

## 搭建过程

镜像环境构建
```python
pip install -r requirements.txt
```
### 输入为图片

1. 判断是否为Align Face 如果不是，则运行脚本 scripts/photo_align_face.py
```python
pyhton align_face.py -unaligned_photo '' -output_photo '' 
```
2. 是则直接输入即可，配置文件存放在 config 
3. 运行
```python
hai run2 python infer_full.py
```

### 输入为视频

1. 运行video_align_face.py 先将视频切帧同时进行aligh_face操作，得到align_face 后的视频
```python
python video_align_face.py
```
2. 视频输入进行Transform Hair ，得到改变发型后的视频
```python
hai run2 python infer_full.py
```
3.  将视频进行paste_back 脚本贴回原视频
```python
python paste_back.py 
```





