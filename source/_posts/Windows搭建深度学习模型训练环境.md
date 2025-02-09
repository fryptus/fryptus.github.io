---
title: Windows搭建深度学习模型训练环境
date: 2025-02-07 11:47:19
tags: [深度学习, 环境搭建, WSL, Cuda, Conda]
categories: [深度学习,环境搭建]
keywords: WSL, Cuda, Conda， 深度学习
description: 在Windows中配置WSL深度学习训练环境
---
## Windows搭建深度学习模型训练环境

### 安装WSL

#### 启用WSL

现今许多知名的深度学习框架与开源软件包如deepspeed、Unsloth等都是在Linux环境下进行开发。这些开源软件并没有提供良好的windows环境支持，直接在windows环境下使用会导致不兼容的问题，因此我们需要一个Linux环境。在windows中，微软为我们提供了WSL（适用于Windows的Linux子系统）。WSL是 Windows 的一项功能，可用于在Windows计算机上运行Linux环境，而无需单独的虚拟机或双引导，WSL可以为我们同时使用windows与linux环境的情况下提供高效无缝的开发体验。

WSL很容易安装，在开始之前，我们首先需要开启windows中的WSL功能。在开始菜单栏搜索`启用或关闭Windows功能`，在其中勾选`适用于Linux的Windows子系统`与`虚拟机平台`两个选项，点击确定，然后重启电脑。

![开启相关windows功能](开启相关windows功能.png)

之后验证WSL，打开命令行，输入如下命令：

```
wsl --help
```

若输出如下帮助信息，则说明WSL成功启用。

![WSL验证](WSL验证.png)

#### 安装Linux发行版

在安装之前，可以使用`wsl --update`安装WSL内核更新包，然后通过`wsl --set-default-version 2`将WSL默认版本设置为WSL2。

启用WSL之后就可以在windows中安装相应的Linux发行版了，这里选择使用Ubuntu发行版。使用：

```
wsl --install -d Ubuntu
```

进行安装，等待一段时间安装完成。安装完成后，设置用户名与密码。

![发行版安装](安装设置.png)

可以通过`wsl --list`命令查看已安装的Linux发行版，在命令行中可以通过`wsl -d Ubuntu`来启动已安装的发行版。

至此WSL环境准备完毕。

#### 更改WSL位置

WSL默认安装在系统盘中，随着系统的使用，会占用我们C盘的空间，所以我们可以选择将其打包放到其它盘去。

使用`wsl -l --all -v`命令查看WSL已安装的发行版的详细信息。

![查看发行版](查看发行版.png)

使用如下命令将发行版打包并导出到D盘：

```
wsl --export Ubuntu d:\wsl-ubuntu.tar
```
![导出发行版](导出发行版.png)

导出发行版后，我们需要注销当前的发行版。使用：

```
wsl --unregister Ubuntu
```
将发行版注销。

将之前的发行版注销后，我们就可以使用如下命令来重新导入发行版，并将其安装在D盘：

```
wsl --import Ubuntu d:\wsl-ubuntu d:\wsl-ubuntu.tar --version 2
```

因为之前我们在WSL中设置了默认用户，所以现在需要重新设置默认用户，使用`Ubuntu config --default-user <username>`设置默认用户。

最后使用`del d:\wsl-ubuntu.tar`删除`.tar`文件，我们就完成了发行版的迁移，将WSL的默认安装目录迁移到D:\wsl-ubuntu目录下了。此目录即为WSL的根文件系统。


### 在WSL中安装与配置Conda

#### 安装Conda

在配置好WSL系统环境后，我们就可以进一步安装我们进行深度学习训练所需的Conda与Cuda环境。其中Conda也允许我们可以方便的创建与管理我们所需的python环境，我们在WSL中对Conda进行安装的步骤与在linux系统下安装的步骤完全一致。

在Ubuntu发行版中，我们可以通过运行如下的命令来安装我们所需的COnda版本：

```
cd

wget https://repo.anaconda.com/archive/Anaconda3-2024.02-1-Linux-x86_64.sh

bash Anaconda3-2024.02-1-Linux-x86_64.sh
```

通过上面的命令行，我们首先切换到根目录下，然后下载Conda的安装包，最后通过运行安装文件来完成安装。我们按照Anaconda的提示进行安装，会默认安装到 /home/用户名/anaconda3的路径下。

安装完成后，我们需要将Conda添加到我们的系统路径中。使用Vim打开系统环境变量文件：

```
vim ~/.bashrc
```

按`i`键进入编辑模式，在文件中插入下面的配置信息，添加Anaconda环境变量：

```
export PATH="/home/用户名/anaconda3/bin:$PATH"
```

之后输入`:wq`保存并推出配置文件，最后使用`source`命令更新我们的环境配置：

```
source ~/.bashrc
```

至此Conda的配置完成，我们可以使用`conda --version`来验证我们的安装，若系统输出Conda的版本信息，则说明Conda安装成功。

#### 使用Conda创建Python环境

配置好Conda后，我们就可以使用Conda来创建我们所需的Python环境了。我们可以通过如下的命令来进行创建：

```
conda create -n my_llm_env python=3.12
```

按照引导创建完成后，我们可以使用`conda activate my_llm_env`来激活我们的Python环境，之后就可以通过`pip`或`conda`的方式来安装我们所需的第三方包了。

### 在WSL中安装与配置Cuda

通俗地说，CUDA是一种协助“CPU任务分发+GPU并行处理”的编程模型/平台，用于加速GPU和CPU之间的计算。我们在训练模型时，如果要使用Nvidia的GPU来加速训练，那么我们就必须安装对应版本的Cuda Toolkit。

首先，在安装之前我们可以使用`nvidia-smi`来查看我们机器的GPU信息以及支持的最高版本的Cuda信息。接下来，我们可以到[官网](https://developer.nvidia.com/cuda-downloads?target_os=Linux&target_arch=x86_64&Distribution=WSL-Ubuntu&target_version=2.0&target_type=runfile_local)去下载相对应的Cuda Toolkit。这里我们选择Linux -> X86_64 -> WSL-Ubuntu -> 2.0 -> runfile(local)的版本的runfile进行下载。运行官网给出的下载与安装命令：

```
wget https://developer.download.nvidia.com/compute/cuda/12.8.0/local_installers/cuda_12.8.0_570.86.10_linux.run

sudo sh cuda_12.8.0_570.86.10_linux.run
```

接下来与Conda时的操作一样，我们需要配置Cuda的环境变量，将下面的配置信息添加到我们的配置文件中：

```
export CUDA_HOME=/usr/local/cuda-12.8 
export PATH=$PATH:$CUDA_HOME/bin       
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$CUDA_HOME/lib64
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$CUDA_HOME/extras/CUPTI/lib64
```

至此我们便完成了Cuda的配置。我们可以通过`nvcc --version`来验证我们的环境，若输出了对应的版本信息，则说明我们的安装成功。

至此我们完成了一个在Windows系统下运行Linux环境的深度学习的基础开发环境的搭建，在完成上述步骤后，我们已经具备了可以运行的Python环境以及可以与GPU进行通讯的Cuda平台。
