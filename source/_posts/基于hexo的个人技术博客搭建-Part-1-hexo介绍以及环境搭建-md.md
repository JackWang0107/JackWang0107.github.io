---
title: 基于Hexo的个人技术博客搭建 —— Part 1 Hexo介绍以及环境搭建
date: 2021-11-13 02:41:28
img: https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211113024418805.png
summary: '本文主要介绍什么是Hexo，为什么我们要使用Hexo搭建我们的博客以及Hexo环境搭建'
categories:
	- Hexo博客搭建
tags:
    - Hexo
---

> 本文主要介绍什么是Hexo，为什么我们要使用Hexo搭建我们的博客以及Hexo环境搭建

![Hexo官网](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211113024418805.png)

<!-- more-->



# 基于Hexo的个人技术博客搭建-Part 1 Hexo介绍以及环境搭建.md

在前一章概述中，我们讲解了为什么我认为一个Geek需要一个个人技术博客，并且简单的展示了最终效果，在接下来的章节里，我将一步步的介绍如何打造这样的效果。



## 1. What is Hexo

> 等等，什么是`Hexo`？

在开始学习前，我们需要**首先**知道我们将要学的东西是什么以及学习它能带来的好处（为什么要学他），这样我们学起来会轻松很多。因为这样做我们首先对需要学的东西搭建了一个大的框架，后续的学习都是在填充它，不断丰满这个框架，并且也有了充足的动力去学习。



- 正如`Hexo`官网上所说：**Hexo是一个快速、简洁且高效的博客框架**。（~~虽然这个官网充满了不少广告~~）

所谓框架，即指已经为我们构建了基本的博客工具和博客结构（框架），我们后续只需要在这个框架上不断的填充（发布自己的文章）、修改（修改`Hexo`的代码）。因为`Hexo`开源，因此我们实际上可以针对`Hexo`进行任意程度的自定义修改，只有你想不到，没有你改不了。非常庆幸的是，`Hexo`的作者是台湾人，因此他的官方文档的中文支持是非常好的，这也为我们使用`Hexo`提供了便利。

1. `Hexo`是基于`Node.js`开发的应用（因此我们稍后在安装的时候会安装`Node.js`的环境）。借助于`Node.js`，`Hexo`可以快速的渲染出漂亮的文章

> **什么是`Node.js`**
>
> 说清这个问题，要说的可不少。接触过网络的人都应该知道，前端页面是由`HTML`、`CSS`、`JavaScript`三大技术/三件套支持的，其中`HTML`负责定义网页改在哪个地方显示什么内容，`CSS`定义哪个地方的哪个内容该以什么样的方式显示，`JavaScript`则定义当你与这个内容交互的时候会有什么样的效果。
>
> 举个简单例子，在[百度](https://www.baidu.com/)的首页，
>
> 1. 为什么百度的Logo会显示在中间，而备案等网站信息显示在底部？--> `HTML`
> 2. 为什么百度的背景是白色的，而不是黑色？  --> `CSS`
> 3. 为什么鼠标悬停在左上角的更多的时候会弹出来浮窗？  --> `JavaScript`
>
> ![百度的首页](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211113031023828.png)
>
> 正是因为有了这三件套，我们的网页变得多姿多彩了起来，有有价值的信息(`HTML`)、漂亮的页面(`CSS`)和友好的交互(`JavaScript`)。
>
> 然而，渐渐地，**本来地位平等的三件套逐渐开始出现区别**。`HTML`和`CSS`都是静态的，在编写结束之后用户能看到的内容就已经固定（关于模板稍后再说），当用户访问这个网页，看到的就是其中的内容；然而`JavaScript`却由于需要处理的用户的操作不同而做不同的处理，例如在百度的页面中，如果我不是悬浮而是右击怎么办？如果我不是悬浮而是掠过怎么办？正是由于需要处理诸多用户导致的多种多样的Event，`JavaScript`也具有了if-else等编程语言常见的语句。**渐渐地，随着技术的进步，JavaScript需要处理的问题越来越多，在其能力变强的同时，JavaScript也变得越来越像一门编程语言**。
>
> 然而，这个时候的`JavaScript`还并不能独当一面。因为在最早的网页中，`JavaScript`、`HTML`、`CSS`是绑定的三件套，当用户访问某个网页，服务器会将该网页的三件套发送给用户，然后由**用户的浏览器解析、渲染、执行HTML、CSS、JavaScript三件套**，从而显示网页。因此这个时候的`JavaScript`是无法脱离浏览器的。我们从另一个角度考虑，由于JavaScript是由浏览器解析执行的，因此其可以看做是一门解释性语言，在执行`JavaScript`的时候，CPU执行的机器码来自于其`解释器`——浏览器。
>
> 后来随着，`JavaScript`的功能越来越强大，简单的在网页中进行交互已经完全发挥其能力了。因此就出现了诸多项目，这些项目独立于传统浏览器，基于浏览器的`JavaScript`解析器内核（学名：引擎）亦或是自己编写了`JavaScript`的解析器来执行`JavaScript`。至此，`JavaScript`已经能够独立于浏览器被单独执行了，而非必须在浏览器中以网页的形式打开。
>
> 在前面介绍了那么多之后，终于，到了我们的主角，`Node.js`。大名鼎鼎的`Node.js`其实就是基于`Chrome V8` 引擎的 `JavaScript` 运行时环境。简单的来说，它能够利用`Chrome V8`引擎来解析、执行`JavaScript`。`Node.js`可以粗暴的理解成`JavaScript`的解释器。基于此，`JavaScript`在很多方面都很像`Python`，包括包管理器、解释器等等



2. `Hexo`利用`Markdown`来作为源文章，其内置`Markdown`的渲染引擎，我们只需要书写Markdown，而后通过`Hexo`就能够生成`HTML`等前端文件，非常方便

> 正如下面这张图，这篇文章也是我用Markdown写出来的，在后续部分除了网站搭建以外，还会讲讲如何搭建写作环境，以实现畅快写作
>
> ![我的Markdown写作环境：Typora+腾讯云图床+Dracula主题](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211113110048831.png)



3. Hexo搭建出的博客网站是一个静态网站，这就意味着单纯`Hexo`本身只能提供博客的显示、有限的交互，如果需要登录、发表评论这类需要后端程序支持的运用，就需要第三方插件了。如果非常需要这些功能，要么使用`WordPress`，要么折腾`Hexo`的插件



到这里，你应该明白了`Hexo`是一个基于`Node.js`的程序，他帮助我们渲染文章、管理文章，通过这个`Hexo`我们可以快速、低成本的部署自己的博客





## 2. Why is Hexo

> Ok，我知道了什么`Hexo`，可是为什么要用它它？

事实上，选取`Hexo`作为我们的博客网站框架来帮助我们搭建技术博客有很多好处：

1. `Hexo`框架的学习成本非常低，学起来非常快速，命令行几条语句就能够学会。在整个`博客搭建过程`中，学习`Hexo`可能只占很少的时间，主要时间在于挑选一个好看的主题并自己修改、添加自己的个人信息
2. `Hexo`搭建的博客非常易于管理。`Hexo`管理的博客本质上是一个文件夹。因为Hexo搭建出的只是静态博客网站，因此不需要后端程序，所以网站所有的资源（`HTML`、`CSS`、`JavaScript`）都放在一个文件夹里就行。如果中间配置出问题了，那么直接删掉整个文件夹就行。
3. `Hexo`搭建的博客部署、迁移非常方便。同样是由于Hexo管理的博客网站是一个文件夹，因此我们可利用Git`来保存`、同步你的博客。此外，由于`Github`和`Gitee`提供静态网页的`Page`服务，所以我们其实可以利用`Github`和`Gitee`等`Git`托管网站来托管我们的网站。防止网络攻击这些都由他们帮我们做好了。
4. `Hexo`搭建的博客成本非常低。正是因为我们利用`Github`和`Gitee`来托管我们的网站，因此我们只需要花钱买域名和图床即可，买公网服务器什么的全部省掉了。
5. `Hexo`具有非常多美观的主题。通过这些主题，我们只需要进行配置和有限的修改就能够做出来非常美观的博客网站。

> `Hexo`的官网上提供了非常多的主题（[点击查看](https://hexo.io/themes/)），截止我写这篇文章的时候一共有348个官方收录的主题。除此以外还有非常多的未被收录的主题，强推我现在正在用的由`闪烁之狐`制作的`Matery`主题，后面也会讲解如何配置`Matery`主题，这是Matery主题的[展示网站](http://blinkfox.com/)
>
> ![Hexo官网收录的主题](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211113112326777.png)



6. ……（暂时只想到这几个点）





## 3. Hexo环境搭建

> 开发第一步：搭建环境

注意，由于我本人长期使用`Debian/Ubuntu`来做开发，因此以下教程都将是基于`Ubuntu`编写的教程。如果你是其他`Linux`发行版用户，适度修改即可；如果你是`Windows`用户，那么你还需要配置不少东西，知乎、简书上你还得查查。

下面我们就将一步步搭建Hexo环境出来



### 1. 安装Node.js环境

前面说道，`Hexo`是基于`Node.js`开发的程序，因此其运行就需要`Node.js`，所以我们第一步就是安装`Node.js`

> 注意：官网上说明，`Hexo`需要的`Node.js`的版本不低于**10.13**，强烈建议`Node.js`**12.0**以上的版本

我们首先用`apt`查一下Ubuntu的仓库里的nodejs的版本

```bash
(base) jack@jack-Alienware-m15-R3:~$ sudo apt search nodejs
```

![apt搜索nodejs的结果](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211113124733292.png)

不难看出来，`apt`仓库里的`nodejs`的版本过低，因此我们需要自己从`nodejs`官网上下载新版本的`nodejs`，[官网传送门](https://nodejs.org/zh-cn/)

![Node.js的官网](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211113124918179.png)

在我写这篇文章的时候最新版是16.13，由于遵循Linux的传统，双数版本是LTS（Long Time Supported）版本，而单数版本都是尝鲜（Beta）版本，所以选择**稳定版即可**



下载之后解压会得到一个文件夹，这个文件夹里`bin`目录放的就是官方替我们已经编译好了的二进制可执行文件，可以看到除了node，别的都是软连接

```bash
(base) jack@jack-Alienware-m15-R3:~/桌面/node-v16.13.0-linux-x64$ tree -L 2
```

> 注意，我这里是已经安装过了`Node.js`，正常情况下是没有`cnpm`的，稍后会安装`cnpm`

![解压之后的文件夹](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211113125500195.png)

由于后续需要在命令行中调用`Node.js`，因此还要把这个`bin`文件的路径加入到`Shell`搜索可执行文件路径的`PATH`环境变量中，这里图方便直接在.`bashrc`中添加了

```bash
(base) jack@jack-Alienware-m15-R3:~/桌面/node-v16.13.0-linux-x64$ vim ~/.bashrc  
```

![添加了Node.js的PATH](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211113125903583.png)

这样未来在命令行就可以正常的调用`node.js`和附带的`npm`了，测试一波

```bash
(base) jack@jack-Alienware-m15-R3:~/桌面/node-v16.13.0-linux-x64$ node 
```

![测试Node.js](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211113130048060.png)

OK，装完了`Node.js`，下一步





### 2. 安装cnpm

前面讲到，Node.js中利用了Chrome V8引擎从而实现了JavaScript的运行时环境，从而做到了类似于解释器的效果。做过`Python`的人应该都会知道`pip`和`conda`，他们都是`Python`的包管理器（当然conda还可以管理环境）。通过`pip install xxx`或者`conda install xxx`就能安装Python的第三方库。

`Node.js`也提供了类似的包管理工具，即`npm（Node.js Package Management）`，通过npm我们就能够安装`Node.js`的第三方库。

但是这些包管理工具的共性就是下载的这些库都是在包管理工具官网上维护的包，我们使用`xxx install xxx`的时候，会去`xxx`的包管理网站上搜索`xxx`包，然后下载。问题的关键就出在了下载这一步，由于这些包管理网站服务器都在国外，因此国内下载就会很慢。为此，我们通过换源来解决，即指定去国内的包管理网站上下载。在`pip`中我们可以指定`-i`参数，从而指定去清华源或者中科大源下载`Python`第三方包。而`npm`我们同样也可以指定`--registry`参数，从而指定国内的源（一般都是淘宝源）。

但是每次都使用`--registry`参数有点蠢，我们直接使用淘宝做好的`cnpm`包管理工具就行。`cnpm`和`npm`在作用上是一样的，不过对国内用户做了很不错的优化。

我们首先通过npm指定`--registry`参数来安装`cnpm`

```bash
(base) jack@jack-Alienware-m15-R3:~/桌面/node-v16.13.0-linux-x64$ npm install -g cnpm --registry="https://registry.npm.taobao.org"
```

稍等片刻即可，警告是已经安装的别的包报的错，都后面没啥影响，因为我已经安装过了cnpm，所以下面的截图和你的可能不一样

![安装cnpm](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211113131609169.png)

安装之后检查下安装是否成功，当然由于路径等环境不一样，输出结果也不太一样，但是能够正常输出就表示安装对了

```bash
(base) jack@jack-Alienware-m15-R3:~/桌面/node-v16.13.0-linux-x64$ cnpm --version
```

![检查cnpm是否安装成功](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211113131849887.png)

OK，下一步安装Git



### 3. 安装git

由于`Hexo`内置了博客的同步、部署等功能，因此`Hexo`依赖于`git`，所以我们首先需要安装`git`

安装git就简单了，直接`apt`安装即可，同样由于我已经安装过了，所以输出会不一样

```bash
(base) jack@jack-Alienware-m15-R3:~/桌面/node-v16.13.0-linux-x64$ sudo apt install git
```

![apt安装git](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211113132146054.png)

同样，查看下`git`的版本确认安装成功

```bash
(base) jack@jack-Alienware-m15-R3:~/桌面/node-v16.13.0-linux-x64$ git --version
```

![确认git安装成功](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211113132257442.png)





### 4. 安装Hexo

在前面所有必备的环境安装成功后，下面将安装`Hexo`

前面由于已经安装了`cnpm`，因此我们直接使用`cnpm`来安装`Hexo`

```bash
(base) jack@jack-Alienware-m15-R3:~/桌面/node-v16.13.0-linux-x64$ cnpm install -g hexo-cli
```

稍等片刻就安装好了

![安装hexo](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211113132546043.png)

为了后续我们能够直接在命令行调用`hexo`的命令，我们把`hexo`的可执行文件添加到`PATH`中去

注意，`npm`和`cnpm`的工作原理都是把下载的包放到`node.js`所在的根目录的`lib`中，和`pip`是异曲同工之秒，我们打开之前node.js在的文件夹就能看到

![hexo的安装位置](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211113133034708.png)

同样用`vim`编辑`.bashrc`

![添加hexo可执行文件](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211113133331507.png)

最后命令行查看下hexo的版本确认成功

```bash
(base) jack@jack-Alienware-m15-R3:~/桌面/node-v16.13.0-linux-x64$ hexo --version
```

![确认hexo安装成功](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211113133455333.png)

至此，我们已经成功搭建了Hexo的环境。





本章结束~

本章我们首先讲解了什么是`hexo`，然后讲解了为什么我们要使用`hexo`，最后讲解了如何安装`hexo`。

下一节预告：`Hexo使用以及配置`

码字不易，4000字，欢迎打赏\~，一起为开源事业做贡献\~

