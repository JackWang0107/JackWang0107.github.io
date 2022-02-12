---
title: PyQt5学习笔记 Chapter 0-Introduction： Part 2.QApplication与QObject
date: 2021-12-17 14:23:55
img: https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211222112408341.png
summary: '本文主要讲解了Qt中的核心对象：QApplication与QObject'
categories:
  - PyQt5 学习笔记
tags:
  - Python
  - GUI开发
  - PyQt
---

> 本文主要讲解了Qt中的核心对象：QApplication与QObject

![Qt可以制作出的高端界面](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211222112408341.png)



# PyQt5学习笔记 Chapter 0-Introduction： Part 2.QApplication与QObject

想要学好Qt，我们就需要掌握Qt的设计与架构，单纯的为掌握API是徒劳的，没有理解终究会忘记。本文作为基础，首先介绍了Qt中的QApplication与QObject。





## 1. QObject

前面我们介绍过，Qt是一个面向对象的应用程序框架。因此在Qt中的一起都是对象，例如我们看到的窗口、按钮等等都是对象，甚至连整个应用程序都是对象。不同的对象有不同的属性和方法，例如对于窗口，窗口大小和锚点（左上角点）的位置就是其属性，窗口移动到某个位置就是其方法。而针对按钮，按钮也可以将按钮的大小作为其属性，而将点击作为其方法。

因此针对所有的对象，Qt提供了QObject作为所有类的基类，所有的类都是QObject的基类。

![Qt官方对QObject的介绍](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211222112125225.png)

在未来，随着我们逐步的深入，我们会了解到诸如信号与槽、定时器等机制。而这些机制其实都是由QObject所提供的。

现在我们只需要记住，Qt所写的GUI程序中的一切都是对象即可





## 2. QApplication

前面我们说了，Qt中一切都是对象，Qt甚至将程序本身也抽象成了一个类。这个类就是QApplication。

**QApplication接管GUI程序的主控制流**，具体包括：线程的管理，事件的分发（例如鼠标点击按钮后由谁来处理），回调函数的唤醒等等。因此我们使用Qt开发程序的时候必须要有一个QApplication对象。

![Qt官方的文档](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211222112030842.png)

程序可以开启也可以关闭，因此QApplication对象就有运行和关闭等功能。而由于运行程序和结束程序是QApplication对象最常见的功能，因此因此我们在未来不断地一段时间内都只会用到QApplication的运行和结束方法。在未来我们会介绍线程等内容。

现在我们只需要知道，使用Qt写程序的时候必须要有一个QApplication，而程序开始运行则需要调用QApplication对象的运行方法
