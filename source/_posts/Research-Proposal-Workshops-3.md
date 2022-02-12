---
title: Research Skills Workshop 3
date: 2022-01-29 00:47:44
img: https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220129005050717.png
summary: '本文是CCSIT Research Skills Workshop 3的笔记。为了保护版权，本文仅用作我的复习只用，因此并不会向所有人公开,抱歉了哈~'
password: 51cbef0ba330f580de8ebad1ddf5c07d5058d296e16c98d1476d68aff26121ef
categories:
  - Cambridge CCISTC AI Research Programme 2022 Winter
tags:
  - Research Proposal Workshops
  - CCISTC
  - University of Cambridge
---

> 本文是本文是CCSIT Research Skills Workshop 3的笔记。

![Research Skills Workshop 3](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220129005050717.png)



密码：ResearchSkillsWorkshop-3



# Research Proposal Workshops 3

上节课我们介绍了Literature Review是什么以及如何编写Literature Review，本节课则关注研究设计与研究方法、定量研究与定性研究等内容。具体而言：

- What is the difference between research design and research method
- Qualitative v.s. quantitative research
- Primary v.s. secondary research
- Resources to draw on
- How to evaluate your AI model





## 1. Research design v.s. research method

我们在进行科研的时候，需要明白研究设计和研究方法之间的区别。

- Research design：**研究实际是一个计划**。例如我们现在的领域是不确定性在点云的运用。而具体的问题是不确定性能否提高点云分割的准确度。那么我们的计划就是，首先找出来描述点云中的不确定性的方法。然后设计出来把不确定性运用到点云上的方法。接下来设计实验、跑实验、收集数据。最后撰写文章。那么像上面这样的计划就是Research Design
- Research method：**研究方法是用来实现计划的**

一个好的Research Design可以确保我们的实验可以获得我们需要的数据，并且确保我们需要的数据能够回答我们的问题

![research design v.s. research method](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220129154637357.png)





## 2. Types of research method



### A. 如何确定需要使用的研究方式

在进行研究的时候，其实有很多的研究的方法，而决定使用什么研究方法的最重要的方法有：

- **我们需要完成的目的**（最重要的）
- 我们需要什么样的信息
- 我们做的实验
- 如何衡量我们的实验是否成功

![Research Method](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220129183721218.png)





### B. Quantitative Research

定量研究是用数字和图表来表示实验的结果，用来检验或者证实理论和假设。

一般来说常见的定量研究的实验方法包括以数字记录的实验结果和封闭式的问卷调查

![定量研究](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220129184055852.png)





### C. Qualitative Research

定性研究是用语言来表述的、用于理解概念、想法和经验。使用定性研究研究课题可以使我们对难以入门的课题有深入的理解。

而常见的定性研究的方法有：

- 开放式问卷调查
- 词汇描述的现象
- 为探索概念和理论做的文献调研

![定性研究](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220129185117755.png)



### D. Comparison between Quantitative and Qualitative

定性研究方法的优点在于它非常的灵活，在样本数量比较少的时候也适用；而其缺点在于无法通过统计给出来比较准确的关系或者说他人很难复现。而且由于主观性比较强，因此很难做到标准化

定量研究的优点在于可以系统的描述大量的数据，并且其结果是可以复现的。然而其缺点在于需要经过统计学的训练来分析数据，而且需要大量的数据

![两种研究方法的比较](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220130234135619.png)







## 3. Types of data used in research



### A. Primary and secondary

在研究中，根据数据的来源，我们会有两种数据：

- 原始数据，Primary Data：Primary Data指的是我们为了回答研究的问题而收集的数据
- 二手数据，Secondary Data：二手数据是指其他研究人员已经收集的数据，例如政府的人口普查和先前的实验

![Types of data](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220130234713089.png)



### B. Comparisons

一手数据和二手数据的比较如下：

- 原始数据的优点在于他可以回答我们所提出的、特定的研究问题。而且我们自己决定控制取样和测量的方法
- 原始数据的缺点在于他的数据的收集需要我们自己做实验，因此成本高昂而且更加花费时间。需要我们接受过数据收集方面的训练
- 二手数据的优点在于非常容易获得，我们只需要去收集文献即可。然而问题就在于我们无法决定数据是如何得到的，我们也需要额外的处理才能够使用二手数据来回答我们的问题

![Comparison between Primary and Secondary data](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220130234930624.png)





## 4. 科研时的思路

一般来说，在做科研的时候，我们的的思路是下面这样的。在最初的时候我们只有一个idea，脑子里完全不知道该怎么样去做出来这个idea。

然后随着更多的文献综述和阅读，我们对问题的理解越来越深入，对自己的方法也越来越清晰

![科研时的思路](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220131000808458.png)





## 5. 数据集来源

这个没啥用，略掉了

![数据集的来源](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220131001609387.png)







## 6. Evaluation method

衡量模型好坏的metrics，同样没啥用，略掉

![Evaluations](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220131001749459.png)
