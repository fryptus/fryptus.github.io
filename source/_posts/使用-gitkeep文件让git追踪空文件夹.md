---
title: 使用.gitkeep文件让git追踪空文件夹
date: 2024-12-25 00:36:44
updated:
tags: Git
categories: Git
keywords: .gitkeep文件
description: 使用.gitkeep文件来让git识别到空文件夹，从而保证项目结构的一致性。
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

## 使用.gitkeep文件让git追踪空文件夹

在一些情况下，我们会将空文件夹推送到我们的git仓库中，但是git作为一个文件追踪系统并不会追踪空文件夹，这样就会导致我们无法将空文件夹推送到git仓库中，这样就会导致别人在我们clone仓库时会导致与我们本地的项目不同的项目结构。因此在一些需要推送空文件的场景下，为了让git能够识别空文件夹，我们只需要在空文件夹下添加一些文件即可。`.gitkeep`就是这样的一个文件。

### 使用.gitkeep文件

为了使git能够识别空文件夹，要往这些文件夹中加入一些文件就行了。加入任何文件都可以，只需要在空目录下加入简单的虚拟文件，就可以使得文件夹被追踪，从而正常推送。我们可以复制一个空的文本文件（如 file.txt）粘贴进去，也可以放一张图片。然而，常见的标准做法是在空文件夹中放一个 .gitkeep 文件。

于是，就有了一个不成文的规定，通常我们放一个名为 .gitkeep 的文件到空文件夹或空目录，以此实现其 Git 跟踪。但是这样的做法并没有在官方文档中定义，只是一个开发者公认的约定。

我们在需要保存的空目录下运行以下命令，就可以创建一个`.gitkeep`文件

```
$ touch .gitkeep
```

此时我们再提交commit，并将仓库推送，就可以看到项目中的空文件夹被git识别，这样我们就能保证项目结构的一致性。

例如一个本地的项目结构：
```
--Ch1. Python Fundamentals
    --asset
    --notebook
```
在未使用`.gitkeep`文件占位时，git仓库中的项目结构如下：

![远端结构](远端结构.png)

使用`.gitkeep`后，仓库中的项目结构与本地一致：

![使用.gitkeep后的远端结构](使用.gitkeep后的远端结构.png)

### 总结

Git默认是不跟踪空文件夹和空目录的，所以要想推送空文件夹或空目录，就必须在里面放一个文件，即使是空文件也行，但必须要有，通常根据开发者默认的规定，我们可以在空文件夹或空目录下放一个名为`.gitkeep`的文件，以此实现其 git 跟踪。
