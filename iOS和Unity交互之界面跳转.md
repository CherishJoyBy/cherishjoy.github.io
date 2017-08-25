---
title: iOS和Unity交互之界面跳转
date: 2017.05.24 14:40:01
tags: [iOS,Unity,iOS和Unity交互]
categories: iOS和Unity交互
---

>本文介绍了iOS和Unity交互,主要涉及两个界面之间的跳转.

>如果对iOS和Unity交互传参方法不熟悉的朋友,可以参考我的另一篇文章
[iOS和Unity交互之参数传递](http://www.jianshu.com/p/86c4d9c9dafe)

### 一.程序启动入口.
####  main.mm
了解OC或者C的朋友一定知道`main方法`,这是整个程序的入口.以下是Unity转iOS工程后的main文件中的部分代码.
```objc
  const char* AppControllerClassName = "UnityAppController";

int main(int argc, char* argv[])
{
	@autoreleasepool
	{
		UnityInitTrampoline();
		UnityParseCommandLine(argc, argv);

		RegisterMonoModules();
		NSLog(@"-> registered mono modules %p\n", &constsection);
		RegisterFeatures();

		std::signal(SIGPIPE, SIG_IGN);
                
        // 程序启动入口
        UIApplicationMain(argc, argv, nil, [NSString stringWithUTF8String:AppControllerClassName]);
	}

	return 0;
    }
```
根据代码得知,程序需要创建UnityAppController对象.那么,程序就来到了UnityAppController文件.
在UnityAppController.mm文件中的以下方法中添加打印:`NSLog(@"%s",__func__);`
```objc
  - (id)init
  - (void)startUnity:(UIApplication*)application
  - (BOOL)application:(UIApplication*)application willFinishLaunchingWithOptions:(NSDictionary*)launchOptions
  - (BOOL)application:(UIApplication*)application didFinishLaunchingWithOptions:(NSDictionary*)launchOptions
  - (void)applicationDidBecomeActive:(UIApplication*)application
```
打印结果为:
```objc
2017-05-24 04:50:09.597338+0800 ProductName[5622:1888712] [DYMTLInitPlatform] platform initialization successful
2017-05-24 04:50:09.693476+0800 ProductName[5622:1888655] -> registered mono modules 0x100df3fa0
2017-05-24 04:50:09.714814+0800 ProductName[5622:1888655](标记) -[UnityAppController init]
2017-05-24 04:50:09.930542+0800 ProductName[5622:1888655] -[UnityAppController application:willFinishLaunchingWithOptions:]
2017-05-24 04:50:09.931002+0800 ProductName[5622:1888655](标记) -[UnityAppController application:didFinishLaunchingWithOptions:]
-> applicationDidFinishLaunching()
2017-05-24 04:50:10.013760+0800 ProductName[5622:1888655] Metal GPU Frame Capture Enabled
2017-05-24 04:50:10.014789+0800 ProductName[5622:1888655] Metal API Validation Enabled
2017-05-24 04:50:10.178127+0800 ProductName[5622:1888655](标记) -[UnityAppController applicationDidBecomeActive:]
-> applicationDidBecomeActive()
2017-05-24 04:50:10.190176+0800 ProductName[5622:1888655](标记) -[UnityAppController startUnity:]
Init: screen size 640x1136
Initializing Metal device caps: Apple A7 GPU
Initialize engine version: 5.3.5f1 (960ebf59018a)
UnloadTime: 2.714958 ms
Setting up 1 worker threads for Enlighten.
Thread -> id: 16ea3b000 -> priority: 1 
```
根据带`(标记)`的打印结果得知
1.程序会先调用`  - (BOOL)application:(UIApplication*)application didFinishLaunchingWithOptions:(NSDictionary*)launchOptions`方法,进行Unity界面初始化并布局UI.
2.准备激活Unity,已激活程序则调用`- (void)applicationDidBecomeActive:(UIApplication*)application`方法,方法中设置了`UnityPause(0);`表示Unity为启动状态,在方法最后,执行`[self performSelector:@selector(startUnity:) withObject:application afterDelay:0];`.
3.调用`- (void)startUnity:(UIApplication*)application`方法,展示Unity界面.

### 二.Unity跳转iOS界面.
程序启动为Unity界面,通过点击`跳转iOS按钮`,调用`unityToIOS`方法创建iOS界面并将iOS创建的控制器设置为窗口的跟控制器.以实现跳转iOS界面.
.cs代码
```c
using UnityEngine;
using System.Collections;
using System.Runtime.InteropServices;
using SClassLibrary;

public class Test : MonoBehaviour
{
    public GameObject cube;

    // DllImport这个方法相当于是告诉Unity，有一个unityToIOS函数在外部会实现。
    // 使用这个方法必须要导入System.Runtime.InteropServices;
    [DllImport("__Internal")]
    private static extern void unityToIOS(string str);

    // 向右转函数接口
    public void turnRight(string num)
    {
        float f;
        if (float.TryParse(num, out f))
        {// 将string转换为float，IOS传递数据只能用以string类型
            Vector3 r = new Vector3(cube.transform.rotation.x, cube.transform.rotation.y + f, cube.transform.rotation.z);
            cube.transform.Rotate(r);
        }
    }
    // 向左转函数接口
    public void turnLeft(string num)
    {
        float f;
        if (float.TryParse(num, out f))
        {// 将string转换为float，IOS传递数据只能用以string类型
            Vector3 r = new Vector3(cube.transform.rotation.x, cube.transform.rotation.y - f, cube.transform.rotation.z);
            cube.transform.Rotate(r);
        }
    }

    public void DllTest()
    {
        var user = new User
        {
            Id = 1,
            Name = "张三"
        };
  #if UNITY_IPHONE && !UNITY_EDITOR
        unityToIOS(user.ToString());
  #else
        Debug.Log(user.ToString());
  #endif
    }
  }
```
添加属性,该属性用来保存创建的iOS控制器(尽量设置为私有属性)
```objc
@interface UnityAppController ()
@property (nonatomic, strong) UIViewController *vc;
@end
```
Unity会调用`unityToIOS`方法,跳转iOS界面之前,先暂停Unity,即`UnityPause(true);`方法.因为在C语言中,不能直接使用self调用对象方法.所以需要通过`GetAppController()`调用`setupIOS`方法.`GetAppController()`即UnityAppController类型的对象.在`setupIOS`方法中,让
UnityAppController对象持有vc后,再将vc直接设置为窗口的跟控制器`GetAppController().window.rootViewController = GetAppController().vc;`
unityToIOS方法
  ```objc
// 跳转iOS界面
 extern "C" void unityToIOS(char *str)
 {
     // Unity传递过来的参数
     NSLog(@"%s",str);
     UnityPause(true);
     
     // GetAppController()获取appController，相当于self
     // 设置iOS界面,GetAppController()获取appController，相当于self
     [GetAppController() setupIOS];
     
     // 点击按钮后跳转到IOS界面,设置窗口的跟控制器为iOS的控制
     GetAppController().window.rootViewController = GetAppController().vc;
 }
```
setupIOS 方法
```objc 
// 设置iOS界面
- (void)setupIOS
 {
    UIViewController *vc = [[UIViewController alloc] init];
    vc.view.backgroundColor = [UIColor greenColor];
    vc.view.frame = [UIScreen mainScreen].bounds;
    
    UIButton *btn = [[UIButton alloc]initWithFrame:CGRectMake(70, 530, 180, 30)];
    btn.backgroundColor = [UIColor whiteColor];
    [btn setTitle:@"跳转到Unity界面" forState:UIControlStateNormal];
    [btn setTitleColor:[UIColor blackColor] forState:UIControlStateNormal];
    [btn addTarget:self action:@selector(setupUnity) forControlEvents:UIControlEventTouchUpInside];
    
    [vc.view addSubview:btn];
     
     self.vc = vc;
     NSLog(@"设置界面为IOS界面");
     self.window.rootViewController = vc;
 }
```
说明:
1.GetAppController()
跳转到`GetAppController()`方法内部,实现如下,可以看出,该方法获取到`UIApplication`的单例类,而它的代理,则为`UnityAppController`对象,最后再使用`(UnityAppController*)`进行强制转换.所以,在`UnityAppController.mm`文件中使用`GetAppController()`相当于self.
```objc
inline UnityAppController *GetAppController()
{
	return (UnityAppController*)[UIApplication sharedApplication].delegate;
}
```
2.UnityGetGLViewController()
返回`Unity的根控制器`,根控制器上的视图是`Unity的视图`.,如果将`窗口的根控制器`设置为`UnityGetGLViewController()`,其实就是将`Unity界面`显示在`手机`上.
```objc
extern "C" UIViewController *UnityGetGLViewController()
{
     return GetAppController().rootViewController; 
}
```
3.UnityGetGLView()
返回Unity视图,这个视图其实就是显示在`UnityGetGLViewController()`上的.
```objc
extern "C" UIView *UnityGetGLView()
{
    return GetAppController().unityView; 
}
```

### 三.iOS跳转Unity界面.
实现iOS界面中的按钮的方法.来跳转到Unity界面.`self.rootViewController`的作用相当于`GetAppController().rootViewController`,然后设置window的rootViewController为Unity的跟控制器
setupUnity 方法
```objc
// 设置Unity界面
 - (void)setupUnity
 {
     // 设置Unity状态为开启状态
     UnityPause(false);
     
     // 设置rootViewController为Unity的跟控制器
     self.window.rootViewController = self.rootViewController;
     // 等同于
     // self.window.rootViewController = UnityGetGLViewController();
     NSLog(@"设置rootView为Unity界面");
 }
```

### 四.封装界面跳转代码.
查看`UnityAppController.mm`文件,发现其中代码太多,为了减少代码以及便于我们管理和维护,我们要创建一个单例类来管理Unity和iOS界面互相跳转的操作.之前在`UnityAppController.mm`文件中写的代码全部删除.
#### 1.需要创建一个自定义类,如:`BYJumpEachOther`,继承至`NSObject`.

#### 2.添加属性
```objc
  // 存储的iOS控制器
  @property (nonatomic, strong) UIViewController *vc;
```
#### 3.入口:unityToIOS,与之前不同的是,调用`setupIOS `方法,改为单例对象去调用`[[BYJumpEachOther sharedInstance] setupIOS];`获取vc也通过单例对象去获取.
```objc
// 跳转iOS界面
extern "C" void unityToIOS(char *str)
{
    // Unity传递过来的参数
    NSLog(@"%s",str);

    // 跳转到IOS界面,Unity界面暂停
    UnityPause(true);
    
    // GetAppController()获取UnityAppController对象
    // UnityGetGLView()获取UnityView对象,相当于_window
    
    [[BYJumpEachOther sharedInstance] setupIOS];
    
    // 点击按钮后跳转到IOS界面,设置界面为IOS界面
    GetAppController().window.rootViewController = [BYJumpEachOther sharedInstance].vc;
}
```
#### 4.添加BYJumpEachOther创建单例的方法
```objc
  + (instancetype)sharedInstance
{
    static BYJumpEachOther *instance;
    static dispatch_once_t onceToken;
    dispatch_once(&onceToken, ^{
        instance = [[BYJumpEachOther alloc] init];
    });
    return instance;
}
```
#### 5.跳转iOS界面代码
```objc
// 设置iOS界面
  - (void)setupIOS
{
    UIViewController *vc = [[UIViewController alloc] init];
    vc.view.backgroundColor = [UIColor greenColor];
    vc.view.frame = [UIScreen mainScreen].bounds;
    
    UIButton *btn = [[UIButton alloc]initWithFrame:CGRectMake(70, 530, 180, 30)];
    btn.backgroundColor = [UIColor whiteColor];
    [btn setTitle:@"跳转到Unity界面" forState:UIControlStateNormal];
    [btn setTitleColor:[UIColor blackColor] forState:UIControlStateNormal];
    [btn addTarget:self action:@selector(setupUnity) forControlEvents:UIControlEventTouchUpInside];
    
    [vc.view addSubview:btn];
    
    self.vc = vc;
    NSLog(@"设置界面为IOS界面");
    
    GetAppController().window.rootViewController = vc;
}
```
#### 6.跳转Unity界面代码
```objc
// 设置Unity界面
- (void)setupUnity
{
    UnityPause(false);
    
    GetAppController().window.rootViewController = UnityGetGLViewController();
    // 等同于
    // GetAppController().window.rootViewController = GetAppController().rootViewController;
    NSLog(@"设置rootView为Unity界面");
}
```

### CSDN
[iOS和Unity交互之界面跳转](http://blog.csdn.net/cherish_joy/article/details/72770624)

### GitHub
[iOS和Unity界面交互Demo(未封装版)](https://github.com/CherishJoyBy/IosToUnityNotEncapsulated)

[iOS和Unity界面交互Demo(封装版)](https://github.com/CherishJoyBy/IosToUnityEncapsulated)
### 简书
[iOS和Unity交互之界面跳转](http://www.jianshu.com/p/edc784385aaf)