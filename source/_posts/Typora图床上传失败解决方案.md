---
title: Typora图床上传失败解决方案
date: 2021-12-29 23:33:27
img: https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211229233841511.png
summary: '本文主要介绍了Typora中使用图床上传图片失败的解决方案'
categories:
  - 杂项
tags:
  - Typora
  - PicGo
  - Ubuntu
---

> 本文主要介绍了Typora中使用图床上传图片失败的解决方案

![Typora 图床上传失败](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211229233841511.png)



# Typora图床上传失败解决方案

我的博客的开发环境是Typora+Hexo+Matery主题，此外为了方便博客的部署，所有的图片使用的都是图床。然而在Typora中使用PicGO图床并不是一帆风顺的，因此本文主要介绍了Typora中PicGO图床出错的一些解决方法





# 一、图片上传403错误

这个错误在我这里发生的现象是这样的，在之前的几个月的使用都是完全正常的，然而某次关机、切换成Windows之后再切换为Ubuntu，Typora的图床就无法使用了，上传图片会报错403。具体的如下图

![上传图片403错误](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211229233841511.png)



这个问题其实是由于时差的问题，因为我是windows和ubuntu双系统，因此开完Ubuntu之后开Windows，时间就会差八个小时，然后在Windows中把时间改对了，在Ubuntu中又会差八个小时。因此这个问题就发生在Ubuntu的事件不对。解决办法很简单，把时间改回来就行了。

在设置中搜索`时间`，然后修改即可

![修改日期和时间](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211229154229847.png)
