---
title: ROS杂篇 ROS中使用Kinect V2 Part1：安装+使用
date: 2021-11-18 13:54:04
img: https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211118214830894.png
summary: 'Kinect是非常流行的获取深度、彩色、点云的相机。本文介绍了Kinect相机的具体性能，以及如何在ROS调用Kinect相机。'
categories:
  - ROS杂篇
tags:
  - ROS
  - Kinect V2
  - Ubuntu
  - melodic
---

> Kinect是非常流行的获取深度、彩色、点云的相机。本文介绍了Kinect相机的具体性能，以及如何在ROS调用Kinect相机。

![最终效果图](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211118214830894.png)

<!-- more -->



# ROS杂篇 ROS中使用Kinect V2 Part1：安装+使用

> 前言：为什么要写这篇博客

由于我目前的工作需要我编写服务机器人的代码，而服务机器人就免不了需要视觉信息来指导机器人完成服务。包括RGB彩色相机获得的彩色图像、深度相机获得的深度图像等等各种视觉传感器。对于简单的RGB相机拍摄到的RGB彩色图像，由于全球统一的接口，因此有很多工具都可以帮助我们从`/dev/video*`中读取到彩色图像。通过OpenCV的`VideoCapture`就可以读到。然而问题的关键在于，针对一些特定的设备，例如深度相机，目前全球尚无统一的标准，因此在数据流中图像的格式各个厂家都是不一样的。因此这个时候想要读取到这些特殊相机的图像就需要花一番功夫了。尤其是在硬件厂商不提供Linux的驱动的时候更难受。

由于我具体的开发平台配置就是 `Ubuntu 18.04` + `ROS Melodic` + `Kinect V2`相机。**所以特地写这篇博客记录一下ROS中如何配置Kinect V2相机**



**下文针对Ubutnu 18.04 + ROS Melodic 验证可行**



## 1. 什么是 Kinect 相机？

> 正如我一向的态度，学什么、做什么前先问问为什么要学/做，再问问自己学/做的是什么。为什么要做上面已经回答了，因为我需要用它。那接下来就是要做的是什么？

个人的角度来说，`Kinect` 相机就是一款特殊的相机，他可以读取到RGB彩色图像和深度图像，通过`Kinect`相机采集到的图像和深度信息，我们就能够快乐的进行开发~

虽然`Kinect V2`在2017年由于`Xbox`取消了`Kinect`的接口而停产，但是发现到`Kinect`相机价值的微软随后推出的 `Kinect V3` 和 `Kinect DK` 都是专门为科研推出的设备。（我有幸用过 `Kinect DK`，上面部署了现成的 `Human Motion Estimation` 的模型，可以直接获得人体的关键点，非常好用）

> 摘自`维基百科`
>
> **Kinect**是由[微软](https://zh.wikipedia.org/wiki/微軟)开发，应用于[Xbox 360](https://zh.wikipedia.org/wiki/Xbox_360)和[Xbox One](https://zh.wikipedia.org/wiki/Xbox_One)主机的周边设备。它让玩家不需要手持或踩踏[控制器](https://zh.wikipedia.org/wiki/控制器)，而是使用[语音指令](https://zh.wikipedia.org/wiki/语音指令)或[手势](https://zh.wikipedia.org/wiki/手势)来操作Xbox 360和Xbox One的系统界面。它也能捕捉玩家全身上下的动作，用身体来进行游戏，带给玩家“免控制器的游戏与娱乐体验”。此设备是[微软研究院](https://zh.wikipedia.org/wiki/微軟研究院)的研究成果之一。
>
> ![维基百科上对Kinect相机的介绍](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211118142428272.png)



可以看得出来，`Kinect`相机最初其实是由Windows为`Xbox`游戏机准备的外设，通过Kinect相机可以玩一些体感游戏。不过多好的设备当然是要我们来折腾的，拿来单纯玩游戏就有点太奢侈了。

不过正式用于Kinect相机是微软家的产品，按照微软的尿性大概是不会给开源驱动的（虽然这几年微软逐渐在拥抱开源，可是KinectV1、V2都是2010年的产品啊），事实也的确如此。Kinect目前微软官方的驱动只有在Windows上才有。这也就是为什么我们稍后等下需要安装`Kinect V2`的驱动。





## 2. Kinect 相机介绍

> 以下内容参考博客：https://www.cnblogs.com/traceplus/p/4136297.html



### A. Kinect V1

2012年美国微软发售的`Kinect V1`，因为可以很方便就能取得Depth（深度）和 skeleton（人物姿势）等信息，被全世界的开发者和研究人员关注。

`Kinect V1`的Depth传感器，采用了`Light Coding`的方式，读取投射的红外线的`pattern`，通过`pattern`的变形来取得Depth的信息。为此，Depth传感器分为投射红外线的IR Projector（左）和读取的IR Camera（右）。此外还有`Kinect V1`中间还搭载了RGB相机。

`Light Coding`是以色列的`PrimeSense`公司的Depth传感器技术，于2013年被美国苹果公司收购。

![Kinect V1的图片](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/012317367015237.jpg)



### B. Kinect V2

`Kinect V2`获得Depth信息采用的则是`Time of Flight(TOF)`的方式，通过从投射的红外线反射后返回的时间来取得Depth信息。红外发射器和接收器在面板底部，因此看不到外观，不过`Color Camera`旁边是红外线`Camera`(左)和投射脉冲变调红外线的`Porjector`（右）。

微软过去收购过基于`TOF`方式的深度传感器技术的公司（注：应该是指的3DV），已经在使用这个技术，不过没有详细的公布。

（额外插一句，TOF是目前大多数点云数据获取的方式）

![Kinect V2](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/012317384052450.jpg)



### C.两者对比

**首先是两者的参数**

![Kinect V1 和 V2 性能对比](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211118144936589.png)



**然后需要注意的是两者的接口**

- V1的要求是USB2.0理论传输速率是60MB/s，v2是USB3.0理论传输速率是500MB/s。
- 对于`Kinect V1`，对XRGB四通道图像，30fps的帧率下每秒所需传输的数据大小为640 x 480 x 4 x 30约为35M；再加上USHORT格式的深度图，30fps，每秒传输数据量为320 x 240 x 2 x 30约为4M。总计约为40MB/s，因为带宽有限，所以在保证画面帧率稳定的情况下，分辨率只能如此，而且基本上必须独占一个USB接口。
- 对于`Kinect V2`的情况，彩色图像1920 x 1080 x 4 x 30 约为237M，深度图像512 x 424 x 2 x 30约为12M，总计约为250M/s。所以非USB3.0不可，否则传输不了这么大的数据量。





## 3. 安装Kinect驱动+ROS中间件

### A. 概述

首先需要说明的是开发的方式，因为Kinect相机是物理硬件，所以获取到的是二进制数据流，我们首先需要使用驱动将二进制数据流转换为有意义的图像，然后在我们自己的程序针在这些图像的基础上去进行开发。

- **将二进制数据流转换为图像的程序对应下图的Kinect Driver**
- **我们自己的程序对应下图的``Application``**
- **需要注意的是，在ROS中通常只会有一个程序调用Kinect Driver，因此不考虑右边的工作模型。右边的模型只有多人、多任务同时要调用Kinect** **Driver时候才会有用，针对单任务其实没有啥影响**

在`Kinect`标准的`Windows`上，`Kinect`的驱动是Windows上直接下载的，然后`Windows`还提供了`Kinect`的SDK，里面有现成的人体骨架识别的`API`，所以可以直接用。**但是在Linux的ROS上想要使用Kinect相机的话，把二进制数据流转换为图像的驱动需要我们自己写，然后SDK+程序得自己开发**。

- 幸运的是，已经有人写好了`Kinect`的驱动，因此我们直接下载驱动即可。但是`Kinect`驱动只能帮助我们能在`Ubuntu`上获得Kinect的图像，所以接下来我们需要做的第二件事就是编写一个`ROS`调用`Kinect`的节点来调用`Kinect`驱动，并将获得的图像以话题的形式发布到`ROS`中去。
- 更加幸运的是，这一步也已经有人帮我们做好了，我们需要需要做的下载、编译这个调用Kinect驱动获得图像、并以`ROS`话题形式发布的`ROS`功能包（相当于中间件）。

![开发方式](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/012317438589516.png)



因此，整个安装流程分成两步，

1. 安装 `Kinect` 的驱动
2. 安装`ROS`功能包



### B. 安装Kinect V2驱动——libfreenect2

首先需要安装`Kinect`在`Ubuntu`上的驱动：`libfreenect2`。

这个驱动是`Github`上开源的用`C++`和`Cmake`写的`Kinect V2`的驱动，所以我们稍后克隆下来之后安装一下依赖，然后配置`CMake`生成`Makefile`、然后make编译，最后make install安装库即可。



#### 1. 安装依赖

实现用apt包管理器安装下稍后编译时候的依赖

```bash
(base) jack@jack-Alienware-m15-R3:~/project$ sudo apt-get install build-essential cmake pkg-config libturbojpeg libjpeg-turbo8-dev mesa-common-dev freeglut3-dev libxrandr-dev libxi-dev libglfw3-dev libglfw3-dev libopenni2-dev libusb-dev libturbojpeg0-dev
```

由于我已经安装过了，所以这一步我不会安装任何东西。

![安装依赖](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211118172852901.png)





#### 2. 下载源码

`git`直接下载源码即可，如果下载的慢的话，要么科学上网，要么先用国内的码云gitee克隆一下，然后再下载码云的仓库。关于如何使用码云加速`github`下载可以参考我的这篇文章（挖个坑，以后补）

```bash
(base) jack@jack-Alienware-m15-R3:~/project$ git clone https://github.com/OpenKinect/libfreenect2.git
```

![下载源码](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211118173143020.png)

下载之后的项目文件结构，可以看出来，是一个非常经典的`CMake`的项目

![下载后的项目结构](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211118173557080.png)



#### 3. 配置&编译

对于一个`CMake`工程，得到项目最后的成果，即最终的动态链接库/共享库、可执行文件一共需要两步。第一步是配置（`Config`），第二步是构建（`Build`）。配置指的是设置项目的一些配置，比如说编译器使用的语法标准、是否禁止编译器在编译阶段优化代码等等。这里不展开讲了，具体内容可以参考我的`CMake`系列文章（挖个坑）。

首先创建一个编译空间

```bash
(base) jack@jack-Alienware-m15-R3:~/project$ cd libfreenect2/
(base) jack@jack-Alienware-m15-R3:~/project/libfreenect2$ mkdir build
(base) jack@jack-Alienware-m15-R3:~/project/libfreenect2$ cd build/
```

接下来再使用`CMake`针对`build`文件夹外的`CMakeLists`进行配置，需要注意的是，由于我已经配好了`Nvidia`的显卡驱动和`CUDA`，因此我指定使用`CUDA`编译，这样运行的时候就会有加速。此外配置的时候要指定安装到的位置，这个安装到的位置稍后安装ROS功能包的时候会用到，所以不建议修改，用默认的就行。

具体来说我们都是在命令行使用-D参数来指定`CMake`的宏及其值(D=define)，`CMakeLists`中会检查这些宏的值。如果你不是`CUDA`的话就把后面指定`CUDA`的宏删掉即可

```bash
(base) jack@jack-Alienware-m15-R3:~/project/libfreenect2/build$ cmake .. -DCMAKE_INSTALL_PREFIX=$HOME/freenect2 -DENABLE_CXX11=ON -DCUDA_PROPAGATE_HOST_FLAGS=off
```

![CMake配置工程](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211118184945046.png)

可以看到`CMake`配置的流程还是很清晰的，首先会去寻找系统中的编译器版本然后再去检查当前编译器是否至此指定版本的语法，接下来去寻找包等等。全部配置过程没有问题的话就会显示最后的三句话，当前`build`编译空间下也多出来很多中间文件。`Makefile`就是稍后`make`解析的对象。

```bash
(base) jack@jack-Alienware-m15-R3:~/project/libfreenect2/build$ ls
```

![配置的结果](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211118185449344.png)

然后我们make编译即可

```bash
(base) jack@jack-Alienware-m15-R3:~/project/libfreenect2/build$ make
```

我这里的编译可以说是顺风顺水，主要原因其实是我已经配置过一遍了，这是写教程时候删掉了全部的文件重头再来的结果。**其实在第一次配的时候在编译这里遇到了两三个问题**，不过我都解决掉了，现在没有办法把这些问题复现出来。这里挖个坑，下一次装机补上。主要的两个问题是第一次会报错不存在一个叫`libGL.so`的共享库，第二次则是在最后编译到100%之后链接的时候报错说没有定义的引用：`undefined reference to _glapi_tls_Current`。

其实这两个问题都和`OpenGL`这个图形库有关（`Graphic Librar`y）。图形负责渲染材质等等，是将数据显示到我们的显示器上的库，包括看到的桌面、窗口等等都是用图形库渲染出来的。而`Kinect`捕获到的数据要转成图片显示，就必然会用到`OpenGL`这个库。第一个报错是因为管道断开（`libGL.so`是一个软连接），重新链接就行，第二个报错则是因为`libGL.so`链接到的共享库版本太老，指定一个更新的即可。

![编译的过程](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211118185715177.png)

最后，由于`libfreenect2`最终生成的是一个驱动，因此他最终生成的结果不仅有可执行文件，还有共享库，我们需要`make install`来安装一下共享库。说是安装，其实就是把共享库复制到指定的位置去，一般的项目都是把共享库复制到系统链接时候的搜索路径，这样编译的时候系统会自动的去搜索。**不过由于我们上面CMake在配置的时候指定了安装位置，因此他其实会安装到我们的家目录下面**

```bash
(base) jack@jack-Alienware-m15-R3:~/project/libfreenect2/build$ make install
```

从命令的输出确实能够看到安装的位置就是我们的家目录

![安装的结果](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211118190908729.png)



#### 4. 设置USB规则

由于`libfreenect2`是一个驱动，因此他需要和底层的硬件做交互，所以需要设置一下`USB`规则。在`Linux`中所有的串口都是由一个叫`udev`的程序负责的，他相当于`Windows`的设备管理器。这里挖个坑，以后补上关于`udev`的内容。更多细节可以参考[这篇简书的博客](https://www.jianshu.com/p/dd6cecd7755a)

> **以下内容引用自简书博客**
>
> **udev在linux的那个位置**
>
> udev的守护进程在linux的位置在systemd中的位置如下所示，举个例子：如果向pc中插入一个usb设备，kernel在总线上发现这个设备，使用dirver初始化，在sysfs创建device目录等操作之后，将通知用户空间的udev，然后上层的显示层才能看到这个usb设备，并最终将它显示在desktop上：
>
> ![Systemd守护进程所处的位置](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/330043-6ecf51d6a519b4fe.png)



我们直接把规则文件复制到`udev`的配置文件夹下即可

```bash
sudo cp ../platform/linux/udev/90-kinect2.rules /etc/udev/rules.d/
```



#### 5. 验证安装

最后上面的步骤一步步做下来，应该是没有问题的，不过在编译驱动的时候我们观察到make最后在生成一个叫做`Protonect`的可执行文件。这个程序其实就是验证安装的程序。我们接上`Kinect`的连接线，然后运行这个程序，如果安装成功就能够看到下面的图片

```bash
(base) jack@jack-Alienware-m15-R3:~/project/libfreenect2/build$ ./bin/Protonect 
```

四张图中左上角是深度图，左下角是RGB图像，右下角是点云的图像，右上角是带有RGB的点云的图像。需要注意的是，点云的图像是通过深度图推算出来的。深度图只有一个Depth通道，数值越大表示越远，看起来就越黑。而点云图是XYZ三个通道，分别表示点相对相机的的XYZ坐标。而右上角的点云则是RGBXYZ六个通道图片。右边看到，有`CUDA`加速喧嚷延迟基本都在10ms左右，速度还是非常快的

![验证程序的运行结果](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211118193300100.png)



#### 6. 安装驱动可能遇到的问题

暂时没有，留待下次重装系统时候补充。







### C. 安装ROS Kinect深度相机功能包——iai_kinect2

就像在`Melodic`上使用`CV_Bridge`一样（挖个`Melodic`编译`CV_Bridge`的坑，以后补上），`iai_kinect2`也需要我们从`github`上下载源码然后编译。

在编译之后，我们想要通过Kinect获得深度图像、点云图像等等直接启动这个`iai_kinect2`中的节点就行。这个节点将会调用libfreenect，并将获取到的图像发布到指定的话题里，因此我们订阅指定的话题即可得到数据。

#### 1. 下载并编译功能包

由于iai-`kinect2`本质上是`ROS`的一个功能包，而`ROS`的功能包的发布都是以功能包的形式发布的，因此我们只需要把这个功能包下载到工作空间中并进行编译即可。为了不污染我们自己的工作空间，避免每次都要编译这个包，我们新创建一个叫做`install_iai_kinect`的工作空间的安装这个包

```bash
(ros) jack@jack-Alienware-m15-R3:~/project$ mkdir install_iai_kinect2
(ros) jack@jack-Alienware-m15-R3:~/project$ cd install_iai_kinect2/
(ros) jack@jack-Alienware-m15-R3:~/project/install_iai_kinect2$ mkdir src
(ros) jack@jack-Alienware-m15-R3:~/project/install_iai_kinect2$ cd src/
(ros) jack@jack-Alienware-m15-R3:~/project/install_iai_kinect2/src$ catkin_init_workspace 
(ros) jack@jack-Alienware-m15-R3:~/project/install_iai_kinect2/src$ cd ..
(ros) jack@jack-Alienware-m15-R3:~/project/install_iai_kinect2$ catkin_make
(ros) jack@jack-Alienware-m15-R3:~/project/install_iai_kinect2$ ll
```

![初始化之后的工作空间](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211118205323865.png)

然后git下载功能并安装依赖，注意这里如果前面改了`libfreenect`的安装路径的话，这里编译的时候要添加一个`CMake`的宏来指定`libfreenect`的路径，参考[文章](https://github.com/code-iai/iai_kinect2#install)。

此外，在使用`rosdep`安装的时候会遇到一个“报错”（见下），这个其实是正常的，因为`kinect2_brideg`、`kinect2_cailbration`、`kinect2_caliration`、`kinect2_viewer`这四个节点被`rosdep`认为是功能包了，但他们其实都只是`iai_kinect2`的一部分。

```bash
(ros) jack@jack-Alienware-m15-R3:~/project/install_iai_kinect2$ cd src/
(ros) jack@jack-Alienware-m15-R3:~/project/install_iai_kinect2/src$ git clone https://github.com/code-iai/iai_kinect2.git
(ros) jack@jack-Alienware-m15-R3:~/project/install_iai_kinect2/src$ cd iai_kinect2
(ros) jack@jack-Alienware-m15-R3:~/project/install_iai_kinect2/src/iai_kinect2$ rosdep install -r --from-paths .
(ros) jack@jack-Alienware-m15-R3:~/project/install_iai_kinect2/src/iai_kinect2$ cd ../../
(ros) jack@jack-Alienware-m15-R3:~/project/install_iai_kinect2$ catkin_make -DCMAKE_BUILD_TYPE="Release"
```

> ERROR: the following packages/stacks could not have their rosdepc keys resolved
> to system dependencies:
> kinect2_bridge: Cannot locate rosdep definition for [kinect2_registration]
> kinect2_calibration: Cannot locate rosdep definition for [kinect2_bridge]
> kinect2_viewer: Cannot locate rosdep definition for [kinect2_bridge]
> iai_kinect2: Cannot locate rosdep definition for [kinect2_registration]

最后没有error就成功安装功能包了，但是不要忘记`source`一下，因为这个功能包不在`ROS`的元功能包中。

![编译的结果](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211118205948236.png)



#### 2. 验证安装

同样，经历过上面的步骤之后，我们脸上`Kinect V2`的连接线来跑跑节点，看看有没有问题。类似于`cv_bridge`，`iai-kinect2`编译之后会得到一个`kinect_bridge`，我们利用其中的launch文件来测试一下。

```bash
(ros) jack@jack-Alienware-m15-R3:~/project/install_iai-kinect$ source devel/setup.bash 
(ros) jack@jack-Alienware-m15-R3:~/project/install_iai-kinect$ roslaunch kinect2_bridge kinect2_bridge.launch
```

![launch启动整个kinect_bridge](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211118204334851.png)

然后我们再开一个终端来看看节点图

```bash
(base) jack@jack-Alienware-m15-R3:~/project/install_iai-kinect$ rqt_graph 
```

看起来还不错，可以看到`kinect_bridge`其实只是一个交互的节点，他的后端数据处理都是依靠的其他的节点，`kinect2`负责调用`libfreenect`驱动，剩下的hd、sd、qhd分别负责处理高中低分辨率的图像。

不过由于rqt是通过rosmaster中的注册信息来画节点图的，因此没有被订阅的话题无法被显示出来，我们看看所有的话题

![Kinect_Bridge节点图](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211118203224904.png)

新开一个终端

```bash
(base) jack@jack-Alienware-m15-R3:~$ rostopic list 
```

可以看到有很多的话题，不过其实所有的话题分成三组，`hd`表示高分辨率，`qhd`表示中分辨率，`sd`表示低分辨率

![Kinect_Bridge发布的所有的画图](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211118203435667.png)

最后其实`kinect_bridge`提供了可视化的节点，不过首先需要对kinect相机进行校准，否则是有偏差的，不过我们这里只是为了验证，因此跑起来看看

```bash
rosrun kinect2_viewer kinect2_viewer kinect2 sd cloud
```

看起来效果还不错，不过由于订阅的是低分辨的图片，因此图片放大之后实际上像素间就会出现一些缺失的黑色点

![可视化的结果](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211118203934128.png)



#### 3. 安装时候的一些说明

> #### 1. 编译的时候弹出来警告
>
> 具体的警告如下图。只是警告而已，不是错误，不用管。因为`catkin`作为`cmake`的上级编译系统，最后还是调用的`g++`，`g++`在编译的是帮我们检查代码，警告我们哪里语法不规范啥的，没必要管，能用就行。
>
> ![编译时候的警告](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211118202235817.png)



#### 4. 安装功能包可能会遇到的问题

> #### 1. **CMake Error at /usr/lib/x86_64-linux-gnu/cmake/Qt5Gui/Qt5GuiConfig.cmake:27 (message)**：Qt5的报错
>
> 具体报错如下图，即缺少了`libEGL.so`这个共享库
>
> ![Qt5报错](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211118200845339.png)
>
> **解决方法：**
>
> 和上面安装驱动时候遇到的报错差不多，估计是一个尿性，即软连接断了，先去`/usr/lib/x86-linux-gnu`下看看。果然，红色的字体，断开的链接。即`/usr/lib/x86_64-linux-gnu/libEGL.so`这个软连接本来指向的文件没了。不过看这个样子有一个1.1.0版本的，估计是某次`apt upgrade`时候更新了这个库。
>
> ```bash
> (base) jack@jack-Alienware-m15-R3:~/project/install_iai-kinect$ ll /usr/lib/x86_64-linux-gnu/libEGL.so* 
> ```
>
> ![libEGL.so断开的软连接](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211118201305342.png)
>
> 那么解决思路就很简单了，我们先删掉原有的断掉的链接，然后让他再指向新版本就行了，毕竟新版本一般都是向后兼容的。除了`Python2`到`Python3`
>
> ```bash
> (base) jack@jack-Alienware-m15-R3:~/project/install_iai-kinect$ sudo rm /usr/lib/x86_64-linux-gnu/libEGL.so 
> (base) jack@jack-Alienware-m15-R3:~/project/install_iai-kinect$ sudo ln -s /usr/lib/x86_64-linux-gnu/libEGL.so.1.1.0 /usr/lib/x86_64-linux-gnu/libEGL.so
> ```
>
> 现在看着就没问题了，管道对了
>
> ![重置libEGL.so的指向](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211118201854870.png)
>
> 然后再进行编译即可
>
> ![功能包可以正常编译](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211118202028422.png)







## 4. 使用kinect_bridge

使用`kinect_bridge`其实特别简单，就是订阅话题而已，为此我们快速写一个`Python`脚本测试，注意测试时候新建的功能包的依赖不要加`iai_kinect2`，因为`kinect_bridge`实现了低耦合性，所有的消息全部都是`ROS`中的标准消息类型，因此没有提供任何ROS编译之后可以引用的库。**下面是是一个错误的示例**。（挖个坑，以后出一个关于`Vscode`开发`ROS`的环境搭建，真的`VScode`越用越爽）

![功能包的依赖](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211118210715487.png)



正因为`kinect_bridge`中的消息全部都是`ROS`的标准消息，所有的图像都是`sensor_msgs/Image`或者`sensor_msgs/CompressedImage`。而点云则是`sensor_msgs/PointCloud2`，我们可以非常快速的写一个测试代码，代码见下（**注意如果你要写C++的话不要在iai_bridge的工作空间中编译，会报错，因为Kinect_Bridge没有提供ROS编译、安装相关的CMake指令**）

关于`ROS`中使用`conda`、还有`cv_bridge`、`ROS`使用`Python`等问题先挖个坑，后面补上文章

```python
#! /home/jack/anaconda3/envs/ros/bin/python

"""rospy 中使用 通过 kinect 获取点云的示例代码 """

"""
@author: Jack Wang
@copyright: Jack Wang
@time: 2021-11-18
"""

import sys
import datetime
import time

import cv2
from colorama import Fore, Style

# 我在 Python3 下编译了cv_bridge，所以要避免import了下载ROS时候一并下载的系统的Python2.7的cv_bridge
for path_idx in range(len(sys.path)):
    if "2.7" in sys.path[path_idx]:
        temp = sys.path.pop(path_idx)
        break
sys.path.append(temp)

import rospy
from sensor_msgs.msg import Image
from cv_bridge import CvBridge


class MyKinectViewer(object):
    bridge = CvBridge()
    today: str = datetime.date.today().__str__()
    start_sec: float = time.time()
    font = cv2.FONT_HERSHEY_DUPLEX
    def __init__(self) -> None:
        super().__init__()
        rospy.loginfo(msg=f"{Fore.GREEN}启动KinectBridge订阅节点！{Style.RESET_ALL}")
        subcriber = rospy.Subscriber(name="/kinect2/sd/image_color_rect", data_class=Image, callback=self.image_cb, queue_size=20)

    def image_cb(self, msg: Image) -> None:
        cv_img = self.bridge.imgmsg_to_cv2(msg)
        cv_img = cv2.putText(cv_img, f"{self.today}, start times: {time.time()-self.start_sec:>.2f}", (10, 380), self.font, 0.8, (0, 255, 0), 2,)
        cv_img = cv2.putText(cv_img, f"-- by Jack Wang", (270, 400), self.font, 0.8, (0, 255, 0), 2,)
        cv2.imshow("kinect_test", cv_img)
        if cv2.waitKey(1) == 27:
            rospy.loginfo(f"{Fore.GREEN}关闭节点{Style.RESET_ALL}")
            rospy.signal_shutdown(reason="pressed esc")


if __name__ == "__main__":
    rospy.init_node(name="kinect_test", anonymous=False)
    mkv = MyKinectViewer()
    rospy.spin()
```

可以看出来，效果还不错

![节点效果图](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211118214830894.png)

![按下ESC键退出节点](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211118214855870.png)



结合我之前写的`YOLO V5`的`ROS`图像处理节点，最后可以实现的效果（`Yolo V5`的`ROS`节点代码后面会发出来）

![image-20211118220436969](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20211118220436969.png)





至此，本文全部结束，我们首先介绍了什么是`Kinect`相机；然后就`Kinect`相机的参数、工作原理进行了介绍；接下来讲解了如何在`Linux(Ubuntu)`上安装`Kinect V2`相机的驱动：`libfreenect2`和安装`ROS`中的`Kinect`相机调用节点；在最后我们给出了一个简单的小例子，来演示如何使用`iai-kinect2`功能包得到的图像数据。

关于iai-kinect2的更多使用教程，请看下一篇文章：`ROS`中`iai-kinect2`功能包的使用。



6200字，码字不易\~，欢迎打赏\~，一起推动开源事业进步



