---
title: 基于hexo的个人技术博客搭建 —— Part 5 Hexo本地编写技巧
date: 2021-12-17 12:46:44
img: https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211217131946247.png
summary: '前面我们成功的在本地搭建出了Hexo博客网站的环境以及完成了Typora编辑器的设置。本文将讲解如何使用Hexo在本地创建文章以及设置为草稿、密码阅读等技巧'
categories:
	- Hexo博客搭建
tags:
    - Hexo
---

> 前面我们成功的在本地搭建出了Hexo博客网站的环境以及完成了Typora编辑器的设置。本文将讲解如何使用Hexo在本地创建文章以及设置为草稿、密码阅读等技巧

![Hexo 博客编写技巧](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211217131946247.png)



# 基于Hexo的个人技术博客搭建 ——  Hexo本地编写技巧

前面我们已经设置好了本地的Hexo博客环境，以及本地的编辑器环境。本文将讲解如何通过Hexo来创建新的博客文章，并且设置文章为草稿（不可见）以及加密（密码阅读等功能）。

前面我们讲过Hexo中博客其实是Markdown文件。针对Markdown，Hexo可以根据我们预先设置好的模板来渲染、生成对应的HTML文件。那么我们要开始编写博客，其实就是开始编写Markdown文档。

然而在我们使用Typora开始编写前，自然就需要创建出来这个Markdown文件。然后在创建的Markdown文件的基础上，我们为这个Markdown文件设置文章的标题、日期等等元信息（mate-information，类比于HTML中的header）。

进一步，有了文章我们有的时候不希望一些文章被人看到，比如写了一半的草稿；也有一些文章，我们希望有访问权限得人（有密码的人）才能阅读，例如你的简历。

因此针对这些功能，Hexo都进行了设置。



## 1. Hexo的博客原理

### 1. Markdown的Front-Formatter

首先针对文章的标题、日期、是否加密等信息都是在Markdown开头的Header区设定，由于设定之后会影响到文章的外观（在稍后会看到），因此这个区域成为Front-Formatter。以我之前写好的一篇文章为例，我们能够看到文章的标题、创建日期、封面图像、所属目录、分类等等信息都在Header区设定。

其实包括后面的密码验证文章也在这里设置。

![Markdown的Header](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211217130309720.png)



### 2. Hexo中的草稿与正式发布文章

由于Hexo是通过Markdown文件来生成文章，因此我们写的草稿和正式要发布的文章其实对于Hexo来说是完全等价的。要区分两者。Hexo实际上是通过文件所处的文件夹来进行的。我们所有的要发布的文章都在`source/_post`文件夹中，而所有的草稿都在`source/_draft`中

```shell
(base) jack@jack-Alienware-m15-R3:/media/jack/JackCode/project/hexo-blogs$ tree -d -L 2 source/
(base) jack@jack-Alienware-m15-R3:/media/jack/JackCode/project/hexo-blogs$ ls source/_posts/
(base) jack@jack-Alienware-m15-R3:/media/jack/JackCode/project/hexo-blogs$ ls source/_drafts/
```

原谅我因为博客要在不同的机器上编写，所以放在了硬盘上，导致文件权限很呆滞:thinking:

![Hexo中的草稿与发布文章的文件夹](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211217130820741.png)

![具体的草稿与发布的文章](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211217131004765.png)



因此用的来说，我们写博客时候需要掌握的技巧就是文章在草稿（文件夹）与正式发布（文件夹）之间的左右横跳与单篇文章内的信息设置。





## 2. Hexo文章草稿与正式发布的博客

### 1. 草稿

#### 1. 创建草稿

我们通过hexo new的时候指定使用的模板是draft即可，此时创建的文章会被自动放到_draft文件夹下面

```shell
hexo new draft 标题
```

例如：

```shell
hexo new draft draft_test
ls source/_drafts/
```

![Hexo创建草稿](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211217134454739.png)

![所有草稿文章](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211217134547721.png)

然后我们hexo gen生成文章，再使用hexo server来查看本地文章的时候，并不会看到这些草稿文章

#### 2. 发布草稿

当我们完成了草稿文章之后想要发布的时候，使用下面的命令来进行发布

```shell
hexo publish 标题
```

需要注意的是，Hexo是一个轻量级的博客，因此不存在像记录草稿、已发布的文章这样的数据库。所有的操作都是基于文件的，因此上面的命令就是把文章从_draft中移动到\_post中。我们直接手动mv也是可以的

```shell
hexo publish draft-test
ls source/_posts/ | grep dra
```

![发布文章](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211217135305343.png)



### 2. 正式发布文章

#### 1. 创建正式发布的文章

直接hexo new就完事，或者自己在_post下面创建，略

#### 2. 改发布文章为草稿

没啥好说的，mv就完事了



## 3. Hexo文章Front-Formatter设置

事实上对于Hexo文章的Front-Formatter而言，Hexo本身支持的Front-Formatter是有限的，需要通过第三方的主题进行扩展。幸运的是，我们前面使用的Matery主题以及有所考虑，支持非常多的Front-Formatter。

由于Matery主题的Front-Formatter在[闪烁之狐对Matery主题介绍的博客](https://blinkfox.github.io/2018/09/28/qian-duan/hexo-bo-ke-zhu-ti-zhi-hexo-theme-matery-de-jie-shao/#toc-heading-19)中已经有了非常详尽的介绍，本文只对一些需要配置的功能进行介绍，一些只需要在Front-Formatter中添加对应的项即可显示的设置就不在介绍，请参考Matery主题闪烁之狐的介绍

![Front-Formatter闪烁之狐的介绍](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211217131946247.png)



### 1. 密码设置

对文章进行密码设置功能并不是Hexo的功能，而是Matery主题中的功能，因此我们需要在Matery主题的配置中进行设置

具体路径为`themes/hexo-theme-matery`下的`_config.yml`，然后找到`verifyPassword`这一项，将其设置为True，然后修改提示语即可

```shell
vim themes/hexo-theme-matery/_config.yml 
```

![设置主题，开启文章加密功能](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211217132432259.png)



需要注意的是，Matery主题中的验证密码方式是通过内置的SHA256算法对访问者输入的密码加密之后和我们在文章的Front-Formatter中设置的经过SHA256 加密的 password 的值进行对比得到的。因此我们首先可以通过一下网站来获得密码对应的值。

- [开源中国](https://tool.oschina.net/encrypt?type=2)
- [chahuo](http://encode.chahuo.com/)
- [站长工具](http://tool.chinaz.com/tools/hash.aspx)

例如我们使用开源中国来进行值的获取，注意选择SHA256算法。

![生成加密后的密码的值](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211217133017741.png)



然后我们新建一篇文章来进行测试。

```shell
hexo new passwd-test
```

然后我们在里面设置password这一项，随便写一点内容

![设置文章密码](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211217133315175.png)

然后我们使用hexo gen来生成文章看看效果

```shell
hexo gen
hexo s
```

接下来我们就能够在首页看到这个加密的文章了

![加密的文章](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211217133600366.png)



我们一旦点进去访问，就需要我们输入密码

![输入密码](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211217133650320.png)

我们输入正确之后进去就能够看到了

![输入正确之后可以看到文章](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211217133745568.png)

而一旦输入错误就会返回主页

![输入错误之后返回主页](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211217133827160.png)



