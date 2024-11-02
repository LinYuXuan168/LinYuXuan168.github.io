---
title: cddfuse
date: 2023-12-8
description: cddfuse论文个人阅读学习
tags: 
- cddfuse图像融合
categories: 
- 计算机视觉
music:
  server: netease
  type: playlist
  id: 8109052701
author: 林宇轩··	
---

## 基本信息

名称： CDDFuse: Correlation-Driven Dual-Branch Feature Decomposition for Multi-Modality Image Fusion (CDDFuse：用于多模态图像融合的相关驱动双分支特征分解)  
作者：Zixiang Zhao (赵子祥)  
关键词：cddfuse,Multi-modality (MM) image fusion,restormer,transformer-CNN 

## 摘要提取

多模态（MM）图像融合旨在渲染融合图像，保持不同模态的优点，例如功能突出和详细纹理。为了应对跨模态特征建模和分解所需的模态特定和模态共享特征的挑战，我们提出了一种新颖的相关驱动特征分解融合（CDDFuse）网络。首先，CDDFuse 使用 Restormer 块来提取跨模态浅层特征。然后，我们引入了双分支 Transformer-CNN 特征提取器，其中 Lite Transformer (LT) 块利用远程注意力来处理低频全局特征，而可逆神经网络 (INN) 块则专注于提取高频局部信息。进一步提出了相关驱动损失，基于嵌入信息使低频特征相关，而高频特征不相关。然后，基于 LT 的全局融合层和基于 INN 的局部融合层输出融合图像。大量的实验表明，我们的 CDDFuse 在多种融合任务中取得了有希望的结果，包括红外 - 可见光图像融合和医学图像融合。我们还表明，CDDFuse 可以在统一的基准测试中提高下游红外 - 可见光语义分割和对象检测的性能。

## 创新点

- 提出了相关驱动特征分解融合（CDDFuse）模型，其中通过双分支编码器实现模态特定和模态共享特征提取，并由解码器重建融合图像

## 研究方法

- 引入了双分支 Transformer-CNN 特征提取器，其中 Lite Transformer (LT) 块利用远程注意力来处理低频全局特征，而可逆神经网络 (INN) 块则专注于提取高频局部信息
- 进一步提出了相关驱动损失，基于嵌入信息使低频特征相关，而高频特征不相关。

## 研究过程

### incroduction

- 将现有的 MMIF 方法与 cddfuse 在八个指标上进行比较，在 MSRS 和 RoadScene 两个数据集上面 cddfuse 具有更先进的性能  
!["性能比较"](https://s2.loli.net/2023/12/08/ijgNFwk2AMhOZTz.png)
- 普通融合架构和 cdduse 融合架构的流程展示  
![](https://s2.loli.net/2023/12/08/MLAH1ceEYzQ6xP2.png)
- 现有存在的三大问题：
  1. CNN 的内部工作机制难以控制和解释，导致跨模态特征提取不足。
  2. 上下文无关的 CNN 仅在相对较小的感受野中提取局部信息，很难提取全局信息来生成高质量的融合图像
  3. 融合网络的前向传播常常导致高频信息的丢失
- 新模型的解决方法：
  1. 对提取的特征添加相关性限制并限制解空间。
  2. 融合 CNN 中局部上下文提取和计算效率的优势以及 Transformer 中全局注意力和远程依赖建模的优势来完成 MMIF 任务。
  3. 采用可逆神经网络 (INN) 的构建模块

### Related Work

- generative adversarial network (GAN)-based models 基于生成对抗网络（GAN）的模型。
- AE-based models 基于 AE 的模型。
- unified models 统一模型
- algorithm unrolling models 算法展开模型。

### Method

#### Encoder 

- 编码器由三部分组成，基于 Restormer 块的共享特征编码器（SFE）、基于 Lite Transformer（LT）块的基本 transformer 编码器（BTE）和基于可逆神经网络（INN）块的细节 CNN 编码器 (DCE)
- Restormer 块可以通过在特征维度上应用自注意力来从高分辨率输入图像中提取全局特征，优点是可以不用增加太多额外的
- 使用 LT 块作为 BTE 的基本单元。通过扁平化前馈网络的结构，扁平化了 Transformer 块的瓶颈，LT 块缩小了嵌入，以减少参数数量，同时保持相同的性能计算量
- INN 模块通过使其输入和输出特征相互生成，使得输入信息能够得到更好的保存，采用具有仿射耦合层的 INN 块

#### Fusion layer

- 采用 LT 模块和 INN 模块构成基本和细节融合层

#### Decoder

- 采用 Restomer 块
- 在第一训练阶段，重构图像作为第二阶段的输入，在第二训练阶段，输出融合图像

### Train

- 采用两次训练，在第一训练阶段，提取特征后连接进行重构图像，在第二训练阶段，将重构图像输入后进行提取特征后融合输出图像
