---
title: iOS开发之host配置
date: 2017.06.06 13:42:01
tags: [iOS,host配置]
categories: iOS开发
---

>本文介绍了host的配置过程.

### 一.打开 Finder,快捷键Command + Shift + G.弹出文本框.

### 二.在路径中输入/private.点击前往.

![文本框中输入/private](http://upload-images.jianshu.io/upload_images/3284707-365ba0b5b03d7d2d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 三.选择etc文件.

![etc文件](http://upload-images.jianshu.io/upload_images/3284707-886ed51a7009c780.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 四.然后按h键,hosts文件就出现了.不用拖动滚动条来回查找.

![hosts文件](http://upload-images.jianshu.io/upload_images/3284707-f662855ffe2a5e05.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 五.想要修改hosts文件,需要修改etc和hosts两个文件的权限.右击etc,点击显示简介.


![显示简介](http://upload-images.jianshu.io/upload_images/3284707-8ebe84a9bb84282a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 六.点击右下角的🔐.

![点击右下角的🔐.](http://upload-images.jianshu.io/upload_images/3284707-208cfe58dfb2d4fe.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 七.输入mac开机密码,点击好.


![输入开机密码](http://upload-images.jianshu.io/upload_images/3284707-0e7d2d64efb9e10c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


### 八.将everyone的权限改为读与写.

![权限改为读与写](http://upload-images.jianshu.io/upload_images/3284707-eaac4c143b01b3f4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 九.hosts权限修改类似.打开hosts文件,便可以往其中添加内容了.

注意:记得修改完权限,还原为只读权限.

### 简书
[iOS开发之host配置](http://www.jianshu.com/p/ceb571348a73) 