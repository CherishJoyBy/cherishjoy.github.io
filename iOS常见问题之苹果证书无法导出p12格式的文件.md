---
title: iOS常见问题之苹果证书无法导出p12格式的文件
date: 2017.06.06 12:03:01
tags: [iOS,常见问题,p12,证书]
categories: iOS常见问题
---

>本文介绍了苹果证书无法导出p12格式的文件的解决方法.
### 一.打开钥匙串导出证书的默认界面,发现p12选项为灰色,无法选择.

![默认导出界面](http://upload-images.jianshu.io/upload_images/3284707-2d38a50e4a07a158.jpeg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

说明:
我对这种情况的证书进行测试,将直接导出.cer的证书,拷贝到其他电脑上并安装,打包项目时,提示:
```objc
No valid signing identities matching the team ID "xxx" were found.
```
在项目中,快捷键 `command + ,`选择对应的AppleIDs,再选择对应的Team,点击ManageCertificates,可以看到证书报一个错误,这个错误就是由于安装.cer证书导致的.
```objc
Missing private key
```
### 二.打开钥匙串之后,选择种类,然后选择证书,再找到对应要导出的证书.

![正确选择证书的方式](http://upload-images.jianshu.io/upload_images/3284707-762bd4f2705a78f5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 三.结果出现p12的选项.

![](http://upload-images.jianshu.io/upload_images/3284707-2823d7be1662b54e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 四.导出后,设置对应密码,再拷贝到其他电脑上安装证书.便能正常打包程序.

### 简书
[iOS常见问题之苹果证书无法导出p12格式的文件](http://www.jianshu.com/p/824bfcef4a13)