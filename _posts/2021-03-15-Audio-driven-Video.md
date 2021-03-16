---
title: 关于声音驱动脸部图像的文章调研
tags: Audio-driven-Face
layout: article
---

## AudioDVP
- [Photorealistic Audio-driven Video Portraits](https://purehost.bath.ac.uk/ws/portalfiles/portal/211657248/AudioDVP_WenEtAl_TVCG2020.pdf) (ITVCG 2020)  
该论文是北航团队发表在 IEEE Transactions on Visualization and Computer Graphics 期刊上的成果，并已开源代码。
作为自己入门audio-driven-video的论文，可以从中寻找相关的参考文献。

从audio到人脸的生成，其技术路线是：  
1）Audio2Expression：首先引入一个3D morphable model来表征人脸，该模型是由四组参数构成的，
其中expression参数控制着人脸的表情，语音改变这组参数的系数，便带来了表情的变化。将语音表征（MFCC）输入AT-Net，
得到说话人无关的语音表示，利用该表示预测上述的expression的系数；将其与3D morphable model其它参数混合在一起，
通过图像处理的手段，得到初步的人脸图片。需要注意的是，这里的预测的expression系数所对应的label 是利用video在face reconstruction中优化得到的。  
2）rendering：初步得到的人脸图片并不够真实，需要着重对下巴等部分进行渲染。利用Conditional GAN的架构完成这一目的。

本文总结到，入门这个领域，有4个方面的经历（related works）是较重要的：  
1）3D face reconstruction：这是人脸重建的基础，解决的是 利用怎样的输入和模型来重构face  
2）Video-driven的脸部重建：即更改source face的expression参数，让它成为target face序列，比如通常的应用有 让一个静止（still）的人脸图片说话动起来  
3）Audio-driven的脸部重建：即从声音到face，这是本文的重点所在，也是待优化的地方  
4）GAN用来render人脸：通常是利用Conditional GAN

## 相关参考文献

### Overview
- 3D morphable face models—Past, present, and future (ACM Trans Graph 2020)
- VR content creation and exploration with deep learning: A survey (Computational Visual Media 2020)
- State of the art on monocular 3D face reconstruction, tracking, and applications (Comput Graph Forum 2018)

### 3D face reconstruction部分
- single-imgae-based方法: Accurate 3D face reconstruction with weakly-supervised learning: From single image to image set (CVPR Workshops 2019)
- CNN-based realtime dense face reconstruction with inverse-rendered photo-realistic face images (TPAMI 2019)
- 3D face model到2D image的过程: An efficient representation for irradiance environment maps (SIGGRAPH 2001)
- Unsupervised training for 3D morphable model regression (CVPR 2018)

### Audio2Expression部分
- AT-net: Hierarchical cross-modal talking face generation with dynamic pixel-wise loss (CVPR 2019)

### 对比的baseline
- DAVS: Talking face generation by adversarially disentangled audio-visual representation (AAAI 2019)
- ATVG: Hierarchical cross-modal talking face generation with dynamic pixel-wise loss (CVPR 2019)
- LipGAN: Towards automatic face-to-face translation (ICMM 2019)
- SDA: Realistic speech-driven facial animation with GANs (IJCV 2020)
- Audio2Obama: Synthesizing Obama: Learning lip sync from audio (ACM Trans Graph 2017)
- Neural voice puppetry: Audio-driven facial reenactment (ECCV 2020)



<!-- more -->