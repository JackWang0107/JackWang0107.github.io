---
title: hexo gen文章渲染不全问题
date: 2022-01-27 18:09:26
img: https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220127181008020.png
summary: '本文主要介绍了hexo deploy失败的解决方案'
categories:
	- 杂项
tags:
    - Hexo
    - git
---

> 本文主要介绍了hexo deploy失败的解决方案

![hexo 文章渲染不全](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220127181008020.png)







# hexo gen文章渲染不全问题

最近在因为参加的一些项目是有版权要求的，因此就需要加上密码保护版权。之前在我写的`hexo博客搭建`系列文章中写了如何给文章设置密码。

因为设置密码的时候需要在博客的Markdown文件里写上SHA256加密后的密码，因此就需要一些网上的工具来帮助进行加密。我在那篇文章里写了一些提供了SHA256位加密工具的网站，可是当我自己查看我的网页的时候，却发现网页渲染不全。



## 1. 问题现象

下面是渲染不全的网页

![渲染不全的网页](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220127181008020.png)

图中红框框出来的部分都是没有渲染出来的内容。

而一个正常的页面应该是这样的

![渲染完全的页面](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220127185215558.png)

可以看到，首先是文章渲染不全，然后顶部的绿色的选项卡没了，右上方的目录没了，右下角的返回顶部和目录按钮都没了。此外我开的樱花特效也没了。



## 2. 解决办法

在经过一番搜索之后，都没有在网上找到类似的问题，于是决定自己分析一下问题在哪。因为hexo框架发布文章的时候是根据Markdown中的标记转换为html，而css、JavaScript等文件都是由主题提前定义好的。考虑到别的文章都没有问题，只有这个文章出了这个问题，因此这个问题大概率不是由于主题的问题，而是发生在我这篇文章上。

更具体的来说，我怀疑问题出来了代码的转换上。于是我扒了一下网页的源码

看到code标签之后就没有了内容了

![源码code之后就没了](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220127185935423.png)



可是我看到为什么会有一个title标签，于是我点开看了一下这个标签里的内容。不看不知道，一看吓一跳，title标签里的内容竟然就是没有显示完的文章

<img src="https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220127190150483.png" alt="title标签吓死人"  />

再看看Markdown源文件，翻到对应的部分，这下就看到了凶手！

<img src="https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220127190329842.png" alt="凶手竟然是代码块里的内容！" style="zoom:67%;" />

这下真相大白了。我在代码块里无意写成了html标签的形式的内容，而hexo在生成html的时候并不会对代码块的字符做转义，相反的，只是把需要转换（例如mathjax）和添加（例如css）的部分添加进去。

而title这个标签在hexo的主题（我用的是matery主题）下并没有进行定义，因此并不会显示出来title内部的文字，因此就出现了上面渲染错误的效果。

因此，解决方法就很简单，要么自己手动给code fence里的内容转义，要么换一种表达。我感觉转义有点丑陋，所以换称中文表达即可

![更改之后的代码块](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220127190850347.png)



最后我们修改之后的效果如下，everything goes fine :tada:

![完美解决:)](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220127191001549.png)

