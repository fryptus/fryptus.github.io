---
title: manim的安装与使用
date: 2025-01-19 16:32:51
tags: [manim，教程]
categories: manim教程
keywords: manim，安装
description: manim动画库的安装以及安装验证
top_img:
comments:
cover:
toc:
toc_number:
toc_style_simple:
copyright:
copyright_author:
copyright_author_href:
copyright_url:
copyright_info:
mathjax:
katex:
aplayer:
highlight_shrink:
aside:
abcjs:
noticeOutdate:
---
## manim的安装与使用

Manim是一个用于生成准确的数学动画的渲染引擎，最早由Grant Sanderson（3Blue1Brown）开发，它允许开发者可以通过编程的方式来生成3Blue1Brown风格的动画。原本manim只是3Blue1Brown用来制作视频的个人项目，但是在开源后立刻受到了社区的欢迎。如今manim有两个主要的分支（版本），一个是Manim（或ManimCE）即由社区维护的版本；而另一个是ManimGL，由3Blue1Brown开发的最新的版本，其中有许多实验性的功能。除上面的两个分支外，manim还有一个分支——（ManimCairo），是manim的旧版本。

Maniim用编程的方法创建数学动画，让数学更加简单易懂。接下来我们将迈出使用manim的第一步，本文将介绍manim的安装以及验证。

### manim的安装

本文在windows环境下安装manim，为了方便快捷的进行安装，本文使用windows包管理工具`winget`进行安装，各位也可以根据自己的情况按照官网所给出的[教程]((https://docs.manim.community/en/stable/installation/windows.html#win-optional-dependencies))使用其他包管理工具或者进行手动安装。

#### 验证winget

Winget是一种命令行工具，使用户能够在windows计算机上发现、安装、升级、删除和配置应用程序。 此工具是 Windows 程序包管理器服务的客户端接口。首先确认系统是否满足需求：WinGet 只可以运行在 Windows10 高于 1709 (Build 16299) 的版本和 Windows11 上。打开终端，使用下面的命令验证计算机上的`winget`是否可用：

```
winget --version
```

若控制台输出如下的版本系信息，说明计算机中的`winget`可用，否则则需要安装`winget`或者采取手动安装的方式对manim进行安装。

```
v1.9.25200
```

#### 安装manim的依赖

为了使manim正常工作，我们需要先安装manim的环境依赖。Manim需要在Python (3.8 or above) 的环境下工作，同时为了保证能输出正确的视频格式，manim需要`ffmpeg`。

首先使用`winget`安装`ffmpeg`,运行下面的终端指令：

```
winget install ffmpeg
```

![ffmpeg安装](ffmpeg安装步骤.png)

输入Y同意，等待下载以及安装完成。

![ffmpeg安装](安装步骤1.png)

重启shell，完成安装。安装完成后，在命令行中输入：

```
ffmpeg -h
```
输出如下的版本信息以及帮助文档，说明安装成功。

```
ffmpeg version 7.1-full_build-www.gyan.dev Copyright (c) 2000-2024 the FFmpeg developers
  built with gcc 14.2.0 (Rev1, Built by MSYS2 project)
  configuration: --enable-gpl 
  ...
```

同时，为了能在manim中使用Latex来正确的渲染数学公式，我们需要安装Latex环境。如果不打算在manim中使用Latex，那么此步可以跳过。

在windows下，推荐安装MikTex的Latex发行版本，我们既可以通过包谷案例工具进行安装，也可以到MikTex的官网下载并进行手动安装。运行下面的命令，安装MikTex：

```
winget install MiKTeX.MiKTeX
```

如果电脑的硬盘空间不足，也可以自行搜索并安装稍小一点的TinyTex发行版。或者可以通过如下的方式（`-l`）来指定`winget`的安装路径：

```
winget install MiKTeX.MiKTeX -l "E:\Program_Files\winget\Tex"
```

安装完成后，可以搜索并打开MikTex Console，安装一些必要的宏包。

![MikTex安装](MikTex安装.png)

以下是一些manim可能会用到的宏包列表：

```
amsmath babel-english cbfonts-fd cm-super ctex doublestroke dvisvgm everysel
fontspec frcursive fundus-calligra gnu-freefont jknapltx latex-bin
mathastext microtype ms physics preview ragged2e relsize rsfs
setspace standalone tipa wasy wasysym xcolor xetex xkeyval
```

#### manim包的安装

接下来，我们可以安装manim包了。我们可以简单地通过`pip`进行安装，在python环境下，在命令行中运行：

```
python -m pip install manim
```

或者我们也可以通过conda进行安装。首先，为manim新创建一个虚拟环境，在conda终端中运行：

```
conda create -n my_manim_env python=3.11
conda activate my_manim_env
```

然后再我们创建的`my_manim_env`下安装manim：

```
conda install -c conda-forge manim
```

以上，我们就完成了manim的安装。

### manim安装验证

在完成manim的安装后，我们可以通过下面的脚本来验证manim是否能够正常使用：

```python
from manim import *


class TransformExample(Scene):
    def construct(self):

        banner = ManimBanner()
        banner.shift(UP * 0.5)
        self.play(banner.create(), run_time=1)
        self.play(banner.animate.scale(0.3), run_time=0.5)
        self.play(banner.expand(), run_time=1)

        t = Text("测试中文能否显示").next_to(banner, DOWN * 2)
        tex = VGroup(
            Text("测试数学公式:", font_size=30),
            Tex(r"$\sum_{n=1}^\infty \frac{1}{n^2} = \frac{\pi^2}{6}$"),
        )
        tex.arrange(RIGHT, buff=SMALL_BUFF)
        tex.next_to(t, DOWN)
        self.play(Write(t), run_time=1)
        self.play(Write(tex), run_time=1)

        self.wait()
```

运行下面的代码，执行脚本：

```
manim -p .\sample.py
```

manim的默认输出为`.mp4`文件，并会在渲染完成后自动播放，使用：
```
manim -i .\sample.py 
```
可以得到`.gif`文件。

最终，得到如下结果，说明manim安装成功。

![测试结果](测试结果.gif)


以上我们就完成了manim的安装与测试，之后我们就可以方便快捷的通过python编程来创建精准的manim动画来进行演示了。


