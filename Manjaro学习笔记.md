# Manjaro学习笔记

by元子

[TOC]



## 安装



## 开始前的基本操作

#### 配置国内源

1. 设置官方镜像（包括 core， extra， community， multilib ）

   ````ruby
   $ sudo pacman-mirrors -i -c China -m rank //更新镜像排名
   $ sudo pacman -Syy //更新镜像源
   ````

   



2. 添加archlinuxCN源

   ````ruby
   $ sudo nano /etc/pacman.conf
   	
   *在文档底部加入如下几行*
   
   	[archlinuxcn]
   
   	SigLevel = Optional TrustedOnly
   
   	Server =https://mirrors.ustc.edu.cn/archlinuxcn/$arch
   
   $ sudo pacman -S archlinuxcn-keyring
   $ sudo vim /etc/pacman.conf //更改最后两行TrustAll
   
   ````



####  常用软件安装

- 搜狗输入法

  ````ruby
  $ sudo pacman -S fcitx-sogoupinyin
  $ sudo pacman -S fcitx-im
  $ sudo pacman -S fcitx-configtool
  ````

- 