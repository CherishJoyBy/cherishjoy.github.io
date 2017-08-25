---
title: iOS读书笔记之Objective-C高级编程(GCD)
date: 2017.07.02 00:54:03
tags: [GCD,读书笔记]
categories: iOS读书笔记
---

>本文主要对GCD的概念、API以及实现进行梳理.

### CCD的概念.
#### GCD,全称是Grand Central Dispatch,它是C语言的API.
GCD的核心 : 将block(任务)添加到queue(队列)中.

根据官方文档[ConcurrencyProgramingGuide](https://developer.apple.com/library/content/documentation/General/Conceptual/ConcurrencyProgrammingGuide/ConcurrencyandApplicationDesign/ConcurrencyandApplicationDesign.html#//apple_ref/doc/uid/TP40008091-CH100-SW8)中的描述:

>One of the technologies for starting tasks asynchronously is Grand Central Dispatch (GCD). This technology takes the thread management code you would normally write in your own applications and moves that code down to the system level. All you have to do is define the tasks you want to execute and add them to an appropriate dispatch queue. GCD takes care of creating the needed threads and of scheduling your tasks to run on those threads. Because the thread management is now part of the system, GCD provides a holistic approach to task management and execution, providing better efficiency than traditional threads.

翻译如下:
>Grand Central Dispatch(GCD)是异步执行任务的技术之一.一般将应用程序中记述的线程管理用的代码在系统级中实现.开发者只需定义想执行的任务并追加到适当的Dispatch Queue中,GCD就能生成必要的线程并执行任务,这样就比以前的线程更有效率.

#### GCD使用步骤
* 创建任务 : 确定具体要做的事.
    GCD中的任务是使用block封装的.
* 将任务添加到队列中
    GCD会自动将队列中的任务取出,放到对应的线程中执行.
    任务的取出遵循队列的FIFO原则 : 先进先出,后进后出.

#### 日常使用
```objc
    dispatch_queue_t queue = dispatch_get_global_queue(0, 0);
    /**
     * 长时间处理
     *
     * 例如 AR 用画像识别
     * 例如数据库访问
     */
    dispatch_async(queue, ^{
        
        /**
         * 长时间处理结束,主线程使用该处理结果
         */
        dispatch_async(dispatch_get_main_queue(), ^{
            
            /**
             * 只有在主线程可以执行的处理
             *
             * 例如用户界面更新
             */
        });
    });
```
在GCD之前,Cocoa框架提供了NSObject类的`performSelectorInBackground:withObject:`实例方法和`
performSelectorOnMainThread:withObject:waitUntilDone:`实例方法等简单的多线程编程技术.
以下方法等价于GCD的实现:
```objc
/**
 * NSObject performSelectorInBackground: withObject:方法中
 * 执行后台线程
 */
- (void)launchThreadByNSObject_performSelectorInBackground_withObject
{
    [self performSelectorInBackground:@selector(doWork) withObject:nil];
}

- (void)doWork
{
    /**
     * 因为书本是基于MRC环境所写,所以包含自动释放池,若在ARC环境下,以下语句会报错.
     */
    NSAutoreleasePool *pool = [[NSAutoreleasePool alloc] init];
    
    /**
     * 长时间处理
     *
     * 例如 AR 用画像识别
     * 例如数据库访问
     */
    
    /**
     * 长时间处理结束,主线程使用该处理结果
     */
    
    [self performSelectorOnMainThread:@selector(doneWork) withObject:nil waitUntilDone:NO];
    
    [pool drain];
}

- (void)doneWork
{
    /**
     * 只有在主线程可以执行的处理
     *
     * 例如用户界面更新
     */
}
```

#### 多线程执行原理总结
```
单核CPU同一时间,CPU只能处理1个线程,只有1个线程在执行任务.
多线程的同时执行 : 其实是CPU在多条线程之间快速切换(调度任务).
如果CPU调度线程的速度足够快,就造成了多线程同时执行的假象.
在多核CPU的情况下,就真正并行执行多个线程.
```

因为`长时间的处理(耗时操作)`,会妨碍主线程的运行循环的执行,所以需要进行多线程编程,如:`异步`创建`子线程`去处理`耗时操作`,耗时操作结束后,再`回到主线程刷新UI`.这种方式不会妨碍主线程的运行循环的执行,并能提高程序响应性能.
如图:

![主线程运行循环图](http://upload-images.jianshu.io/upload_images/3284707-6bc3e528d1f6af77.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/500)


### CCD的API.

#### **Dispatch Queue**

开发者要做的只是定义想执行的任务并追加到适当的Dispatch Queue中.
源代码表示:
```objc
    dispatch_async(queue, ^{
        
        /**
         * 想执行的任务
         */
    });
```
Dispatch Queue是执行处理的等待队列.Dispatch Queue按照追加的顺序(先进先出FIFO,First-In-First-Out)执行处理.
如图:

![通过Dispatch Queue执行处理](http://upload-images.jianshu.io/upload_images/3284707-f45caa56257e4edc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/500)

Dispatch Queue分为4种队列,分别是`Serial Queue`(串行队列)、`Concurrent Queue`(并发队列)、`Main Dispatch Queue`(主调度队列)、`Global Dispatch Queue`(全局并发队列).

根据官方文档[Dispatch Queues](https://developer.apple.com/library/content/documentation/General/Conceptual/ConcurrencyProgrammingGuide/OperationQueues/OperationQueues.html#//apple_ref/doc/uid/TP40008091-CH102-SW1)中的描述:

1.Serial Queue
>Serial queues (also known as private dispatch queues) execute one task at a time in the order in which they are added to the queue. 
If you create four serial queues, each queue executes only one task at a time but up to four tasks could still execute concurrently, one from each queue. 

翻译如下:
>串行队列（也称为私有调度队列）按顺序将其中一个任务添加到队列中,并且一次只执行一个任务.
如果创建四个串行队列,每个队列一次只执行一个任务,但最多四个任务可以并发执行,每个队列中有一个任务.

如图:

![只使用一线程](http://upload-images.jianshu.io/upload_images/3284707-fb678966aa43d656.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/500)

代码:
```objc
dispatch_queue_t queue = dispatch_queue_create("mySerialDispatchQueue", DISPATCH_QUEUE_SERIAL);
    dispatch_async(queue, ^{
        NSLog(@"一个丁老头,欠我两个蛋");
    });
    dispatch_async(queue, ^{
        NSLog(@"他说三天还,四天还没还");
    });
    dispatch_async(queue, ^{
        NSLog(@"仔细想一想,就是大傻蛋");
    });
    dispatch_async(queue, ^{
        NSLog(@"买了三根葱,花了3毛3");
    });
    dispatch_async(queue, ^{
        NSLog(@"买个大西瓜,用了6毛6");
    });
    dispatch_async(queue, ^{
        NSLog(@"买串糖葫芦,没了7毛7");
    });
```

根据串行队列特点,从上到下执行每个block中的任务,即`丁老头`,再`还钱`,再到`大傻蛋`,同时执行的处理只有1个.
结果如下:

![丁老头](http://upload-images.jianshu.io/upload_images/3284707-0056ed1e30144d29.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/200)

2.Concurrent Queue

>Concurrent queues (also known as a type of global dispatch queue) execute one or more tasks concurrently, but tasks are still started in the order in which they were added to the queue. The currently executing tasks run on distinct threads that are managed by the dispatch queue. 

翻译如下:

>并发队列（也称为全局调度队列）同时执行一个或多个任务，但任务仍然按照它们添加到队列的顺序执行.当前执行的任务运行在不同线程上,而这些线程由调度队列所管理.

如图:

![使用多个线程](http://upload-images.jianshu.io/upload_images/3284707-f3653596b94fc20c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/500)

将`DISPATCH_QUEUE_SERIAL`更改为`DISPATCH_QUEUE_CONCURRENT`.

代码:
```objc
dispatch_queue_t queue = dispatch_queue_create("myConcurrentDispatchQueue", DISPATCH_QUEUE_CONCURRENT);
```

那么丁老头的执行顺序就是不确定的,即并行执行.
注意:`在不能改变执行的处理顺序或不想并行执行多个处理时使用Serial Dispatch Queue.`

3.Main Dispatch Queue

>The main dispatch queue is a globally available serial queue that executes tasks on the application’s main thread. This queue works with the application’s run loop (if one is present) to interleave the execution of queued tasks with the execution of other event sources attached to the run loop. 

翻译如下:

>主调度队列是一个全局可用的串行队列,它在应用程序的主线程上执行任务.该队列与应用程序的运行循环（如果有的话）一起工作,即该队列的任务与运行循环的其他事件源交叉执行.

如图:

![主队列执行情况](http://upload-images.jianshu.io/upload_images/3284707-6c3c25dd9e0997a8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/500)

4.Global Dispatch Queue

>The system provides each application with four concurrent dispatch queues. These queues are global to the application and are differentiated only by their priority level. Because they are global, you do not create them explicitly. Instead, you ask for one of the queues using the [dispatch_get_global_queue](https://developer.apple.com/documentation/dispatch/1452927-dispatch_get_global_queue)
 function

翻译如下:

>系统为每个应用程序提供四个并发调度队列.这些队列对应用程序而言是全局的,而且只对它们的优先级进行区分.因为它们是全局的，所以不需要明确地创建它们.相反,你可以用dispatch_get_global_queue函数的获取其中一个队列.

优先级如下:

名称 | Dispatch Queue的种类 | 说明
:-----------: | :-----------: | :-----------: 
Main Dispatch Queue | Serial Diapatch Queue | 主线程执行
Global Dispatch Queue(High Priority) | Concurrent Dispatch Queue | 执行优先级:高(最高优先级)
Global Dispatch Queue(Default Priority) | Concurrent Dispatch Queue | 执行优先级:默认
Global Dispatch Queue(Low Priority) | Concurrent Dispatch Queue | 执行优先级:低
Global Dispatch Queue(Background Priority) | Concurrent Dispatch Queue | 执行优先级:后台

代码:
```objc
// 主队列
dispatch_queue_t mainDiapatchQueue = dispatch_get_main_queue();
// 高优先级全局并发队列
dispatch_queue_t globalDiapatchQueueHigh = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_HIGH, 0);
// 默认先级全局并发队列
dispatch_queue_t globalDiapatchQueueDefault = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0);
// 低优先级全局并发队列
dispatch_queue_t globalDiapatchQueueLow = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_LOW, 0);
// 后台先级全局并发队列
dispatch_queue_t globalDiapatchQueueBackground = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_BACKGROUND, 0);
```

总结:
- 串行队列
串行队列里面的任务,都是`按顺序依次调度执行`,前面一个任务`未执行完`,后面的任务不会被调度执行.

- 并发队列 == 全局并发队列
可以同时调度多个任务同时的执行.

- 主队列
主队列由系统创建好的,可直接获取.
主队列中的任务,一定是在主线程中执行.主要进行线程间的通信.
只有在主线程空闲的时候,主队列才会调度其中的任务执行.

- 全局队列 
全局队列又叫全局并发队列.
由系统创建,可直接获取,调度任务的效果等同于并发队列.

#### **dispatch_queue_create**

该ApI主要是为了生成dispatch queue.
代码:
```objc
/**
 * 第1个参数:指定队列的名称
 * 第2个参数:指定队列的类型
 */
// 生成串行队列
dispatch_queue_t mySerialDiapatchQueue = dispatch_queue_create("MySerialDiapatchQueue", DISPATCH_QUEUE_SERIAL);
// 生成并发队列
dispatch_queue_t myConcurrentDiapatchQueue = dispatch_queue_create("MyConcurrentDiapatchQueue", DISPATCH_QUEUE_CONCURRENT);
```
因为一个串行队列,只生成并使用一个线程,所以创建几个串行队列就生成几个线程,线程过多,会消耗大量内存,影响系统响应性能.

问题:
多个线程竞争同一资源时,会出现数据安全问题.

![数据安全问题](http://upload-images.jianshu.io/upload_images/3284707-586f1a2a16ddbc3f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/500)


例子:
线程A、线程B、线程C都准备使用数据data,data的值为36,`线程A先将data值从36改为20`,此时线程B要使用data,他要读取data值,在正常情况下,`线程B读取到的值应该为36`,但是data的值在线程A中被改成20,导致线程B获取到data值不正确.C的事就不说了,我B都错了,难道你C还想要对的数据?

解决:
使用Serial Diapatch Queue可以保证数据的安全.

![解决](http://upload-images.jianshu.io/upload_images/3284707-a95e193a3e60e949.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/500)

例子:
还是A、B、C三兄弟,A在用data,B也想用data,但是B只能乖乖等着,A用完data,data值是否改变不重要,因为,B这时候用的是A的结果.不会产生安全问题.`好像C就是打酱油的,没排上用场,好吧,此处省略C一万字...`

书中提及:`虽然存在ARC自动管理内存,但是生成的Dispatch Queue必须由程序员负责释放`.所以我测试了,在ARC环境下,用书中方法对队列进行release操作,但是提示`ARC forbids explicit message send of 'release'`错误,所以ARC已经对队列进行了引用计数的增减,不需要我们手动release了.

#### **dispatch_set_target_queue**

该API主要用于变更生成的队列的执行优先级的.
通过`dispatch_queue_create`生成的队列,不论串行队列还是并发队列,它们与`默认优先级的全局并发队列`的优先级相同.所以,可以通过`dispatch_set_target_queue`,将串行队列的优先级改为在后台执行.
代码:
```objc
// 串行队列
dispatch_queue_t mySerialDiapatchQueue = dispatch_queue_create("MySerialDiapatchQueue", DISPATCH_QUEUE_SERIAL);
// 后台先级全局并发队列
dispatch_queue_t globalDiapatchQueueBackground = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_BACKGROUND, 0);
/**
 * 第1个参数:准备变更优先级的队列
 * 第2个参数:变更后的相同优先级的的队列
 */
// 变更优先级
dispatch_set_target_queue(mySerialDiapatchQueue, globalDiapatchQueueBackground);
```
书中提及此方法的作用:多个串行队列,假设有3个串行队列A、B、C,A、B、C串行队列原本是并行执行的,每个队列执行自己的一个任务.但是通过`dispatch_set_target_queue`方法,将A的优先级设置成B的优先级,此时,只能执行A中的任务.因此这种方法,可以防止处理并行执行.

#### **dispatch_after**

dispatch_after这个是大家常用的API,主要进行延迟操作.但是延迟只说明再一定时间后,将任务添加到队列中执行,真正开始执行任务,可能因为主线程本身的处理有延迟,导致时间不准确.
代码
```objc
/**
 * dispatch_time:获取dispatch_time_t类型的时间
 * DISPATCH_TIME_NOW:当前时间
 * NSEC_PER_SEC:单位秒
 */
dispatch_time_t time = dispatch_time(DISPATCH_TIME_NOW, (int64_t)(3ull * NSEC_PER_SEC));

    //  dispatch_after:在3秒后,将任务添加到队列中
    dispatch_after(time, dispatch_get_main_queue(), ^{
        NSLog(@"wait at least three second");
    });
```
附上第2个参数的时间说明
```objc
NSEC：纳秒
USEC：微妙
SEC：秒
PER：每
#define NSEC_PER_SEC 1000000000ull 每秒有多少纳秒
#define NSEC_PER_MSEC 1000000ull 每毫秒有多少纳秒
#define USEC_PER_SEC 1000000ull 每秒有多少微秒
#define NSEC_PER_USEC 1000ull 每微妙有多少纳秒
```

#### **dispatch group**

dispatch group即调度组.
作用:调度组中的所有异步任务执行结束之后,会得到统一的通知.

使用场景
监听一组异步任务是否执行结束,如果执行结束就能够得到统一的通知.

```objc
// 创建默认优先级的全局并发队列
dispatch_queue_t queue = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_HIGH, 0);

// 创建调度组
dispatch_group_t group = dispatch_group_create();
dispatch_group_async(group, queue, ^{
    NSLog(@"下载图片A");
});
dispatch_group_async(group, queue, ^{
    NSLog(@"下载图片B");
});
dispatch_group_async(group, queue, ^{
    NSLog(@"下载图片C");
});
dispatch_group_notify(group, dispatch_get_main_queue(), ^{
    NSLog(@"处理下载完成的图片");
});
```
打印结果:
```objc
2017-07-01 13:26:15.620007+0800 BYGCDDemo[368:102016] 下载图片B
2017-07-01 13:26:15.620175+0800 BYGCDDemo[368:102016] 下载图片C
2017-07-01 13:26:15.620219+0800 BYGCDDemo[368:102016] 下载图片A
2017-07-01 13:26:15.635798+0800 BYGCDDemo[368:101977] 处理下载完成的图片
```
或
```objc
2017-07-01 13:26:15.620007+0800 BYGCDDemo[368:102016] 下载图片C
2017-07-01 13:26:15.620175+0800 BYGCDDemo[368:102016] 下载图片B
2017-07-01 13:26:15.620219+0800 BYGCDDemo[368:102016] 下载图片A
2017-07-01 13:26:15.635798+0800 BYGCDDemo[368:101977] 处理下载完成的图片
```
总之,下载图片A、B、C是并行执行顺序不一,但是,处理下载完成的图片的操作一定是在最后执行,且只在A、B、C操作结束后,调度组收到结束通知,才去执行`处理下载完成的图片`.

可以使用`dispatch_group_wait`方法实现以上相同功能
```objc
dispatch_queue_t queue = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_HIGH, 0);
dispatch_group_t group = dispatch_group_create();
dispatch_group_async(group, queue, ^{
    NSLog(@"下载图片A");
});
dispatch_group_async(group, queue, ^{
    NSLog(@"下载图片B");
});
dispatch_group_async(group, queue, ^{
    NSLog(@"下载图片C");
});

/**
 * 第1个参数:执行任务的调度组
 * 第2个参数:等待时间
 */
long result = dispatch_group_wait(group, DISPATCH_TIME_FOREVER);
if (result == 0)
{
    NSLog(@"处理下载完成的图片");
}
else
{
    NSLog(@"以上调度组中存在未完成的任务");
}
```
当调度组中的方法未执行完成,`dispatch_group_wait`方法中的`DISPATCH_TIME_FOREVER`属于永久等待状态,且`dispatch_group_wait`只调用不返回,即result的值恒不等0,执行`dispatch_group_wait`方法所在的线程停止,直到调度组中的任务完成,执行`dispatch_group_wait`方法所在的线程开始工作,result为0,所以可以根据result值判断,图片是否下载完成.

若将以上代码中的`DISPATCH_TIME_FOREVER`更改为`DISPATCH_TIME_NOW`,那么当程序执行到`dispatch_group_wait `方法,直接进行判断,而不用等待.此时,A、B、C的下载工作可能都还没执行完,就出现下列结果:
```objc
2017-07-01 14:52:14.498329+0800 BYGCDDemo[421:115796] 以上调度组中存在未完成的任务
2017-07-01 14:52:14.498421+0800 BYGCDDemo[421:115825] 下载图片B
2017-07-01 14:52:14.498473+0800 BYGCDDemo[421:115825] 下载图片C
2017-07-01 14:52:14.498350+0800 BYGCDDemo[421:115828] 下载图片A
```
不过推荐使用`dispatch_group_notify`方法,主要是简化代码.

#### **dispatch_barrier_async**

`dispatch_barrier_async`在并发执行任务的队列中追加处理任务,该任务在等待前面并发任务执行完成之后才执行,当`dispatch_barrier_async`中的任务执行完成,才会继续执行后续的并发执行的任务.

如图:

![dispatch_barrier_async追加的处理](http://upload-images.jianshu.io/upload_images/3284707-185cbde8895abd82.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/500)

代码:

```objc
dispatch_queue_t queue = dispatch_queue_create("MyConcurrentDiapatchQueue", DISPATCH_QUEUE_CONCURRENT);
dispatch_async(queue, ^{
    NSLog(@"第1次读取data值");
});
dispatch_async(queue, ^{
    NSLog(@"第2次读取data值");
});
dispatch_async(queue, ^{
    NSLog(@"第1次写入data值");
});
dispatch_async(queue, ^{
    NSLog(@"第3次读取data值");
});
dispatch_async(queue, ^{
    NSLog(@"第4次读取data值");
});
```

分析:
假设,data初始值为`36`,因为这些任务是并发执行的,可能先执行了`第1次写入data值`,写入之后的值为`20`之后再进行`第1次读取data值`,导致读取到值`20`与期望值`36`不符合,所以为了避免这种情况发生,将`第1次写入data值`的方法改为:`dispatch_barrier_async`,这样,程序先并发执行`第1次读取data值`和`第2次读取data值`,等待这两个任务执行完成,才开始`dispatch_barrier_async`中的任务,当`dispatch_barrier_async`中的任务完成后,继续并发执行`第3次读取data值`和`第4次读取data值`.这样既保证了`前两次读取数据`的正确以及`第1次写入操作`的成功,也保证`后续读取操作`能正确读取`被改写过的值`.

总结:`dispatch_barrier_async 方法可实现高效率数据库的数据访问和文件访问.`

#### **dispatch_sync**

`dispatch_sync`方法意味着`同步`,即将指定的任务`同步`地添加到指定的队列中,在追加其他任务之前,会一直等待.
在主线程中调用`dispatch_sync`方法会引起死锁问题.
代码:
```objc
dispatch_queue_t queue = dispatch_get_main_queue();
    dispatch_sync(queue, ^{
        NSLog(@"我非要在主队列中执行同步任务");
    });
```
或
```objc
dispatch_queue_t queue = dispatch_get_main_queue();

dispatch_async(queue, ^{
    
    dispatch_sync(queue, ^{
        NSLog(@"我非要在主队列中执行同步任务");
    });
});
```

分析:
在上文`主队列`的概念中提到,只有在主线程空闲的时候,才会执行主队列中的任务.仔细查看以上两种方法,当主队列想要执行队列中的block,就必须等待主线程空闲,但是,此时主队列的代码正在主线程中运行,所以主线程是不空闲,既然不空闲,那么主队列的block就无法执行.
一句话就是:主队列的block等待主线程空闲,但主队列自己却占着主线程的位置.

建议使用:`dispatch_async` + `主队列`.

#### **dispatch_apply**

dispatch_apply也会等待全部处理执行结束.
代码:
```objc
/**
 * 第1个参数:重复的次数
 * 第2个参数:执行任务的队列
 * 第3个参数:追加的处理
 */
dispatch_queue_t queue = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0);
dispatch_apply(10, queue, ^(size_t index) {
    NSLog(@"%zu",index);
});
NSLog(@"done");
```
结果:
```objc
4
1
0
3
5
2
6
8
9
7
done
```
第3个参数是`带有参数的block`,为了区分追加到队列中block.
如果对数组中的每个对象进行处理.推荐使用以下方法:
```objc
NSArray *arr = [NSArray arrayWithObjects:@"10",@"20",@"30", nil];
dispatch_queue_t queue = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0);
dispatch_async(queue, ^{
   
    // 等待 dispatch_apply 方法中全部处理执行结束
    dispatch_apply(arr.count, queue, ^(size_t index) {
        // 并列处理包含在数组中全部对象
        NSLog(@"%zu:%@",index,[arr objectAtIndex:index]);
    });
    dispatch_sync(dispatch_get_main_queue(), ^{
       
        NSLog(@"用户界面更新");
    });
});
```

#### **dispatch_suspend**

dispatch_suspend挂起队列,表示队列中还未执行的任务会暂停,已经执行的任务会继续执行.
```objc
dispatch_suspend(queue);
```

#### **dispatch_resume**

dispatch_suspend恢复队列,表示队列中还未执行的任务会恢复执行.
```objc
dispatch_resume(queue);
```

#### **dispatch semaphore**

`dispatch semaphore`是持有计数的信号.计数为0等待,计数为1或大于1,减去1而不等待.
`dispatch semaphore`创建方式:
```objc
// 1表示计数的初始值
dispatch_semaphore_t semaphore = dispatch_semaphore_create(1);
```
遇到以下情况:
```objc
dispatch_queue_t queue = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0);

NSMutableArray *arr = [[NSMutableArray alloc] init];
for (int i = 0; i < 10000; i++)
{
    dispatch_async(queue, ^{
        
        [arr addObject:[NSNumber numberWithInt:i]];
    });
}
```
开启太多子线程,势必导致程序响应问题,以及内存错误,导致崩溃.
解决:
```objc
dispatch_queue_t queue = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0);
/**
 * 生成dispatch semaphore
 *
 * dispatch semaphore的计数初始值为1
 *
 * 保证可同时访问NSMutableArray类对象的线程
 * 同时只能有一个
 */
dispatch_semaphore_t semaphore = dispatch_semaphore_create(1);
NSMutableArray *arr = [[NSMutableArray alloc] init];
for (int i = 0; i < 10000; i++)
{
    dispatch_async(queue, ^{
        /**
         * 等待dispatch semaphore
         *
         * 一直等待,直到计数值大于等于1
         */
        dispatch_semaphore_wait(semaphore, DISPATCH_TIME_FOREVER);
        
        /**
         * 由于dispatch semaphore的计数值大于等于1
         * 所以将dispatch semaphore的计数值减去1
         * dispatch_semaphore_wait方法执行返回
         *
         * 执行到此时的dispatch semaphore的计数恒为0
         * 由于可访问NSMutableArray类对象的线程只有1个
         * 因此可安全地进行更新
         */
        [arr addObject:[NSNumber numberWithInt:i]];
        
         // dispatch_semaphore_signal 方法将dispatch semaphore的计数加1.
        dispatch_semaphore_signal(semaphore);
    });
}
```
分析:
通过计数值去判断当前线程能否处理数组对象,计数初始值是1,跳过`dispatch_semaphore_wait`方法,处理对象之后,再减去1,值为0,此时若继续添加线程,肯定无法跳过`dispatch_semaphore_wait`方法,只能等待上一个线程通过`dispatch_semaphore_signal`方法把计数值改成1,即上一个线程已经完成对数组的处理,才能开始下一个线程的处理任务.

#### **dispatch_once**

这是常用来创建单例的方法.
```objc
static ViewController *instance;
static dispatch_once_t onceToken;
dispatch_once(&onceToken, ^{
    instance = [[ViewController alloc] init];
});
```
分析:
在执行到`dispatch_once(&onceToken, ^{`时打断点,可以看到onceToken结果是0,但是在末行`});`打断点,结果是1027,这个值是不确定,所以,每次执行`dispatch_once`时,都会判断`onceToken`的值,如果是0,执行block中的代码,非0,则不执行,达到创建单例的效果.

### CCD的实现.
用于实现Dispatch Queue而使用的软件组件.

组件名称 | 提供技术
:-----------: | :-----------: 
libdispatch | Diapatch Queue
Libc ( pthreads ) | pthread_workqueue
XNU 内核 | workqueue

GCD的API全部为在libdispatch库中的C语言函数.
Dispatch Queue执行步骤:
1.GCD初始化时,使用`pthread_workqueue_create_np`函数生成`pthead_workqueue`.`pthead_workqueue`包含在Libc提供的`ptheads API`,它使用`bsdthead_register`和`workq_open`系统调用.
2.`dispatch queue`通过结构体和链表,被实现为FIFO队列.block不是直接加入FIFO队列.它先加入`dispatch continuation`这一`dispatch_continuation_t`类型结构体中,然后再加入FIFO队列.
3.当在`global dispatch queue`中执行block时,`libdispatch`从`global dispatch queue`自身的FIFO队列中取出`dispatch continuation`,调用`pthread_workqueue_additem_np`函数.将`global dispatch queue`自身、符合其优先级的workqueue信息以及为执行`dispatch continuation`的回调函数传递给参数.
4.`pthread_workqueue_additem_np`函数使用`workq_kernreturn`系统调用,通知`workqueue`增加应当执行的项目.根据该通知,XNU内核基于系统状态判断是否要生成线程.如果是`OverCommit`优先级的`global dispatch queue`,workqueue则始终生成线程.
5.`workqueue`的线程执行`pthread_workqueue`函数,该函数调用`libdispatch`的回调函.在该回调函数中中执行加入到`dispatch continuation`的`block`.
6.`block`执行结束后,进行通知`dispatch group`结束、释放`dispatch continuation`等处理,开始准备执行加入到`global dispatch queue`中的下一个`block`.

### 简书
[iOS读书笔记之Objective-C高级编程(GCD)](http://www.jianshu.com/p/0ceae11385a7) 