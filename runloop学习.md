# Runloop

---

## 是用来做什么的？
#####RunLoop 实际上就是一个对象，这个对象管理了其需要处理的事件和消息，并提供了一个入口函数来执行上面 Event Loop 的逻辑。线程执行了这个函数后，就会一直处于这个函数内部 "接受消息->等待->处理" 的循环中，直到这个循环结束（比如传入 quit 的消息），函数返回。
---

## 功能：
#####- 保持程序的持续运行
#####- 处理App中的各种事件（比如触摸事件、定时器事件、Selector事件
#####- 节省CPU资源，提高程序性能：该做事时做事，该休息时休息

#####当没有runloop的情况下，程序是从头执行到return 0就结束退出，生命周期结束。
#####当有runloop得情况下，程序可以循环执行，当前进程不会终止。
---

## 使用
###NSRunLoop（Foundation中的）
###CFRunLoopRef（Core Foundation中的）
- NSRunLoop和CFRunLoopRef都代表着RunLoop对象。
- NSRunLoop是基于CFRunLoopRef的一层OC包装，所以要了解RunLoop内部结构，需要多研究CFRunLoopRef层面的API（Core Foundation层面）。

#####CFRunLoopModeRef 列表


######简单的例子（计时器）
- 在textView中拖动时，才会调用run里面的方法。
- UITrackingRunLoopMode scrollView的拖动不受影响
```
NSTimer * timer = [NSTimer timerWithTimeInterval:3 target:self selector:@selector(run) userInfo:nil repeats:YES];
 [[NSRunLoop currentRunLoop] addTimer:timer forMode:UITrackingRunLoopMode];
self.imageView.image = [UIImage imageNamed:@"QQ20160805-0.png"];
```

###NSRunLoop与线程之间的使用
#####观察者（观察runloop的使用情况）
#####CFRunLoopActivity状态返回
- kCFRunLoopEntry = (1UL << 0)   <tr>返回值为1
- kCFRunLoopBeforeTimers = (1UL << 1)    <tr>返回值为2
- kCFRunLoopBeforeSources = (1UL << 2)  <tr>返回值为4
- kCFRunLoopBeforeWaiting = (1UL << 5)  <tr>返回值为32
- kCFRunLoopAfterWaiting = (1UL << 6)   <tr>返回值64
- kCFRunLoopExit = (1UL << 7)     <tr>返回值为128
- kCFRunLoopAllActivities = 0x0FFFFFFFU

- 创建一个观察者
```
CFRunLoopObserverRef observer = CFRunLoopObserverCreateWithHandler(CFAllocatorGetDefault(), kCFRunLoopAllActivities, YES, 0, ^(CFRunLoopObserverRef observer, CFRunLoopActivity activity) {
        NSLog(@"------ 活动的状态 -----%zd", activity);
    });
```

- 将创建的observer添加到当前loop中
```
CFRunLoopAddObserver(CFRunLoopGetCurrent(), observer, kCFRunLoopDefaultMode);
```

- 手动release（CF开头，有create的要删除。即使是在ARC）
```
CFRelease(observer);
```