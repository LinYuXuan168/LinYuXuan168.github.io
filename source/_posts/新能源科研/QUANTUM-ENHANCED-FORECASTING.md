---
title: QUANTUM-ENHANCED FORECASTING
date: 2024-10-26
description: 时间序列转化成图片，方法论：QGAF
tags: 
- 序列转图像
categories: 
- 新能源科研
music:
  server: netease
  type: playlist
  id: 8109052701
author: 林宇轩
---

## 基本信息
**名称：**QUANTUM-ENHANCED FORECASTING: LEVERAGING QUANTUM  GRAMIAN ANGULAR FIELD AND CNNS FOR STOCK RETURN  PREDICTIONS

**中文：**量子增强预测：利用量子格拉米角场和 CNN 进行股票回报预测

**期刊：**一区  **时间：**2024年10月26日  **类别：**时序转图像



## **文章主要创新点：**

1. 提出了一种名为**量子格拉米安角场（QGAF）**的时间序列预测方法。该方法融合了量子计算技术与深度学习的优势，旨在提高时间序列分类和预测的精度。

2. 通过设计特定的量子电路，成功地**将股票收益时间序列数据转换为适合卷积神经网络（CNN）训练的二维图像**。

   

## **文章摘要：**

我们提出了一种名为量子格拉米安角场（QGAF）的时间序列预测方法。该方法融合了量子计算技术与深度学习的优势，旨在提高时间序列分类和预测的精度。我们通过设计特定的量子电路，成功地将股票收益时间序列数据转换为适合卷积神经网络（CNN）训练的二维图像。与经典的格拉米安角场（GAF）方法不同，QGAF的独特之处在于不需要数据归一化和反余弦计算，简化了从时间序列数据到二维图像的转换过程。为了验证该方法的有效性，我们对中国A股市场、香港股市和美国股市这三个主要股票市场的数据集进行了实验。实验结果表明，与经典GAF方法相比，QGAF方法显着提高了时间序列预测精度，平均绝对误差（MAE）预测误差平均降低25%，均方误差（MSE）预测误差平均降低48%。这项研究证实了将量子计算与深度学习技术相结合在金融时间序列预测中的潜力和前景



## **模型方法：**

我们研究的总体目标是**利用历史时间窗口内封装的信息来预测后续相邻时间窗口的间隔回报**

![img](https://yloc26cujy.feishu.cn/space/api/box/stream/download/asynccode/?code=OTE0NzI3ZGI0YmVhZWIyZmUyMGZiYWYzZTY5OTg1YzFfY1Q2SFlxNmJoT0pLbzdWVGxGSEJqUTMwSEtSYWMzR3lfVG9rZW46S3MwYWJrNmxEbzVwYzB4NmIyb2NjUmgybmtoXzE3MzA0ODA1OTI6MTczMDQ4NDE5Ml9WNA)



### 生成图像数据

#### 数据切割

采用滚动时间窗口，跨越 30 天的回报，生成 30×30 像素的图像。

连续窗口之间选择 10 天的步长**以及 20 天的重叠**。这种配置确保了一个窗口的历史数据与后续窗口之间有大量重叠。（**即窗口与窗口之间会存在20天重叠**）确保了平衡过渡，模型能够更好地推断和预测下一个时间窗口的区间回报

Eg: 当第一个窗口从第 1 天到第 30 天截取完成后，下一个窗口并不是从第 31 天开始，而是从第 11 天开始（因为步长是 10 天），到第 40 天结束。这样，第一个窗口和第二个窗口就有 20 天的重叠部分，即从第 11 天到第 30 天这 20 天的数据是两个窗口共有的

![img](https://yloc26cujy.feishu.cn/space/api/box/stream/download/asynccode/?code=NDgzMDZiNmU0ZjcwZTFlNmE0YmJmZDFjNWY0OWFjNWNfZ1haZUJnY3UyOUxuM21BNmt4R1FoVW5wdTlhUEtHTG1fVG9rZW46Sm5uRWJudENZb3k5d0F4NDBOQWMxempGbnhiXzE3MzA0ODA1OTI6MTczMDQ4NDE5Ml9WNA)



#### 图像生成

 GAF 通过将时间序列归一化为 [-1,1] 或 [0,1] 区间并将其值表示为极坐标中的角余弦，并以时间戳为半径来实现此目的。此转换将时间序列转换为极坐标表示形式。在此基础上，我们定义了**格拉米亚角求和场（GASF）和格拉米亚角差场（GADF）矩阵。这些矩阵通过余弦和正弦运算捕获不同时间跨度的时间序列中的值之间的相关性，形成时间信号的图像表示。这种 GAF 图像表示方法提供了一定程度的数据压缩，同时保留了时间序列内的时间依赖性。**

1. 先将数据进行归一化处理，使得值落到[0,1]或者[-1,1]：

![img](https://yloc26cujy.feishu.cn/space/api/box/stream/download/asynccode/?code=OTdkYThiMzU1M2YzM2I0NWJmZTg5OWMzYjYyM2NhYmZfaWJhc1BwRVFMbG5xMjlDZVBscU14MnZCaFU4NmxPTXRfVG9rZW46WmpweWJUZ0JYb1Z4ZjB4WTVhQmNxOXZybmxoXzE3MzA0ODA1OTI6MTczMDQ4NDE5Ml9WNA)

1. 将值转换成极坐标进行表示:

![img](https://yloc26cujy.feishu.cn/space/api/box/stream/download/asynccode/?code=OWExNjFmNDBjMDU5OTA2NjQ2OTk5MjUwMDRkODAwYjZfdHNTZ1dkRjNMU0I5NW5JNEJMRmsyRGZwcGFUMlhtNTNfVG9rZW46RnF3b2JHYkxWb0pwdmt4SEtWV2NuWmV5bnljXzE3MzA0ODA1OTI6MTczMDQ4NDE5Ml9WNA)

1. 经过GAF得到两个场图：

![img](https://yloc26cujy.feishu.cn/space/api/box/stream/download/asynccode/?code=ZmZkNmVmMTIyNjZiYTE0MTUwOGFmYThhNTYxNmFjZWZfVGMzTzVkbWJ4S3VzOWdGbTlyWWRUalhQNlA3TUc3aW5fVG9rZW46VGhFSWJtcGZYb3lnMWp4eDRUUmNIbVhKbng4XzE3MzA0ODA1OTI6MTczMDQ4NDE5Ml9WNA)