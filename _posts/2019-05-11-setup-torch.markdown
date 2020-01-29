---
layout: post
title: Tutorial for setup environment for lua
date: 2019-05-11 10:03:00 +0800
categories: cheatsheet
---



1. Install cudatoolkit 8.0 and cudnn 5.1

   If you are setting environment on Suzhou HPC, you can use the following command to add them to your environment.

   ```shell
   module add cuda/8.0 cudnn/5.1
   ```

   Otherwise, you need to download the cudatoolkit install runfile from nvidia website https://developer.nvidia.com/cuda-80-download-archive and install it manually. Don’t use anaconda to install, it won’t work.

   And for cudnn, you need to register a nvidia developer account (free), and then download it from https://developer.nvidia.com/rdp/cudnn-archive 

   Remember to download cuDNN v5.1 (Jan 20, 2017), for CUDA 8.0

2. Set cudnn path:

   ```shell
   export CUDNN_PATH=/mnt/lustre/cm/shared/global/src/dev/cudnn/5.1/lib64/libcudnn.so.5
   ```

   (depends on where you install the cudnn library)

3. Install OpenBLAS:

   ```shell
   git clone https://github.com/xianyi/OpenBLAS.git
   cd OpenBLAS
   make NO_AFFINITY=1 USE_OPENMP=1
   sudo make PREFIX=[path you want to install OpenBLAS] install
   ```

   Set environment variable:

   ```shell
   export CMAKE_LIBRARY_PATH=[path you install OpenBLAS]/include:[path you install OpenBLAS]/lib:$CMAKE_LIBRARY_PATH
   ```

   (Mind the line break while copying)

4. Install torch7:

   ```shell
   git clone https://github.com/torch/distro.git ~/torch --recursive
   cd ~/torch./install.sh
   ```

5. Install cudnn for lua:

   ```shell
   git clone https://github.com/soumith/cudnn.torch.git -b R5
   cd cudnn.torch
   luarocks make cudnn-scm-1.rockspec
   ```

6. Install luarocks to make torch work with cuda:

   ```shell
   luarocks install cutorch
   luarocks install cunn
   ```

7. If you want to install other lua libraries:

   luarocks install [library you need]

All set. You should be able to use torch7 with lua now! For more information about torch, check this website: [http://torch.ch/docs/getting-started.html#](http://torch.ch/docs/getting-started.html#)

