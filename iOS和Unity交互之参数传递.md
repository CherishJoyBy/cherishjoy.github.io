---
title: iOS和Unity交互之参数传递
date: 2017-04-20 16:22:08
tags: [iOS,Unity,iOS和Unity交互]
categories: iOS和Unity交互
---

>本文介绍了iOS和Unity交互,主要涉及参数的传递.(整个程序都处于Unity界面)

## 调用方法一:
### Unity调方法传参,有返回值.
Unity代码:
```c
[DllImport("__Internal")]
// 给iOS传string参数,有返回值,返回值通过iOS的return方法返回给Unity
private static extern string getIPv6(string mHost, string mPort)
```
### iOS通过return方法,将值返回给Unity
iOS代码:
```c
/**
获取IPv6的值,并返回给Unity

 @param mHost 主机名
 @param mPort 端口号
 @return IPv6值
 */
extern "C" const char *getIPv6(const char *mHost, const char *mPort)
{
    // strdup(const char *__s1) 复制mHost字符串,通过Malloc()进行空间分配 
    return strdup(mHost);
}
```
**注意:**
1.如果Unity传参为**string**类型,不执行strdup()方法而使用return mHost方法,导致mHost没有分配内存空间而报错.
报错信息:
```c
skins(2509,0x1a8e5cb40) malloc: *** error for object 0x16fdc9114: pointer being freed was not allocated
*** set a breakpoint in malloc_error_break to debug
```
2.如果Unity传参为**int**类型,可以使用return mHost方法.
Unity代码:
```c
[DllImport("__Internal")]
// 给iOS传int参数,无返回值,返回值通过iOS的return方法返回给Unity
private static extern int setMyInt(int date);
```
iOS代码:
```c
// 返回int值
extern "C" int setMyInt(int date)
{
    return date;
}
```
3.Unity方法中参数的变量名为date,在iOS中的**extern "C" int setMyInt(int date)**方法中设置的参数变量名可以与Unity的相同,设置为date,也可以是a、b、c等自定义参数变量,但是为了代码规范,尽量和Unity参数保持一致.
4.调用**DllImport("")**方法,需要引入命名空间:
**using System.Runtime.InteropServices;**
5.extern "C"修饰的变量和函数是按照C语言方式编译和连接的.

## 调用方法二:
### Unity调方法传参,无返回值.
Unity代码:
```c
// 传数据给iOS
[DllImport("__Internal")]
// 给iOS传string参数,无返回值,返回值通过iOS的UnitySendMessage方法返回给Unity
private static extern void setDate(string date);
```

```objc
// 接收iOS的数据
public void GetDate(string date)
{
}
```

### iOS调方法,传参给Unity
iOS代码:
```objc
/**
 返回Unity日期

 @param date 日期
 @return 无返回值
 */
extern "C" void setDate(const char *date)
{
    /**
     发送数据给Unity
     @param obj 模型名
     @param method Unity接收iOS数据的方法名
     @param msg 传给Unity的数据
     UnitySendMessage(const char* obj, const char* method, const char* msg);
    */
    UnitySendMessage("PublicGameObject", "GetDate", date);
}
```
**注意**
iOS通过UnitySendMessage方法返回数据给Unity时,需要传正确的date值,如果UnitySendMessage方法中的第三个参数不是将date作为参数,而是自定义的NSString类,需要做类型转换(Unity中的字符串为string,OC中的字符串为NSString)如下代码:
```objc
extern "C" void setDate(const char *date)
{
    NSString *dateStr = @"Hello Word";
    UnitySendMessage("PublicGameObject", "GetDate", [dateStr UTF8String]);
}
```

## 以下是全部代码
### Unity的.cs文件:
```c
using UnityEngine;
using System.Collections;
using System.Runtime.InteropServices;
using UnityEngine.UI;
public class Iossdk : MonoBehaviour
{
    // getIPv6方法单独使用,setDate和GetDate配合使用
    public InputField[] ips;

    [DllImport("__Internal")]
    // 给iOS传string参数,有返回值,返回值通过iOS的return方法返回给Unity
    private static extern string getIPv6(string mHost, string mPort)

    [DllImport("__Internal")]
    // 给iOS传string参数,无返回值,返回值通过iOS的UnitySendMessage方法返回给Unity
    private static extern void setDate(string date);

    [DllImport("__Internal")]
    // 给iOS传int参数,无返回值,返回值通过iOS的return方法返回给Unity
    private static extern int setMyInt(int date);

    // 传int参数给iOS
    public void SetMyInt()
    {
      #if UNITY_IPHONE && !UNITY_EDITOR
          int result = setMyInt(int.Parse(ips[1].text));
          Debug.Log(result);
      #else
          Debug.Log(int.Parse(ips[1].text));
      #endif
    }

    // 传string参数给iOS
    public void SetDate()
    {
      #if UNITY_IPHONE && !UNITY_EDITOR
          setDate(ips[0].text);
      #else
          Debug.Log(ips[0].text);
      #endif
    }

    // 接收iOS的数据
    public void GetDate(string date)
    {
      ips[1].text = date;
      Debug.Log(date);
    }

    // 通过主机名和端口号获取IPv6
    public static string GetIPv6(string mHost, string mPort)
    {
      #if UNITY_IPHONE && !UNITY_EDITOR
          string mIPv6 = getIPv6(mHost, mPort);
          return mIPv6;
      #else
          return mHost + " : " + mPort;
      #endif
    }
    
    // 程序入口1
    public void Click1()
    {
      string s = GetIPv6(ips[0].text, ips[1].text);
      Debug.Log(s);
    }

    // iOS程序入口2 
    public void Click2()
    {
      SetDate();
    }

    // iOS程序入口3 
    public void Click3()
    {
     SetMyInt();
    }
}
```
### iOS代码:
.h文件
```objc
  #import <Foundation/Foundation.h>

  @interface IOSToUnity : NSObject

  @end
```
.m文件
```objc
#import "IOSToUnity.h"
  
  @implementation IOSToUnity
  
  /**
   获取IPv6的值,并返回给Unity
  
   @param mHost 主机名
   @param mPort 端口号
   @return IPv6值
  */
extern "C" const char * getIPv6(const char *mHost, const char *mPort)
{
    // strdup(const char *__s1) 复制mHost字符串,通过Malloc()进行空间分配 
    return strdup(mHost);
}
  
  /**
   返回Unity日期
   
   @param date 日期
   @return 无返回值
  */
extern "C" void setDate(const char *date)
{
    /**
     发送数据给Unity
     @param obj 模型名
     @param method Unity接收iOS数据的方法名
     @param msg 传给Unity的数据
     UnitySendMessage(const char* obj, const char* method, const char* msg);
    */
    UnitySendMessage("PublicGameObject", "GetDate", date);
}
  
  /**
   返回int值
   
   @param mHost 主机名
   @return 主机名
   */
  extern "C" int setMyInt(int date)
  {
    return date;
  }
   
   @end
```

### 如果已经掌握本文参数传递的方法,不妨看下界面交互的部分.

### 简书
[iOS和Unity交互之参数传递](http://www.jianshu.com/p/86c4d9c9dafe)

[iOS和Unity交互之界面跳转](http://www.jianshu.com/p/edc784385aaf)