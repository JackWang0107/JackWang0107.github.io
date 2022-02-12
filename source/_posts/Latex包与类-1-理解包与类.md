---
title: 'Latex包与类-1: Understanding packages and class files'
date: 2022-01-27 21:13:13
img: https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220127231805688.png
summary: '本文主要介绍了Latex中的包（Package）与类（Class）'
categories:
  - Latex包与类
tags:
  - Latex
  - Overleaf
---

> 本文主要介绍了Latex中的包（Package）与类（Class）

![Latex Project](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220127231805688.png)



# Latex包与类-1: Understanding packages and class files

我们在使用Latex编写PDF文件的时候，我们通常关注的有两件事：

- 我们首先关注PDF的外观，即PDF的看起来是什么样的，例如什么样的纸A4还是A3、页边距是多少、行间距是多少等等。**这些规定了一个PDF文档看起来是什么样的内容加在一起，统称为格式（Formatting）**。
- 我们其次会关注PDF具有的一些功能，例如我们通过`\cite`来引用文献，通过`\url`来实现点击网页链接（超链接跳跃）。**这些为一个PDF提供了额外的功能的内容加在一起，统称为功能(性)（Functionality）**。

> PS：有些英文的内容翻译成中文真的不好把握，因此下面还是使用Formatting和Functionality来分别指代上面

因此，**外观**和**功能**就是我们在通过Latex写一个PDF文件时候所关注的两个方面。为了控制一个PDF的外观和实现更多的功能：

- Latex提供了一些命令来控制一个PDF的格式
- Latex还提供了一些命令来为一个PDF实现更多的功能

因此，Latex中就提供了分别用于控制格式和功能的命令。

本文后面就将对这些命令及其连带的概念进行讲解。

注意，本文的前提是通过docker已经运行了一个Overleaf（ShareLatex的镜像），本文后面的代码也将基于此进行演示。





## 1. tex、cls、sty文件

为了实现内容、格式、功能的分离，Latex中分别提供了三种文件来分别保存内容、格式和功能的命令，这三种文件分别是：`.tex`、`.cls`和`.sty`文件



### A. .tex文件

`.tex`文件中存放着当前这个PDF所表示的文章的内容。我们在`.tex`文件中编写我们需要表达的内容。

> 注意，这里的文章指论文、书信、书等记录信息的文字。他们只是在格式上有所不用。

在编写`.tex`文件的过程中，我们调用`.sty`文件中提供的命令（可以理解为函数）即可实现一定的功能，例如插入图片`\includefigure`，插入超链接`\url`，进行引用`\cite`等等。

而在编译生成PDF文件的时候，又会根据我们在`.cls`文件中写好的排版格式来安排我们书写的内容

例如下面的`.tex`文件

![.tex文件](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220127221811704.png)

而最后通过编译得到的PDF如下。可以看到，Latex排版系统不仅帮我们完成了排版，还帮我们实现了图片的交叉引用、图片的插入等功能。

![编译后得到的PDF文件](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220127222107535.png)





### B. .cls文件

`.cls`文件中存放着当前这个PDF文件格式，包括但不限于上面说的行间距、段间距。

一般来说，一种题材/格式的文章就会有一个自己的`.cls`文件。例如下面，`letter`格式就有自己的`.cls`文件。

我们可以看到，`.cls`文件中定义了包括各种型号的纸的大小、字体大小等内容。

![letter题材的文章有对应的cls文件定义格式](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220127221222508.png)

我们可以在`.tex`文件中指定使用哪一种格式。如果没有指定的话，Latex在排版的时候就会使用默认的行间距的配置参数，即默认格式。



### C. .sty文件

`.sty`文件中存放着当前这个PDF可以调用的诸多命令（定义了很多函数）。通过调用这些命令我们就可以实现不同的功能。

例如`hyperref`包的sty文件中提供了`\url`命令，通过这个命令我们就可以实现点击超链接跳转到网页去这个功能

![hyperref包的sty文件中定义了该包提供的命令（函数）](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220127215723865.png)





## 2. 包（Package）与类文件（class file）



### A. Package

在上面`.sty`文件的图中，我们能看到sty文件里的内容和编程语言非常像，包括变量定义、程序流程控制语句if……else……、函数定义等等，基本上一应俱全。

也正是因为和编程语言如此相似，甚至都出现了函数定义，因此`.sty`文件也会存在代码复用性的问题。

所以，我们就可以把自己写好的`.sty`文件上传到网上，让大家来下载，从而实现代码复用。类似于其他语言中的库的这个概念，我们一般说的Latex包其实指的就是指我们上传到网上的`.sty`文件。

我们可以看看有多少包

```shell
find ./usr/ -name "*.sty" | tr / "\n" | grep ".sty"
find ./usr/ -name "*.sty" | tr / "\n" | grep ".sty" | wc -l
```

![有5809个包](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220127224443223.png)

当然，这个数字是比较粗略的，因为有的比较大的包里面会有多个`.sty`文件。



### B. Class file

我们在前面说过，在现实的写作中往往是一类体裁的文章就会有对应的格式。例如书信有书信的要求，论文有论文的要求。而这些就某一类体裁，其格式也会有微小的差异。例如对于论文，有学位论文、会议论文、期刊论文等等具体的区分。

虽然这些论文体裁不同，但是在大体上他们遵从的格式都是一样的，例如相同/类似的参考文献罗列格式、相同/类似的图表插入、引用格式等等。

所以，在Latex中，除了包以外的另一个概念就是Class。Latex中将一类大的体裁视为一类对象，例如`letter`，`letter`类中定义了大体上信的格式，同时针对家书、公函、联络信等不同的信提供了选项。**一个Class定义了这一大类体裁的文章所共有的格式，并且为多种不同的具体的体裁的文章提供了选项**

在Latex定义格式的文件是`.cls`文件，因此`.cls`文件又被称为class file（其实缩写就能看出来。。。）

我们在编写`.cls`文件的时候就一定要有一个`\ProvidesClass`命令，以在`.tex`文件中使用该类体裁作为文章的体裁。

![letter.cls文件中提供了letter类](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220127225654975.png)



最后关于具体的Package、Class file怎么写，我们在后面会进行讲解





## 3. Difference between classes and packages

最后，我们讲讲class和package的区别。因为在我们自己动手开始写的时候，我们要自己定义格式和功能，往往会搞不太清楚到底是要写一个package出来还是写一个class出来。因为两者都比较类似，都是通过定义提供了新的命令。这个我们在后面讲解如何写class和package的时候就会有所体会

其实，我们可以通过一个简单的原则来判断到底是要写一个package出来还是一个class出来，即：我们要实现的内容是否会给独立于体裁外的内容添加新的功能，如果是那么就要写一个package，即写一个`.sty`文件；而如果我们要实现的是用于控制最终PDF的长相（外观）的命令，那么就需要写一个class出来，即写一个`.cls`文件。

举例来说，如果一个公司现在需要一个新的命令来便捷的高亮一些词句，那么高亮这个功能是和体裁无关的，因此我们应该写一个package出来。而如果一个公司想要一个以公司logo作为水印的宣传海报，那么这个是和格式强相关的，因此我们就应该写一个class文件出来。
