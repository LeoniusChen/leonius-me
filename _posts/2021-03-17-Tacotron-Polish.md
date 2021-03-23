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
[CorentinJ/Real-Time-Voice-Cloning](https://github.com/CorentinJ/Real-Time-Voice-Cloning)

## Tacotron1
Tacotron1中，Attention_RNN的实现是[torch.nn.GRUCell(input, hidden)](https://pytorch.org/docs/stable/generated/torch.nn.GRUCell.html)  
这里input是[mel, 前一时刻attention context vector(bs, 256)]，hidden是前一时刻RNNCell的输出  
在Attention_RNN中包含了BahdanauAttention的计，这一模块是重复调用的，并没有利用**self.**这样的全局变量  
Tacotron1中已经支持reduction_factor，没有stop_net

BahdanauAttention的计算：
1) [attention_context, mel]经过一层RNN成为计算attention的query，维度是[B, 1024]，并被降维到[B, 1, 128]    
2) text_encoder_outputs经过处理成为key，维度是[B, T, 128]  
3) 计算得到alignment_energy，维度是[B, T, 128]，并降维到[B, T, 1]->[B, T]，该matrix的每一行表示 当前decoder步关注text序列中各个时间步的权重（未归一化）  
4) 进行mask，将text的padding部分的权重设置为**无穷小**  
5) 进行归一化，利用softmax对每一行进行归一化，得到了alignment，即**当前mel关注哪些时刻的text**


## Tacotron2
Tacotron2中，RNN换成了LSTM，其实现是[torch.nn.LSTMCell(input, (h0, c0))](https://pytorch.org/docs/stable/generated/torch.nn.LSTMCell.html)，
因此每次输入的隐状态有2个  
在[r9y9/tacotron_pytorch](https://github.com/r9y9/tacotron_pytorch)中，AttentionWrapper集成了计算attention的功能  
其具体的实现过程是：  
AttentionWrapper -> LSA -> BahdanauAttention