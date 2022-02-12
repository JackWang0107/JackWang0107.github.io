---
title: hexo deploy失败解决方案
date: 2022-01-27 11:58:41
img: https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220127120521462.png
summary: '本文主要介绍了hexo deploy失败的解决方案'
categories:
	- 杂项
tags:
    - Hexo
    - git
---

> 本文主要介绍了hexo deploy失败的解决方案

![hexo deploy失败](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220127120521462.png)







# hexo deploy失败解决方案

我的博客是使用hexo作为框架的，而具体的网站的访问则是将静态网页部署在github上，以github page的的形式进行展示。因此，文章的发布其实就是将写好的文章同步到GitHub的仓库上去。然而在使用git进行仓库同步的时候却会发生很多的问题，因此本文记录了所有我在hexo deploy的过程遇到的的问题以及相应的解决方案。





## 1. 无法访问 '仓库地址'：GnuTLS recv error (-110): The TLS connection was non-properly terminated.

这个报错具体的截图如下

<img src="https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220127120521462.png" alt="报错截图" style="zoom: 67%;" />

这个问题发生的原因是由于代理的问题。因为最近西安解封了，我放寒假回家了。学校里是通过openwrt做的旁路由。而家里面则没有树莓派做旁路由。因此代理出了问题。

问题的解决也非常简单。直接按照下面的命令即可

```shell
sudo apt-get install gnutls-bin
git config --global http.sslVerify false
git config --global http.postBuffer 1048576000
```

