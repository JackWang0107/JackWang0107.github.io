---
title: PyQt5学习笔记 Chapter 0-Introduction： Part 0.初心
date: 2021-12-16 20:32:45
img: https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211216205416019.png
summary: '本文作为PyQt系列学习笔记的第一篇，记录了我为什么要学习PyQt，以及本系列文章的组织形式'
categories:
  - PyQt5 学习笔记
tags:
  - Python
  - GUI开发
  - PyQt
---

> 本文作为PyQt系列学习笔记的第一篇，记录了我为什么要学习PyQt，以及本系列文章的组织形式

![Qt的Python绑定：PySide](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211216205416019.png)





# PyQt5学习笔记： Part 0-初心

作为系列文章的第一篇，本文将讲解我为什么要学习PyQt、本系列文章的目标以及未来文章的组织形势



## 1. 为什么要学习PyQt



### 1. What is User Interface

一般来说，我们写出的程序都是直接运行的，通常与用户只有有限的交互（例如input等待用户输入），这样的程序在程序员看来就以及很可以了。然而从用户的角度来说，这样的程序体验非常差。因为用户无法与程序进行交互。

而对于提供了用户界面（User Interface）的程序用户体验就会好很多，而且看起来就会非常高级。

例如下面有了文字用户界面，用户就可以指定不同的参数和选项，从而实现不同的功能。

![用户界面之一：图形文字界面文件管理器（Text User Interface File Manager)](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/Fdedit.png)



### 2. Classification of User Interface

一般来说，用户界面分为两种，一种是**文字用户界面**，另外一种就是**图形用户界面**。



#### 1. Text(-based) User Interface （TUI）

文字用户界面即指程序与用户的交互界面就是由文本构成的，即用文本模拟出来的界面。例如我们上面看到的**FreeDOS Edit**。

很多程序都提看TUI，例如make。有些程序源码中在Makefile中提供了menuconfig规则，我们使用make来利用这个规则进行编译时配置的时候就会看到TUI界面。如下图我们进行内核编译参数配置的时候使用menuconfig。

![TUI编译内核](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211216214559133.png)

然后我们就能看到TUI界面

![Menuconfig的TUI界面](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211216214636804.png)

可以看到TUI的界面其实就是把文本背景设置为不同的颜色，以此来表达出界面。



#### 2. Graphic(-based) User Interface

图形用户界面则是现在程序的标配，我们在Windows上用的QQ、微信等等都是GUI程序。在Linux上，所谓的桌面也是一个GUI程序。

![Linux Mint的桌面](https://upload.wikimedia.org/wikipedia/commons/4/48/Linux_Mint_19.1_"Tessa"_(Cinnamon).png)





所以话说回来，我们学习PyQt的目的其实就非常明确了，我们就是想要能够做出来一个GUI程序，即类似于QQ、微信这样的程序。





## 2. 本系列文章的组织形式

本系列文章将从0开始，记录我学习PyQt的过程，同时也将作为PyQt的教程。

由于需要学习得内容比较多，因此类似于一本书，全书将会分为多个章节。每个章节围绕一个主题进行讲解。

讲解内容包括Qt Designer、Layout、Signal与Slot等等内容
