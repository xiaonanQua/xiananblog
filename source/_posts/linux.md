---
layout: post
title: 'linux常见命令汇总'
date: 2018-7-22 22:29
comments: true
tags:
    - linux

brief: '记录我老忘的linux操作'
---
>汇总在使用ubuntu时常用到的linux命令
<!-- more -->
# 1.tar、zip打包解压常见命令

-c: 建立压缩档案
-x：解压
-t：查看内容
-r：向压缩归档文件末尾追加文件
-u：更新原压缩包中的文件

这五个是独立的命令，压缩解压都要用到其中一个，可以和别的命令连用但只能用其中一个。下面的参数是根据需要在压缩或解压档案时可选的。

-z：有gzip属性的
-j：有bz2属性的
-Z：有compress属性的
-v：显示所有过程
-O：将文件解开到标准输出

下面的参数-f是必须的

-f: 使用档案名字，切记，这个参数是最后一个参数，后面只能接档案名。

 tar -cf all.tar *.jpg
这条命令是将所有.jpg的文件打成一个名为all.tar的包。-c是表示产生新的包，-f指定包的文件名。

 tar -rf all.tar *.gif
这条命令是将所有.gif的文件增加到all.tar的包里面去。-r是表示增加文件的意思。

 tar -uf all.tar logo.gif
这条命令是更新原来tar包all.tar中logo.gif文件，-u是表示更新文件的意思。

 tar -tf all.tar
这条命令是列出all.tar包中所有文件，-t是列出文件的意思

 tar -xf all.tar
这条命令是解出all.tar包中所有文件，-t是解开的意思

## 压缩

tar -cvf jpg.tar *.jpg //将目录里所有jpg文件打包成tar.jpg 

tar -czf jpg.tar.gz *.jpg   //将目录里所有jpg文件打包成jpg.tar后，并且将其用gzip压缩，生成一个gzip压缩过的包，命名为jpg.tar.gz

 tar -cjf jpg.tar.bz2 *.jpg //将目录里所有jpg文件打包成jpg.tar后，并且将其用bzip2压缩，生成一个bzip2压缩过的包，命名为jpg.tar.bz2

tar -cZf jpg.tar.Z *.jpg   //将目录里所有jpg文件打包成jpg.tar后，并且将其用compress压缩，生成一个umcompress压缩过的包，命名为jpg.tar.Z

rar a jpg.rar *.jpg //rar格式的压缩，需要先下载rar for linux

zip jpg.zip *.jpg //zip格式的压缩，需要先下载zip for linux

## 解压

tar -xvf file.tar //解压 tar包

tar -xzvf file.tar.gz //解压tar.gz

tar -xjvf file.tar.bz2   //解压 tar.bz2

tar -xZvf file.tar.Z   //解压tar.Z

unrar e file.rar //解压rar

unzip file.zip //解压zip

## 总结

1、*.tar 用 tar -xvf 解压

2、*.gz 用 gzip -d或者gunzip 解压

3、*.tar.gz和*.tgz 用 tar -xzf 解压

4、*.bz2 用 bzip2 -d或者用bunzip2 解压

5、*.tar.bz2用tar -xjf 解压

6、*.Z 用 uncompress 解压

7、*.tar.Z 用tar -xZf 解压

8、*.rar 用 unrar e解压

9、*.zip 用 unzip 解压

 

*tar解压jdk到指定文件夹：*
```
tar -xzvf jdk-8u131-linux-x64.tar.gz -C /usr/local/java
```
*zip解压文件到指定文件夹：*
```
unzip 文件.zip -d /文件夹
```


## 2.无法访问windows系统磁盘
>causes:(首先我的电脑是双系统，这也只有双系统的情况才会发生。)
>应该上一次使用win系统使电脑睡眠，没有完全关机,（应该是非正常关机，要解决的话可以再进一次win系统，正常关机。）这次开机进入ubuntu就出现这种情况。

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

## 3.复制文件(夹)和移动文件(夹）
>复制、移动、删除的操作命令：cp、mv、rm

一、文件复制命令操作（cp）
```
命令格式：cp [-adfilprsu] 源文件(source) 目标文件(destination)
cp [option] source1 source2 source3 ... directory
```
参数说明：
-a:是指archive的意思，也说是指复制所有的目录
-d:若源文件为连接文件(link file)，则复制连接文件属性而非文件本身
-f:强制(force)，若有重复或其它疑问时，不会询问用户，而强制复制
-i:若目标文件(destination)已存在，在覆盖时会先询问是否真的操作
-l:建立硬连接(hard link)的连接文件，而非复制文件本身
-p:与文件的属性一起复制，而非使用默认属性
-r:递归复制，用于目录的复制操作
-s:复制成符号连接文件(symbolic link)，即“快捷方式”文件
-u:若目标文件比源文件旧，更新目标文件 

如将/test1目录下的file1复制到/test3目录，并将文件名改为file2,可输入以下命令：
```
cp /test1/file1 /test3/file2
```

二、文件移动命令操作（mv）
```
命令格式：mv [-fiv] source destination
```
参数说明：
-f:force，强制直接移动而不询问
-i:若目标文件(destination)已经存在，就会询问是否覆盖
-u:若目标文件已经存在，且源文件比较新，才会更新

如将/test1目录下的file1复制到/test3 目录，并将文件名改为file2,可输入以下命令：
```
mv /test1/file1 /test3/file2
```
三、文件删除命令操作（rm）
```
命令格式：rm [fir] 文件或目录
```
参数说明：
-f:强制删除
-i:交互模式，在删除前询问用户是否操作
-r:递归删除，常用在目录的删除

如删除/test目录下的file1文件，可以输入以下命令：
```
rm -i /test/file1
```

## 使用apt-get方法出现的错误，由于上次使用时未正常结束（强制杀死终端），出现再次使用无法再次使用

如标题所示发生的错误如下所示：
![apt-get再次使用错误](/assets/linux/error1.png)
解决代码：
```
强制解锁命令：
sudo rm /var/cache/apt/archives/lock
sudo rm /var/lib/dpkg/lock
```