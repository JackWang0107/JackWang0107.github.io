---
title: Latex基础：Overleaf本地安装
date: 2022-01-17 09:22:26
img: https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/screenshot.png
summary: '本文主要介绍了如何在本地搭建一个Overleaf写作环境'
categories:
  - Latex基础
tags:
  - Latex
  - Overleaf
  - paper writing
---

> 本文主要介绍了如何在本地搭建一个Overleaf写作环境

![最终效果图](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/screenshot.png)



# Latex基础：Overleaf本地安装



## 1. 前言

对于我个人而言，我经常用Markdown拿来快速的记录文档、书写内容，因为对格式要求不高的时候，使用Markdown记录文件非常方便。

可是当需要进行论文写作的时候，由于需要对三线表、图片的Caption、位置、大小、交叉引用等等因素进行设置，此外还需要进行参考文献的引用等等。此时Markdown就显得力不从心了，我们需要用更加强大的Latex排版系统。关于Latex的介绍，请看我的其他文章，本文不会进行介绍。

![OveaLeaf的控制台](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220117143121996.png)

然而，现在很多的Latex编辑器没有做到开箱即用，最开始使用Latex的时候经常一编译就是一堆bug，此外由于Latex诞生的很早，不像Python一样有pip、conda这样好用的包管理器，Latex包的本地安装繁琐而且复杂。上面这些因素让本来简单、便捷的Latex变成了需要折腾的工具，让人望而却步。

直到后来，遇到了Overleaf。

Overleaf是开源的在线Latex编辑器软件，个人用户可以在Overleaf官网注册并免费使用Overleaf，Overleaf官网还具有Review等团队协作功能。但是Overleaf官方提供的网站在国内的访问速度不佳，**科学上网后速度才满足日常需求**。

![Overleaf的Github项目主页](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220117143845940.png)

而对于我来说，平时使用的是校园网，科学上网之后速度确实提升上去了，但是会时不时的掉线、重连，非常烦人。因此我决定在本地搭建一个Overleaf的环境出来方便我自己的写作

因此，对于科研团队来说，在自己的服务器上部署Overleaf，从此为整个团队都省去了安装Latex各种包的繁琐，多么幸福的事。需要说明的事，目前开源的个人版本的Overleaf功能没有Overleaf官网齐全，也许还有些小bug，但是就我目前的使用来说，足够日常使用了。



## 2. Overleaf本地安装

就像在前面所说的，Overleaf本身是一个开源的项目，Overleaf官方则是在Overleaf的基础上提供了团队协作、云文件存储等云服务功能。因此我们在本地完全可以搭建出来一个Overleaf环境出来。只不过我们自己搭建的Overleaf只能给自己用，没有办法和别人协作。



Overleaf程序本身是运行在docker里的，然后通过docker的端口映射提供了浏览器的localhost访问。因此首先需要安装docker



### 1. Docker安装

本地机器首先卸载一下老版本的docker

```shell
sudo apt-get remove docker docker-engine docker.io containerd runc
```

然后从apt的仓库里安装依赖

```shell
sudo apt-get update
sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
```

接下来添加Docker官方的GPG Key

```shell
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

设定下载稳定版本的Docker

```shell
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

然后安装即可

```shell
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose
```

![docker安装过程](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220117145809738.png)

安装完了之后运行一下helloword验证一下安装

```shell
sudo docker run hello-world
```

![验证的结果](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220117145921325.png)

出现上面的信息就表示安装成功





### 2. 把当前用户加入到docker组里

在安装完docker后，默认会创建一个docker组，只有组内的用户才可以有权限使用docker的命令，因此还需要把当前用户加入到docker组里去

```shell
sudo tail -n 5 /etc/group
```

![可以看到新创建的docker组](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220117151049856.png)

把当前登录用户加入到docker组里去

```shell
sudo gpasswd -a $USER docker
# 更新一下docker组的信息
newgrp docker
```

然后我们不用sudo运行一下上面的docker命令看看

```shell
docker run hello-world
```

![运行结果](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220117151603677.png)





### 3. 安装以及配置Overleaf项目

Overleaf项目被打包成了docker的一个镜像，我们把Overleaf的镜像从dockerhub里拉下来然后在本地创建容器运行即可

```shell
docker pull sharelatex/sharelatex
```

因为docker的文件系统是分层的，因此等待一段时间等所有层下载完之后就ok了

![正在拉去镜像](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220117153754354.png)



接下来拉取一下官方的Overleaf配置文件

```shell
cd ~/projects
mkdir overleaf
cd overleaf
wget https://raw.githubusercontent.com/sharelatex/sharelatex/master/docker-compose.yml
```

我们下载配置文件后，可以根据需要修改一下启动Overleaf的容器的时候的配置，例如：因为默认的启动端口是80往往有Apache或者Nginx，因此把Overleaf容器的80端口改到本机的其他端口，例如改到本机的8000上去

```shell
sudo vim docker-compose.yml
```

![修改容器的端口映射](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220117154606461.png)





### 4. 初次启动Overleaf容器以及texlive安装

启动Overleaf需要在配置文件在的文件夹下启动。

```shell
docker-compose up -d
```

初次启动需要下载一下数据库之类的东西

![等待下载](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220117155148568.png)

然后进入容器即可

```shell
docker exec -it sharelatex bash
```

注意进入容器后命令行的提示符会发生改变

![进入容器](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220117155315366.png)



接下来在容器内安装编译所需要的完整的texlive，包含大概4000个常用包。考虑到速度可能会比较慢，所以走清华源

```shell
# 换源
tlmgr option repository https://mirrors.tuna.tsinghua.edu.cn/CTAN/systems/texlive/tlnet/
# 更新
tlmgr update --self --all
# 下载包
tlmgr install scheme-full 
```

<img src="https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220117155828288.png" alt="4183个包，慢慢等吧" style="zoom: 67%;" />



最后，等了一个半小时左右，总算是把4138个包全部下载好了

![下载成功的结果](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220117173418961.png)



这里下载好了之后，我们对下载了包的镜像进行一下提交，这样我们后面换了机器之后也可以随时部署，不需要再去下载一遍这些包

```shell
docker commit sharelatex sharelatex/sharelatex:with-texlive-full
```





## 3. Overleaf的使用

接下来我们直接访问`http://ip:端口/`就可以使用Overleaf了，不过在此之前，我们先创建一下管理员账户



### 1. 创建管理员账户

管理员（后台）登录在`http://ip:端口/launchpad`

![后台登录](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220117175418366.png)

我们注册之后就可以进入后台的控制面板了

![后台的控制面板](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220117175758402.png)







### 2. 普通用户登录

普通用户登录直接访问`http://ip:端口`即可，会自动重定向到`http://ip:端口/login`界面，然后register 或者 login即可

![登录界面](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220117180153749.png)



登录之后就可以正常使用了

![熟悉的感觉又回来了](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220117180311035.png)





最后写作的体验就是很流畅~，完全不会断线，编译、展示速度快到飞起！



开始快乐的写作:tada::tada::tada:





## 4. 后记

下面是我在后面的日常使用中发现的一些问题、对应的解决方案以及一些建议



### 1. 关于模板

我们在新建项目的时候，本地是没有网页版的template中的各种模板可以用的，所以这个时候我们可以先从官网上下载下来temple然后在本地上传即可

![从本地上传新项目](https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220117180513493.png)





### 2. 关于镜像

我们前面在下载好了各种latex的包之后，通过`docker commit`命令把我们的修改提交到了镜像上。然而有的时候我们可能需要在新的机器上，例如课题组的服务器上跑一个私有Overleaf。那么这个时候我们就需要把下载好包的镜像复制到服务器上。所以我们还需要把镜像打包出来方便迁移。

当然，关于docker更多的内容，这里不会介绍，在我的后面的docker专栏会有所介绍

#### A. 打包镜像

因为docker的文件系统是分层存储的，而直接以分层文件形式迁移镜像的话就很不方便，所以我们需要首先把镜像打包为一个tarball包

```shell
docker save -o ./OverLeaf.tar sharelatex/sharelatex:with-texlive-full
```

打包时间会比较长，因为下载了所有的包之后会比较大高达7.6G，打包之后就会得到一个镜像，未来我们只需要把这个镜像复制到目标计算机上然后完成下面的步骤就可以了

<img src="https://jack-1307599355.cos.ap-shanghai.myqcloud.com/img/image-20220118192015987.png" alt="打包之后的文件非常大"  />



#### B. 导入镜像

我们上面是通过docker把分层的image打包为了tarball文件，然后我们把tarball文件复制到目标计算机上后，其实是没有办法直接解压tarball文件然后直接使用的，我们还需要通过docker把这个文件通过docker还原为一个镜像。

主语，下面这条命令要在目标机器（没有Overleaf的机器）上运行

```shell
docker load -i OverLeaf.tar
```

注意，由于我们在提交镜像后可能有新写了文章，这个时候打包的镜像里其实是没有你写的文章的，因此文章还是需要另外保存的
