---
title: 'Latex包与类-2: Writing my own class files'
date: 2022-01-27 23:40:55
img: https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220127231805688.png
summary: '本文主要介绍了Latex中的包（Package）与类（Class）'
categories:
  - Latex包与类
tags:
  - Latex
  - Overleaf
  - Latex Package
  - Latex Class
---

> 本文主要介绍了如何编写Latex中的类（Class）

![Latex Project](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220127231805688.png)







# Latex包与类-2: Writing my own class

有的时候，我们需要高度定制化一个文章出来，那么这个时候最佳的选择就是自己从头开始写一个`.cls`类文件。本文介绍了一个`.cls`类文件的主要的结构以及一些必要的命令。



## 1. Before Writing

在开始编写自己的`.cls`类文件之前，我们首先需要干两件事以确保自己是不是真的要写一个`.cls`类文件出来：

1. 在[CTAN (Comprehensive Tex Archive Network)](http://www.ctan.org/ctan-portal/search/)上看看有没有已经写了并上传一个我们最终目标相似的`.cls`文件。如果有的话其实我们只需要改改就可以了，这样就可以节省我们大量的时间，因为我们的重点在于组织内容而非格式编排
2. 根据前一篇文章，确定自己到底是要写一个类文件还是包。因为如果决定错的话，未来做出来的模板的扩展性就很差，而且也会影响到最终成果的美观程度



## 2. General Structure

一般来说，一个`.cls`文件需要有以下四个部分/可以被分为以下四个部分：

1. 定义类（Identification）：在根本上`.cls`和`.tex`、`.sty`文件没什么区别，都是文本文件，因此Latex判断一个文件到底是不是`.cls`文件是根据特殊的命令的。因此一个`.cls`文件首先需要通过Latex提供的命令来声明自己是Latex的类文件。
2. 声明依赖及变量定义（Preliminary Declarations）：类文件之间、类文件与包之间会相互引用，因此需要首先声明我们自己写的类文件以来的类和包。此外，一些全局变量的声明也是在这里定义的。
3. 声明/提供选项（Options）：一个类通过提供不同的选项，来实现不同的配置。因此在声明完依赖和变量之后就声明该类提供哪些选项以及该类如何处理这些选项
4. 更多的定义（More Declarations）：类文件的主题





### A. Identification

Identification的作用就是让一个类文件声明自己是一个类文件，定义该类文件提供的类。

而进行Identification的语法就两句话

```latex
\NeedsTeXFormat{LaTeX2e}[2021/1/28]
\ProvidesClass{exampleclass}[2021/01/28 Example LaTeX class]
```

其中：

- `\NeedsTexFormat`定义了编译这个类需要的Latex编译器版本，后面的`[]`内的内容是可选选项，表示日期
- `\ProvidesClass`定义了这个类文件提供的类，花括号中是提供的类名，而后面的`[]`中的内容也是日期，可以加上一些说明文字
- 日期按照`yyyy/mm/dd`的格式书写





### B. Preliminary declarations

一般来说，我们的文章都是需要使用到别的类、包的，因此我们就可以在`.cls`文件中定义这些依赖

例如下面我们扩充了上面的话

```latex
\NeedsTeXFormat{LaTeX2e}
\ProvidesClass{exampleclass}[2014/08/16 Example LaTeX class]

\newcommand{\headlinecolor}{\normalcolor}
\LoadClass[twocolumn]{article}
\RequirePackage{xcolor}
\definecolor{slcolor}{HTML}{882B21}
```

其中：

- 我们首先通过`\newcommand`命令自己定义了一个新的命令，后面我们可能会用到这个命令。`\newcommand`第一个参数是新的命令的名称，第二个参数是调用了新的命令执行的代码。这里的`\normalcolor`就是恢复到默认颜色。在word中我们设置了前面的字符是某种颜色之后，如果我们不修改的话那么后面的字符也成了相同的颜色，在Latex中也是一样的。
- 接下来`\LoadClass`命令加载了`article`体裁的文章。`\LoadClass`类似于`import`和`#include`都是文本意义上的复制粘贴。因此我们这里就表示我们自己的这个类是在`article`的基础上修改得到的。同样类比于`import`和`#include`，我们在完成了加载其他类之后，我们就可以使用这个类里面定义的名了。此外我们这里就是在加载`article`这个class的同时指定了`twocolum`参数，即双栏文章。
- 然后，`\RequirePackage`指明了我们这个类文件以来的包。而`\RequirePackage`其实除了指明我们用的包以外，还直接帮助我们导入了这个包，因此就和`\usepackage`一样，我们也可以在花括号`{}`前使用中括号`[]`来传入参数
- 最后，使用`\definecolor`命令定义了一个颜色缩写，第一个参数是缩写的名称，第二个参数是色彩系统，第三个参数是色彩系统中该色彩具体的值





### C. Options

为了增加类的扩展性以适应同一体裁不同的文章的具体要求，我们接下来在类中定义一系列的选项，以及处理具体参数值的语句。

注意，在Latex中，参数是在导入类的时候通过`[]`来选择的，如果把整个class当做一个函数的话，这里有点类似于我们这里定义函数的参数，而在`.tex`导入类的时候则类比于传入具体的参数值

我们下面就添加了更多的语句

```latex
\NeedsTeXFormat{LaTeX2e}
\ProvidesClass{exampleclass}[2014/08/16 Example LaTeX class]

\newcommand{\headlinecolor}{\normalcolor}
\RequirePackage{xcolor}
\definecolor{slcolor}{HTML}{882B21}

\DeclareOption{onecolumn}{\OptionNotUsed}
\DeclareOption{green}{\renewcommand{\headlinecolor}{\color{green}}}
\DeclareOption{red}{\renewcommand{\headlinecolor}{\color{slcolor}}}
\DeclareOption*{\PassOptionsToClass{\CurrentOption}{article}}
\ProcessOptions\relax
\LoadClass[twocolumn]{article}
```

其中：

- `\DeclareOption`命令接受两个必须的参数，第一个参数是该选项的名称，而第二个参数是若指定该参数/给参数传入数值后需要执行的命令
  - `\OptionNotUsed`会在日志里输出一句话，表示这个选项不能使用。这样的话当用户在使用`\OptionNotUsed`这个参数的时候就会被忽略掉，这样就强制文章是双栏的，这样做是为了覆盖掉基类的选项
  - `\renewcommand`用于重新定义一个已经定义过的命令，这里就是说如果传入了`green`选项的话，那么就把标题的颜色设置为绿色，如果传入了`red`选项的话，那么就使用上面定义的红色
  - `\DeclareOption*{}`则表示没有传入参数时候执行的代码，因此只需要一个参数，即执行的命令即可
  - `\PassOptionsToClass{options-list}{package-name}`用于将optons-list参数中列出的选项（不止一个时用逗号隔开）传递给包package-name。即执行了package-name中这些option对应的代码，相当于在`\RequirePackage`中指定/传入了option-list中的选项。
  - `\CurrentOption`表示当前所有的选项
- 注意，前面`\DeclareOption`都只是声明了选项和如果指定该选项执行的代码，而`\ProcessOptions\relax`这一句话才表示真正的处理传入的参数，找到对应的定义，执行代码。所以这一句话必须在所有的`\DeclaraOption`后面



### D. More declarations

在声明完选项之后，我们就可以开始对文章中的细节进行设置了，包括行间距、标题字号等等。例如

```latex
\NeedsTeXFormat{LaTeX2e}
\ProvidesClass{exampleclass}[2014/08/16 Example LaTeX class]

\newcommand{\headlinecolor}{\normalcolor}
\RequirePackage{xcolor}
\definecolor{slcolor}{HTML}{882B21}

\DeclareOption{onecolumn}{\OptionNotUsed}
\DeclareOption{green}{\renewcommand{\headlinecolor}{\color{green}}}
\DeclareOption{red}{\renewcommand{\headlinecolor}{\color{slcolor}}}
\DeclareOption*{\PassOptionsToClass{\CurrentOption}{article}}
\ProcessOptions\relax
\LoadClass[twocolumn]{article}

\renewcommand{\maketitle}{%
    \twocolumn[%
        \fontsize{50}{60}\fontfamily{phv}\fontseries{b}%
        \fontshape{sl}\selectfont\headlinecolor
        \@title
        \medskip
        ]%
}

\renewcommand{\section}{%
    \@startsection
    {section}{1}{0pt}{-1.5ex plus -1ex minus -.2ex}%
    {1ex plus .2ex}{\large\sffamily\slshape\headlinecolor}%
}

\renewcommand{\normalsize}{\fontsize{9}{10}\selectfont}
\setlength{\textwidth}{17.5cm}
\setlength{\textheight}{22cm}
\setcounter{secnumdepth}{0}
```

而这些内容都涉及到Latex具体细节的设置了，例如：字号、字体等等。因此这里就不展开了。

然而More Declaration这一部分才是一个`.cls`文件关键的地方。真正决定长什么样子的地方就在这里，因此我们要学习怎么学`.cls`文件的编写，另外一个重要的内容就是学习如何写More Declaration。

而学习的方法只有一个，就是看别人是怎么写的，从他们写的`.cls`中来学习







## 3. Handling errors

因为我们的类中也会执行代码，因此也会存在代码运行出错的可能，此时我们就需要去处理错误。而Latex并不完全是一个编程语言，因此不存操作try……except……、catch……throw……这类异常处理机制。

为此，Latex中的处理错误的机制其实就是抛出异常，然后让写文章的人自己根据日志来进行修改，因此常用的在日志中报错的命令有：

- `\ClassError{*class-name*}{*error-text*}{*help-text*}`：Error直接中止编译，对应Overleaf中的红色。此外三个参数分别是报错的时候说哪一个类在报错，保存的信息，提供的帮助信息
- `\ClassWarning{*class-name*}{*warning-text*}`：Warning只会警告，但是编译过程并不会停下来，对应Overleaf中的黄色。参数和Error类似
- `\ClassWarningNoLine{*class-name*}{*warning-text*}`：NoLine的Warning效果和上面的warning一样，但是不会报行号
- `\ClassInfo{*class name*}{*info-text*}`：打印出来信息就结束了，对应Overleaf中的蓝色







## 4. Commonly used commands

下面列出来了在class中常用的一些命令

- `\newcommand{*name*}{*definition*}`：上面介绍过了，略。参考[new command](https://www.overleaf.com/learn/latex/Commands%23Defining_a_new_command)，其中有更深入的讲解
- `\renewcommand{}{}`：同样说过了
- `\providecommand{}{}`：和`\newcommand`一样的效果，但是如果命令已经存在，那么这个命令不会覆盖之前的命令，而知直接被无视掉
- `\CheckCommand{}{}`：语法和`\newcommand`一样，但是会先检查一下这个命令是否存在，以及如果存在了时候和`\CheckCommand{}{}`中提供的定义有一样。不一样的话就会产生一个warning。
- `\setlength{}{}`：第一个参数是要展示的内容，第二个参数是长度
- `\mbox{}`：创建一个装着`{}`里内容的盒子。
- `\fbox{}`：和`\mbox{}`一样，但是这个盒子的位置会浮动。效果则类似于`\begin{figure}`环境时候会根据内容自动寻找合适的位置
