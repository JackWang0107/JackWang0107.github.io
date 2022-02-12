---
title: PyQt5学习笔记 Chapter 0-Introduction： Part 1.Qt、PyQt与PySide
date: 2021-12-16 22:17:55
img: https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/pyqt_vs_pyside.png
summary: '本文介绍了Qt、PyQt与PySide间的关系。'
categories:
  - PyQt5 学习笔记
tags:
  - Python
  - GUI开发
  - PyQt
---

> Before learn something, you'd better know what it is. 
>
> 本文介绍了Qt、PyQt与PySide间的关系。

![PyQt vs PySide](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/pyqt_vs_pyside.png)





# PyQt5学习笔记 Chapter 0-Introduction： Part 1.Qt、PyQt与PySide

在学习一个东西之前，我们需要首先明白这个东西是什么。



## 0. 库与框架

在介绍Qt前，我们需要明白什么是库，什么是框架。

以下内容参考知乎问题[库，框架，架构，平台，有什么明确的区别？](https://www.zhihu.com/question/29643471)博主[苏莉安的回答](https://www.zhihu.com/people/aton)与[ze ran的回答](https://www.zhihu.com/people/ze.ran)以及[程墨Morgan的回答](https://www.zhihu.com/people/morgancheng)



### 1. 库 Library

库可以理解成一个工具箱，工具箱里提供了很多的工具来帮助我们进行工作。

例如我们在淘宝上买了一个书架，快递发过来的是书架的零件，我们需要自己去进行组装。而在组装架子的时候，我们会用螺丝刀。而自己手动拧螺丝刀则会拧到手痛。所以这个时候我们就会去买一个电动螺丝刀。利用这个电动螺丝刀，我们的效率马上会有极大地提升。我们的生产力也会有极大地进步。

而无论螺丝刀还是电动螺丝刀，都是不同的工具。

对应的，库里提供了很多的函数来帮助我们进行工作。利用这些函数，我们的效率马上会翻几倍。

螺丝刀和电动螺丝刀则是完成同一个工作的不同的库，库与库之间也会存在差异。

**然而库之于程序就像工具之于人，是人决定怎么用工具，工具在哪里用；库也是程序（我们程序员）决定在哪里使用、怎样使用；库只是提供了便利，不需要我们自己手动实现，而且在一些情况下高效率的库（e.g., numpy）能够避免我们自己手动低效率的实现（e.g., for 循环）**

所以说，**库**（Library）是一系列预先编写好的代码集合，供开发者在编程中调用，大大减少重复工作量。我们自己写一个字符串处理函数，包装好之后调用，也是库。

库的概念很宽泛。程序员第一次输出hello world用的printf就来自C语言标准库；各种SDK都是库；从npm、Maven、Nuget下载的包都是库；pip中绝大部分也是库

**对于库而言，最重要的一点就是程序的主控制流在我们的程序中；或者说是我们的程序在调用库**



### 2. 框架 FrameWork

**框架**（Framework）可以理解成是库的进阶版。很多人会把框架和普通库的区别仅仅理解为规模和复杂度，其实不然。

**框架最大的特点就是会接管程序的主控制流；或者说框架在调用我们的代码**

我们只需要编写业务的逻辑代码，具体的工作执行（例如后面要说的GUI框架Qt中的图形显示、信号处理/槽函数调用；未来会出的网页程序框架Flask的服务器运行等等）都是由框架执行的。

框架是一个半成品的应用，和库的最大区别是框架直接左右了应用怎么来写。

---





## 1. What is Qt

在前面介绍完库与框架的区别之后，我们就可以正式的开始介绍Qt了。

简单的来说，**Qt是一个C++的应用程序开发框架**；更具体的介绍，则看看下面维基百科和百度百科的介绍

> **From Wikipedia**：
>
> Qt（/ˈkjuːt/，发音同“cute”）是一个跨平台的C++应用程序开发框架。广泛用于开发GUI程序，这种情况下又被称为部件工具箱。也可用于开发非GUI程序，例如控制台工具和服务器。Qt被用于OPIE、Skype、VLC media player、Adobe Photoshop Elements、VirtualBox与Mathematica以及被Autodesk 、欧洲空间局、梦工厂、Google、HP、KDE、卢卡斯影业、西门子公司、沃尔沃集团, 华特迪士尼动画制作公司、三星集团、飞利浦、Panasonic所使用。
>
> 它是Digia公司的产品。Qt使用标准的C++和特殊的代码生成扩展（称为元对象编译器（Meta Object Compiler, moc））以及一些宏。通过语言绑定，其他的编程语言也可以使用Qt。
>
> Qt是自由且开放源代码的软件，在GNU宽通用公共许可证（LGPL）条款下发布。所有版本都支持广泛的编译器，包括GCC的C++编译器和Visual Studio。
>
> ![Qt的维基百科介绍](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211217102149620.png)

> **From Baidubaike**：
>
> Qt 是一个1991年由Qt Company开发的跨平台C++图形用户界面应用程序开发框架。它既可以开发GUI程序，也可用于开发非GUI程序，比如控制台工具和服务器。Qt是面向对象的框架，使用特殊的代码生成扩展（称为元对象编译器(Meta Object Compiler, moc)）以及一些宏，Qt很容易扩展，并且允许真正地组件编程。
> 2008年，Qt Company科技被诺基亚公司收购，Qt也因此成为诺基亚旗下的编程语言工具。2012年，Qt被Digia收购。
> 2014年4月，跨平台集成开发环境Qt Creator 3.1.0正式发布，实现了对于iOS的完全支持，新增WinRT、Beautifier等插件，废弃了无Python接口的GDB调试支持，集成了基于Clang的C/C++代码模块，并对Android支持做出了调整，至此实现了全面支持iOS、Android、WP,它提供给应用程序开发者建立艺术级的图形用户界面所需的所有功能。基本上，Qt 同 X Window 上的 Motif，Openwin，GTK 等图形界面库和 Windows 平台上的 MFC，OWL，VCL，ATL 是同类型的东西。
>
> ![Qt的百度百科介绍](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211217102306283.png)



关于Qt，我们需要知道的重要的点如下：

1. **Qt是一个C++的框架**。因此使用C++进行Qt开发是很舒服的，而Python解释器中Python基金会官方提供的解释器本身是由C语言写的，因此直接动态加载C++的库（挖个坑，后面会将Python调用C++的库）。因此，我们能够早Python中调用Qt
2. **Qt是一个应用程序框架，而非仅仅是一个带图形界面的应用程序框架（GUI应用程序框架）**。因此在Qt看来，GUI只是Qt的一个部分，而不是全部。对于一个成熟的应用来说，涉及到的内容用：进程/线程/协程管理、网络编程、蓝牙/影响等设备管理、GUI图形显示等等功能。因此Qt作为一个应用程序框架，对于这些内容都是可以胜任的。因此Qt不仅仅是GUI的库，而是一个应用程序框架。更加具体的来说，Qt是因为他的GUI出名，但不仅仅只有GUI。

针对第一个点，我们稍后就会讲解什么是PyQt，什么是PySide。而针对第二个点，我们明白了未来要怎么样学习Qt，即分模块学习。因此在未来的有的文章会关注PyQt的GUI部分，有的部分会关注PyQt的多线程管理，而有的部分会关注PyQt的网络编程。





## 2. What is Python/Language Binding 

在进一步介绍之前，我们需要知道什么是Python Binding，什么是Language Binding。

从概念上来说，Language Binding是Python Binding的父类/超集。而具体来说，Language Binding指的是某种语言中可以让该语言调用其他语言或者一些系统服务的该语言的代码，这些代码成为胶水代码（glue code）。

关于Language Binding，百度百科上没有介绍，因此只有维基百科的英文介绍

> **From Wikipedia**:
>
> In programming and software design, binding is an application programming interface (API) that provides glue code specifically made to allow a programming language to use a foreign library or operating system service (one that is not native to that language).
>
> ![Introduction of Language Binding in Wikipedia](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211217104203893.png)



因此对于Python来说，Python Binding即指能够让Python调用由其他语言开发的库或者是系统提供的功能（系统调用）的Python代码。

事实上，得益于Python官方的CPython解释器是由C语言开发的，因此Python具有的直接调用C/C++库的这个能力，Python中的不少库都是C++的库，然后提供了Python Binding，从而使得在Python中能够调用这些库。最著名的一个例子就是OpenCV了。

我们可以用mlocate查一下

```shell
(base) jack@jack-Alienware-m15-R3:~$ locate *opencv*.so
```

我们其实就能够看到，Python的opencv库里没有opencv的so，所有的共享库文件都放在了C++源码编译时候安装的位置了，而Python的OpenCV库里本身只是提供了一些OpenCV 的Python Binding代码，真正的功能还得看预编译好的so文件。

![查询结果](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211217110010408.png)





## 3. What is PyQt & PySide

我们前面说道Qt是一个C++的应用程序框架，因此PyQt和PySide其实是两种不同的Qt的Python Binding。两者之间的区别在很多时候都只是`from xxx.yyy import zzz`中的`xxx`不同，毕竟底层调用的Qt的so都是一样的，Binding 本身只是为Qt编译成so之后里面的API提供Python的类或者函数的签名。

因此其实我们学习的时候两者任意选一个就行了。但是PyQt由于推出的早，因此资料比较全，而PySide则虽然推出的晚，但是Qt公司的产品，因此两者在学习的时候我们推荐学习PyQt，因为资料会很全。未来PySide也期待他会变得更好。

注意在未来我们的系列教程中更加关注Qt的Python Binding，而非具体的PyQt、PySide之争。两者在API上的区别正如上面所说，往往是非常小的。

关于PyQt与PySide其实在技术上的介绍以及结束了，但是我们不禁会想问为什么一个Qt会有PyQt与PySide两套Binding？

为了解决这个问题，~~我们就要介绍一下PyQt与PySide的历史了~~（吃瓜）

注：以下关于PyQt、PySide的历史参考知乎问题：[如何评价Qt官方推出的Qt for Python(PySide)？](https://www.zhihu.com/question/306793447/answer/560109210)中[饶锦蒙的回答](https://www.zhihu.com/people/phantask93)



### 1. PyQt / PyQt5

PyQt本身Riverbank Computing这家英国公司在2005年以前的时候开发的，那个时候Python刚刚开始流行。

前面介绍过，Qt身GPL V3协议下的一个项目，因此当使用PyQt程序就被要求需要强制开源，因此PyQt对商业软件的开发就非常不友好。而Qt本身所属的公司在零几年被诺基亚收购之后就成了诺基亚的一个项目。那个时候诺基亚就与Riverbank Computing展开了多轮磋商，希望PyQt能够支持对商业更加友好的LGPL协议，即开源项目在商业软件中被使用了之后，商业软件不需要强制进行开源。因此LGPL对商业更加友好。

然而PyQt所属的母公司Riverbank Computing十动然拒，最后诺基亚就和Riverbank Computing谈崩了。

因此从PyQt的角度来看，PyQt是最早支持Qt的Python Binding的库，在发展上也是非常顺利的。

因此针对不同的Qt版本（Qt4，Qt5），PyQt就有不同的版本支持PyQt4、PyQt5



### 2. PySide / Pyside2

前面我们说到，诺基亚和Riverbank Computing谈崩之后，诺基亚手握Qt的版权，又不忍放过Python中Qt的商业运用，因此2009年一气之下最终决定自己从头单干，重新开发一套Qt的Python Binding，即PySide。

然而诺基亚后面的故事我们都很熟悉了，2009年PySide发布了同时支持GPL和LGPL的PySide之后，就芜湖了。因此PySide项目就一度搁浅。

直到2012年，诺基亚把包括PySide在内的所有有关Qt的全部内容卖给了Digia公司。Digia公司在收购了Qt之后，决定大力支持Qt的全方位发展，因此在2014年9月将Qt分拆成一家独立全资子公司The Qt Company。

虽说加大了投入，该补的课也绕不开，C++的Qt在Digia收购Qt之后就推出了Qt5版本，PySide对Qt5提供支持的计划从2014年才开始筹备，也就是2015年上马的Qt for Python项目，该项目开发的模块命名为PySide2，以表示与老一代PySide的不同。然而反观PyQt，在Qt5出来不到半年时间就支持了PyQt5。

PySide2在Qt公司和Qt社区开发者的共同努力下，也只在2018年6月才正式发布了第一个版本，稳定性还是个问题。不过从0到1是最难的，后面就容易了。到今天为止，PySide2已经日趋完善，又是亲生的，还有LGPL的加持，今后PySide2有足够的理由变得越来越好。







## 4. Which Version of Qt to Use？

其实经过前面的介绍，我们也应该知道了Qt5的版本在2012年推出的，Qt4就更早了。虽然最近也推出了Qt6，但是目前市场、社区里的还是Qt5居多，因此我们直接使用Qt5的Python Binding即可。即PyQt5与PySide2

正如前面所强调的，在未来的文章中我们不会过分强调PyQt5与PySide2，只有在两者API差异比较大的时候我们才会特地的注明到底每一个是怎么样的。
