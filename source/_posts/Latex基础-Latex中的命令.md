---
title: Latex基础：Commands
date: 2022-01-28 10:10:09
img: https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220128162651753.png
summary: '本文主要介绍了Latex中的命令'
categories:
  - Latex基础
tags:
  - Latex
  - Overleaf
  - Latex Commands
---

> 本文主要介绍了Latex中的命令

![Latex中的Command](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220128162651753.png)





# Latex基础：Commands

Latex对我们写好的文章进行排版，主要是通过我们在文章中写好的一些特殊标记（Special Tags）来完成的。这些标记称为命令（Command）。

Latex内置了不少命令，还有许多第三方包也提供了不少新的命令。有的时候现有的命令可能无法满足我们的需求，这个时候我们就需要自己定义新的命令了。

下面，本文就将介绍什么是命令以及如何编写新的命令。





## 1. Commands



### A. Introduction to Commands

**Latex中的命令是由词语和符号组成的一段具有特殊的语义和功能的、能被Latex编译器识别的字符序列**。

例如下面这段代码

```latex
In a document there are different types of \textbf{commands} 
that define the way the elements are displayed. This 
commands may insert special elements: $\alpha \beta \Gamma$
```

![编译出来的效果](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220128105239982.png)

上面的这段代码中出现了多个不同的命令，例如：`\documentclass`、`\title`、`\begin`、`\end`、`\textbf`、`\alpha`等等。



### B. Commands的分类

从不同的角度出发，我们能够给Latex中的命令以不同的标准进行分类。这样做的好处就是可以帮助我们更深入的理解Latex中的命令。



从是否接受参数的角度出发，Latex中的命令可以分为两类：

- 有一类命令接受参数。我们其实可以当做编程语言中的函数，而后面花括号`{}`里的内容就对应了函数的参数。因此这这类命令接受不同的参数，实现不同的功能，例如：`\textbf`命令将会加粗传入的参数
- 还有一类命令不接受参数。这类命令根据具体命令的不同，有不同的效果，例如：`\alpha`、`\beta`、`\Gamma`等等



而从命令使用者的角度出发，Latex中的命令可以分为三类：

- 第一类是给文章（`.tex`）作者使用的命令，例如上面截图中的所有命令
- 第二类是给包（`.sty`）和类（`.cls`）的作者使用的命令，例如`\ProvidesClass`、`\ProvidesPackage`等命令
- 第三类是用于实现Latex本身的内部命令（类比于C语言的关键字），例如`\@tempcnta`、`\@ifnextchar`、`\@eha`等命令。这类命令中通常包含`@`，并且这些命令只能用于类和包中，而不能用于普通的文档中

对于一般的写作而言，我们只需要用第一类命令即可，只有要创建Latex包和类，我们才会用到第二类、第三类命令。而`Latex基础`的系列文章不会讲解Latex类和包。类和包请参考我的`Latex类和包`系列文章。



### C. 命令与环境

Latex中的另外一个重要的概念就是环境。所谓环境，在剑桥词典中有两个意思：

- **自然环境**，例如：一些化学药品因为对环境有破坏作用因此已经被禁止使用
- **周围的状况**，例如：办公室明亮、通风，工作环境还不错。

而在Latex中，环境即指一段文本周围的状况。状况包括：行间距、对齐格式、字号、字体等等因素。因此在一个环境内的文本都将会具有该环境特定的行间距、字号、对齐方式等格式。

例如下面一段文本，我们开始了一个`itemize`环境和`center`环境。在`itemize`环境中，我们可以进行编号项的排序。而在`center`环境中，一句话将会居中对齐

```latex
This is a normal sentence. It will with a tab in front of it. and automatically return to another line.
\begin{itemize}
    \item This is an \textbf{itemize} environment
    \item To begin an item in itemize environment, use $\backslash$item command.
\end{itemize}

This is a sentence without center environment. It will starts from the left with a tab in front of it.
\begin{center}
    A sentence nested in center environment\\
    will be placed in the center of the page.
\end{center}
```

具体的效果如下

![itemize环境和center环境的效果](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220128142131557.png)



而Latex中是使用命令来控制一切格式的，因此，环境的开启、设置、结束也是通过命令来完成的。因此从环境的角度出发，命令又可以分为：

- 普通命令：不涉及到环境的命令
- 环境命令：涉及到环境的设置、结束的命令

普通环境用于特殊字符的显示、格式的控制；而环境命令用于环境的开启、结束以及环境内字符显示和格式控制。

其实如果观察仔细的话，就可以发现我们的文章都是在`document`这个环境里的，其实这是Latex设计的的一个哲学，更多的内容就不展开了。

具体的关于Latex环境更多的介绍请参考我在`Latex基础`系列文章中的`Latex环境`文章。





## 2. Defining a new command

虽然Latex本身自带了很多命令以满足不同需求的任务，然而在有的时候，我们确实有需要来自己定义一些新的特殊的命令来简化不必要的重复或者简化复杂格式的创建。

例如在每一页都需要设置页眉，那么如果我们写一个命令来自动的执行设置页眉的一系列命令，那么在20多页的PDF里我们在新的一页只需要调用我们新建的命令即可，而避免了每页都要进行复杂的格式设置



### A. 新建简单命令

在Latex中提供了`\newcommand`命令帮助我们新建一个简单命令，新建简单命令的格式如下

```latex
\newcommand{调用格式}{实际执行的代码}
```

例如我们为一个专用词语新建一个简单命令

```latex
\newcommand{\PRC}{\textbf{People's Republic of China}}
```

然后我们写出来下面的一句话来调用我们新建的简单命令

```latex
I'm Jack, I live in \PRC. There is the Grate Wall in \PRC, welcome to \PRC.
```

编译之后的效果如下

![新建简单命令的效果](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220128143426060.png)

所以我们通过新建命令的一个好处就是可以减少重复。

一般来说，自己定义的命令最好放在文章的最前面，这样的话实现定义和内容的分离。





### B. 接受参数的命令

命令我们可以视为参数来对待，因此我们自己定义的命令也可以接受新的参数，具体而言，定义接受新的参数的命令如下

```latex
\newcommand{调用格式}[参数个数]{实际执行的代码}
```

而在实际执行的代码（即函数体中），通过`#1`、`#2`等方式来调用实际传入的参数。

而我们在调用命令的时候，我们写参数个数个`{}`，并在括号里写入真实的参数值

```latex
\调用命令{参数1的值}{参数2的值}...{参数n的值}
```

注意，Latex中最多支持九个参数

例如下面的命令

```latex
\newcommand{\generatePolynomial}[3]{$$#1^3+#2^2+#3^1$$}

We just define a command to generate a polynomial, so we next call it and see what happened:
\generatePolynomial{x}{y}{z}
```

编译之后的效果如下

![接受参数的命令](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220128145129309.png)





### C. 带默认参数/可选参数的命令

同样类比于编程语言的中的函数，我们同样可以给一些参数指定默认值，这样的话我们在调用命令的时候其实就可以不用指定这些参数的值。格式如下

```latex
\newcommand{\调用格式}[参数个数][第一个参数的默认值]...[第n个参数的默认值]{实际执行的代码}
```

而在实际调用的时候，我们通过`{}`指定的参数就是第一个没有默认值的参数

例如下面

```latex
\newcommand{\sumSquare}[3][\alpha]{$$(#2+#3)^#1$$}
\sumSquare{x}{y}
```

我们首先指定了这个命令接受三个参数，然后指定第一个参数的默认值是\alpha。

编译之后的效果如下

![使用参数的默认值](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220128160734885.png)

如果我们想要改变默认参数的值的话，我们就在一半参数前用`[]`来指定这些默认参数的新的值

```latex
\newcommand{\sumSquare}[3][\alpha]{$$(#2+#3)^{#1}$$}
\sumSquare[12]{x}{y}
```

编译之后的效果如下

![改变参数的默认值](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220128161038000.png)







## 3. Overwrite a command

Latex中其实是有条件判断语句的，因此我们其实可以搭配着条件判断语句来实现程序流程的控制。因此，在有些情况下我们就需要重写一下已经定义过的命令。此时如果我们使用`\newcommand`的话编译器就会报错。

![直接使用newcommand会报错](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220128161742577.png)

因此，我们此时就需要用`\renewcommand`来重写一个命令。`\renewcommand`和`\newcommand`一模一样，只不过重复的时候会重写。因此下面就举一个简单的例子

```latex
\newcommand{\overWrite}{x+y}
\overWrite
\newline


\renewcommand{\overWrite}{a+b}
\overWrite
```

![重写命令的例子](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220128162613742.png)
