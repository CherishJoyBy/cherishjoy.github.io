---
title: iOS开发之App上架流程(2017)
date: 2017.05.17 15:12:01
tags: [iOS,App上架]
categories: iOS开发
---

>本文主要介绍了App上架流程,以及上架过程中会遇到的一些问题.

### 一.App上架前的准备.
上架前,需要开发人员有苹果开发者账号,具体请阅读[苹果开发者账号注册申请流程](http://www.jianshu.com/p/e8f0830c21dd).本文是在已经拥有开发者账号的前提下而开展的.

----------

### 二.登陆苹果开发者官网.
#### 1.进入[苹果开发者官网](https://developer.apple.com/).

![苹果开发者官网](http://upload-images.jianshu.io/upload_images/3284707-88bcbd34c0655165.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)

#### 2.点击Acount.

![登陆界面](http://upload-images.jianshu.io/upload_images/3284707-a13fc86e25d1f120.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)

#### 3.输入开发者账号,点击Sign in(登陆)

![输入账号密码](http://upload-images.jianshu.io/upload_images/3284707-28be2c0a321307e7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)

----------

### 三.生成发布证书
#### 1.点击Certifcates,Identifiers & Profiles(证书,id,配置)

![点击证书](http://upload-images.jianshu.io/upload_images/3284707-56ac226fb4391d06.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)

#### 2.点击All,再点击”+”号,添加发布证书.

![添加发布证书](http://upload-images.jianshu.io/upload_images/3284707-3611a09fc4ae421a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)

#### 3.选择App Store and Ad Hoc.之后continue.

![选择App Store and Ad Hoc](http://upload-images.jianshu.io/upload_images/3284707-944e67f72e5a378e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)

#### 4.还是continue.

![创建CSR文件](http://upload-images.jianshu.io/upload_images/3284707-3e47f29920dfa132.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)

#### 5.这里需要选择CSR文件.

![选择CSR文件](http://upload-images.jianshu.io/upload_images/3284707-570399013777b51a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)

#### 6.打开钥匙串,钥匙串在Launchpad的Other文件夹中

![Launchpad](http://upload-images.jianshu.io/upload_images/3284707-3f9fe6a941e00e8f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)

![Other文件夹](http://upload-images.jianshu.io/upload_images/3284707-2d2d31748777b1d5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)

![钥匙串访问](http://upload-images.jianshu.io/upload_images/3284707-d785e8153cd6944c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)

#### 7.点击钥匙串访问 -> 证书助理 ->从证书颁发机构请求证书.

![钥匙串](http://upload-images.jianshu.io/upload_images/3284707-1e6f2169f9127eee.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)

#### 8.电子邮件地址随意填写,邮箱常用名可不填,存储到磁盘.

![证书信息](http://upload-images.jianshu.io/upload_images/3284707-74b746d940be0e86.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)

#### 9.存储在磁盘上的CSR文件

![生成的CSR文件](http://upload-images.jianshu.io/upload_images/3284707-96e5b6bcb10c476e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)

#### 10.将CSR上传苹果服务器

![CSR上传苹果服务器](http://upload-images.jianshu.io/upload_images/3284707-6802569d889c6613.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/480)

#### 11.点击Download下载CER文件,保存并双击运行,运行完成后,点击Done.

![下载CSR文件](http://upload-images.jianshu.io/upload_images/3284707-b74247997ed41077.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)

#### 12.运行之后,在钥匙串里生成证书,确保证书有效.


![钥匙串访问](http://upload-images.jianshu.io/upload_images/3284707-5ec19c8fc0b20588.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)

----------

### 四.创建App IDs并绑定App的Bundle Identifier

#### 1.点击App IDs,点击”+”号.


![添加AppID](http://upload-images.jianshu.io/upload_images/3284707-52cd5e00cc7a71e8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)


#### 2.填写AppID 和 Bundle Identifier,name可以根据公司项目名来填写,日期只是为了标记这个App ID创建的时间.建议填写.Bundle Identifier则为项目的Bundle ID.

![添加AppID和Bundle Identifier](http://upload-images.jianshu.io/upload_images/3284707-9abf84f1e0e68d9d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)

#### 3.选择App Services,默认为两项,(根据具体需求选择),点击continue完成创建.

![App Services](http://upload-images.jianshu.io/upload_images/3284707-a3cc1dc056fecf85.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)

#### 4.点击Register

![App ID描述](http://upload-images.jianshu.io/upload_images/3284707-fea30f94e5b266d3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)

#### 5.点击Done

![点击Done](http://upload-images.jianshu.io/upload_images/3284707-b5cebd0e0d89dcff.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)

#### 6.注册成功后内容

![App ID](http://upload-images.jianshu.io/upload_images/3284707-868fea691150401c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)

注意:
1.上传App所用的Bundle Identifier为英文 + 数字的组合,而且是固定的,不能使用占位符和特殊符号.
2.如果工程中的Bundle Identifier改变,则开发者账号中添加的App ID需要重新绑定.

----------

### 五.生成描述文件
#### 1.描述文件是描述哪台电脑能对哪个Bundle Identifier的工程进行打包测试或发布.点击Provisioning Profiles,点击All,再点击右上角"+"号.

![生成Provisioning Profile](http://upload-images.jianshu.io/upload_images/3284707-ecba916d579c9bef.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)

#### 2.如果是发布,选择App Store这个描述文件,英译为:创建发布描述文件以提交你的app到App Store;
如果仅是安装到不同手机上进行测试,选择Ad Hoc,英译为:创建发布描述文件以安装你的app到已经注册的设备上(注册的设备上限为100台),点击Continue.

![描述文件选择](http://upload-images.jianshu.io/upload_images/3284707-35911e2e214f1a02.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)

#### 3.在App ID这个选项栏里面找到你刚刚创建的:App ID,点击Continue.

![App ID选择](http://upload-images.jianshu.io/upload_images/3284707-3bfa8dc804aa4c2e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)

#### 4.选择你刚创建的发布证书，根据自己电脑上的发布证书日期来选择，点击Continue.

![选择发布证书](http://upload-images.jianshu.io/upload_images/3284707-51d6de60da450dcb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)

#### 5.在Profile Name栏里输入一个名称,这个是Provisioning Profile(简称PP文件)文件的名称,可随便输入,文件名后缀可带上日期,方便以后使用.然后点击Continue.

![添加Provisioning Profile名称](http://upload-images.jianshu.io/upload_images/3284707-dc3060fa51c742b2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)

#### 6.Download文件,并双击运行,点击done完成.

![Download PP文件](http://upload-images.jianshu.io/upload_images/3284707-45ea554935f9e633.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)

#### 7.Download生成的PPFile.


![生成的PP文件](http://upload-images.jianshu.io/upload_images/3284707-38f1dc93f67b90d9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)

----------

### 六.在iTunes Connect中添加上传App信息并提交到Appstore.  

#### 1.用开发者账号登陆[iTunes Connect](https://itunesconnect.apple.com/login).


![iTunes Connect](http://upload-images.jianshu.io/upload_images/3284707-24eea2dc333ec0a3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)


#### 2.点击我的App


![我的App](http://upload-images.jianshu.io/upload_images/3284707-c70436bf2f9b5d4c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)


#### 3.点击”+"号,然后新建App.



![新建App](http://upload-images.jianshu.io/upload_images/3284707-fcc51a2da2cee825.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)



#### 4.按要求填写信息,SKU是公司用于做统计数据之类的id，根据公司需求填写



![App信心](http://upload-images.jianshu.io/upload_images/3284707-610e9270495b2511.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)



#### 5.每个描述后面的?号是苹果提供的提示  

平台:


![平台](http://upload-images.jianshu.io/upload_images/3284707-c30006fefaaa7099.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)


名称:


![名称](http://upload-images.jianshu.io/upload_images/3284707-ff2e02db8c0e9348.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)


语言:


![语言](http://upload-images.jianshu.io/upload_images/3284707-7f80894530c3607f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)


套装ID:


![套装ID](http://upload-images.jianshu.io/upload_images/3284707-5ae90c3362a81834.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)


SKU:


![SKU](http://upload-images.jianshu.io/upload_images/3284707-b53553ddaf592113.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)


#### 6.填写App名称、套装ID、类别.分级位置显示:无分级.具体分级需要在后面填写.


![App具体信息](http://upload-images.jianshu.io/upload_images/3284707-7235107e238f52c3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)


#### 7.填写价格、销售范围、批量购买计划.


![价格、销售信息](http://upload-images.jianshu.io/upload_images/3284707-b0f851387e989d72.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)


#### 8.需要3.5寸、4寸、4.7寸、5.5寸预览图片,每个尺寸都要至少3张.


![App预览图](http://upload-images.jianshu.io/upload_images/3284707-0670b4decd551269.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)


3.5寸:640 x 960


![3.5寸](http://upload-images.jianshu.io/upload_images/3284707-ab933a70e1515067.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)


4寸:640 x 1136


![4寸](http://upload-images.jianshu.io/upload_images/3284707-478c55ac975f6296.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)


4.7寸:750 x 1334


![4.7寸](http://upload-images.jianshu.io/upload_images/3284707-141d9889ee241f0a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)


5.5寸:1242 x 2208


![5.5寸](http://upload-images.jianshu.io/upload_images/3284707-27954b61205eb33e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)


#### 9.App的描述根据公司需求填写,如果App名称为”XX助手",关键词可以写:”XX、助手、XX助手”,关键词主要是为了让用户在AppStore上搜索应用时,能通过对应关键词能找到匹配的App.


![XX助手](http://upload-images.jianshu.io/upload_images/3284707-b971340b2aac2b90.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)


#### 10.iMessage App图中已经说明很清楚,一般不用处理


![iMessage](http://upload-images.jianshu.io/upload_images/3284707-4cdf11cde468104f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)


#### 11.填写App图标


![App图标](http://upload-images.jianshu.io/upload_images/3284707-63b663721fc2eee9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)


#### 12.分级填写,如图分级定为17+,如果想要4+,无限制的网络访问改为否.


![分级填写](http://upload-images.jianshu.io/upload_images/3284707-06078cf5a09e787e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)

#### 13.如果XX助手App涉及和带有蓝牙的硬件连接,需要上传App和硬件使用操作的视频演示地址,我上传的是优酷.(只要是App和硬件进行交互,就需要有App操作视频演示地址)


![XX助手审核信息](http://upload-images.jianshu.io/upload_images/3284707-c3eaca06427cce24.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)


#### 14.Apple Watch图中也说明很清楚,一般不用处理.


![Apple Watch图标](http://upload-images.jianshu.io/upload_images/3284707-85ae6b3a36a9d0a1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)

----------

### 七.xcode打包项目

#### 1.构建版本,需要到Xcode中去打包.


![构建版本](http://upload-images.jianshu.io/upload_images/3284707-b3eafa382a5b2411.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)


#### 2.版本选择的问题


![版本选择](http://upload-images.jianshu.io/upload_images/3284707-98a5700a314b417a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)

#### 3.打开Xcode,设备选择Generic iOS Device.然后使用快捷键Command + B,进行编译.下图中有一些简单说明


![配置说明](http://upload-images.jianshu.io/upload_images/3284707-20548d16d383170b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)


#### 4.选择Product -> Scheme -> Edit Scheme 或者使用快捷键Command + < ,打开界面.


![Edit Scheme](http://upload-images.jianshu.io/upload_images/3284707-22023ec88ace1d6d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)


#### 5.把Run、Test、Profile、Analyze、Archive中的Build Configuration全部改为Release.之后Close.


![修改为Release的位置](http://upload-images.jianshu.io/upload_images/3284707-46d5a5007e94cf6b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)


#### 6.编译成功,选择Product -> Archive.进行打包.


![Archive](http://upload-images.jianshu.io/upload_images/3284707-b86c07b0f731195d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)


#### 7.打包后弹窗,上传给苹果团队进行测试的包不能带有iPhone等字样,因为,苹果对打包的文件名称有要求.所以,我把"Unity-iPhone"改了.


![打包的文件名](http://upload-images.jianshu.io/upload_images/3284707-d78e501c26d8bfb7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)


#### 8.选择Validate进行验证,再Choose对应的付费过的开发者团队.


![Validate](http://upload-images.jianshu.io/upload_images/3284707-872e71252f8e3530.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)


#### 9.点击Validate

![点击Validate](http://upload-images.jianshu.io/upload_images/3284707-1a32bb7ae10eb8f0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)

#### 10.等待验证结果


![等待验证](http://upload-images.jianshu.io/upload_images/3284707-44e62ef6f9dbf1a9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)


#### 11.验证失败,点击done.因为之前已经出现build为3的版本,所以,将build改为4,从步骤(五.6)再走一次流程.如果没错则继续.


![验证失败](http://upload-images.jianshu.io/upload_images/3284707-d6035c68609ed2ca.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)


#### 12.验证成功,如下,选择done.


![验证成功](http://upload-images.jianshu.io/upload_images/3284707-c1fb8d5aa36a0d51.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)


#### 13.选择Upload to App Store,弹窗后还是选择付费的开发者团队.


![Upload to App Store](http://upload-images.jianshu.io/upload_images/3284707-bf2200c4f33da8e1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)


#### 14.选择Upload


![Upload](http://upload-images.jianshu.io/upload_images/3284707-15c28b2111b7fc5e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)


#### 15.等待上传


![等待上传](http://upload-images.jianshu.io/upload_images/3284707-4162937eb9ec015e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)


#### 16.上传中


![上传中](http://upload-images.jianshu.io/upload_images/3284707-57e2eccecc4a47b7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)


#### 17.上传成功


![上传成功](http://upload-images.jianshu.io/upload_images/3284707-fb60e68702fe2d01.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)

#### 18.返回到iTunes Connect网站中,我的App -> 准备提交 -> 选择构建版本右侧的”+"号.


![添加构建版本](http://upload-images.jianshu.io/upload_images/3284707-06db1dba6070dfc1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)


#### 19.选择构建版本,点击完成.

![选择构建版本](http://upload-images.jianshu.io/upload_images/3284707-f2738dd1ee04d4e7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)

#### 20.提交审核.


![提交审核](http://upload-images.jianshu.io/upload_images/3284707-daea367bb4498b9a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)


#### 21.出现广告标识,根据情况填写,再提交.


![广告表示符](http://upload-images.jianshu.io/upload_images/3284707-3025b9885ed3f124.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)


#### 22.App等待审核.


![等待审核](http://upload-images.jianshu.io/upload_images/3284707-202ccdba77f290b5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)


### CSDN 
[iOS开发 -- App上架流程(2017)](http://blog.csdn.net/cherish_joy/article/details/72628809)
### 简书
[iOS开发之 App上架流程(2017)](http://www.jianshu.com/p/440ea5a2bb54)