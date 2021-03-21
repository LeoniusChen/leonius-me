---
title: 优化Nvidia/Tacotron2
tags: TTS
layout: article
---

由于Nvidia版本的Tacotron2实在是比较差，不支持redcution tactor>2，不支持multi_speaker，LSA不够robust...
因此非常有必要构建一个自己用的顺手的Tacotron版本，以方便后面的工作。  
本工作将借鉴很多现有的TTS复现工作

[r9y9/tacotron_pytorch](https://github.com/r9y9/tacotron_pytorch)  
[NVIDIA/tacotron2](https://github.com/NVIDIA/tacotron2)