---
title: '李宏毅ML2021-Spring-7: Convolutional Neural Network (CNN)'
date: 2022-02-02 09:21:11
img: https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220202093153305.png
summary: '本文是Machine Learning 2021 Spring 第七节课的笔记，本节课主要讲解了Neural Network架构中的Convolutional Neural Network。'
categories:
  - 李宏毅ML2021 Spring Notes
tags:
  - Deep Learning
  - Hungyi Li
  - Machine Learning
  - Neural Network
  - CNN
---

> 本文是Machine Learning 2021 Spring 第七节课的笔记，本节课主要讲解了Neural Network架构中的Convolutional Neural Network。

![第七节课：Conolutional Neural Network (CNN)](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220202093153305.png)







# 李宏毅ML2021-Spring-7: Convolutional Neural Network (CNN)

从本节课开始，就将开始讲解Neural Network的架构，而本节课将讲解Neural Network中一种专门为图像相关任务设计的Neural Network的架构（Architecture）：Convolutional Neural Network。

本节课将会讲解CNN这样的Neural Architecture的设计背后的原理，以及为什么不同的Network的架构可以让网络的结果更好。





## 1. Backgroud: Image Classification

CNN是专门为图像相关的任务设计的Neural Network的架构，因此我们在讲解CNN的时候就需要一个Image的任务来作为Context。

我们最终选择的Context就是Image Classification任务，即图像分类任务。



### 1. Image Classification Basics

图像分类这个任务说的其实就是我们现在给Model一张图片，Model可以告诉我们这个图片里面东西的类别，例如下面我们希望图片在经过Model之后它能够告诉我们这张图片的类别，或者说图片中的物体的类别是猫

![Image Classification](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220202094600208.png)

因为图像是一个二维栅格图，因此图像的大小，或者说分辨率（Resolution）也会有影响，例如对于Lena来说，低分辨率的图片看起来要模糊很多，而在极限情况下，假设分辨率只有3*3，那么很有可能什么我们人类什么都分辨不出来，更何况机器了。

![不同分辨率的Lena](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220202095334195.png)

因此，为了简化问题的讨论，我们假设图像都是一样的size（resolution），都是100*100的。而在实践中，有的人会把图像rescale到同一个大小，而有的人用专门的模块来处理resolution的问题，让网络可以接受不同size的input。

![假设都是100*100的图像](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220202095527322.png)

而分类这件事情我们在前面就已经讲过了，因此对于图像分类而言，其本质也是一个分类任务，因此我们的label取的也是class的one-hot vector，而Model的输出也是one-hot vector。使用的Loss Function是label和output的Cross-Entropy/Negative Log Likelihood（NLL）

![Image Classification](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220202100348906.png)





### 2. Image to vector

我们上面介绍了图像分类的基础知识，那么接下来一个问题就是，我们需要把图像转变为一个可以量化的值。在前面说过，不论是Neural Network也好还是其他的Machine Learning的Algorithm，我们都需要把一个object数值化为一个向量或者一个矩阵。这样我们的Learning Algorithm才可以利用。因此，我们其实也需要把一个图像转换为一个可以量化的值。

对于电脑来说，一张图像是一个3维的tensor。注意，tensor指的是维度大于二维的矩阵。而这三个维度分别是：height、width和channel。而一个pixel的颜色可以用红、绿、蓝三种颜色表示，因此三个channel分别指的就是red、green和blue

![计算机内部一张图像是三个Channel](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220202101626433.png)

然后我们就可以把这张图片的三个Channel的matrix拉成一个vector，然后拼接在一起，就得到了最后的vector。这个vector中每一个Dimension就是对应位置的pixel的表示的光的强度。

![Image to vector](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220202102010737.png)







## 2. Fully Connected Neural Networks?

我们前面已经讲过了Fully Connected Neural Network了，而全连接的网络的input是一个vector，因此我们其实可以直接把表示一个图片的vector丢给全连接网络即可。

我们假设第一层的输出是有1000个Neuron，那么全连接网络的图像如下

![全连接来进行分类](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220202103206035.png)

可是这样做的一个问题就是，因为是全连接，所以input的vector的每一个Dimension都和100个Neuron相连，因此我们计算一下就会发现，这样做的话参数的数量实在是太多了。

而参数多就会带来两个问题（参考前面网络训练攻略）：

- 参数太多的话就会非常难Train，表现出来即在Training Set上loss大且在Testing Set上loss也大，即Optimization失败
- 如果Train出来了，非常容易Overfitting，表现出来就是Training Set上loss小但在Testing Set上loss大

因此，我们自然就会问真的需要使用全连接网络来进行分类么？

![真的有必要使用全连接网络么？](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220202103520204.png)

那么接下来，我们就对图像进行观察，以获得Domain Knowledge，以指导我们进行更好的完成图像得任务





## 3. Observations and Simplifications



### 1. Observation 1: Pattern Recognition

对于图像的任务来说，我们就会想说网络里的每个神经元学的是不是图片中的pattern？

> **什么是模式（Patter）？**
>
> 模式指的就是现实世界（人感知到的世界）或者说人造的设计、抽象思想中的**规律**。所以模式识别指的其实就是规律的识别
>
> ![维基百科上对模式的介绍](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220202105225302.png)

例如对于下面识别鸟的网络来说，有可能有的Neuron负责识别鸟嘴，有的神经元负责眼镜而有的神经元负责鸟爪。

![第一个猜想：网络中的神经元负责识别图像中的Pattern](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220202105414722.png)

那么我们为什么会这样认为呢？因为类比于人类的大脑，我们人在看到了一张图像后对其进行分类、识别的时候也是进行了模式的提取，例如下面的图。

我们第一眼可能会把这张图片认成鸟，为什么呢，因为我们第一眼看到这张图片就会发现鸟嘴和鸟眼，所以就以为它是鸟；直到我们更仔细的观察下，发现了猫的另外一只眼镜和猫嘴、鼻子，所以我们才会明白过来这张图片是一个猫。

那么鸟嘴和鸟眼就是一开始比较明显的、易于抽取的Pattern，而另外一只眼睛和鼻子、嘴都是不太容易抽取的Pattern，我们需要第二眼（更仔细的观察）才能够抽取到。

所以我们就会有上面网络中的神经元，每个学习的其实都是不同的Pattern。

![被当做鸟的猫](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220202110220850.png)

因此，根据这个观察我们就会发现，其实没有必要样一个Nueron看到整个图片，因为反正也是抽取特征，只让一个Neuron看图像中的一小部分就可以了

![只让一个神经元看图像中的一小部分就可以了](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220202111138506.png)





### 2. Simplification 1: Receptive field

根据我们的观察，我们就可以做出来第一个对全连接网络的简化，原来是让一个Neuron看完整个图像，然后再得到一个输出，而现在让一个神经元只看一部分区域，就得到一个输出。这样的相当于这个神经元只会关注图像中的这一小部分，提取该部分中的Pattern。

而我们在图像中取一个小范围的区域，称为Receptive Field。

<img src="https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220202141543963.png" alt="Simplification 1: Receptive Field" style="zoom:67%;" />

然后我们把这个Receptive Field拉成一个向量，丢给一个Neuron，让它计算得到一个output即可。

<img src="https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220202141946110.png" alt="一个Neuron现在只看一个Receptive Field范围的图片" style="zoom:67%;" />

此外，不同的Receptive Field可以是不同的Neuron来计算，例如下图

<img src="https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220202142436527.png" alt="不同的Receptive Field可以是不同的Neuron来计算" style="zoom:67%;" />

而Receptive Field之间也是可以重叠的

<img src="https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220202142538840.png" alt="Receptive Field之间可以是重叠的" style="zoom:80%;" />

即便是用一个Receptive Field，也可以用多个不同的Neuron来计算，因为不同的Neuron提取到的特征是不同的，而一个区域内的特征往往是很多的，因此完全可以用多个Neuron来计算一个Receptive Field的Pattern

<img src="https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220202143227006.png" alt="Receptive Field可以重叠也可以具有多个Neuron" style="zoom: 80%;" />

所以，Receptive Field其实没有那么多的限制，具体的选取都是我们自己来定的，所以我们可能会想：

- 不同的Neuron的Receptive Field大小可不可以不一样？
- Receptive Field可不可以只包含某个/某些channel而不用包含全部的channel？
- 可不可以用不是正方形的Receptive Field？

以上问题的答案都是可以的，我们甚至可以让一张图像上固定只有左上角、右上角、左下角和右下角四个Receptive Field。当然，这样做的效果不一定会比较好。我们得看这些想法背后有没有合理的假设。例如有没有Pattern只是固定的出现在四个角

![image-20220202144310201](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220202144310201.png)



### 3. Classical Receptive Field

虽然我们说Receptive Field的设计是由我们随心所欲的设定的，我们还是介绍一下Receptive Field经典的设置。

第一个设置就是一个Receptive Field包含所有的channel。那么这样的话我们说一个Receptive Field就不用说他的Channel数了，只需要说一个Receptive Field高和宽即可。一个Receptive Field的高和宽称为Kernel Size

<img src="https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220202160837827.png" alt="Setting 1: Kernel Size & All Channel" style="zoom:67%;" />

一般来说，Kernel Size设的都是3\*3的，像7\*7和9\*9这样的Kernel都是比较大的了。可是这样的话我们就会有一个问题，就是有的Pattern是不是一个3\*3的Receptive Field装不下，我们这样做的话就是假设绝大部分Pattern都是在3\*3这个范围内的，而必定有一些大的Pattern会被割裂开。关于这个问题等一下会讲

第二个设置就是同一个Receptive Field会有多个神经元来观察它，例如64个或者128个

<img src="https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220202153935610.png" alt="Setting 2: Miltiple Neuron" style="zoom: 67%;" />

上面的两个setting都是针对一个Receptive Field的，那么针对多个Receptive Field，我们有下面的第三个setting。

我们会把左上角的Receptive Field向右移动一点，然后形成一个新的Receptive Field。而这个移动的距离称为stride。stride也是一个我们自己决定的Hyper-parameter。stride一般不会设置的很大，往往都是1或者2，因为如果取得太大的话，那么会漏掉很多的Pattern

<img src="https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220202154426474.png" alt="Setting 3: Stride" style="zoom:67%;" />

第四个setting是随着滑动到过程中产生的。随着我们Receptive Field向右滑动的时候，有可能会超出边界，即最后一个区域的边长和Kernel Size不相同。

那么这个时候我们就会进行padding。padding指的就是就是给图片的周围补0。补的值也可以是整张图片里所有pixel强度的平均值，或者说用边上的数字来补，这些各种补值的方式都可以，反正只需要最后补完了之后形状是符合计算要求的形状即可。

<img src="https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220202155444485.png" alt="Settring 4: Padding" style="zoom:67%;" />

最后一个Setting就是所有的Receptive Field在一个cover住了整张图片。因为Receptive Field不仅可以横向移动，还可以在竖直方向移动。所以最后图片上的每一个Pixel都会在一个或者多个Receptive Field里面

<img src="https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220202161211514.png" alt="Setting 5: Covering All Pixels" style="zoom: 80%;" />





### 4. Observation 2: Spatial Shifts of Patterns

我们对图片的第二个观察就是，同一个Pattern可能会出现在一张图片的不同的位置，例如鸟嘴这个Pattern

<img src="https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220202161503136.png" alt="同一个Pattern可能出现在不同的位置" style="zoom:67%;" />

按照上面讨论的Classic的CNN的设置，这个现象其实是没有问题的。因为不论是左上角还是中心的鸟嘴，都必定落在一个Receptive Field里，而一个Receptive Field会有多个Neuron来抽取其中的Pattern，因此在左上角的鸟嘴和中间的鸟嘴所在的Receptive Field里，都会有一个Neural来抽取鸟嘴这个Pattern。

<img src="https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220202161955478.png" alt="不同的Receptive Field会有抽取相同/类似Pattern的Neuron" style="zoom:67%;" />

因此同一个Pattern在空间上的Shift其实并不会导致Pattern可能无法被识别出来。可是我们就有一个疑问了，那就真的有必要让每个Receptive Field都有一个抽取类似特征的Neuron么？

<img src="https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220202163119124.png" alt="每个Receptive Field独立的Neuron是否是必须的？" style="zoom: 67%;" />

这样的重复会不会导致参数量太多了，或者说参数存在冗余



### 5. Simplification 2: Parameter Sharing

那么为了解决我们上面观察到的问题，我们就想减少这种参数量的冗余，既减少重复的Neuron。为此，我们就可以让不同的Neuron之间共享参数

<img src="https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220202164517466.png" alt="Simplification 2: 不同的参数之间共享参数" style="zoom:67%;" />

所谓共享参数，意思就是不同的两个Receptive Field共享同一个Neuron的参数。即假设每个Receptive Field都有64个Neuron，那么就让所有的Receptive Field的第32个Neuron的参数保持一样。不过由于输入不同，因此即便是参数相同，最后得到的输出的值也不同

<img src="https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220202164924225.png" alt="Sharing Parameter的计算过程" style="zoom:67%;" />



### 6. Classic Parameter Sharing

类似于Receptive Field，我们下面讲讲一般大家是怎么样进行Parameter Sharing的。

一般来说，每个Receptive Field都会有多个Neuron。

<img src="https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220202165144174.png" alt="每个Receptive Field都有很多的Neuron" style="zoom: 67%;" />

那么传统的Parameter Sharing的方式就是让同一个位置的Neuron之间共享参数。而每一个共用参数的Neuron又称为filter

<img src="https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220202165910476.png" alt="传统的Parameter Sharing的方式" style="zoom: 67%;" />





## 4. Benefits of Convolutional Layer

我们上面已经讲解完了Convolution的计算，我们接下来就讲讲Convolution计算的好处。

首先对于全连接网络来说，他的模型的参数量比较多，因此比较Flexible，学习能力强，但是非常难train，容易Optimization失败，而且train出来也容易Overfitting。

因为参数比较多，因此能够表示的模型的范围很广，因此我们用一个大的Set来表示Fully Connected Layer的Function Set

<img src="https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220202171310903.png" alt="Fully Connected Layer的Function Set" style="zoom:67%;" />

然后接下来，我们首先对Fully Connected Layer做出第一个限制，即让每个Neuron只看一个Receptive Field范围的input，所以减少了参数量，那么对应的可以表达的Function Set就会小很多

<img src="https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220202171737979.png" alt="第一个限制: Receptive Field" style="zoom:67%;" />

接下来我们再进行限制，添加参数共享，让不同的Neuron的参数相同

<img src="https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220202171943159.png" alt="第二个限制: Parameter Sharing" style="zoom: 67%;" />

经过这样的两个限制，我们的全连接层就成为了卷积层

<img src="https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220202183913693.png" alt="卷基层" style="zoom:67%;" />

而参数越少，其实就意味着模型能够表达的Function Set是有限的，而结合我们提出限制的Domain Knowledge，就能直到Convolutional Layer的Model Bias是很大的。那么我们就会问，前面不是说过Model Bias不是是导致Model性能差的原因么？那卷积层现在又Model Bias了，那么该怎么办呢？

其实Model Bias不一定是一件坏事，只有在我们的Model不足以学习到好的Function的时候才是坏事情，而对学习能力强的Model，对其进行限制以使得其学习能力变弱，从而变得更加易于学习，而且如果能够保证最好的Function仍然在我们限制之后的Function Set中的话，那么就意味着我们所引入的Model Bias其实是好的，是有助于我们完成任务的。

![Model Bias帮助网络更加适合某一类任务](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220202185432107.png)







## 5. Another Story: filter

我们上面通过从Model的学习能力的角度出发，介绍了CNN，我们接下来再从另外一个角度出发，介绍另外一种观点下的CNN，即filter视角下的CNN。



### 1. Neuron as filter

在filter视角下看来，网络中的每一个神经元都是一个filter（滤波器）。

所谓过滤指的是让某些物质通过而让某些物质无法通过，因此对于图像而言，这里的filter（过滤器、滤波器）指的是让某些Pattern通过（计算后得到的数值大），而让其他的值无法通过。

因此，在Filter视角看来，每一个神经元就是提取一个指定的特征

<img src="https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220202185721002.png" alt="filter视角小每一个滤波器提取一个特征" style="zoom:67%;" />



### 2. How filter works

那么我们接下来就来看看filter是如何提取特征的。在一开始的时候，网络里的filter的参数都是随机初始化的。我们最后通过Gradient Descent来得到最优的参数。那么假设我们现在已经得到最终的filter，及我们已经知道最终的filter里的参数具体的值。

为了简单起见，我们假设只有一个channel，不过多个channel的情况是一样的。

<img src="https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220202210714239.png" alt="一开始参数的值是未知的，我们假设已经知道了参数的值" style="zoom:67%;" />

那么我们首先把filter放到左上角相乘，然后得到第一个值

<img src="https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220202211354930.png" alt="计算得到第一个值" style="zoom:50%;" />

然后我们把这个filter向右移动一点再相乘得到第二个值，移动的距离称为stride。这里我们的stride设置的是1，所以我们把filter向右移动一格，然后相乘得到第二个值

<img src="https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220202212340045.png" alt="计算得到第二个值" style="zoom:50%;" />

然后我们依次类推，直到计算完第一行所有的值，然后我们把filter向下移动一格，继续计算

<img src="https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220202213244568.png" alt="filter向下移动一格" style="zoom:50%;" />

直到最后，我们让这个filter在整个图像上滑动一次，计算得到了全部的值

<img src="https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220202213420049.png" alt="filter在图像上滑动过一次" style="zoom:50%;" />

因为我们的filter的特点是主对角线都是1，因此我们这样的filter计算的时候，如果图像上对应的区域的也是主对角线上的元素都是1的话，那么计算得到的值就会很大。

所以我们就会说，针对这个filter，图片的左上角和左下角出现了filter 1对应的Pattern

<img src="https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220202214315683.png" alt="filter 1提取对应的Pattern" style="zoom:50%;" />

对于第二个filter，我们进行同样的操作，就得到了第二组数值

<img src="https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220202214902925.png" alt="filter 2提取对于的Pattern" style="zoom:50%;" />

因此，现在对于一张图像来说，我们有几个filter，就会得到几组数字。因为filter是提取的特征（feature），因此这些值也称为feature map

<img src="https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220202215224238.png" alt="一组filter得到一个feature map" style="zoom: 67%;" />



### 3. feature map

所以原始图像在通过一个Convolutional Layer之后，就成了一个有64个Channel的feature map。

<img src="https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220202215518247.png" alt="通过第一层之后得到了64个Channel的通道图" style="zoom: 67%;" />

我们可以把这个64个Channel的feature map视为一张图像，那么我们下面就可以继续接一个Convolutional Layer。

接下来的这个Convolutional Layer，他的filter的形状是3\*3\*64形状的filter

<img src="https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220202221925713.png" alt="第二个Convolutional Layer的filter的形状" style="zoom: 67%;" />





### 4. Larger Receptive field

我们前面说了一个问题，就是我们的filter只有3\*3的大小，会不会不能看到size比较大的Pattern。那么我们现在来回答这个问题。

对于第二层的filter来说，我们假设他的大小也是3\*3，则其计算feature map上的一个3\*3大小的位置，如右下角的图所示。那么现在，对于第二层左上角的这个数值，对应到原始图片上其实是一个3\*3的范围，同理，右下角的这个值对应在原始图像上也是一个3\*3范围。

所以，现在第二层的一个3*3的Receptive Field就表示了原来图像上5\*5范围的区域。因此随着Network层数的加深，同一个filter能够看到的Receptive Field就越来越大。

因此，越深层的filter就能够提取到越高级的feature/Pattern，所以我们不必担心比较大的Pattern提取不到，因为浅层的filter因为Receptive Field的限制确实无法提取，但是深层的filter是可以提取到的。

![越深层的filter的Receptive Field越大](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220202223020505.png)







## 6. Comparison of Two Stories

我们上面讲了两个CNN的故事，即从两个不同的角度出发，解释了CNN的两个基本操作，我们接下来就来比较一下这两个故事。

<img src="https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220202225220472.png" alt="Shared Neuron对应Filter" style="zoom: 67%;" />

我们在第一个故事中，是以固定的Receptive Field、共享的参数来进行解释的，而第二个故事则是固定的filter，在图像上滑动。

那么第一个版本的故事中shared parameter的Neuron其实就是第二个故事中的filter。

其次，我们第一个故事中，相同的Neuron进行Parameter Sharing的过程就对应了filter在图像上进行滑动。

<img src="https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220202225919970.png" alt="Parameter Sharing对应filter滑动" style="zoom: 67%;" />

最后，filter在图像上滑动的过程其实就对应了卷积的计算，我们把积分的式子运用到这里就会发现这里其实就是一个卷积的式子，因此卷积层才称为卷积层

所以，我们上面讲的两个故事，其实就是同一个故事

![They are the same story](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220202230659725.png)







## 7. Pooling

最后，对于CNN来说，前面讲的两个Simplification是必要的，其实还有第三个非常常用的，但是不是必要的Simplification，那就是Pooling。



### 1. Observation 3: Subsampling don't hurt

我们对图像进行的第三个观察就是我们对图像进行下采样的时候并不会改变图像中原有的物体。例如下面我们把所有偶数的列和行去掉，图像成了原有图像的的1/4，但是并没有改变其中的鸟。

而减小图片的大小能够带来的一个显著地好处就是能够降低图片的大小，进而减少需要学习的参数量

![Observation 3](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220202232158498.png)







### 2. Pooling: Max Pooling

Pooling的方式有很多，可以是min pooling、max pooling、mean pooling等等，我们这里用Max Pooling。

Pooling本身只是对图像进行降采样的操作，因此是不包含任何learnable的参数，所以pooling有的时候又被称为operator，类似于Sigmoid、Relu等激活函数一样，都是operator。

假设我们现在通过filter计算得到了feature map之后得到下面的feature map

<img src="https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220202232951918.png" alt="before max pooling" style="zoom:50%;" />

那么具体来说，pooling的操作就是按照2\*2的分组内，取最大的来代表这四个值。这里的分组的大小、Pooling计算值的方式都是看我们自己的。

而Pooling是针对每个Channel内部的，因此Pooling只会改变feature map的height和width，并不会改变Channel的数量

<img src="https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220202233128888.png" alt="Max Pooling" style="zoom:67%;" />



因此，在一个一般的CNN的网络里，我们在Convolution之后接一个Pooling，如下图

<img src="https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220202233620213.png" alt="一般CNN的结构，Convolution之后接Pooling" style="zoom: 67%;" />



最后，Pooling在带来参数量减少的同时，其实还是有可能会给我们模型的最终表现带来伤害的，因为如果我们需要检测非常微小的东西的话，Pooling可能就会把这个东西给丢掉。而Pooling又不是必须的，因此现在有的工作中的网络就是完全抛弃了Pooling，进行全卷积。

而完全抛弃Pooling得益于现在计算能力越来越强，足够支撑我们不用使用Pooling来减少参数量。





## 8. The whole CNN

上面我们讲解完了CNN相比全连接的三个Simplification以及对应的两个层：Convolution和Pooling

那么我们CNN在一开始就是Convolution + Pooling，Convolution + Pooling这样重复几次

<img src="https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220203000246891.png" alt="CNN前半部分" style="zoom: 67%;" />

因为我们现在是分类任务，因此接下来我们把最终的feature map flatten得到一个vector，然后传入分类的全连接和softmax进行分类

<img src="https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220203000727697.png" alt="CNN的完整结构" style="zoom: 67%;" />

这个就是一个经典的CNN的结构，前面用Convolution来提取特征，后面用全连接来进行分类。





## 9. CNN Applications

到这里，我们其实已经讲完了CNN的全部的结构，因此下面我们就来讲讲CNN的运用。





### 1. Playing Go

CNN的第一个运用就是下围棋。

我们说下围棋模型要给出来下一步的位置，因此就是一个19\*19的分类任务。

<img src="https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220203001130320.png" alt="下围棋模型的Framework" style="zoom:67%;" />

那么接下来的任务就是要把棋盘数值化，这个其实也不是很困难，我们完全可以把一个棋盘表示成一个19\*19维的向量。如果这个格子上有黑子，那么就是1，白子则是-1，没有子则是0。因此，一个全连接的网络当然可以用于下象棋

<img src="https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220203001920742.png" alt="全连接网络完全可以胜任下围棋" style="zoom:67%;" />

然而就像我们前面说的，全连接的网络的潜力很大，但是不好train。因此相比于用全连接网络，卷积网络的表现其实更好。

我们把一个棋盘看成一张19\*19分辨率的图片，然后Channel的话可以是单纯的黑白无子的三个channel。不过在AlphaGo中，棋盘是一个48个Channel的图片。这48个channel不仅包括黑白灰的棋子的表示，还有周围是不是有其他颜色的棋子、是否被叫吃等等这些被围棋高手设计出来的standard

<img src="https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220203002713618.png" alt="AlphaGo中使用49个Channel的CNN" style="zoom:67%;" />

我们说CNN是专门为图像识别设计的，而CNN又能够用于下围棋。那么其实就意味着，图像和围棋其实是有共同之处的

具体来说，都有：

- 很多重要的Pattern都是出现在某个局部，例如为围棋里的吃，因此AlphaGo里第一层的Kernel的size取得是5\*5
- 其次，同一个Pattern可能会出现在不同的位置，可以出现在左上角，也可以出现在右下角。

<img src="https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220203002914750.png" alt="图像和围棋有共同之处" style="zoom: 67%;" />

然而需要注意的一个不同就是，对于image来说，我们做pooling的话其实是不会改变object的，可是对棋盘进行Pooling的话就不对了，棋局就变了。

![Pooling用在围棋上会有问题](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220203003305785.png)

那么AlphaGo中到底是则么样的呢？其实AlphaGo中就没有使用Pooling

<img src="https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220203003647760.png" alt="AlphaGo没有用Pooling" style="zoom:80%;" />

所以这就给我们了一个启示，就是神经网络的设计存乎一心。不是别人都用了Pooling我们就一定要用Pooling，我们的结构的设计时一定要看这样设计是不是有利于我们的任务的





### 2. More Application: Speech & Natural Language Processing

除了下围棋以外，CNN还有更多的应用，例如语音识别和自然语言处理。

不过关于这个我们就不深入讲解了，想要了解的话去看Paper即可

![More Application](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220203004244644.png)







## 10. To Learn More

最后，CNN其实有一个问题就是它对于spatial的Transformation是有invariance的，但是如果我们对图片进行rescale的话，CNN就没有办法了。这个其实是非常反直觉的，因为在我们人看来一张图片放大缩小都是一样的。

这个其实也很好理解，对于CNN来说，放缩之后原有的Pattern就变了，因此这个时候就不行了。所以我们为什么说需要使用data augmentation。

不过也有一个特殊的结构可以实现对scale的invariance，即在网络中添加spatial transformer layer即可实现scale invariance。

![To Learn More: Spatial Transformer Layer](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220203004716170.png)
