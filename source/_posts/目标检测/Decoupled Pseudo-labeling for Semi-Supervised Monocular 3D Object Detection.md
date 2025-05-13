---
title: Decoupled Pseudo-labeling for Semi-Supervised Monocular 3D Object Detection
date: 2024-10-28
description: 伪标签迭代挖掘，3d单目检测
tags: 
- 伪标签
categories: 
- 目标检测
music:
  server: netease
  type: playlist
  id: 8109052701
author: 林宇轩

---

## 基本信息

**名称：Decoupled Pseudo-labeling for Semi-Supervised Monocular 3D Object Detection**

**中文：半监督单目3D目标检测的解耦伪标记技术**

**期刊：CVPR 2024** **时间：**2024年10月26日  **类别：伪标签，迭代挖掘**

## **文章主要创新点：**

1. 基于**单应性的深度标签挖掘（HPM）模块**构建了 decoupled pseudo-label generation(DPG)  Model **解耦伪标签生成模块**，为2D属性和3D属性生成可靠的伪标签。
2. 构建了depth gradient projection (DGP) **深度梯度投影模块**，以减轻噪声深度伪标签可能造成的不利影响

## **文章摘要：**

我们深入研究了**半监督单目 3D 物体检测 (SSM3OD)** 的伪标记，发现了两个主要问题：**3D 和 2D 属性的预测质量之间的不一致**，以及从伪标签导出的深度监督有噪声的趋势，导致与其他可靠形式的监督存在重大优化冲突。为了解决这些问题，我们为 SSM3OD 引入了一种**新颖的解耦伪标记 (DPL) 方法**。我们的方法具有**解耦伪标签生成（DPG）模块**，旨在通过单独处理 2D 和 3D 属性来有效生成伪标签。该模块采用了一种独特的基于单应性的方法，用于识别鸟瞰 (BEV) 空间中可靠的伪标签，特别是针对 3D 属性。此外，我们提出了**深度梯度投影（DGP）模块**，以减轻由伪标签的噪声深度监督引起的优化冲突，有效地解耦深度梯度并消除冲突的梯度。这种双重解耦策略（在伪标签生成和梯度级别）显着提高了 SSM3OD 中伪标签的利用率。我们对 KITTI 基准的综合实验证明了我们的方法相对于现有方法的优越性

## **模型方法：**

![img](https://yloc26cujy.feishu.cn/space/api/box/stream/download/asynccode/?code=MWFjOTZjMGJiZGQ2OWJkMjMyMTc2NGY0ZDZlZmU4NzJfcUk4QmpwUTJsa1ZtdUROeG13MFY5ZUdFM1F0THJLWFZfVG9rZW46U0pUVmJyb2hHbzNwWVB4VW44NmNJT2RVblNjXzE3MzA2MTU3NTU6MTczMDYxOTM1NV9WNA)

SSM3OD 的目标是通过利用有限的标记图像和额外丰富的未标记图像来实现单目 3D 物体检测。 SSM3OD的优化可以表述为：
$$
L = L_{sup} + \alpha L_{unsup}
$$
采用经典的师生框架，其中涉及教师和学生网络，两者均由有监督的预训练模型初始化**。教师模型在未标记的图像上生成伪框，而学生模型则使用带有真实注释的标记图像和带有伪标签的未标记图像进行训练**。

### **Decoupled Pseudo-label Generation DPG 解耦伪标签生成模块**

![img](https://yloc26cujy.feishu.cn/space/api/box/stream/download/asynccode/?code=ZGZkZWU3OWM2ZDJjZmQ1ZDczZGU4ZmM0YjgxMWU4YTFfSVdndE83NWo5clhMZ3dDV1ZQN2F4SUFGSE9rN1pFVTFfVG9rZW46TmVqeGJlcmtkb2RVeWh4YWZkVWNKZnV4bm9oXzE3MzA2MTU3NTU6MTczMDYxOTM1NV9WNA)

主要由基于单应性的**深度标签挖掘（HPM）模块构成**

#### **深度标签挖掘（HPM）模块**

##### **Homography Transformation单应性变换：**利用几何不变关系将预测从2D图像转换到3D图像

![img](https://yloc26cujy.feishu.cn/space/api/box/stream/download/asynccode/?code=YjZmYTA5Nzc3NjBiMDgyYWFjOTAxNTUzNWE2ZmM3YTlfcTZqQlFFSlF4ZzJFemZla2R1ZVhyN2ppU0xDTHZjRzZfVG9rZW46VTlPRGI2WGszb0h2aFp4MGlmZGN2OFhWbm5nXzE3MzA2MTU3NTU6MTczMDYxOTM1NV9WNA)

这里的M为单应性矩阵，根据平坦地面假设，图像中的不同对象将共享相同的单应矩阵，**即同一图像不同对象的M相同**

##### **Iteratively Pseudo-Label Mining(伪标签迭代挖掘）**

1. 挑选相对准确的 3D 属性预测的伪标签来可靠地估计初始单应矩阵（采用**Laplacian aleatoric uncertainty loss**

![img](https://yloc26cujy.feishu.cn/space/api/box/stream/download/asynccode/?code=YmY0MGFjNzZlMjQ0YjdhNzVkMDNmMDI2MGRlNTcxM2ZfUzNRSXlMR0t2RFl6QzZEY0xpOXVkVEc4d3R5aXVQbXFfVG9rZW46QlIwZ2JOcFdZb2Z4cnF4T0R1dmNuMjZUbmlmXzE3MzA2MTU3NTU6MTczMDYxOTM1NV9WNA)

1. 计算单应性矩阵

选择**每个初始 3D 伪边界框的底部角点和底部中心点（5 个点**）作为候选点来估计单应性矩阵，利用上述的转换公式来进行计算

- 2D图像的坐标由教师网络直接预测
- 3D图像的坐标根据外参矩阵变换得到

根据上面给出的坐标计算出单应性矩阵M

1. 将估计的单应矩阵M应用于**尚未选择为伪标签**的预测边界框的候选点得到在BEV空间中获得所需的坐标
2. 进行迭代挖掘：

没有获得新的伪标签，要么达到了预定义的最大迭代限制（表示为 tmax），设定一个阈值

![img](https://yloc26cujy.feishu.cn/space/api/box/stream/download/asynccode/?code=NGEzODc3OGUyNTU3NzIxMDg2Y2RjN2RlNWQ0ZWMwZTFfYll3QVRyd29BWmNlNHVTN21vRUdyY0J0dmZGRWRXODBfVG9rZW46UllaRGJYYW9Fb0xLenV4NlF5QWNiUW50bmVoXzE3MzA2MTU3NTU6MTczMDYxOTM1NV9WNA)

ps:一个为估计得到的3D坐标，一个是计算得到的3D坐标

### **Depth Gradient Projection (DGP) 深度梯度投影模块**

![img](https://yloc26cujy.feishu.cn/space/api/box/stream/download/asynccode/?code=NWQ0M2QyNTU0NWFjMjJlM2E4N2RjY2FkNDEwZTEyNGRfTUdrUTFmbEc5NXRZRWsxU3NpUXZrUENoQzBobWU0RDFfVG9rZW46RlMzRWJCTTBnb3I4QnN4T2JRRWNVOHkxbmdoXzE3MzA2MTU3NTU6MTczMDYxOTM1NV9WNA)

将冲突的深度梯度**投影到主要可靠梯度，**噪声无监督深度损失始终保证与可靠监督共享共同利益，并提供均衡优化目标，**总损失 = 其他属性损失 + 地面真实深度损失 + 伪标签深度损失** 