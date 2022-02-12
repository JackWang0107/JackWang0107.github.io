---
title: Pytorch中使用TensorBoard
date: 2022-02-04 10:45:00
img: https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/tensorboard.gif
summary: '本文记录了如何在Pytorch中使用Tensorboard（备忘录）'
categories:
  - Deep Learning Blogs
tags:
  - Deep Learning
  - Pytorch
  - AI
  - Python
  - TensorBoard
---

> 本文记录了如何在Pytorch中使用Tensorboard（主要是为了备忘）

![TensorBoard的界面](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/tensorboard.gif)



# Pytorch中使用TensorBoard

虽然我本身就会用TensorBoard，但是因为TensorBoard只有在写训练代码的框架的时候才会写，因此实际上写的频率的还是很低的，所以我每次要写训练代码、使用TensorBoard的时候都需要看自己之前写的代码，或者查一下别人写的博客。而且不少博客写的都是一鳞半爪的，不少用法都要查很多博客，久而久之就会觉得很烦。而且很多技巧随着时间的流逝也逐渐的忘记。

因此为了方便以后自己的查询（备忘），同时也是能够留下一个不错的教程，因此决定自己写一个比较全面的TensorBoard的教程。



## 1. Introduction to TensorBoard

在炼丹的时候，经常需要追踪模型在训练过程中性能的变化，例如：Regression任务中的MSE、分类任务中的准确率、生成（图片）任务中图片的生成质量、此外还有合成语音的质量……

大体上来说，所有需要追踪的数据包括：标量（scalar）、图像（image）、统计图（diagram）、视频（video）、音频（audio）、文本（text）、Embedding等等

除了有大量的数据需要追踪外，我们还需要很好的把这些数据显示出来，即数据的写入和显示（读取）要有异步IO，有的时候服务器在学校的机房托管，因此还需要能够通过内网提供可视化……

因此，在种种需求之下，使用一个网页程序来帮助我们进行数据的追踪就成了一个很好的解决方案。具体来说，网页程序实现了前后端的分离，后端只需要专注于数据的记录，而前端专注于数据的显示。此外，网页程序可以进一步扩展，提供网络服务。

因此，就有了TensorBoard这个网页程序实现了我们上面的需求。TensorBoard最早是TensorFlow中的模块，不过现在经过Pytorch团队的努力，TensorBoard已经集成到了Pytorch中。

![TensorFlow官网上的TensorBoard](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220204112804634.png)







> TensorBoard的教程主要分为两部分，一部分是如何使用TensorBoard（即在训练过程中添加数据，然后在浏览器中监视训练的这整个pipeline）的教程，另外一部分是TensorBoard如何添加不同种类数据（即TensorBoard的API）的教程



## 2. TensoBoard Pipeline

上面说道，TensorBoard是分为前段显示和后端数据记录的，因此其Pipeline也分为两步：

- 第一步：后端数据记录
- 第二步：前段查看数据



### A. 后端数据记录

类似于`Flask`和`Django`中把后端程序（服务器）被抽象为了一个类，然而这个类中提供了方法来开启和关闭服务，TensorBoard中也是把后端服务器抽象成了一个类：`SummaryWriter`，不过不同的是，TensorBoard中的`SummaryWriter`类在被声明后就开启了对应的服务，直到我们使用了`SummaryWriter`关闭服务的API。

此外，还有一个不同的之处在于，TensorBoard的前段数据显示和后端数据记录是`异步I/O`的，即后端程序（`SummaryWriter`类的实例）将数据写入到一个文件中，而前端程序读取文件中的数据来进行显示。因此后端所谓的服务指的就是数据的记录，而非提供前端的显示。数据记录的实现方式即通过`SummaryWriter`类中的方法

然后在开启了后端程序的服务器之后，我们就可以通过各种API来添加数据了



#### 0. 导入包

我们首先导入包

```python
import torch
from torch.utils.tensorboard import SummaryWriter
```



#### 1. SummaryWriter类

`SummaryWriter`声明之后就会开启后端数据记录的服务，因此在实例化该类的时候我们就需要保存数据的位置。声明保存数据的位置有好几种方式

`SummaryWriter`的签名如下：

```python
class torch.utils.tensorboard.writer.SummaryWriter(log_dir=None, comment='', purge_step=None, max_queue=10, flush_secs=120, filename_suffix='')
```

其中：

- `log_dir` (str)：指定了数据保存的文件夹的位置，如果该文件夹不存在则会创建一个出来。如果没有指定的话，默认的保存的文件夹是`./runs/现在的时间_主机名`，例如：`Feb04_22-42-47_Alienware`，因此每次运行之后都会创建一个新的文件夹。在写论文的时候我们会涉及一系列实验，从不同的角度来说明一些问题，例如我们的假设是否正确、模型性能是否更好……因此最好不要用默认的实现来直接作为存放数据的文件夹，而是使用具有含义的二级结构，例如：`runs/exp1`。这样的话，所有的实验1的数据都在这个文件夹下，这样我们就可以方便的进行比较。
- `comment` (string)：给默认的`log_dir`添加的后缀，如果我们已经指定了`log_dir`具体的值，那么这个参数就不会有任何的效果
- `purge_step` (int)：TensorBoard在记录数据的时候有可能会崩溃，例如在某一个epoch中，进行到第$T+X$个step的时候由于各种原因（内存溢出）导致崩溃，那么当服务重启之后，就会从$T$个step重新开始将数据写入文件，而中间的$X$，即`purge_step`指定的step内的数据都被被丢弃。
- `max_queue` (int)：在记录数据的时候，在内存中开的队列的长度，当队列慢了之后就会把数据写入磁盘（文件）中。
- `flush_secs` (int)：以秒为单位的写入磁盘的间隔，默认是120秒，即两分钟。
- `filename_suffix` (string)：添加到`log_dir`中每个文件的后缀。更多文件名称设置要参考`tensorboard.summary.writer.event_file_writer.EventFileWriter`类。

因此，一个成熟的数据记录方式就是在`runs`文件夹下按照一定的意义来划分二级文件夹，例如`网络结构1`、`网络结构2`、`实验1`、`实验2`等等。





#### 2. 添加数据

想后端服务程序添加数据使用的是`SummaryWriter`类中的一系列方法，这些方法都以`add_`开头，例如：`add_scalar`、`add_scalars`、`add_image`……具体来说，所有的方法有：

```python
import pprint
pprint.pprint([i for i in SummaryWriter.__dict__.keys() if i.startwith("add_")])
```

- add_hparams，add_scalar，add_scalars，add_histogram，add_histogram_raw，add_image，add_images，add_image_with_boxes，add_figure，add_video，add_audio，add_text，add_onnx_graph，add_graph，add_embedding，add_pr_curve，add_pr_curve_raw，add_custom_scalars_multilinechart，add_custom_scalars_marginchart，add_custom_scalars，add_mesh

后面在第二部分会详细的讲解每个方法，这里先讲共性。

<img src="https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220205000043632.png" alt="SummaryWriter中所有添加数据的API" style="zoom:67%;" />

每个方法根据需要添加的数据的不同，方法中具体的参数也不同，但是所有的方法终归都是要添加数据的，因此会存在相同的参数。具体来说，相同的参数包括：

- `tag` (str)：用于给数据进行分类的标签，标签中可以包含父级和子级标签。例如给训练的loss以`loss/train`的tag，而给验证以`loss/val`的tag，这样的话，最终的效果就是训练的loss和验证的loss都被分到了`loss`这个父级标签下。而`train`和`val`则是具体用于区分两个参数的标识符（identifier）。例如我们现在有两个tag，`cos/dense`和`cos/sparse`，那么最终展示下来的效果是这样的。此外，**只支持二级标签**

  ![二级标签可视化后的效果](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220205003612607.png)

- `globa_step` (int)：首先，每个epoch中我们都会更新固定的step。因此，在一个数据被加入的时候，有两种step，第一种step是数据被加入时当前epoch已经进行了多少个step，第二种step是数据被加入时候，累计（包括之前的epoch）已经进行了多少个step。而考虑到我们在绘图的时候往往是需要观察所有的step下的数据的变化，因此`global_step`指的就是当前数据被加入的时候已经计算了多少个step。计算`global_step`的步骤很简单，就是$global\_step=epoch * len(dataloader) + current\_step$

- `wlltime` (int)：从`SummaryWriter`实例化开始到当前数据被加入时候所经历时间（以秒计算），默认是使用`time.time()`来自动计算的，当然我们也可以指定这个参数来进行修改。这个参数一般不改



以添加标量（add_scalar）为例，演示一下添加数据的方法的用法。其他的方法第二部分会讲

```python
writer = SummaryWriter()
for epoch in range(n_epoch := 10):
    for step in range(total_step := 100):
        # 训练代码
        # ...
        # ...

        # 计算 loss
        loss = np.sin(step * 0.01)

        # 添加标量
        writer.add_scalar(tag="loss/train", scalar_value=loss,
                          global_step=epoch * total_step + step)
```

然后可以看到的效果如下：

![添加数据的效果](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220205010449745.png)







#### 3. 关闭SummaryWriter

我们刚才说过，SummaryWriter这样的后端程序在被实例化出来就自动开启了数据记录的服务，而我们在完成了所有的数据记录只有，需要关闭服务。

关闭服务很简单，就是直接调用`close`方法即可

```python
writer.close()
```





#### 4. Summary

最终，总结一下整个后端数据记录的流程，其实就三步：

- 实例化`SummaryWriter`类，同时指定数据保存的文件夹
- 利用`SummaryWriter`类提供的方法，添加不同类型的的数据
- 关闭`SummaryWriter`类，中止服务





### B. 前端显示数据

因为TensorBoard是异步I/O的网页服务程序，因此后端程序在把数据写入到文件的时候，前端程序可以读取数据来进行显示。

具体来说，后端数据记录程序会把所有的数据记录到同一个文件夹下的一个文件内。**因此，前端显示程序在启动的时候需要指定读取的文件夹**。多次实验就会产生多个文件，我们通过显示这个文件夹，就可以很方便的来进行多个实验的比较



#### 1. 默认使用

前端显示程序提供了CLI（命令行）界面，因此我们直接在命令行启动就行了

```shell
tensorboard --logdir=数据文件夹
```

其中数据文件夹就是在声明SummaryWriter时候指定的文件夹。

例如：

```shell
tensorboard --logdir=./Feb05_01-00-48_Alienware/
```

而在我们启动前端显示程序之后，就会得到一个端口，访问这个端口就能看到显示的效果

<img src="https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220205012353764.png" alt="命令行启动tensorboard后会看到程序启动的端口" style="zoom: 67%;" />

访问该端口就能看到程序

<img src="https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220205012630314.png" alt="浏览器中访问就能看到效果" style="zoom:67%;" />





#### 2. 修改端口

有的时候，在服务器上训练模型的时候为了避免和别人的TensorBoard的端口撞了，我们需要指定新的端口。或者有的时候我们在docker容器里跑TensorBoard，我们通过一个端口映射到主机上去，这个时候就需要指定TensorBoard使用特定的端口。

具体来说就是通过CLI的`--port`参数

```shell
tensorboard --logdir=数据文件夹 --port=端口
```

例如我们现在指定上面的例子端口为10000

```shell
tensorboard --logdir=./Feb05_01-00-48_Alienware/ --port=10000
```

![修改后的端口](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220205013842301.png)







### C. Summary

最后，总结一下使用TensorFlow的Pipeline，首先在训练的过程中使用SummaryWriter来记录数据，记录的过程中需要注意文件夹需要来合理的划分。

然后我们在前端查看的时候，运行`tensorboard`的CLI程序即可，一般用的最多的就是`--log_dir`和`--port`两个参数。

此外，如果是服务器上的话，那么tensorboard的CLI运行在服务器上，然后在自己的电脑上，利用浏览器，通过内网来查看训练过程。











## 3. SummaryWriter APIs

上面讲完了SummaryWriter的Workflow/Pipeline，剩下的就是SummaryWriter添加数据的API的讲解了。关于这些API的话，正如上面介绍的，他们都以`add_`开头，具体有：

- 标量类：`add_scalar`、`add_scalars`、`add_custom_scalars`、`add_custom_scalars_marginchart`、`add_custom_scalars_multilinechart`、
- 数据显示类：
  - 图像：`add_image`、`add_images`、`add_image_with_boxes`、`add_figure`
  - 视频：`add_video`
  - 音频：`add_audio`
  - 文本：`add_text`
  - Embedding：`add_embedding`
  - 点云：`add_mesh`
- 统计图：`add_histogram`、`add_histogram_raw`、`add_pr_curve`、add_pr_curve_raw
- 网络图：`add_onnx_graph`、`add_graph`
- 超参数图：`add_hparams`

因为我目前主要在做CV、点云和NLP，对于语音、视频设计的比较少，因此关于这些API以后用到了我再慢慢补充。

其实主要就是对官网上的翻译，可以直接看官网上的[介绍](https://pytorch.org/docs/stable/tensorboard.html)：https://pytorch.org/docs/stable/tensorboard.html





### 1. add_scalar

`add_scalar`主要用于添加一个标量。其签名为

```python
def add_scalar(tag, scalar_value, global_step=None, walltime=None, new_style=False, double_precision=False)
```

其中：

- `tag`、`global_step`、`walltime`是前面讲过的，这里不再细讲
- `scalar_value` (float or string/blobname) ：是要保存的值
- `new_style` (boolean)：是否使用新的内存格式，即把值保存为tensor的形式。新的格式读取速度会快一点
- `double_precision`(boolean)：是否使用双精度(double)来保存每个值

例如：

```python
with SummaryWriter(log_dir="./runs/add_scalar") as writer:
    for epoch in range(n_epoch := 200):
        for step in range(total_len := 1000):
            writer.add_scalar(tag="sin/1", scalar_value=np.sin(step),
                            global_step=epoch * total_len + step)
```

效果如下

![add_scalar徐爱公益](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220205105158529.png)







### 2. add_hparams

`add_hparams`用于添加超参数列表，主要用于调参。

```shell
def add_hparams(hparam_dict, metric_dict, hparam_domain_discrete=None, run_name=None)
```

其中：

- `hparam_dict` (dict)：字典中的每个键都是一个超参数的名字，而对应的值就是该超参数的值。参数的值可以是bool、string、float、int或者None
- `metric_dict` (dict)：字典中的每个键都是衡量标准的名字，对应的值就是该衡量标准的值。注意，`metric_dict`字典中的键要在tensorboard所有的记录（`tag`、其他的dict的键）中唯一。
- `hparam_domain_discrete` (Optional[Dict[str, List[Any]]])：有的超参数只是一个值，而有的超参数在训练过程中是动态变化的，因此对于这些动态变化的超参数，使用该参数来进行传递，其中储存了动态变化的超参数的名字和他们的值的字典
- `run_name` (str)：本次运行时候的名称，如果没有指定的话就默认用现在的时间

一般来说有特殊需求的话不用指定第三个参数，第四个看个人

例子如下：

```python
import torch
from torch.utils.tensorboard import SummaryWriter



with SummaryWriter(log_dir="./runs/add_hparams") as writer:
    # train codes
    # ...
    # ...
    # ...

    # add hparams
    writer.add_hparams(
        hparam_dict={
            "lr": 5e-4,
            "batch_size": 64,
            "optimizer": "SGD",
            "weight_decay": 1e-5,
        },
        metric_dict={
            "accuracy": 0.76,
            "cross-entropy": 0.01,
        }
    )
```

显示的效果如下：

![hparam的效果](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220205120855376.png)

需要注意的是，我们多个实验就会在同一个文件夹下面得到多个数据，例如我们改一下上面的参数，再跑一遍，生成新的数据。然后刷新一下tensorboard就可以看到第二次实验的数据。

![第二组实验](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220205132631667.png)

