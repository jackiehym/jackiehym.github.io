---
title: tf_albumentations(1)-模糊
author: handsome boy
date: 2023-03-04 20:32:00 +0800
categories: [数据增强, 深度学习]
tags: [Python, TensorFlow, OpenCV]
toc: true
comments: true
---


## Blur

- albumentations的blur是通过opencv实现的，tensorflow没有，但是可以使用卷积来代替  
- 查询opencv所使用的核  
- 流程  
	- 了解cv2.blur和tf卷积的大致原理  
- 输入输出大小一致，ksize必须是奇数，对于tensorflow，在卷积前必须pad，pad的个数为`(ksize-1)/2`，而且为镜像填充BORDER_DEFAULT  
- 暂定输入大小为四维BHWC  
- 为什么不对呢？灰度就行，rgb就不行  
	- 为什么rgb生成的结果，三个维度的(H,W)都是一致的呢  
	- 图像处理上的卷积和深度学习上的卷积不一致，dip的是every conv on every channel，相当`HW*KK`，而dl的是一起的，相当`HWC*KKC`  
	- 深度可分离卷积可以解决，深度可分离卷积的filters大小为`HWC1`，其中1代表的是对于每个通道卷积所使用的filters数量  
- tensorflow操作要注意数据类型要一致  


## 参考链接

- [tensorflow2.4-api](https://tensorflow.google.cn/versions/r2.4/api_docs/python/tf)  
- [opencv3.4-api](https://docs.opencv.org/3.4.16/)  
- [opencv-border_default-默认的填充方式](https://docs.opencv.org/3.4.16/d2/de8/group__core__array.html#gga209f2f4869e304c82d07739337eae7c5afe14c13a4ea8b8e3b3ef399013dbae01)  
- [opencv-blur](https://docs.opencv.org/3.4.16/d4/d86/group__imgproc__filter.html#ga8c45db9afe636703801b0b2e440fce37)  
- [tf.nn.conv2d](https://tensorflow.google.cn/versions/r2.4/api_docs/python/tf/nn/conv2d) 
- [tf.nn.depthwise_conv2d](https://tensorflow.google.cn/versions/r2.4/api_docs/python/tf/nn/depthwise_conv2d)
- [tf.pad](https://tensorflow.google.cn/versions/r2.4/api_docs/python/tf/pad)  
- [none](https://stackoverflow.com/questions/48981022/what-is-the-difference-between-tf-nn-convolution-and-tf-nn-conv2d)
