---
title: Ubuntu下QQ和微信解决方法
date: 2021-12-28 17:12:42
img: https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211229230820817.png
summary: 'QQ和微信作为国民级的应用，无论是聊天还是传输文件都非常的方便。本文介绍了如何在类似Ubuntu这类的Linux平台上使用微信和QQ。'
categories:
  - 杂项
tags:
  - QQ
  - 微信
  - Wechat
  - Linux
  - Ubuntu
  - Virtualbox
---

> QQ和微信作为国民级的应用，无论是聊天还是传输文件都非常的方便。本文介绍了如何在类似Ubuntu这类的Linux平台上使用微信和QQ。

![最终效果图](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211229230820817.png)



# Ubuntu/Linux下QQ和微信解决方法

作为一个开发者，通常在进行开发的时候往往会选择使用Ubuntu、CentOS等Linux发行版作为开发的平台。然而中国的开发者难以避开的就是QQ、微信等等应用。除了聊天以外，QQ、微信的一个重要的功能就是在多端设备之间传输文件（电脑-->手机、平板-->电脑）。然而QQ和微信都仅仅支持MacOS和Windows这两大平台，因此本文就介绍了在Ubuntu/Linux平台上使用QQ和微信的方法。





## 方案一：虚拟机

微信和QQ都支持windows平台，因此使用虚拟机的话就是我们在ubuntu上安装一个windwos的虚拟机，然后在虚拟机中安装qq和微信。

使用虚拟机的原因在于使用docker和wine等方式使用QQ和微信都只是权宜之计，只能够解决某一个版本的QQ和微信的使用。此外一个API还会可能有问题，因此如果想到比较完美的使用体验就还是需要虚拟机。



### 1. 检查内存大小

使用虚拟机的一个大问题就是需要电脑有足够大的内存，这样在运行windows虚拟机的时候就不会很卡。

由于我的机器的性能还是不错的，内存有32个G，因此对我来说最合适的解决方案就是安装一个Windows的虚拟机。

在后续的步骤前首先需要查看自己的机器的内存和交换空间的大小，使用下面的命令，就可以看到。

```shell
watch free -h
```

![我的机器的内存信息](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211228173025146.png)







### 2. 安装Virtualbox虚拟机

目前运行比较流畅的虚拟机就是Oracle的VirtualBox和VMware公司的VMware两个软件。由于Virtualbox免费并且多平台适用的的特点（VMware收费），本文使用VirtualBox来作为虚拟机的软件。

安装VirtualBox虚拟机使用下面的命令就行，由于我已经安装过了，因此不会有任何反应

```shell
sudo apt-get install virtualbox
```

![安装VirtualBox](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211228173626767.png)



安装完了之后我们就可以在命令行中通过virtualbox命令或者在应用程序图标里点击运行VirtualBox，这里继续使用命令行

```shell
virtualbox
```

然后我们就能够看到VirtualBox开始运行。

![image-20211228173825872](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211228173825872.png)





### 3. 下载windows镜像

windows的镜像文件我们可以直接取[windows的官网](https://www.microsoft.com/zh-cn/software-download/windows10ISO)上下载：https://www.microsoft.com/zh-cn/software-download/windows10ISO。考虑到浏览器本身就会占用很多的资源，影响下载速度，因此使用wget从命令行加速下载。

我们选择完windows版本之后会看到官网给出的两个下载链接，选择自己需要的版本然后复制链接即可

![复制下载链接](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211228174324033.png)



接下来在命令行中wget下载即可，注意复制来的网页链接中有`(`之类的shell的语法字符，因此需要用`""`把网址包围起来禁止转义

```shell
wget -c "你的下载地址"
```

![下载Windows的镜像文件](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211228174604765.png)

然后耐心等待即可。下载完大小5.5G左右

![下载完成的镜像文件](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211228190833912.png)

最后我们给这个文件改个名字，把后缀改成iso，因为稍后VirtualBox是根据后缀名来识别文件的

![修改后的文件](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211228192040466.png)



### 4. 创建虚拟机

点击右上角的`新建`，然后选择虚拟机文件的存储位置和对应的系统，完成后点下一步。

![虚拟机的基本设置](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211228190934329.png)

接下来给虚拟机分配内存，这个就看个人了，我的内存比较大，所以就给虚拟机分配12G内存

![内存分配](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211228191153552.png)

接下来给虚拟机的文件分配大小，默认即可

![为虚拟机分配磁盘1](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211228191420175.png)

![为虚拟机分配磁盘2](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211228191450549.png)

![为虚拟机分配磁盘2](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211228191511960.png)

![为虚拟机分配磁盘3](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211228191536355.png)





### 5. 安装windows虚拟机

上面我们只是用VirtualBox创建好了所有需要的环境，接下来我们就是要把windows安装进去

![点击设置](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211228191700557.png)

然后在存储中点击盘片，选择windows的映像文件

![选择windows的映像文件](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211228191834378.png)

然后点击启动即可

![点击启动](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211228192121402.png)

然后按照指引进行安装即可

![安装windows](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211228192211784.png)

安装完之后的分辨率有点问题，因此我们还需要设置一下分辨率

![有问题的分辨率](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211228200126547.png)

点击`设备`-->`安装增强功能`，然后按照提示安装即可

![安装增强功能](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211228200320807.png)

安装完成之后在C盘外有一个程序，点击运行即可

![image-20211228201049520](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211228201049520.png)

安装完之后重启，就可以在视图中进行设置，选择自动调整窗口大小即可

![全屏显示](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211228202752076.png)

最后在设置里设置双向剪贴板

![设置共享剪切板](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211228203017531.png)



### 6. 安装微信和QQ

最后安装微信和QQ即可，最终效果图如下

![](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211229230820817.png)

此外，我们安装完的Windows实际上是未激活的的Windows，只能够使用一些有限的功能。为此，要么去微软官方买一个激活码，或者去淘宝买一个激活码即可。
