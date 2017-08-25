---
title: iOS开发之苹果开发者账号注册申请流程
date: 2017.05.16 12:23:01
tags: [iOS,iOS账号注册]
categories: iOS开发
---

>本文介绍了苹果开发者账号注册申请流程的详细过程.

![](http://upload-images.jianshu.io/upload_images/3284707-64c20ec273296c37.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/480)

### 准备工作
一张visa或者万事达国际信用卡（开通visa或master功能的信用卡）、公司邮箱、公司网站（需与邮箱后缀一致）。
### 苹果企业开发者账号，分为两种。
第一种Enterprise Program为公司内部员工打包测试用，不可公开下载；对外发布的是第二种，为Developer Program。

[![](http://upload-images.jianshu.io/upload_images/3284707-1124e07143567d27.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1024)](http://www.niaogebiji.com/data/attachment/portal/201603/03/114927r9uucjhjjfmjzuuu.png)

### 一.Enterprise Program(苹果公司售价$299，约合￥1988).

此账号的作用：企业账号是苹果给企业用户用来进行内部测试用的一种账号，我们可以通过该账号生成的证书打包APP，放于企业的内部网站上（不可上传AppStore），可供苹果用户下载安装，不过值得注意的是通过这种方式安装APP，一旦账号一年有效期到期，手机上已经安装的APP无法启动，也无法在网站上下载安装，必须重新打包发布。因此账号按期续费非常重要。此证书主要是没有安装设备数量限制（由于此特点，在测试和分发 App 时，给开发者带来了极大的便利，尤其是多人协作）。但是要注意：此账号仅仅用于内部测试，不可公开下载，苹果的管控是非常严格的，任何违背苹果条款使用企业账号，都会有企业账号被封的风险，封号之后使用该证书的APP将会闪退。

#### 1.进入[苹果开发者官网](https://developer.apple.com/)，鼠标滚动到页面最下方，选择Enterprise Program。

[![](http://upload-images.jianshu.io/upload_images/3284707-b8245be22d001dfa.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](http://www.niaogebiji.com/data/attachment/portal/201603/03/112441mvkg3py7nyn36yui.jpg)

#### 2.进入Enterprise Program页面之后，再选择页面中间部分的“Get started with enrollment”（开始登记）；

[![](http://upload-images.jianshu.io/upload_images/3284707-a102b20919a2da26.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](http://www.niaogebiji.com/data/attachment/portal/201603/03/112441wyzsyuuotho8gufz.jpg)

#### 3.跳转到What You Need to Enroll的页面，这里就是描述了一下大概需要准备什么之类的，选择下方的“Start Your Enrollment”即可。

[![](http://upload-images.jianshu.io/upload_images/3284707-1696f19e09c720d8.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](http://www.niaogebiji.com/data/attachment/portal/201603/03/112442uoeegew6gke2ykhh.jpg)

#### 4.若没有登录的话，随后会弹出苹果开发者账号的登录界面。当然没有苹果账号的话，就需要去注册一个，点击“Create Apple ID”，去创建一个苹果账号。

[![](http://upload-images.jianshu.io/upload_images/3284707-3f41e8ba6a91d6fb.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](http://www.niaogebiji.com/data/attachment/portal/201603/03/112442ia3ighc3iwvcvz6c.jpg)

#### 5.创建苹果账号，需要填写一些信息，比如APPID（邮箱）、姓名（拼音）、密码（至少需要8位，含数字和大写字母）、密保问题、出生日期、国籍等。

（1）Apple ID一栏填写邮箱地址（建议为公司企业邮箱）；

（2）强烈建议申请苹果证书的时候建一个word文档，一方面是记录信息，另外一方面是为了下次申请或者复制粘贴信息方便，因为有可能填完一系列信息提交之后验证码已经失效了，有时页面更新一下又要重新填写（后面申请邓白氏编码时也建议如此）……

[![](http://upload-images.jianshu.io/upload_images/3284707-6570a71a89e41905.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](http://www.niaogebiji.com/data/attachment/portal/201603/03/112442enol5vum8wo982w8.jpg)
[![](http://upload-images.jianshu.io/upload_images/3284707-93d45eeb93291caa.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](http://www.niaogebiji.com/data/attachment/portal/201603/03/112442bf2c275z52p7c925.jpg)
[![](http://upload-images.jianshu.io/upload_images/3284707-440625eec987cf85.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](http://www.niaogebiji.com/data/attachment/portal/201603/03/112442nqjeo1ktetfoa01j.jpg)

#### 6.填写完之后，点击“Continue”进行下一步。在邮箱收取验证码。

[![](http://upload-images.jianshu.io/upload_images/3284707-37ca5df02efa1949.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](http://www.niaogebiji.com/data/attachment/portal/201603/03/112442f5847bzba8kr6b44.jpg)

#### 7.进入邮箱，收取邮件，点击“Verify”，即可验证成功。

#### 8.回到第4步，使用刚申请的账号登录，点击Sign In。

#### 9.同意一下协议，提交。

[![](http://upload-images.jianshu.io/upload_images/3284707-84293ef4cc72d55c.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](http://www.niaogebiji.com/data/attachment/portal/201603/03/112443gfmuz4b3j6wo4o44.jpg)

#### 10.选择为“Company/Organization”（公司/组织）。

[![](http://upload-images.jianshu.io/upload_images/3284707-243b3a8c78f536b5.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](http://www.niaogebiji.com/data/attachment/portal/201603/03/112443vyqq3qjxqqjrqjqx.jpg)

#### 11.选择后弹出警告，大意就是申请苹果开发者证书之前需要申请邓白氏编码（美国的邓白氏编码用于企业的识别码），没有邓白氏编码是无法申请证书的。此处选择D-U-N-S Number下方的“Check now”进行邓白氏编码的申请。

[![](http://upload-images.jianshu.io/upload_images/3284707-6ad45310624f3fe3.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](http://www.niaogebiji.com/data/attachment/portal/201603/03/112443md6hk0q4qq4dfin6.jpg)

#### 12.在邓白氏申请页面上填写公司相关信息（均使用英文），填写完之后提交。座机需要填写能联系到本人的座机（中国区格式为86-区号+号码+分机号，如86-0755XXXXX-XX）。

[![](http://upload-images.jianshu.io/upload_images/3284707-5a87d8e70533439a.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](http://www.niaogebiji.com/data/attachment/portal/201603/03/112443yf7r7ehxec77nzff.jpg)
[![](http://upload-images.jianshu.io/upload_images/3284707-3e2666eb198743bb.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](http://www.niaogebiji.com/data/attachment/portal/201603/03/112443yl3qptgzt9l7lgz1.jpg)
[![](http://upload-images.jianshu.io/upload_images/3284707-d2774bff34daac3b.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](http://www.niaogebiji.com/data/attachment/portal/201603/03/112443vnxa63n4rw7xnhar.jpg)

#### 13.提交后出现以下页面。

这里不需要选择（列表中的均为邓白氏中已经有记录的地址），因为刚申请是不在列表内的。在此页面点击“submit your information”，之后提示“D-U-N-S Number Creation Request”，则表示信息提交成功。随后苹果公司会发送一封邮件，里面含有邓白氏申请下来的大致时间点以及该提交请求的响应码。

操作到此步，需要暂时告一段落，等待邓白氏编码的申请完成，申请下来会以邮件的形式告知，注意查收邮件。时间大概是一周，如果没下来可以根据响应码咨询。

邓白氏邮箱：applecs@dnb.com 苹果咨询热线：4006 701 855

[![](http://upload-images.jianshu.io/upload_images/3284707-46aeac6ed728fde9.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](http://www.niaogebiji.com/data/attachment/portal/201603/03/112443mmmg60c5uj3amcaq.jpg)
[![](http://upload-images.jianshu.io/upload_images/3284707-fa477086364f9d56.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](http://www.niaogebiji.com/data/attachment/portal/201603/03/112444dslllxm6w2r5md6s.jpg)[![](http://upload-images.jianshu.io/upload_images/3284707-b5fdefe24a72b209.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](http://www.niaogebiji.com/data/attachment/portal/201603/03/112444z99h8z45n5l2h1hs.jpg)

#### 14.经过一周漫长的等待……苹果公司会来电话，核实一些信息，比如公司名称、地址等，还会去查一下公司是否已经注册，申请人联系方式等。核实完毕就发放邓白氏编码。苹果公司会提示，得到编码后最好是过14个工作日之后使用，如要提前使用，失败不要超过3次，然后走后续流程。

#### 15.从邮件中获得邓白氏编码（D-U-N-S Number）。回到第11步，这次选择“Continue”。

[![](http://upload-images.jianshu.io/upload_images/3284707-7540440a1153a088.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](http://www.niaogebiji.com/data/attachment/portal/201603/03/112444eukyfg29rppkr666.jpg)

#### 16.这一步如果是公司所有者，选择第一个，如果不是公司所有者但是公司授权人，选择第二个。选择第二个区别就是需要多填一点授权人的资料。填写一下就OK。

[![](http://upload-images.jianshu.io/upload_images/3284707-baba235498508af5.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](http://www.niaogebiji.com/data/attachment/portal/201603/03/112444i1xbhgb1w1jb1ehw.jpg)
[![](http://upload-images.jianshu.io/upload_images/3284707-ea3c735131f09aa4.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](http://www.niaogebiji.com/data/attachment/portal/201603/03/112444t57c5pecq7qccdzd.jpg)

#### 17.这里需要填写一下邮箱上收到的邓白氏编码、公司名、公司主页、总部座机、工作邮箱。其中，总部电话Country Code填写86，Phone Number填写公司座机号（前面加
区号），Extension填写分机号。例：86-0755XXXXXXXX-XXX。邮箱后缀xxx.com和公司网址域名后缀xxx.com需要保持一致，现在苹果对苹果开发者证书审核很严格，不小心就容易被拒。填写完后，点击“Continue”。
[![](http://upload-images.jianshu.io/upload_images/3284707-76106a84af2c74f1.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](http://www.niaogebiji.com/data/attachment/portal/201603/03/112444pyzmz8knniovstrq.jpg)

#### 18.这一步就是验证信息，如果信息没错，在Address栏打勾，并选择Submit提交。

[![](http://upload-images.jianshu.io/upload_images/3284707-edca3e3331ca5aeb.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](http://www.niaogebiji.com/data/attachment/portal/201603/03/112445agzgz59k91i1xflx.jpg)

#### 19.提交之后，会提示“Your enrollment is being processed”，说明开发者证书申请已经提交。好吧，接下来又是等了，大概一周左右。如果申请期间，因为资料缺失或准备不完整，造成审核退回，可以联系苹果公司4006 701 855。如果需要更新邓白氏的资料，可以发送邮件至邓白氏（若需要邓白氏协助，一般苹果回复的邮件里含有邓白氏公司的邮件地址），可以用中文或英文撰写邮件。

[![](http://upload-images.jianshu.io/upload_images/3284707-5487a66f09bb7236.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](http://www.niaogebiji.com/data/attachment/portal/201603/03/112445g45hhiq5q4nhn6h1.jpg)

#### 20.正常的话大概一周可以收到苹果公司的电话，确认基本信息和用途（公司内部测试），则收到可以继续的邮件。打开邮件的链接或者登录网站，即可继续申请。首先，同意一下协议。点击提交。

[![](http://upload-images.jianshu.io/upload_images/3284707-03da89b42361344c.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](http://www.niaogebiji.com/data/attachment/portal/201603/03/112445fd0js8wcsvp80jaw.jpg)

#### 21.点击购买。Apple Developer Enterprise Program证书需要的费用为人民币￥1988。

[![](http://upload-images.jianshu.io/upload_images/3284707-42fa2f3d1b4e1a10.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](http://www.niaogebiji.com/data/attachment/portal/201603/03/112445lqwyrmg3q3w3q236.jpg)

#### 22.选择付款方式（VISA或者MasterCard），点击继续进行购买。

[![](http://upload-images.jianshu.io/upload_images/3284707-1b62372ff0fef488.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](http://www.niaogebiji.com/data/attachment/portal/201603/03/112445xsbz888irrnuuuw4.jpg)

#### 23.付款完成。邮箱会收到订单邮件，这时就可以使用苹果证书了，后面快到期的时候记得续缴费用。

[![](http://upload-images.jianshu.io/upload_images/3284707-354db3eaf19bedca.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](http://www.niaogebiji.com/data/attachment/portal/201603/03/112446mtizil91tl3sez9p.jpg)

### 二.Developer Program(苹果公司售价$99，约合￥699).

****

此账号分为个人版，公司版。作用：用于对外发布苹果端APP，可将APP上传到AppStore（发布到AppStore就是审核比较麻烦，如果想尽快测试APP情况，可用Enterprise Program测试完，再用此证书进行对外发布到AppStore审核，否则发现问题再审核就慢了），也可以用来给开发者生成证书调试APP，利用该账号发布到AppStore上的APP可以供任一苹果用户通过AppStore下载安装。此证书有安装设备数量限制，即用于开发的设备数量为100个。

#### 公司版申请流程（个人版更简单且不用申请邓白氏编码，在此不赘述，参考下面的就行）：

公司开发者账号申请详情准备：
（1.）只能用visa或者万事达国际信用卡（开通visa或master功能的信用卡）
（2.）公司法人姓名公司法人的职称
（3.）公司的DUNS码（邓白氏编码）
（4.）公司的英文名，公司网址。

#### 1.进入苹果开发者官网：https://developer.apple.com/，拉到页面最下方，选择Developer Program。

[![](http://upload-images.jianshu.io/upload_images/3284707-c00f8c2bf9ee4783.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](http://www.niaogebiji.com/data/attachment/portal/201603/03/112446w11viazvd8hdvlk5.jpg)

#### 2.点击右上角的“Enroll”进行申请。

[![](http://upload-images.jianshu.io/upload_images/3284707-a5fd127bd3a3665f.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](http://www.niaogebiji.com/data/attachment/portal/201603/03/112446efrdvv4hyyhfd0x3.jpg)

#### 3.这边的注册流程可以参考第一部分的3-7步。
创建Apple ID或者已经有就直接登录(创建Apple ID 这里就不讲了)
[![](http://upload-images.jianshu.io/upload_images/3284707-67bae1c3fa1eb23c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](http://www.niaogebiji.com/data/attachment/portal/201603/03/155526mjfytxfsjdcxfjpn.png)

顺带一提，Developer Program和Enterprise Program都想申请的话，是需要注册两个账号的，一个账号只能用于一个证书。
#### 4.注册了账号之后，需要选择账号类型。若是个人开发者，则选择第一项：“Individual / Sole Proprletor / Single Person Business”,公司版则选择“Company / Organization”,政府组织选择则第三个。

[![](http://upload-images.jianshu.io/upload_images/3284707-c4ab6ad69e288faf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](http://www.niaogebiji.com/data/attachment/portal/201603/03/155527mt75htxx788obn4z.png)

注意：公司版的还需要申请邓白氏编码（DUNS Number）。不过，作为公司的唯一标识，邓白氏编码可以通用。若之前申请过邓白氏编码，就可以不用申请。

这部分的申请步骤也可参考第一部分的8-14步。

#### 5.点击continue之后的跳转到如下界面：注意 ：填写号码时，country Code填86Extension不填
[![](http://upload-images.jianshu.io/upload_images/3284707-025930e36dcf1059.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](http://www.niaogebiji.com/data/attachment/portal/201603/03/155527voqj7izjqt1qtl3j.png)
[![](http://upload-images.jianshu.io/upload_images/3284707-47318a4135911dc2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](http://www.niaogebiji.com/data/attachment/portal/201603/03/155528fimup6kxnitokk61.png)

填好信息之后就按下continue 
#### 6.等待邮件
#### 7.按照苹果发的邮箱里的内容操作下一步，首先同意他们的证书和协议，然后填写信用卡信息，如下图：
[![](http://upload-images.jianshu.io/upload_images/3284707-7895f344d0f1ba34.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](http://www.niaogebiji.com/data/attachment/portal/201603/03/155529fbn12ej52ru71jsi.png)
[![](http://upload-images.jianshu.io/upload_images/3284707-1b0802e273c4a02f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](http://www.niaogebiji.com/data/attachment/portal/201603/03/155529yr7i84fxu8m77fmj.png)

填写信用卡的基本信息，一般用你们老板（或者是你们负责人）的信用卡就行，但一定要是visa卡，即国际信用卡，支持双币的。
#### 8.到此整个申请流程就完了，接下来苹果会扣费和激活开通了。

后续步骤与第一部分一样，申请完之后，就可以在itunesconnect.apple.com发布appstore的应用的上线审核了，将代码打包上传即可。

### 参考
[苹果开发者账号注册申请流程](http://www.niaogebiji.com/article-9882-1.html)

### 简书
[Markdown之简书书写换行代码缩进](http://www.jianshu.com/p/e8f0830c21dd) 