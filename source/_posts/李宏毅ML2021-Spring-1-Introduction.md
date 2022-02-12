---
title: '李宏毅ML2021-Spring-1: Introduction'
date: 2022-01-09 10:58:28
img: https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220109122555074.png
summary: '本文是李宏毅Machine Learning 2021 Spring 第一节课Introduction的笔记，记录了课程相关的要求'
mathjax: true
categories:
  - 李宏毅ML2021 Spring Notes
tags:
  - Deep Learning
  - Hungyi Li
  - Machine Learning
  - Neural Network
---

> 本文是李宏毅Machine Learning 2021 Spring 第一节课Introduction的笔记，记录了课程相关的要求

![第一节课：Introduction](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220109122555074.png)







# 李宏毅ML2021-Spring-1: Introduction

李宏毅老师2021年的Machine Learning课程是完全可以通过线上上课的方式来学习的，因为所有的资料，包括课程、作业等等都可以通过线上的方式来完成。因此我们通过线上上课完全可以实现在教室里同等的听课效果



## 1. Prerequisite

2021年春季的课程没有考试也没有前置的要求、先修课等要求。

<img src="https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220109123003194.png" alt="课程没有先修课的要求" style="zoom: 33%;" />

虽然这样说，但是其实还是需要一些基础的能力，因为想要听懂、想要能够完成作业这些能力都是必须的：

- **数学知识**：
  - 微积分
  - 线性代数
  - 概率论
- **编程能力**：
  - 所有的作业都是基于Python的，并且所有的作业都有助教提供的例程。其实只需要运行助教的例程就能够完成作业。但是想要高分的话还是需要自己能够写
  - 所有的作业都会用到Pytorch这个框架
- **硬件要求**：
  - 除了前几个作业，后面的作业是需要真实训练模型的，因此会有硬件（GPU）的要求。但是所有的作业都可以通过谷歌的Colab云平台来完成，因此硬件其实不是问题。当然我自己是有机器的，因此就懒得花费时间去学习Google的colab了

<img src="https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220109123452250.png" alt="要求具有的出能力" style="zoom:50%;" />





## 2. Orientation

![课程的方向](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220109124302620.png)

- 虽然名字叫做Machine Learning，但是2021年Spring的课程完全关注于Deep Learning。
  - 虽然在知识的划分上，DL是ML中的进阶知识。然而其实在李宏毅老师的讲解下，并不需要要ML的基础知识就能够听懂。
  - 因此，本课程其实可以作为机器学习的第一堂课，从中我们可以学习到机器学习的基础知识，熟悉机器学习
  - 此外，这门课和林轩田老师的机器学习技法的课程的重叠会很少，因为林老师的课程主要关注于传统的机器学习，只在最后会将到关于深度学习的内容
- 此外，本课程还会讲解机器学习中最新、最前沿的技术。
  - 本课程会从最基础的知识讲到最新的知识

因此，在学完这门课程之后，完全可以：

- 继续深入的学习机器学习的相关知识
- 将学习到的知识用于其他领域

![写完课程后可以干的事](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220109125213775.png)





## 3. Assignment

本课程作业有两种形式：

- 多选题：多选题的结果需要通过国立台湾大学的NTU COOL网站来提交
- 编程题（排行榜）：在kaggle或者JudgeBoi上根据模型性能排名的排行榜

最后，关于编程题的代码需要在NTU COOL上提交。

![课程的作业](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220109125410367.png)









## 4. Grading Criterion

![成绩的评判](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220109125727983.png)

首先是关于作业分数的评判：

- 课程一共有15次作业，每个作业都有十分。只会记录15个作业中的10个最高的来记录分数。
  - PS：在老师看来，想要涵盖机器学习（深度学习）所有内容至少需要15个作业
- 不强制要求完成所有的作业，选择自己感兴趣的作业完成即可
- 非常鼓励学生完成所有的15个作业
  - PS：15个作业全部完成老师会送一件T恤:laughing:

下面是所有作业的时间表

![作业安排](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220109130152524.png)



最后课程成绩的评判如下：

- C-：这门课不会挂人，因此单纯的跑完了助教的例程就可以及格。
- A-：根据每个作业的提示对助教的例程进行改进、提升，完成这些作业就可以是A-，大多数同学都是A-。
- A+：每个作业其实都设置了一些难关，如果克服了他们就可以得到A+，而完成这些挑战需要通过自己的思考以及阅读文章

![课程成绩的评判](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220109130415174.png)







## 5. Rules

![基础的规则](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220109132756818.png)

课程的一些规定如下：

- 不允许抄袭别人的代码或者直接上传别人的代码
  - 别人包括宇宙中所有的生物
  - 修改别人的代码里的变量名也是没有用的
- 不要让把自己的代码分享给别人。
  - 如果分享代码、结果给别人那么自己也会受到同样的惩罚

此外，因为部分作业是在Kaggle上提交的，因此对于Kaggle也会有一些规定：

- 每天的上传次数是有限制的
  - 不允许使用多个账号
  - 不要相互借账号
  - 因为有的作业在之前的几个学期也有布置，因此不要提交之前学习公开的代码
  - 不要试图通过任何方式提高提交的次数
- 所有的结果应该是由机器产生的
  - 不要试图手动打label的方式得到答案
- 作业中使用的数据集都是开源的。因此他们的测试集和训练集都是公开的。因此不要把测试集用于训练
  - 此外包括但不仅仅是一下行为是禁止的：
    - 把测试数据加入到训练集中
    - 使用测试数据来调参
  - 注意
    - 不要使用其他的第三方数据，例如在imagenet上训练好了跑到mnist上finetune
    - 只使用作业中提供的数据集

![Kaggle的规则-1](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220109133332643.png)

![Kaggle的规则-2](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220109133527767.png)



最后是关于代码的一些规则

- 作业的代码需要上传到NTU COOL上

- 上传的代码应该能够产生和你在leaderboard上的结果相近的模型
  - 如果没法产生，那么就会被视为作弊
  - 助教会抽查代码、运行检查
  - 如果你的作业拿到了10分，那么就会向全班展示
  - 助教和老师有权判断是否作弊

![代码的规则](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220109134233249.png)





## 6. Punishment

如果违反了上面的规则，那么乘法如下：

- 第一次违反规则，那么期末总成绩打九折
- 第二次违反规则，直接零分，下学期再见

![惩罚](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220109134637455.png)





## 7. Information

最后是关于课程的一些信息

![课程的主页](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220109134751615.png)
