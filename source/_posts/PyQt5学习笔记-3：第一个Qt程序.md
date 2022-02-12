---
title: PyQt5学习笔记 Chapter 0-Introduction： Part 3.第一个Qt程序
date: 2021-12-20 14:50:22
img: https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211220145356314.png
summary: '本讲作为Introduction的最后一讲，实现了一个简单的Qt的GUI程序'
categories:
  - PyQt5 学习笔记
tags:
  - Python
  - GUI开发
  - PyQt
---

> 本讲作为Introduction的最后一讲，实现了一个简单的Qt的GUI程序

![第一个Qt Gui程序](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211220145356314.png)



# PyQt5学习笔记 Chapter 0-Introduction： Part 3.第一个Qt程序

在Introduction章节中，我们介绍了什么是Qt、PyQt以及Qt中的一些基本概念。本文作为Introduction的终节，展示一个Qt程序。



## 1. Qt程序

上面的Qt GUI程序的代码如下

```python
import sys

from PyQt5.QtWidgets import QApplication, QMainWindow


if __name__ == "__main__":
    app = QApplication(sys.argv)

    w = QMainWindow()
    w.resize(300, 150)
    w.move(300, 300)
    w.setWindowTitle("第一个Qt应用")
    w.show()

    # 运行主循环，循环扫描窗口上的事件，并分发给对应的回调函数
    sys.exit(app.exec_())
```

我们运行之后就会弹出来一个窗口。

![弹出来的窗口](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211220145739135.png)





## 2. 程序解释

在上面，我们写了一个极简的Qt GUI程序，下面我们对他进行讲解。

1. Qt5本身有非常多的功能，例如网络引擎、蓝牙设备管理等等。因此在Qt5中，一个功能就是一个模块。我们这里的QtWidgets模块中包含了所有的Qt常见的功能。例如按钮、窗口、程序等等。
2. 首先正如前面所讲，我们实例化一个QApplication类
3. 接下来我们实例化了一个窗口。并将这个窗口大小设置为300像素宽、150像素高；将窗口锚点（左上角点）移动到距离屏幕左上角宽300像素、高300像素的位置；设置窗口名称为第一个Qt应用；最后显示窗口。
4. 最后我们运行这个程序，并将程序运行的结果（返回值）作为参数传给sys.exit

其实从上面我们能够看到，一个窗口本身并不是整个程序，而是程序的一部分。这也是Qt的设计思想之一，即功能和显示分离。我们目前只是利用一些对象进行了显示，而功能的实现需要稍后面介绍的信号与槽机制。
