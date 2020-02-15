---
layout: post
title: Motion Transfer Survey
date: 2020-02-13 11:34:00 +0800
categories: CV
typora-root-url: ..
---

# **Everybody Dance Now**

- video-to-video translation using pose as an intermediate representation.

- Simple method, surprising results -> propose a forensics tool for reliable synthetic content detection.

- Release a dataset for training motion transfer.

Appearance of target video + Motion of source video -> Result video

1. Obtain pose stick figures of the source.
2. Train image-to-image translation model between pose stick figures and images of target person. 
3. Obtain images of target subject in the same pose as source.



# **Liquid Warping GAN**

Proposed a unified framework for the following three tasks:

- Motion Imitation
- Novel View Synthesis
- Appearance Transfer

![img](/assets/images/LiquidWarpingGAN_3.png)

**Keyword**: HMR, SMPL, Neural Mesh Renderer

# **Synthesizing Images of Human in Unseen Poses**

<img src="/assets/images/Synthesizing_1.png" alt="img" style="zoom: 67%;" />

<img src="/assets/images/Synthesizing_2.png" alt="img" style="zoom: 100%;" />