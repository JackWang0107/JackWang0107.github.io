---
title: ROS杂篇 Melodic下Python3中使用CV_Bridge与TF
date: 2021-11-23 12:50:40
summary: 'Melodic版本中Python3下使用这两个包却会报错，本文将解决这Python3CV_Bridge与TF的使用问题。'
categories:
  - ROS杂篇
tags:
  - ROS
  - cv_bridge
  - Ubuntu
  - melodic
  - OpenCv
  - tf2
  - sensor_msgs
  - roslaunch
---

>TF与CV_Bridge是ROS中非常好用的工具，前者用于帮助我们进行坐标转换，后者帮助我们在ROS中使用OpenCV。然而在Melodic版本中Python3下使用这两个包却会报错，本文将解决这Python3CV_Bridge与TF的使用问题。





<!-- more -->





# ROS杂篇 Melodic下Python3中使用CV_Bridge与TF

> 前言：为什么要写这篇博客

在使用ROS驱动机器人的时候，经常遇到的一个问题就是我们需要使用摄像头来获得视觉信息，并且在此基础上，指导机器人进行操作。



## Updates ! ! !

由于最近一个周我的工作变动，目前需要在Ubuntu 20.04 LTS进行ROS开发，20.04中TF和CV_Bridge默认都支持Python3，因此目前我不需要解决这个问题。这篇文章以后更完
