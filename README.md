# segmentation-fcn
A blog about the semantic segmentation using Fully Convolutional Networks

## 概述 ##
 * __基础__：深度网络和迁移学习
 * __目的__：训练一个end-to-end、pixel-to-pixel的网络用于图像的语义分割
 * __创新__：构建一个高效的“full convoluational”网络，支持任意大小的输入和对应大小的输出
 * __方法__：当前流行的分类网络（AlexNet、VGG、GoogLeNet）适用于全卷积网络，并采用fine-tuning迁移网络的表达
 * __策略__：合并deep coarse层的语义信息和shallow fine信息的外观信息

## 思考 ##
1. 如何支持任意大小的图像输入？
2. 如何直接对整幅图像进行语义分割？

## 语义分割 ##
语义分割的定义：语义分割就是预测每个像素的语义标签，也就是对图像每个像素进行分类。参考《机器学习及其应用2015》  
当前语义分割普遍的方法是面向块（超像素）的训练方法，但对于全卷积神经网络来说缺乏效率；FCN的方法则*避免预处理和后处理导致的复杂化*，比如：超像素、proposal或者随机场和分类器精化。
语义分割和位置：全局的信息关注于是什么，而局部信息关注在哪里。


## 方法 ##
- [ ] fully connected layer转化fully convoluational network
这部分对应问题一，思想来源于overfeat，是FCN算法的灵魂：1. 卷积层->全连接层，看成对整幅图像的卷积运算；2. 全连接层->全连接层，看成采用1*1大小的卷积核进行卷积运算。《OverFeat: Integerated Recognition, Localization and Detection using Convolutional Networks》
![]({{site.baseurl}}//1.jpg)

