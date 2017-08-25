---
title: iOS开发之规范文档
date: 2017.04.21 00:18:01
tags: [iOS,规范文档]
categories: iOS开发
---

## 一.语言
采用US(美式)英语,不使UK(英式)英语或汉字拼音.
```obj
US: UIColor *myColor =[UIColor blueColor];
UK: UIColor *myColour =[UIColor blueColor];
拼音: UIColor *wodeYanSe =[UIColor blueColor];
```
## 二.命名规则
### 1.常量的命名
在前面加上小写字母k作为标记.其余遵循小驼峰命名法(第一个单词全部小写,后面单词首字母大写).
```objc
NSTimeInterval kAnimationDuration = 0.3;
```
### 2.宏的命名
以两个大写字母作为前缀,后面遵循大驼峰命名法.
```objc
#define KKScreenWidth ([UIScreen mainScreen].bounds.size.width)
#define KKAppVersion @"appVersion"
```
### 3.枚举的命名
遵循Objective-C内部框架定义方式.
Enum中枚举内容的命名需要以该Enum类型名称开头.
```objc
typedef NS_ENUM(NSInteger, FulowersMoveDestination)
{
	FulowersMoveDestinationTop,
	FulowersMoveDestinationBottom,
	FulowersMoveDestinationLeft,
	FulowersMoveDestinationRight,
};
```
### 4.类的命名
整体采用大驼峰式命名(每个单词的首字母大写).
类前缀:采用开发者姓名的首字母大写.
类后缀:采用对应类的全称.
```objc
NavigationController  导航控制器: LBYNavigationController
ViewController        主页视图控制器: LBYHomeViewController
TableViewController   表格控制器: LBYTableViewController
TabBarController      标签控制器: LBYTabBarController
```
### 5.方法的命名
当方法参数在三个以及三个以上,换行保持对齐(冒号对齐,冒号前是参数变量,冒号后是参数值).
方法声明:
```objc
+ (instancetype)initWithPersonName:(NSString *)name
	                       withAge:(int)age
		                   withSex:(NSString *)sex
	                    withHeight:(float)height 
	                    withWeight:(float)weight;
```
方法调用:避免使用冒号对齐的方式.
### 6.属性和对象的命名
采用修饰+类型的方式命名,BOOL类型添加is前缀,单词遵循小驼峰命名法.声明属性时,小括号中的顺序依次是:nonatomic,readonly,strong.
```objc
@property (nonatomic, assign) BOOL isLogin;
@property (nonatomic, weak) UITextField *loginNameTextField;
@property (nonatomic, copy) NSString *studentClientName;
@property (nonatomic, weak) UILabel *loginTipLabel;
@property (nonatomic, weak) UIButton *loginButton;
```
## 三.注释
注释是为了解释说明,方法或变量等命名规范合理,清楚易懂,可以不添加注释,含有复杂逻辑的代码必须添加注释.注释需要实时更新,跟随代码的变动进行更改或者删除.
### 1.公开类方法注释
在.h文件中声明类方法,采用文档注释,要写明方法的具体作用,所有参数的含义以及返回的参数值.
```objc
/**
 创建person对象的类方法

 @param name 姓名
 @param age 年龄
 @param sex 性别
 @param height 身高
 @param weight 体重
 @return 返回person类对象
*/
+ (instancetype)initWithPersonName:(NSString *)name
	                       withAge:(int)age
	                       withSex:(NSString *)sex
	                    withHeight:(float)height
	                    withWeight:(float)weight;
```
### 2.私有的对象方法注释
在.m文件中实现对象方法,采用文档注释, 要写明方法的具体作用, 如果有参数和返回值,需要添加所有参数的含义以及返回的参数值.
```objc
/**
 搭建tableview的UI
*/
- (void)setupTableViewUI
```
### 3.方法内部逻辑代码注释
复杂逻辑代码在代码上方进行注释,注释方式采用双斜杠+单个空格+具体注释内容
```objc
- (void)viewDidLoad
{
	[super ViewDidLoad];
	// 注释
	if(...)
	{
		...
	} 
}
```
### 4.属性注释
```	objc
/**
 登陆按钮
*/
@property (nonatomic, weak) UIButton *loginBtn;
```
### 5.标记
在函数分组和使用#pragma mark - 给重要逻辑代码添加标记,方便阅读
```objc
#pragma mark - Lifecycle
- (instancetype)init 
- (void)dealloc 
- (void)viewDidLoad 
- (void)viewWillAppear:(BOOL)animated 
- (void)didReceiveMemoryWarning 

#pragma mark - IBActions
- (IBAction)submitData:(id)sender 
	
#pragma mark - Public
- (void)publicMethod 
	
#pragma mark - Private
- (void)privateMethod 
	
#pragma mark - Custom Protocol
- (void)tabbarBottomView:(LBYTabbarBottomView *)tabbarBottomView didSelectIndex:(NSUInteger)index didSelectBtn:(BYBottomButton *)selectBtn

#pragma mark - UITextFieldDelegate
- (void)textViewDidBeginEditing:(UITextView *)textView;
- (void)textViewDidEndEditing:(UITextView *)textView;
	
#pragma mark - UITableViewDataSource
- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section;
- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath;
	
#pragma mark - UITableViewDelegate
- (CGFloat)tableView:(UITableView *)tableView heightForRowAtIndexPath:(NSIndexPath *)indexPath;

#pragma mark - NSObject
- (NSString *)description 
```
### 6.打印
关于NSLog,在项目或者SDK完成最终版本时,需要去掉打印的注释(可以使用pch预编译文件来禁止NSLog打印)
## 四.格式化
### 1.赋值
在”=”号左右两边各间隔一个空格.
```objc
static const int count = 0; 
```
类方法或对象方法
方法的”+/-”号和左侧小括号间隔一个空格,大括号换行,上下大括号对齐.
```objc
- (void)viewDidLoad 
{

}
```
### 2.属性声明
property和左侧小括号间隔一个空格,逗号和下一个属性修饰符间隔一个空格,右侧小括号和属性类型间隔一个空格,属性类型和属性变量间隔一个空格.声明字符串类型时,NSString和*号间隔一个空格,*号和属性变量相连,应是```NSString *studentClientName```,不是```NSString* studentClientName```,也不是```NSString * studentClientName```.
```objc
@property (nonatomic, assign, readwrite) BOOL isLogin;
@property (nonatomic, copy, readwrite) NSString *studentClientName;
```
### 3.for循环
for和左侧小括号间隔一个空格, i和”<=”间隔一个空格, ”<=”和”3”间隔一个空格, ”3”后面紧跟着封号 ,封号和i间隔一个空格.大括号换行,一对大括号上下位置对齐.
```objc
for (int i = 0; i <= 3; i++)
{
	// 语句
}
```
### 4.条件语句
关于大括号,任何需要大括号的都不能省略.
采用
```objc
if (isLogin)
{	
	return success;
}
```
不是
```objc
if (isLogin)
	return success;
```
也不是
```objc
if (isLogin) return success;
```
### 5.case语句
当一个case语句包含多行代码时,大括号应该加上.如case 2所示.
```objc
switch (condition) 
{
	case 1:  
		// ...
		break;  
	case 2: 
	{
		// ...  
		// Multi-line example using braces  
		break;  
	}  
	case 3:   
		// ...  
		break;  
	default:   
		// ...  
		break;  
}
```
当在switch使用枚举类型时，default是不需要的.  
```objc
switch (FulowersMoveDestination) 
{
	case FulowersMoveDestinationTop:  
		// ...  
		break;  
	case FulowersMoveDestinationBottom:  
		// ...  
		break;  
    case FulowersMoveDestinationLeft:  
		// ...  
		break;  
	case FulowersMoveDestinationRight:  
		// ...  
		break;  
}
```
## 五.单例
统一采用shared+类名作为单例类的方法名
```objc
@implementation LBYNetworkTool
+ (instancetype)sharedBYNetworkTool
{
	static LBYNetworkTool *instance;
	{    
		static dispatch_once_t onceToken;
		dispatch_once(&onceToken, ^{        
			instance = [[LBYNetworkTool alloc]init];
		});
	}    
	return instance;
}
@end
```
## 六.UI整理
搭建UI时,使用setup作为方法名前缀,将相应UI布局放在对应方法中
```objc
- (void)setupTableViewUI
- (void)setupNavigationUI
- (void)setupCollectionViewUI
```

### 简书
[iOS开发之规范文档](http://www.jianshu.com/p/6fa5e599bbae) 