---
title: Latex基础：Environments
date: 2022-01-31 19:36:46
summary: '本文主要介绍了Latex中的环境'
img: https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220201014340608.png
categories:
  - Latex基础
tags:
  - Latex
  - Overleaf
  - Latex Environment
---

> 本文主要介绍了Latex中的环境

![Latex中的环境](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220201014340608.png)





# Latex基础：Enviroments

上文介绍到，Latex的源代码类似于Markdown和HTML，都是含有特殊标记的纯文本。而Latex的排版引擎（编译器）通过解析这些特殊的标记来得知我们对某些文本的排版要求（例如字号、粗细）等等。而后Latex排版引擎而后通过排版算法，最终将我们文档中的所有的元素按照我们希望的方式完成排版。**这些特殊的标记称为命令**。

在命令的基础上，Latex的另外一个概念就是环境（Environment）。通过环境，我们能够更好的组织我们的文档，本文下面就将介绍Latex中的环境（Environment）。







## 1. Concept of Latex Environment 

为了能够准确的把握Latex中环境（Environment）这个概念，我们先来看看环境（Environment）这个词语的意思。

由于Surrounding和Environment等多个词语翻译成中文都是环境的意思，因此为了准确，我们还是得查英文的词典中Environment的意思。

在剑桥词典中，Environment有两个意思：

- 自然环境

  ![环境的第一个意思](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220131211630440.png)

- 周围的状况

  ![环境的第二个意思](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220131204735055.png)



因此，**对于Latex而言，环境指的就是一段文本周围状况**。文本周围的状况具体包括这段文本的字号、字间距、字体、字体风格（斜体、加粗、下滑线、删除线等等）。**形象的理解，对于一段含有格式的文本而言，我们把文本从中抽取出来，剩下的所有字体格式等内容包含在一起就称为环境**。

例如我们下面的这个`center`环境。这个环境的作用可以使一段文本居中对齐

```latex
\begin{center}
This text will be centred since it is inside a special 
environment. Environments provide a efficient way of modifying 
blocks of text within your document.
\end{center}
```

![居中环境的效果](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220131214956715.png)





## 2. Introdcution

我们上面介绍了Latex的环境的概念，下面就将更深入的介绍Latex的环境。



### A. 一般的环境

正如我们在最前面所讲的，Latex中的排版是通过命令完成的，而环境是建立在命令基础上的概念。因此，环境也是通过命令来实现的。具体而言，就是用过`\begin`和`\end`两个命令来开启和结束环境的。例如上面的居中环境

```latex
\begin{center}
This text will be centred since it is inside a special 
environment. Environments provide a efficient way of modifying 
blocks of text within your document.
\end{center}
```

我们在命令那一节中说过，命令可以理解为函数，那么`center`这里就是我们传递给`\begin`和`\end`命令的参数，即`\begin`和`\end`通过参数的形式来开启和关闭环境。

我们说过环境是一段文本周围包括字号等格式在内的状况，而我们使用`\begin`开始一个环境和`\end`结束一个中间都处于我们开启的环境中，因此，被`\begin{环境名}`和`\end{环境名}`包围的文字，都将拥有环境的格式。

因此，环境是一个非常方便的能够设置一块（block）文本的格式。





### B. 含参数的环境

我们前面说，环境是用`\begin`和`\end`两个命令来开启和结束，而命令可以接受参数和可选参数，因此我们在开启环境的时候也可以指定参数或者可选参数。

参数的例子例如表格环境，我们的第一个参数是每列单元格的就这种格式

```latex
\begin{tabular}{ c c c } 
  cell1 & cell2 & cell3 \\ 
  cell4 & cell5 & cell6 \\ 
  cell7 & cell8 & cell9 \\ 
 \end{tabular}
```

![表格环境的效果](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220131225137991.png)

我们也可以用`[]`来传入可选参数，例如表格环境中使用`[htbp]`来指定排版算法在排版时优先排版的位置





## 3. Defining a new environment

在Latex中，我们可以使用`\newcommand`来定义一个新的命令，从而简化我们编写源代码。

我们其实也可以自己定义一个新的环境，在新的环境中定义我们需要的格式，这样的话就可以减少不必要的重复。





### A. 定义简单的环境

环境因为包含开始和结束两部分，因此我们在自己定义环境的时候也需要环境的开始和结束部分。而定义新环境所使用的命令为`\newenvironment`，具体使用如下

```latex
\newenviroment{新环境名}
{
	环境开始部分代码
}
{
	环境结束部分代码
}
```

注意，环境开始部分的代码会在环境开始的`\begin`之后立即执行，而环境结束部分代码则紧跟在`\end`前运行

而在定义结束之后，我们和使用正常的环境一样进行使用即可，即

```latex
\begin{新环境名}
...
\end{新环境名}
```

例如我们现在定义用一个单元格的表格给四周加上线来表示一个方框的环境，代码如下

```latex
\newenvironment{myBox}{
    % 开始环境代码
    \begin{center}
    \begin{tabular}{|c|}
    
    \hline
    \newline
}{
    % 结束环境代码
    \\
    \hline
    \end{tabular}
    \end{center}
}
```

然后我们使用新定义的环境之后的效果如下

```latex
\begin{myBox}
    This sentence will be placed in the center with a box.
\end{myBox}
```

![新定义的box](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220201004826372.png)





### B. 接受参数的环境

同样，在定义新环境的时候我们也可以接受一个参数，然后在代码部分通过`#1`、`#2`的形式来调用。而和命令一样的是，我们在指定了环境的名称之后使用`[]`来指定参数个数，具体如下

```latex
\newenvironment{新环境名}[参数数量]
{
	环境开始部分代码
}
{
	环境结束部分代码
}
```

例如我们现在要给前面的box加上名字

```latex
\newenvironment{myNamedBox}[1]
{
    % 开始环境代码
    \begin{center}
    Title: #1\\[1ex]
    \begin{tabular}{|c|}
    \hline
    \newline
}{
    \\
    \hline
    \end{tabular}
    \end{center}
}

\begin{myNamedBox}{Named Box Example}
    This is a named box.
\end{myNamedBox}
```

结果如下

![使用参数的环境](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220201010602125.png)





### C. 含默认参数的环境

含默认参数的环境和命令一样，都是

```latex
\newenvironment{新环境名}[参数个数][默认参数值]
{
	开始部分代码
}
{
	结束部分代码
}
```

例如

```latex
\newenvironment{indexedBox}[2][A]
{
    \begin{center}
    Title #1. #2\\
    \begin{tabular}{|c|}
    \hline
}
{
    \\
    \hline
    \end{tabular}
    \end{center}
}

\begin{indexedBox}{Indexed Box A}
    This is box A
\end{indexedBox}

\begin{indexedBox}[B]{Indexed Box B}
    This is box B
\end{indexedBox}
```

编译之后的效果如下：

![含默认参数的环境](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220201013215957.png)





## 4. Overwriting existing environments

要重写一个现有的环境也非常简单，直接用`\renewenvironment`即可

```latex
\renewenvironment{原环境名}[参数个数]
{
	开始代码
}
{
	结束代码
}
```

例如我们重写一下`itemize`环境

```latex
\renewenvironment{itemize}
{
	\begin{center}
	\em
}
{
	\end{center}
}
```

结果如下

![覆盖环境](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220201012457406.png)





## 5. 带编号的环境

实际上，`enumerate`、`itemize`都是带编号的环境，在其中我们可以使用环境内部定义的命令来生成编号项。

而我们其实也可以自己的来实现，即运用Latex自带的`\newcounter`命令来实现，或者用`amsmath`包中提供的`\newtherum`宏来实现。例如

```latex
%Numbered environment
\newcounter{example}[section]
\newenvironment{example}[1][]
{
    \refstepcounter{example}\par\medskip
    \noindent \textbf{Example~\theexample. #1} \rmfamily
}
{
    \medskip
}

\begin{example}
    User-defined numbered environment 1
\end{example}
\begin{example}
    User-defined numbered environment 2
\end{example}


%Numbered environment defined with Newtheorem
\newtheorem{SampleEnv}{Sample Environment}[section]

\begin{SampleEnv}
User-defined environment created with the \texttt{newtheorem} command.
\end{SampleEnv}
```

编译后得到的效果如下，注意第二种方法需要在导言区`\usepackage{amsmath}`

![带编号的环境](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220201013827348.png)
