---
title: 语音合成相关论文
tags: Papers
layout: article
---

本文用以记录语音合成 (Speech Synthesis) 领域相关论文，包括经典的和未来的方向。

## 声学模型

### 非并行
- Tacotron1: [Tacotron: Towards End-to-End Speech Synthesis](https://arxiv.org/abs/1703.10135) (Interspeech 2017)
- Tacotron2: [Natural TTS Synthesis by Conditioning WaveNet on Mel Spectrogram Predictions](https://arxiv.org/abs/1712.05884) (ICASSP 2018)

### 并行
- FastSpeech1: [FastSpeech: Fast, Robust and Controllable Text to Speech](https://openreview.net/pdf/c2b7c145443ef7be7946e5dc58f88f12d442e307.pdf) (NIPS 2019)
- FastSpeech2: [FastSpeech 2: Fast and High-Quality End-to-End Text to Speech](https://arxiv.org/abs/2006.04558) (arXiv 2020)
- Glow-TTS: [Glow-TTS: A Generative Flow for Text-to-Speech via Monotonic Alignment Search](https://arxiv.org/abs/2005.11129) (NIPS 2020)
- EfficientTTS: [EfficientTTS: An Efficient and High-Quality Text-to-Speech Architecture](https://arxiv.org/abs/2012.03500) (arXiv 2020)
- BVAE-TTS: [Bidirectional Variational Inference for Non-Autoregressive Text-to-Speech](https://openreview.net/pdf?id=o3iritJHLfO) (ICLR 2021)


## 声码器 Vocoder

### 非并行
- WaveNet: [WaveNet: A Generative Model for Raw Audio](https://www.isca-speech.org/archive/SSW_2016/pdfs/ssw9_DS-4_van_den_Oord.pdf) (ISCA SS Workshop 2016)
- waveRNN: [Efficient Neural Audio Synthesis](http://proceedings.mlr.press/v80/kalchbrenner18a/kalchbrenner18a.pdf) (ICML 2018)

### 并行
- WaveGlow: [WaveGlow: A Flow-based Generative Network for Speech Synthesis](http://128.84.4.18/abs/1811.00002) (ICASSP 2019)

## 韵律建模 Prosody Modeling

### Coarse-Level
- Prosody-Tacotron: [Towards End-to-End Prosody Transfer for Expressive Speech Synthesis with Tacotron](http://proceedings.mlr.press/v80/skerry-ryan18a/skerry-ryan18a.pdf) (ICML2018)
- GST-Tacotron: [Style Tokens: Unsupervised Style Modeling, Control and Transfer in End-to-End Speech Synthesis](http://proceedings.mlr.press/v80/wang18h/wang18h.pdf) (ICML2018)
- VAE-Tacotron: [Learning Latent Representations for Style Control and Transfer in End-to-End Speech Synthesis](https://arxiv.org/abs/1812.04342) (ICASSP 2019)
- VAE-Flow: [Using VAEs and Normalizing Flows for One-shot Text-To-Speech Synthesis of Expressive Speech](https://arxiv.org/abs/1911.12760) (ICASSP 2020)

### Fine-Grained
- Fine-grained-Attention: [Robust and Fine-Grained Prosody Control of End-to-End Speech Synthesis](https://arxiv.org/abs/1811.02122) (ICASSP 2019)
- Manual-feature-based: [Fine-grained robust prosody transfer for single-speaker neural text-to-speech](https://arxiv.org/abs/1907.02479) (Interspeech 2019)
- CopyCat-VC: [CopyCat: Many-to-Many Fine-Grained Prosody Transfer for Neural Text-to-Speech](http://www.interspeech2020.org/uploadfile/pdf/Thu-2-9-1.pdf) (Interspeech 2020)


<!-- more -->