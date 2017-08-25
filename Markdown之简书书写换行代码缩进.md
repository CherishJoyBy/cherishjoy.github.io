---
title: Markdown之简书书写换行代码缩进
date: 2017.06.06 15:19:01
tags: [Markdown,简书,缩进]
categories: Markdown
---


>本文介绍了简书书写换行代码缩进的方法.
>初次使用简书的富文本写技术类的文章时,容易出现换行代码块错乱的问题.

### 一.单纯的换行.
代码块上的标题没有以下格式.

![导致换行错误的标题格式](http://upload-images.jianshu.io/upload_images/3284707-334027229450ae28.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

注意:圆点由`-`、`*`、`+`等产生.

简书代码:


![示例1](http://upload-images.jianshu.io/upload_images/3284707-c016551d83624316.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/250)

显示效果:

![示例1效果图](http://upload-images.jianshu.io/upload_images/3284707-f189488eca73d72f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/250)

### 二.标题带有圆点格式换行.
代码块上的标题含有以下格式.
![导致换行错误的标题格式](http://upload-images.jianshu.io/upload_images/3284707-334027229450ae28.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

简书代码:


![示例2](http://upload-images.jianshu.io/upload_images/3284707-8c92b1510b7063d2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/250)


显示效果:

![示例2效果图](http://upload-images.jianshu.io/upload_images/3284707-6eb5f63905b9fea2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 三.解决.
使用tab键将以下各行向右缩进一个单位.

![需要缩进的行](http://upload-images.jianshu.io/upload_images/3284707-b08ea14e98265a64.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

显示效果:


![示例2正确效果](http://upload-images.jianshu.io/upload_images/3284707-d63c94edaa3ea70c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


提示:
多尝试,多摸索,你会发现,只要用tab键将`int b = 1;`所在行向右缩进一个单位即可.
如果复制的代码来源中,本身带有空行的,在简书中可以自动显示为空行,而不需要调格式.

### 四.文本段落需要带有大于号(`>`)的时候.

简书代码:

![示例3](http://upload-images.jianshu.io/upload_images/3284707-e16fa98a866b05d1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/250)

显示效果:

![示例3效果图](http://upload-images.jianshu.io/upload_images/3284707-09962c1fe1de9ca2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 五.解决.
每行首个字符前加上`>`号.

简书代码:

![加上`>`后代码](http://upload-images.jianshu.io/upload_images/3284707-d650e42e6385a33a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


显示效果:

![加上`>`后效果图](http://upload-images.jianshu.io/upload_images/3284707-8aa1459026910d08.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

再用tab键从`>`之后将`int b = 0`所在行向右缩进一个单位.

简书代码:

![向右缩进之后的代码](http://upload-images.jianshu.io/upload_images/3284707-3443b0f336c3eebd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

显示效果:

![示例3正确效果图](http://upload-images.jianshu.io/upload_images/3284707-89f62984a6a752cf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

提示:
多尝试,多摸索,你会发现,只要在`int b = 1;`所在行添加`>`,之后从`>`后面向右缩进一个单位即可.

### 简书
[Markdown之简书书写换行代码缩进](http://www.jianshu.com/p/ae2fa398b46b) 