# Pytorch

## 基础

Pytorch Tensor 的通道排序：[batch, channel, height, width]

batch 是本批次图像的个数

channel是图像通道数



### 卷积后矩阵尺寸大小计算

$$
N = (W - F + 2P)/S + 1
$$

* 输入图片大小W x W
* 卷积核Filter大小 F x F
* 步长 S
* padding 的像素数 P

