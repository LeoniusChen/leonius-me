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

在[r9y9/tacotron_pytorch](https://github.com/r9y9/tacotron_pytorch)中，AttentionWrapper集成了计算attention的功能  
BahdanauAttention的计算，以下所有的计算被包裹在AttentionWrapper：
1) [attention_context, mel]经过一层RNN成为计算attention的query，维度是[B, 1024]，并被降维到[B, 1, 128]    
2) text_encoder_outputs经过处理成为key，维度是[B, T, 128]  
3) **tanh(query + key)**，计算得到alignment_energy，维度是[B, T, 128]，并降维到[B, T, 1]->[B, T]，该matrix的每一行表示 当前decoder步关注text序列中各个时间步的权重（未归一化）  
4) 进行mask，将text的padding部分的权重设置为**无穷小**  
5) 进行归一化，利用softmax对每一行进行归一化，得到了alignment，即**当前mel关注哪些时刻的text**
6) 将alignment与原始的text_encoder_outputs相乘 [B, 1, T] * [B, T ,512] = [B, 1, 512] -> [B, 512]，即为attention_context


## Tacotron2
Tacotron2中，RNN换成了LSTM，其实现是[torch.nn.LSTMCell(input, (h0, c0))](https://pytorch.org/docs/stable/generated/torch.nn.LSTMCell.html)，
因此每次输入的隐状态有2个  
  
LSA具体的实现过程是：  
AttentionWrapper -> LSA -> BahdanauAttention  
1) 同样的得到query与key  
2) 利用cumulative [B, 1, T]，通过卷积的操作得到location [B, 32, T] -> [B, 128, T] -> [B, T, 128]  
3) **tanh(query + key + location)**，计算得到alignment_energy，同样的降维到[B, T] **这里引入了location，因此称之为location sensitive**  
4) softmax进行归一化，得到alignment  
5) cumulative += alignment，这里的cumulative累加了每一次的alignment，相当于引入了历史alignment的信息  

## 关于stop-net不能停止
1) 尝试在text末尾加EOS截止符  
[issue1](https://github.com/NVIDIA/tacotron2/issues/407)  [issue2](https://github.com/NVIDIA/tacotron2/issues/254#issuecomment-523707805)  
实验证明区别不大  
2) 对stop-net输出的imblanced learning问题做优化  
[issue3-imiblanced](https://github.com/NVIDIA/tacotron2/issues/319#issuecomment-603600457)  
实验证明有效

## 关于替换Attention的尝试
1) Forward Attention version1 (FA1) 有效，但是目前尚没有集成在Repo中
2）Stepwise Monotonic Attention (SMA) 在同样的reduction factor下表现优于FA1  
出现skip的次数明显减少  
3) GMM-based Attention的复现，见下面

## GMM-based Attention
GMM Attention是**purely location**的，它在计算的过程中与memory(text)、前一时刻的alignment都没有关系
- 原始出处 [Generating Sequences With Recurrent Neural Networks](https://arxiv.org/abs/1308.0850) (arXiv 2013)
- TTS中的应用 [Towards End-to-End Prosody Transfer for Expressive Speech Synthesis with Tacotron](http://proceedings.mlr.press/v80/skerry-ryan18a/skerry-ryan18a.pdf) (ICML2018)
- V0详细公式及改进版V1/V2 [Location-Relative Attention Mechanisms For Robust Long-Form Speech Synthesis](https://arxiv.org/abs/1910.10288)
- Tensorflow版本的GMM V0 [ref code in keithito](https://github.com/keithito/tacotron/issues/136)
- PyTorch版本author自己改进的离散型的GMM Attention [ref code in mozilla/TTS](https://github.com/mozilla/TTS/blob/dev/TTS/tts/layers/attentions.py)及[公式解读](https://erogol.com/two-methods-for-better-attention-in-tacotron/)
- [issue in Rayhane-mamah/Tacotron-2](https://github.com/Rayhane-mamah/Tacotron-2/issues/265) 关于GMM效果的讨论  