---
title: 基于Hexo的个人技术博客搭建 —— Part 2 Hexo快速建站以网站基础信息配置
date: 2021-11-13 16:46:10
img: https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211113193858319.png
summary: '本文首先讲解如何使用Hexo快速搭建出一个博客网站，接下来对`Hexo`搭建的博客网站进行介绍，最后对Hexo搭建的博客网站进行网站基础信息的设置。'
categories:
	- Hexo博客搭建
tags:
    - Hexo
---

> 本文首先讲解如何使用Hexo快速搭建出一个博客网站，接下来对`Hexo`搭建的博客网站进行介绍，最后对Hexo搭建的博客网站进行网站基础信息的设置。

![快速建站最终实现的效果](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211113193858319.png)

<!-- more -->



# 基于Hexo的个人技术博客搭建 —— Part 2 Hexo快速建站以网站基础信息配置

在前面的章节中，我们已经讲解了配置`Hexo`的开发环境，本节将讲解如何使用Hexo快速建立自己的个人博客以及对个人博客进行基本信息设置



## 1. Hexo快速建站

> 前面说过，`Hexo`搭建得到的博客本质上就是一个文件夹，因此`Hexo`进行的各种操作都是对这个文件夹里的文件进行操作。



### 1. 初始化博客 ——  `hexo init`

`Hexo`提供了init命令来初始化一个博客，为此我们首先新建一个目录用来存放博客的所有文件

```bash
(base) jack@jack-Alienware-m15-R3:~/project$ mkdir blog-test
(base) jack@jack-Alienware-m15-R3:~/project$ hexo init blog-test/
```

> `hexo init`本质上就是去`github`上克隆`hexo starter`项目

![hexo init初始化博客的过程](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211113194931504.png)





### 2. 运行博客 —— `hexo server`

接下来，在博客所在的文件夹的根目录下运行`hexo server`，来启动`hexo`的服务程序，这样就可以显示出我们的博客网站。`hexo`默认是带有一个欢迎界面的，因此即便我们什么都不做也可以正常的运行

```
(base) jack@jack-Alienware-m15-R3:~/project$ cd blog-test/
(base) jack@jack-Alienware-m15-R3:~/project/blog-test$ hexo server
```

![image-20211113195611688](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211113195611688.png)

运行之后浏览器中访问该端口就能够看到默认的界面，不过默认的界面的问题还是比较多的，比如网站用的默认主题并不是非常好看、网站的信息都不是，后面我们会慢慢优化。这里看到的效果如下

可以看到左下角的版权信息是默认的人名，左上角的选项卡选项也很少，右侧的菜单栏消息也很少，下一步我们就将慢慢改掉默认的界面

![朴素的Hexo](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211113205754034.png)

为了等下讲解`Hexo`的原理，我们这里先创建三个文件

```bash
(base) jack@jack-Alienware-m15-R3:~/project/blog-test$ touch before_gen.txt
(base) jack@jack-Alienware-m15-R3:~/project/blog-test$ touch after_gen.txt
(base) jack@jack-Alienware-m15-R3:~/project/blog-test$ touch added_gen.txt
(base) jack@jack-Alienware-m15-R3:~/project/blog-test$ tree >> before_gen.txt 
```



### 3. 生成博客 —— `hexo clean` & `hexo generate`

前面说道，`Hexo`的工作原理是通过`Markdown`引擎将`Markdown`格式的文本渲染成`HTML`，因此每一次我们在写完文章之后都需要生成一下博文，注意`hexo`提供了简写命令，因此g、gen、generate都是可以的

```
(base) jack@jack-Alienware-m15-R3:~/project/blog-test$ hexo gen
```

可以看到生成了不少文件内容

![hexo gen生成博文](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211113215116668.png)

最后输出一下目录方便后面查看hexo的运行原理

```bash
(base) jack@jack-Alienware-m15-R3:~/project/blog-test$ tree >> after_gen.txt
```



### 4. 新加一篇博客 —— `hexo new`

我们自己添加博客，需要使用`hexo new 文章名`，如果文章名称中含有特殊字符，需要用`''`包裹起来

```bash
(base) jack@jack-Alienware-m15-R3:~/project/blog-test$ hexo new 第一篇博文
```

新添加的博客在`source/_posts/`下，使用编辑器打开即可，后面会写一个`Typora + 腾讯云床`的博客编写环境教程，挖个坑

```bash
(base) jack@jack-Alienware-m15-R3:~/project/blog-test$ echo "# 大标题" >> source/_posts/第一篇博文.md 
```

接下来继续生成博客，然后我们启动server来看看我们新写的博文，server的简写是s

注意，在生成前需要用clean清除一下中间的文件

```bash
(base) jack@jack-Alienware-m15-R3:~/project/blog-test$ hexo clean
(base) jack@jack-Alienware-m15-R3:~/project/blog-test$ hexo gen
```

![新添加一篇博文](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211113220005285.png)

可以看到多了一篇文章

![添加文章之后的结果](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211113220330894.png)

同样，我们记录一下，方便后面讲解原理

```bash
(base) jack@jack-Alienware-m15-R3:~/project/blog-test$ tree >> added_gen.txt 
```





以上就是关于`Hexo`的一些基础命令使用





## 2. Hexo的运行原理



### 1. Hexo的目录结构

我们通过`tree`命令来查看

```bash
(base) jack@jack-Alienware-m15-R3:~/project/blog-test$ tree -L 1
```

其中，每个文件/文件夹的作用为：

- `_config.yml`：网站的**基础配置**信息，可以在这里配置网站的一些基本参数，例如作者等等。**之所以是基础配置信息，是因为使用不同的主题将会在极大程度上修改、覆盖这里的配置信息**。
- `package.json`：Hexo生成网页、运行服务器等的应用程序信息。`EJS`, `Stylus` 和 `Markdown renderer` 已默认安装，可以由我们自由移除。
- `scaffolds`：模版文件夹。当您生成、新建文章时，`Hexo` 会根据 `scaffold` 来建立文件。**不建议修改**
- `source`：资源文件夹是存放用户资源的地方。除 `_posts` 文件夹之外，开头命名为 `_` (下划线)的文件 / 文件夹和隐藏的文件将会被忽略。`Markdown` 和 `HTML` 文件会被解析并放到 `public` 文件夹，而其他文件会被拷贝过去。
- `themes`：主题文件夹。`Hexo` 会根据主题来生成静态页面。

这些文件、文件夹中对我们而言最重要的就是`_config.yml`、`themes`、`source`这三个，其他的其实一般我们用不到也不需要改

![Hexo博客的架构](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211114095916621.png)



### 2. Hexo是如何生成和发布的博文

上面从整体上介绍了`Hexo`的目录结构。下面我们将深入了解一下`Hexo`生成博文的原理。

我们上面使用tree生成了三次目录结构，接下来我们查看下生成博文和添加博文之后博客项目的变化，使用`vim -d`比较下三个文件的内容

```bash
(base) jack@jack-Alienware-m15-R3:~/project/blog-test$ vim -d before_gen.txt after_gen.txt added_gen.txt 
```

可以看到，我们编写的博客都被添加到了`source/_posts`文件夹下，而我们运行`hexo generate`后生成的静态博文网页资源就都放在`public`文件夹下，具体来说，文章放在以日期为名的系列文件夹下，`CSS`等资源文件则是单独放置

![hexo new、add之后文件目录的变化](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211113223040926.png)





### 3. Hexo是如何管理主题的

下面我们通过我当前的这个博客来了解下`Hexo`是如何管理主题的，以及主题是如何工作的

```bash
(base) jack@jack-Alienware-m15-R3:~/project/hexo-blogs$ tree themes/  -L 3
```

可以看到，`theme`文件夹下，一个文件夹就是一个主题。一个主题内部又有其自身的结构，不同的目录有不同的作用。例如`language`负责不同语言的博文的设置，`source`则存放主题的`css`、`js`等资源文件，`_config.yml`则负责主题的配置。

**因此在后续我们美化、深度定制Hexo时候就是修改下载好的主题**

![Hexo的主题目录](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211114021748798.png)







## 3. Hexo 基础信息配置

前面我们讲到，`Hexo`自身的`_config.yml`虽然负责整个项目的配置，但是通常会被我们自己下载的主题的`_config.yml`所覆盖，因此我们只在Hexo的`_config.yml`中进行一些基础信息的配置，以便于我们为我们的博客打上自己的信息。

首先通过`vim`或者其他编辑器打开根目录下的`_config.yml`

```bash
(base) jack@jack-Alienware-m15-R3:~/project/blog-test$ vim _config.yml 
```

![Hexo 根目录下的配置文件](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211114022512202.png)





### 1. 个人信息设置

我们只需要修改 `# Site`中的信息即可，这样将网站的默认信息改为我们自己的信息

![修改后的个人信息](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211114023546668.png)





### 2. URL修改

注意，`# URL`下的内容修改则在未来将会在别人从我们的网站中复制内容之后作为后续的内容提醒版权，此外也会在文章底部说明版权。

我们暂时先不做这个修改，等后续博客上线、具有具体的域名/网址后再进行修改。





进行完上面所有的配置之后，我们就可以查看最终的效果了，我们开启下server查看效果

可以看到首页的文字和页脚的信息已经改掉了，当然，每篇文章里作者的信息也已经改变了。

![修改后的首页](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211114023717905.png)

![修改后的页脚信息](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211114023757242.png)





至此，本章就已经结束了。本章我们首先讲解了`Hexo`的基础命令，然后研究了`Hexo`的目录结构和工作原理，方便我们后续修改，最后我们对网站的基础信息做了修改，更多的网站内容和信息的设置详见下一章~



码字不易，2200多字，欢迎打赏\~，一起推动开源事业进步\~
