---
title: 常用的Linux命令
date: 2025-02-10 15:13:52
tags: [WSL, Linux, Shell]
categories: [Linux, Shell]
keywords: Linux, Shell
description: 一些常用的Linux命令
---
## 常用的Linux命令

### 文件与目录操作
#### mkdir——创建文件目录

```
mkdir -p <dirname>
```
在当前目录下创建一个名为`<dirname>`的子目录或文件。`-p`为可选参数，若文件或目录名不存在则创建一个。

```
mkdir -p <dirname>/<subdir_name>
```
在已创建的目录下新建一个名为`<subdir_name>`的子目录。为了演示，使用`mkdir`命令创建如下的目录结构：
```
MyProject
├── README.md
├── script
└── test
    ├── text1.txt
    ├── text2.txt
    ├── text3.txt
    └── text4.txt
```

#### tree——查看项目树

可以在项目文件夹下运行`tree`命令来以目录树的形式查看项目的文件结构。常用的参数：

- `-a`：显示所有文件目录
- `-d`：只显示目录
- `-L n`：指定显示的目录深度，显示n层。

```
tree <ProjectName> -a -d -L 1
```
我们可以使用`tree`命令来查看我们刚才创建的目录的文件树：
```
tree MyProject
```

注：有些Linux发行版中没有安装`tree`命令，因此在使用前需要对`tree`进行安装。在Ubuntu发行版下，可以使用` sudo apt install tree`命令安装`tree`。

#### ls——列出目前工作目录所含的文件及子目录

在之前的目录下应用`ls`，可以看到如下结果：

```
ls MyProject

README.md  script  test
```

`ls`的常用参数：

- `-a`：显示所有文件及目录 (`.`开头的隐藏文件也会列出)
- `-R`：递归显示目录中的所有文件和子目录 
- `-l`：以长格式显示文件和目录信息，包括权限、所有者、大小、创建时间等；`ls -l`可以简写为`ll`
- `-t`：按照修改时间排序显示当前目录中的文件和目录

在当前目录下，使用`ll`可以得到如下的结果：

```
total 20
drwxr-xr-x 5 root root 4096 Feb 15 15:57 ./
drwxr-xr-x 4 root root 4096 Feb 15 15:43 ../
drwxr-xr-x 2 root root 4096 Feb 15 15:44 README.md/
drwxr-xr-x 2 root root 4096 Feb 15 15:57 script/
drwxr-xr-x 6 root root 4096 Feb 15 15:44 test/
```

#### cd——改变当前工作目录，切换到指定位置

```
cd <dirname>
```

在使用`cd`命令时，`~`表示为`home`目录，`.`则是表示目前所在的目录，`..`则表示目前目录位置的上一层目录。如果省略目录名，则返回至`home`默认登陆目录下。同时，也可以通过连续多次使用`..`来切换到更高级的目录。

#### pwd——显示当前工作路径

执行`pwd`可立刻得知目前所在的工作目录的绝对路径名称。

```
pwd # 在当前目录下执行pwd

/home/MyProject
```

### 文本文件的编辑与阅读

#### cat——连接文件并打印到标准输出设备

```
cat [parameters] <filename>
```

`cat`命令用于查看和连接文件。也可以使用`cat`来创建文件。`cat`的常见参数如下：

- `-n`：显示行号，会在输出的每一行前加上行号
- `-b`：显示行号，但只对非空行进行编号
- `-s`：压缩连续的空行，只显示一个空行
- `-E`：在每一行的末尾显示`$`符号
- `-T`：将 Tab 字符显示为`|`
- `-v`：显示一些非打印字符

可以通过如下的方式来链接文件：
```
cat file1 file2 > combined_file
```

#### head/tail——查看文件的开头/结尾

可以使用`head`（`tail`）命令来查看文件的开头（结尾）。可以通过`--lines=n`参数来指定显示开头（结尾）的几行。

```
head --lines=5 README.md
```

通过上面的方式可以显示`README.md`文件的前5行内容。

#### less/more——查看文件的全部内容

在linux中可以通过`less/more <filename>`的方式来查看一个文件的全部的内容。这两个命令的主要区别在于，`less`可以上下滚动，而`more`在linux中只能向下滚动查看文件的内容。

在查看结束后，按键盘`Q`键，即可退出文档的阅读模式。

#### vi/vim——启动vim文件编辑器

在Linux中，我们通常使用vim编辑器来方便快捷的对文件进行编辑。Vim是从vi发展出来的一个文本编辑器，所有的Unix Like系统都会内建vi编辑器。我们可以通过如下的方式来启动vim来打开我们要编辑的文件。

```
vim <filename>
```

vim编辑器有三种模式，分别是：命令模式（Command Mode）、输入模式（Insert Mode）和命令行模式（Command-Line Mode）。

一启动vim，便进入了命令模式，此时我们并不能直接对文档进行编辑。此状态下键盘输入会被vim识别为命令，而非输入字符。以下时常用的几个命令：

- `i` -- 切换到输入模式，在光标当前位置开始输入文本
- `x` -- 删除当前光标所在处的字符
- `:` -- 切换到底线命令模式，以在最底一行输入命令
- `a` -- 进入插入模式，在光标下一个位置开始输入文本
- `o` -- 在当前行的下方插入一个新行，并进入插入模式
- `O` -- 在当前行的上方插入一个新行，并进入插入模式
- `dd` -- 剪切当前行
- `yy` -- 复制当前行
- `p` -- 粘贴剪贴板内容到光标下方
- `P`-- 粘贴剪贴板内容到光标上方
- `u` -- 撤销上一次操作
- `Ctrl + r` -- 重做上一次撤销的操作
- `:w` -- 保存文件
- `:q` -- 退出 Vim 编辑器
- `:q!` -- 强制退出Vim 编辑器，不保存修改

若想要编辑文本，只需要启动 Vim，进入了命令模式，按下`i`切换到输入模式即可。在命令模式下按下 i 就进入了输入模式，使用`Esc`键可以返回到普通模式,在底线命令模式下，按`ESC`键可随时退出底线命令模式。

### 文件属性

#### file——辨识文件类型

```
file <filename>
```

通过file指令，我们得以辨识该文件的类型。使用`file`指令查看`README.md`的文件类型：

```
file README.md
```

#### where——查找文件的位置

```
where <filename>
```

我们可以方便的通过`where`指令来获取一个文件的绝对路径。通过`where`查询`README.md`的位置：

```
where README.md
```

### 总结

本文介绍了linux中常用的一些与文件以及目录操作有关的常用命令。其中mkdir、tree、ls、cd、pwd与文件路径操作相关的常用命令；cat、head/tail、more/less、vim是与文件的编辑阅读相关的常用命令；file、where是与文件属性相关的常用命令。
