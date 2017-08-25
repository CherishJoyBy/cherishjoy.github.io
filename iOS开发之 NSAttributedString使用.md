---
title: iOS开发之NSAttributedString使用
date: 2017.04.28 03:15:01
tags: [iOS,NSAttributedString]
categories: iOS开发
---

>本文介绍了NSAttributedString和NSMutableAttributedString的用法.

### 一. NSAttributedString介绍
摘自NSAttributedString.h文件
```objc
@interface NSAttributedString : NSObject <NSCopying, NSMutableCopying, NSSecureCoding>
```
它由2部分组成
1.文字内容 : `NSString *`
2.文字属性: `NSDictionary *`
```objc
文字颜色 - NSForegroundColorAttributeName
字体大小 - NSFontAttributeName
下划线 - NSUnderlineStyleAttributeName
背景色 - NSBackgroundColorAttributeName
```

### 二.NSMutableAttributedString介绍
摘自NSAttributedString.h文件
```objc
@interface NSMutableAttributedString : NSAttributedString
```
NSMutableAttributedString常用的有三种方法:
1.设置range范围的属性, 重复设置同一个范围的属性, 后面一次设置会覆盖前面的设置.
```objc
  - (void)setAttributes:(nullable NSDictionary<NSString *, id> *)attrs   range:(NSRange)range;
```
2.添加range范围的属性, 同一个范围, 可以不断添加属性.
```objc
  - (void)addAttribute:(NSString *)name value:(id)value range:(NSRange)range;
```
3.一次性添加一个范围内的多个属性.
```objc
  - (void)addAttributes:(NSDictionary<NSString *, id> *)attrs range:(NSRange)range;
```

### 三.需求
-给文本框设置占位文字的字体颜色、背景颜色以及下划线.
通过xib或者storyboard创建的界面,在界面右侧是找不到对应的设置属性.

### 四.解决
#### 方法1.
通过NSAttributedString实现,自定义一个继承至UITextField的类,在awakeFromNib方法中写以下代码.
```objc
NSMutableDictionary *attributes = [NSMutableDictionary dictionary];
attributes[NSForegroundColorAttributeName] = [UIColor yellowColor];
attributes[NSBackgroundColorAttributeName] = [UIColor redColor];
attributes[NSUnderlineStyleAttributeName] = @YES;
self.attributedPlaceholder = [[NSAttributedString alloc] initWithString:@"o惜乐o" attributes:attributes];
```
效果图如下:

![效果图](http://upload-images.jianshu.io/upload_images/3284707-2b2d87419fc17267.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/250)

注意 : 别忘记指定UITextField的Class
如图:

![关联UITextField的Class](http://upload-images.jianshu.io/upload_images/3284707-609ac89bf4ecb83f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/480)


#### 方法2:
通过NSMutableAttributedString实现.代码如下:
```objc
NSMutableAttributedString *string = [[NSMutableAttributedString alloc] initWithString:@"o惜乐o"];
NSMutableDictionary *attributes = [NSMutableDictionary dictionary];
attributes[NSForegroundColorAttributeName] = [UIColor yellowColor];
attributes[NSBackgroundColorAttributeName] = [UIColor redColor];
attributes[NSUnderlineStyleAttributeName] = @YES;
[string setAttributes:attributes range:NSMakeRange(0, 4)];
self.attributedPlaceholder = string;
```
#### 方法3:
```objc
NSMutableAttributedString *string = [[NSMutableAttributedString alloc] initWithString:@"o惜乐o"];
[string addAttribute:NSForegroundColorAttributeName value:[UIColor yellowColor] range:NSMakeRange(0, 4)];
[string addAttribute:NSBackgroundColorAttributeName value:[UIColor redColor] range:NSMakeRange(0, 4)];
[string addAttribute:NSUnderlineStyleAttributeName value:@YES range:NSMakeRange(0, 4)];
self.attributedPlaceholder = string;
```

#### 方法4:
重写drawPlaceholderInRect方法
```objc
NSMutableDictionary *attributes = [NSMutableDictionary dictionary];
attributes[NSForegroundColorAttributeName] = [UIColor whiteColor];
attributes[NSBackgroundColorAttributeName] = [UIColor redColor];
attributes[NSFontAttributeName] = self.font;
attributes[NSUnderlineStyleAttributeName] = @YES;
CGPoint placeholderPoint = CGPointMake(0, (rect.size.height - self.font.lineHeight) * 0.5);
[self.placeholder drawAtPoint:placeholderPoint withAttributes:attributes];
```
#### 方法5:
通过视图分层可以看出,UITextField中包含UITextFieldLabel.

![视图分层](http://upload-images.jianshu.io/upload_images/3284707-62fc07adab44038a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/480)

而占位视图是通过什么来显示呢?
根据`self.subviews.lastObject.class`,可知占位图是通过UITextFieldLabel显示的,根据`self.subviews.lastObject.superClass`可知UITextFieldLabel的父类是UILabel,所以可以使用`.textColor`方法去显示文字颜色.**但是**,不能保证`self.subviews.lastObject.class`方法中取到的一定是UITextFieldLabel.所以运行时就上场了.

因为UITextFieldLabel在UITextField.h头文件中找不到具体内容,只是简单的@class声明一下,所以需要利用运行时,查看UITextField的成员变量或属性,结果,你高兴的发现了这个家伙 -- placeholderLabel,这时候可以理解为placeholderLabel属性指向UITextFieldLabel所对应的内容,所以占位视图也是placeholderLabel啦!!!那么,也可以通过`.textColor`设置颜色.
```objc
unsigned int count;
Ivar *ivarList = class_copyIvarList([UITextField class], &count);
for (int i = 0; i < count; i++)
{
    Ivar ivar = ivarList[i];
    NSString *str = [NSString stringWithUTF8String:ivar_getName(ivar)];
    NSLog(@"%@", str);
    }
free(ivarList);
```

用KVC最终得出:
```objc
static NSString * const kPlaceholderColorKey = @"placeholderLabel.textColor";
static NSString * const kPlaceholderBGColorKey = @"placeholderLabel.backgroundColor";
[self setValue:[UIColor yellowColor] forKeyPath:kPlaceholderColorKey];
[self setValue:[UIColor redColor] forKeyPath:kPlaceholderBGColorKey];
```
设置下划线无法用KCV实现,如果非要用,还是会绕回第一种写法
```objc
NSMutableDictionary *attributes = [NSMutableDictionary dictionary];
attributes[NSUnderlineStyleAttributeName] = @YES;
NSAttributedString *attributeText = [[NSAttributedString alloc] initWithString:@"o惜乐o" attributes:attributes];
[self setValue:attributeText forKeyPath:kPlaceholderUnderLineKey];
```
说明:
UITextFieldLabel的父类为UILabel.UILabel中有TextColor属性,而UILabel继承自UIView,UIView中有backgroundColor属性.所以UITextFieldLabel就可以设置文字颜色和背景颜色.而placeholderLabel是程序内部私有的属性,指向UITextFieldLabel的内容,所以也能设置文字颜色和背景颜色.

### GitHub
[NSAttributedStringDemo](https://github.com/CherishJoyBy/NSAttributedStringDemo)

### 简书
[iOS开发之NSAttributedString使用](http://www.jianshu.com/p/fd9382ec2a4d) 