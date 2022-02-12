---
title: 基于hexo的个人技术博客搭建 —— Part 3 matery主题下的Hexo博客优化.md
date: 2021-11-14 20:48:59
img: https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211115010655463.png
summary: 'hexo本身只是提供了一个博客的框架，博客网站的美化和优化还是需要靠自己配置主题。本文讲解如何利用Matery主题对Hexo搭建的博客进行深度优化'
categories:
	- Hexo博客搭建
tags:
    - Hexo
---

> hexo本身只是提供了一个博客的框架，博客网站的美化和优化还是需要靠自己配置主题。本文讲解如何利用Matery主题对Hexo搭建的博客进行深度优化

![最终效果展示图](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211207132452913.png)





<!-- more -->



# 基于Hexo的个人技术博客搭建 ——  Part 3 matery主题下的Hexo博客优化

在前面的一章中，我们已经通过`hexo`在本地搭建出了一个博客。但是目前，这个博客还存在一些问题

1. **目前博客网站运行在本地，所以只有我们自己能看到**
2. **Hexo默认的博客不够美观、功能不够多**

针对上面两个我们，本章和下一章就将进行解决。本章首先解决第二个问题，即优化Hexo博客。





## 1. 为什么使用其他的Hexo主题？

在前面一章中我们讲过，`Hexo`主题的工作原理其实就是`Hexo`的主题里面写的`JavaScript`和`CSS`覆盖掉了`hexo`的`JavaScript`和`CSS`。而CSS决定了`Hexo`博客的外观，因此是否美观实际上取决于主题里的`CSS`。同样JavaScript决定了网页和我们的交互，因此网页的功能如何实际上也取决与我们的主题。因此，一个优秀的主题是有一个完美的`Hexo`博客的先决条件。有了一个优秀的主题，我们的博客不仅更加美观，功能也会更加强大。

因此我们对`Hexo`博客进行优化，实际上就是使用其他主题，并对这些主题进行配置、

后面，就将以`Hexo`的`matery`主题为例，进行优化





## 2. 基于Matery主题的Hexo博客优化

`Matery`主题是由国内的闪烁之狐（blinkfox）制作的一款美观的主题，包括我在写这篇博客时候我的博客所用的主题就是`Matery`。

之所以使用`Matery`主题，美观只是一个方面，更重要的是`Matery`以插件的形式提供了非常多优秀的程序，通过这些程序使得我们能够极大地优化我们的网站。

访问闪烁之狐的[Hexo博客](http://blinkfox.com/)和他的`Matery`主题的[Github](https://github.com/blinkfox/hexo-theme-matery)查看更多的信息

![闪烁之狐的Matery主题示例](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211115010655463.png)

接下来我们就将基于`Matery`主题来对我们的`Hexo`博客进行优化



> 注意，下面的很多教程在`Matery`的`Github`上已经有介绍了，因此下面的介绍更多的是关注`Matery Github`上没有讲到的点





### 1. 安装Matery

`Matery`的`Github`中的中文说明已经讲过了，在`Hexo`博客文件夹下的`theme`文件夹`clone` 下`Matery`项目即可

```bash
(base) jack@jack-Alienware-m15-R3:~/project/blog-test$ cd themes/
(base) jack@jack-Alienware-m15-R3:~/project/blog-test/themes$ git clone https://github.com/blinkfox/hexo-theme-matery.git
```

安装完成之后，我们hexo博客看看效果

```bash
(base) jack@jack-Alienware-m15-R3:~/project/blog-test/themes$ cd ..
(base) jack@jack-Alienware-m15-R3:~/project/blog-test$ hexo s
```

**发现和之前的博客并没有任何变化**，这是因为我们需要在`Hexo`的配置文件中指定使用`Matery`主题

![博客没有任何变化](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211115011512753.png)





### 2. 切换Matery主题

`vim`修改`hexo`博客根目录下的`_config.yml`，将`theme`的值改为 `hexo-theme-matery`，这样就启用了Matery主题

![修改之后的值](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211115011735922.png)

接下来运行一下博客，就能够看到效果了

```bash
(base) jack@jack-Alienware-m15-R3:~/project/blog-test$ hexo s
```

可以看到，默认配置就已经非常漂亮了

![Matery主题的首页](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211115011929561.png)

![Matery主题的底部](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211115012203467.png)

但是这个默认的主题也有一些小问题，需要我们去修改，比如说网站的左上角的logo，中间浮动打印的语句，中间的github链接需要修改成我们自己的，我的梦想栏的语句需要修改成自己的，右下角的联系方式需要改成我们自己的……

除了这些个人信息配置以外，还有最关键的一点就是右上角选项卡除了`首页`以外，其他的点击都会直接没有对应的界面显示。**这是因为Matery默认没有这些界面，需要我们自己配置**

![点击后显示错误](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211115012428235.png)





### 3. 添加缺失的页面

在前面的一章中，我们讲到`hexo`中一篇博客的源文件是一个`.md`文件，通过使用`hexo generta`命令，hexo自动的为我们的博文生成一个网页以及对应的资源文件。而其实在`hexo`，中一个页面对应的也是一个`.md`文件，同样我们稍后使用`hexo generta`来生成新的页面。只不过页面对应的`.md`文件会和博客的`.md`存在一些不同

下面就将以添加`关于`界面为例，讲解Hexo的页面工作原理的同时带领读者配置`Matery`的页面



#### A. 添加`关于About`页面

我们首先在根目录下使用`hexo new page xxx`命令来生成一个新的页面。同样为了后续的讲解，我们首先保存下现在的目录结构

```bash
(base) jack@jack-Alienware-m15-R3:~/project/blog-test$ touch before_about.txt
(base) jack@jack-Alienware-m15-R3:~/project/blog-test$ touch after_about.txt
(base) jack@jack-Alienware-m15-R3:~/project/blog-test$ tree >> before_about.txt 
(base) jack@jack-Alienware-m15-R3:~/project/blog-test$ hexo new page "about"
```

接下来我们需要修改刚刚新生成的`.md`来说明这个是一个页面而非博客，这样稍后生成博客的时候就会生成出来一个新的页面

```bash
(base) jack@jack-Alienware-m15-R3:~/project/blog-test$ vim source/about/index.md 
```

将`index.md`修改为如下内容，当然日期可以按照你自己的来

```markdown
---
title: about
date: 2021-11-15 01:37:51
type: "about"
layout: "about"
---
```

![修改后的index.md](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211115014047478.png)

接下来我们生成一下添加后的博客，别忘了生成之前`clean`一下，清除掉之前的博客。同时下面由于要讲解原理，因此生成后我们输出一下生成之后的目录结构

```bash
(base) jack@jack-Alienware-m15-R3:~/project/blog-test$ hexo clean
(base) jack@jack-Alienware-m15-R3:~/project/blog-test$ hexo gen
(base) jack@jack-Alienware-m15-R3:~/project/blog-test$ tree >> after_about.txt 
```

然后我们运行下博客看看效果

可以看到，此时点击`关于`就可以正常显示处页面了，底部则显示出我们的制作的项目

![正常显示的关于页面](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211115014533558.png)

![关于页面的底部](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211115014620968.png)



当然，这些信息都是默认的信息，我们稍后是肯定需要修改成我们自己的。不过在这之前，先别着急，我们先了解一下`hexo`是怎么样新生成一个页面的



#### B. Hexo是怎么样生成新的页面的？

我们利用上面两次输出的目录结构，来看看添加、生成页面之后博客目录结构的变化

```bash
(base) jack@jack-Alienware-m15-R3:~/project/blog-test$ vim -d before_about.txt after_about.txt 
```

首先是发布的博客的`public`文件夹下多了非常多的东西。除了我们之前添加的第一篇博客外，还多出了`about`、`css`、`js`等一些文件夹

![Public文件夹的变化](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211115015145727.png)

此外，我们的源代码文件夹中也多了`about`文件夹和在其下面的`index.md`

![source文件夹下的不同](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211115015222432.png)



实际上到这里你应该能够猜出来了，`hexo`和`matery`生成新的页面的工作原理就是：

- 首先`hexo`认为`source`文件夹下的一个文件夹就是一个页面，这个页面必须要有一个`index.md`来说明这个页面的信息，例如上面指定生成页面使用的模板
- 其次，`matery`在生成项目的时候，会在public下生成新的网站的配置文件来修改默认的页面。因此我们的确可以修改这个`public`下面的`matery`生成的资源文件。但是并不推荐这样做，因为所有的修改在后续`generate`之后就会丢失。与此相比，下面会介绍更好的修改、配置`matery`主题的方式

最后，我们依照`Matery Github`（[链接](https://github.com/blinkfox/hexo-theme-matery/blob/develop/README_CN.md)）中的`README`的指引，如法炮制添加剩下的所有页面。

完成了之后，你可以尽情的探索一下新得到的页面。





### 4. 配置Matery主题

上面我们通过简单的Matery主题的配置得到了一些界面，下面我们就将进一步配置Matery主题。

授人以鱼不如授人以渔，因此下面我会首先讲解Matery主题的配置是如何工作的而非单纯的罗列，在讲解完原理之后会留下我参考过的不错的链接，读者可以去里面根据自己的需求配置。



下面将以安装文章字数插件为例进行讲解。



#### A. 文章字数统计

细心地读者已经发现，在我们上面的博客底部其实是没有文章字数统计的，而闪烁之狐的实例网页中却具有文章字数统计。

![我们的界面](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211115021145362.png)

![闪烁之狐的网站](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211115021216163.png)



实际上，这个功能是依靠第三方插件`+Matery`配置完成的。我们首先下载这个插件，不过注意，我们在前面安装了`cnpm`，因此使用`cnpm`安装即可

```bash
(base) jack@jack-Alienware-m15-R3:~/project/blog-test$ cnpm i --save hexo-wordcount
```

安装完成之后，我们还要修改`theme`文件夹下`Matery`主题的配置文件来激活插件，**注意是Matey主题的配置文件**。因为页面的样式信息都是Matery负责的。

```bash
(base) jack@jack-Alienware-m15-R3:~/project/blog-test$ vim themes/hexo-theme-matery/_config.yml 
```

修改为

```yaml
postInfo:
  date: true
  update: false
  wordCount: true # 设置文章字数统计为 true.
  totalCount: true # 设置站点文章总字数统计为 true.
  min2read: true # 阅读时长.
  readCount: true # 阅读次数.
```

![修改后的_config.yml](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211115021824246.png)

然后我们同样clean之后generate看看效果

可以看到，已经出现统计信息了

![底部具有了统计信息](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211115021949461.png)







#### B. Matery的配置是如何工作的？

事实上，`Matery`主题依靠其主题文件夹下的`_config.yml`来进行配置。这个文件中提供了诸如：网站上方选项卡的选项、个人信息、头像、logo等资源文件位置这类的配置，还有是否激活xx插件、xx效果等配置。因此我们通过`Matery`的`_config.yml`来进行配置。

在我们配置完了之后，在`generate`的过程中，`matery`就会根据我们的配置来生成对应的文件。可以说是非常方便。

例如我们修改在关于页面中显示的个人信息

![修改个人信息](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211115023301903.png)

同样，我们修改之后clean、gen、server来看看效果

可以看到，相关信息已经被修改了，不过由于我没有改头像，因此头像没有变

![修改后的效果](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211115023432763.png)



此外，由于闪烁之狐大佬良好的代码风格，因此在`Matery`的配置文件夹下，几乎可以看到所有的配置以及修改方式~。大家可以多试试

此外，推荐一个我参考过的博客，他也是基于`Matery`进行了配置和优化，并且自己改了一些`JavaScript`，以实现更好的效果，[零下三度的极寒的博客](https://m3df.xyz/2020/06/13/e9fff968/)





最后，本章到了这里就结束了。在本章我们首先讲解了为什么要使用第三方的`Hexo`主题，以及为什么使用`Matery`主题。接下来我们结合两个案例讲解了如何对`Matery`主题进行配置并解释了`Matery`配置的工作原理，在明白工作原理之后，大家去修改自己的博客网站就会得心应手很多。最后我们提供了其他的参考链接来帮助大家配置自己的博客网站。



在下一章中，我们将讲解如何低成本的把自己的博客部署到公网上去，使得所有人都能够访问你的博客。



码字不易，3100字，欢迎打赏\~，一起推动开源事业\~
