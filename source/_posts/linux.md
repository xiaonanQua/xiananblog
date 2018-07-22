---
layout: post
title: 'linux常见命令汇总'
date: 2018-7-22 22:29
comments: true
tags:
    - linux

brief: '记录我老忘的linux'
reward: true
---
>汇总常忘的linux命令 :smile:

## tar、zip打包解压常见命令
*1.zip解压到指定文件夹*
```zip
unzip xxx.zip -d 目标文件夹（绝对路径）
```
## 无法访问windows系统磁盘
>causes:应该上一次使用win系统使电脑睡眠，没有完全关机（应该是非正常关机，要解决的话可以再进一次win系统，正常关机。），这次开机进入ubuntu就出现这种情况。:wink::wink::wink:

**解决方法：**

*a.在终端输入如下命令，查看分区挂载情况*
```
sudo fdisk -l
```
如图所示：
![分区挂载情况](/assets/linux/disk1.png)
*b.修复挂载错误的相应的分区,如提示中的/dev/sda5，输入：*
```
sudo ntfsfix /dev/sda5
```
这样就修复成功:smiley::smiley:：
![修复成功](/assets/linux/disk2.png)



