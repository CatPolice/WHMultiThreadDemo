# WHMultiThreadDemo
iOS多线程详细Demo

Demo的运行gif图如下：

![example5.gif](http://upload-images.jianshu.io/upload_images/3873004-91f6923a8fc230d3.gif?imageMogr2/auto-orient/strip)

### iOS多线程详细讲解请看这里：**[iOS多线程](http://www.jianshu.com/p/7649fad15cdb)**

下面是文章的摘选：

# 目的
本文主要是分享iOS多线程的相关内容，为了更系统的讲解，将分为以下7个方面来展开描述。
> 1. 多线程的基本概念
> 2. 线程的状态与生命周期
> 3. 多线程的四种解决方案：pthread，NSThread，GCD，NSOperation
> 4. 线程安全问题
> 5. NSThread的使用
> 6. GCD的理解与使用
> 7. NSOperation的理解与使用

Demo  在这里：**[WHMultiThreadDemo](https://github.com/remember17/WHMultiThreadDemo)**
Demo的运行gif图如下：

![example5.gif](http://upload-images.jianshu.io/upload_images/3873004-91f6923a8fc230d3.gif?imageMogr2/auto-orient/strip)

# 一、多线程的基本概念
> 进程：可以理解成一个运行中的应用程序，是系统进行资源分配和调度的基本单位，是操作系统结构的基础，主要管理资源。

> 线程：是进程的基本执行单元，一个进程对应多个线程。

> 主线程：处理UI，所有更新UI的操作都必须在主线程上执行。不要把耗时操作放在主线程，会卡界面。

> 多线程：在同一时刻，一个CPU只能处理1条线程，但CPU可以在多条线程之间快速的切换，只要切换的足够快，就造成了多线程一同执行的假象。

线程就像火车的一节车厢，进程则是火车。车厢（线程）离开火车（进程）是无法跑动的，而火车（进程）至少有一节车厢（主线程）。多线程可以看做多个车厢，它的出现是为了提高效率。
多线程是通过提高资源使用率来提高系统总体的效率。

> 我们运用多线程的目的是：将耗时的操作放在后台执行！

![多线程](http://upload-images.jianshu.io/upload_images/3873004-d3cd2ad81b66685d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 二、线程的状态与生命周期
线程的生命周期是：新建 - 就绪 - 运行 - 阻塞 - 死亡

下面分别阐述线程生命周期中的每一步
> 新建：实例化线程对象

> 就绪：向线程对象发送start消息，线程对象被加入可调度线程池等待CPU调度。

> 运行：CPU 负责调度可调度线程池中线程的执行。线程执行完成之前，状态可能会在就绪和运行之间来回切换。就绪和运行之间的状态变化由CPU负责，程序员不能干预。

> 阻塞：当满足某个预定条件时，可以使用休眠或锁，阻塞线程执行。sleepForTimeInterval（休眠指定时长），sleepUntilDate（休眠到指定日期），@synchronized(self)：（互斥锁）。

> 死亡：正常死亡，线程执行完毕。非正常死亡，当满足某个条件后，在线程内部中止执行/在主线程中止线程对象

> 还有线程的exit和cancel
[NSThread exit]：一旦强行终止线程，后续的所有代码都不会被执行。
[thread cancel]取消：并不会直接取消线程，只是给线程对象添加 isCancelled 标记。

# 三、多线程的四种解决方案
多线程的四种解决方案分别是：pthread，NSThread，GCD， NSOperation。
>pthread：运用C语言，是一套通用的API，可跨平台Unix/Linux/Windows。线程的生命周期由程序员管理。
>NSThread：面向对象，可直接操作线程对象。线程的生命周期由程序员管理。
>GCD：代替NSThread，可以充分利用设备的多核，自动管理线程生命周期。
>NSOperation：底层是GCD，比GCD多了一些方法，更加面向对象，自动管理线程生命周期。

# 四、线程安全问题
当多个线程访问同一块资源时，很容易引发数据错乱和数据安全问题。就好比几个人在同一时修改同一个表格，造成数据的错乱。

解决多线程安全问题的方法
> 方法一：互斥锁（同步锁）
```objc
@synchronized(锁对象) {
    // 需要锁定的代码
}
```
判断的时候锁对象要存在，如果代码中只有一个地方需要加锁，大多都使用self作为锁对象，这样可以避免单独再创建一个锁对象。
加了互斥做的代码，当新线程访问时，如果发现其他线程正在执行锁定的代码，新线程就会进入休眠。

> 方法二：自旋锁
加了自旋锁，当新线程访问代码时，如果发现有其他线程正在锁定代码，新线程会用死循环的方式，一直等待锁定的代码执行完成。相当于不停尝试执行代码，比较消耗性能。
属性修饰atomic本身就有一把自旋锁。

下面说一下属性修饰nonatomic 和 atomic
```objc
nonatomic 非原子属性,同一时间可以有很多线程读和写
atomic 原子属性(线程安全)，保证同一时间只有一个线程能够写入(但是同一个时间多个线程都可以取值)，atomic 本身就有一把锁(自旋锁)

atomic：线程安全，需要消耗大量的资源
nonatomic：非线程安全，不过效率更高，一般使用nonatomic
```

### NSThread，GCD，NSOperation的理解与使用，请到这里继续阅读：**[iOS多线程](http://www.jianshu.com/p/7649fad15cdb)**