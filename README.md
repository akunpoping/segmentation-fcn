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
如上图所示，图中绿色部分表示：卷积核大小。假设一个CNN模型输入图像大小14*14，一层卷积后得到10*10大小图像，池化后得到5*5大小的图像。关键步骤（5*5大小->1*1大小）：
1. 传统CNN：这个过程是全连接层，会把5*5大小的图像展成一维向量；
2. FCN：直接采用5*5的卷积核，对整张图像进行卷积。
两者本质上相同，但角度不同，FCN对整幅特征图进行卷积，所有的全连接层也全部当做1*1大小的卷积核进行卷积运算。  
![]({{site.baseurl}}//2.jpg)  
由于设计好的网络卷积核大小是固定的，若稍微改变一下图像大小，采用上述网络进行前向传导，最后输出一张2*2的图片。这样FCN网络就可以输入任意大小的图片，而输出图像的大小和输入图像大小息息相关。
- [ ] 如何对整幅图像做语义分割，实现dense pixels
采用反卷积升采样方法。由于卷积和池化操作会缩小图像，接下来就需要将图像放大至原始大小。文献[2]中提到论文[1]中的反卷积存在的问题：
1. 滤波器固定，且不具备学习能力（直接采用双线性插值进行升采样）。
2. 只采用了一层反卷积层进行升采样，直接按比例放大特征图像（论文中最后采用multi-scale思想进行精细化）




## 参考 ##
[1] 2013_Sermanet et al._OverFeat Integrated Recognition, Localization and Detection using Convolutional Networks  
[2] http://blog.csdn.net/hjimce/article/details/50268555  
