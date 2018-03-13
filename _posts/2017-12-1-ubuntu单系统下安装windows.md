---
title: '在Ubuntu系统下装Win7并引导双系统'
layout: post
tags:
    - ubuntu 
    - 引导
    - 双系统
    - win7
---



win平台下安装Linux双系统很容易，但是Linux下安装win经常会出现问题，这里较为详细的展示出一些遇到的问题。

<!--more-->



# 

本人的系统原先是就单ubuntu系统，而且是未分区情况下自动安装的，现在又装了个windows7，为了方便，自己笔记记录下，也给不知道同学参考下。

首先解释下ubuntu的 live CD即你将ubuntu系统的安装光盘或USB HDD硬盘镜象.

装好后情况：ubuntu一个主要盘（/dev/sda1），winodws7一个盘（/dev/sda2），还有两个ubuntu的（/dev/sda3，Extended； /dev/sda5， linuxswap）

思路：ubuntu是用grub2引导的，装了windows7后mbr会被修改，grub2就会没用。

所以表现就是装好windows7后会直接进入windows，没有给你选择系统的grub2选择界面，也没有开机引导界面。

下面按我自己的情况说下过程（本人情况很简单）：

1、需要工具（2个）：一个windows7的安装光盘/安装U盘，一个ubuntu12.04的安装光盘/安装U盘。-------这两个都可以自己制作哦

2、分区：我是默认一个分区（装了ubuntu），所以要分区，已经有分区的孩子就可以跳过了。

我这里分区有点麻烦了。我是先用ubuntu安装光盘用光驱启动到ubuntu的install里面，用里面的正式安装前的“手动分区”选项把原来都给ubuntu的ext4的盘分出了50G的空闲区域，然后退出。

进入到ubuntu系统里，安装ubuntu的分区工具：

图形化分区工具：gparted 安装命令：sudo apt-getinstall gparted

把之前分出来50G空闲做成ntfs主分区（不知道可不可以直接就在gparted里分出空闲然后再做ntfs，有兴趣可以试试）

3、安装windows7：分出个区后就可以用U盘启动安装windows7了（我是U盘的），选择那个你分出的区域，安装步骤不用多说了。

安装完毕后电脑表现为只能进入windows7（grub2没用了）

4、修复grub2：这里就用到ubuntu安装光盘（我是光盘）里的liveCD了，就是CD盘里的ubuntu（这样简单点）。

进入live CD后打开terminal（终端），输入：sudofdisk -l （小写的L哦），会显示你系统盘里系统的情况：

我的：

Disk /dev/sda: 100.0 GB, 100030242816 bytes
……………………………………

……………………

Device
Boot 
Start 
End 
Blocks  Id 
System
/dev/sda1 
1 
5286 
39956055  83 
Linux
/dev/sda2 
\* 
5286 
12390 
53710848 
7  HPFS/NTFS
/dev/sda3 
12391 
12922 
4016129 
5  Extended
/dev/sda5 
12391 
12922 
4016128  82 
Linux swap / Solaris

那个/dev/sda1就是我ubuntu的盘了，在其他盘的同学可以看看Id和System，Id是83，System是Linux

然后输入：sudo-i （得到root权限，无需再输入密码，便于下面操作）

输入：mkdir/media/tempdir （用来挂载sda1的，就是创建一个tempdir，名字什么的自己定）

输入：mount /dev/sda1/media/tempdir （将sda1挂载在tempdir下）

输入：grub-install--root-directory=/media/tempdir/dev/sda （重新安装grub2到硬盘的主引导记录（mbr））

操作成功出现：Installation finished.No Error Reported.

输入：reboot （重启电脑）

5、修复windows7在grub2下的引导：

重启后系统就可以进入ubuntu12.04了，但是windows暂时无法引导，下面就是更新grub2让它可以引导windows7.

进入到ubuntu后打开Terminal，输入：sudoupdate-grub2

输入密码。

应该出现一堆表示成功的话，多少可以看懂一些。

最下面有windows7的什么什么。

done

没出现的话到新立得搜索grub，安装带ubuntu标志的grub-pc。

成功后再输入命令：sudoupdate-grub2 就可以了

​                