---
typora-copy-images-to: ipic
---

[TOC]

## runloop

> 一个线程只有一个runloop 主线程默认开启 ，子线程刚创建时并没有`RunLoop`子线程默认没有开启

- 子线程开启任务一般只能跑一次 就被销毁 需要开启线程保活来不停运行任务 自己设置销毁时机

```c
struct __CFRunLoop {            
    pthread_t _pthread;
    CFMutableSetRef _commonModes; //标记为common的mode的集合
    CFMutableSetRef _commonModeItems;  //commonMode的item集合
    CFRunLoopModeRef _currentMode;// 当前的模式
    CFMutableSetRef _modes;// CFRunLoopModeRef类型的集合，相对NSArray有序，Set为无序集合
};

typedef struct __CFRunLoopMode *CFRunLoopModeRef;
struct __CFRunLoopMode {
    CFMutableSetRef _sources0;
    CFMutableSetRef _sources1;
    CFMutableArrayRef _observers;
    CFMutableArrayRef _timers;
}
```

### Mode

> 每个runloop有多个mode  多个`mode`里，有且仅有一个为`currentMode`

![WeChata76d35e93d3e44025489eae76f129709](https://tva1.sinaimg.cn/large/e6c9d24egy1h4b8l1uxkmj20rs0ocdi7.jpg)

- 如果要切换`Mode`，只能退出当前 Loop，再重新指定一个`Mode` 进入。这样做主要是为了分隔开不同组的 `Source/Timer/Observer`，让其互不影响

####  事件源

> 触发事件的方式

- input source

  ```objective-c
    // 一个联合体，说明 source 要么为 source0，要么为 source1
      union {
          CFRunLoopSourceContext version0;  /* immutable, except invalidation */
          CFRunLoopSourceContext1 version1; /* immutable, except invalidation */
      } 
  ```

  - source0 不能唤醒runloop 调用source0时内部可能是通知mach port来唤醒runloop

    - 触摸事件处理


    - 手动触发 custom事件 hitTest:withEvent
    - performSelector事件 
    
    ​```objective-c
    //点击屏幕 bt 函数调用栈信息
    - (void)touchesBegan:(NSSet<UITouch *> *)touches withEvent:(UIEvent *)event {
        NSLog(@"点击屏幕");
    }
    frame #8: 0x0000000180360820 CoreFoundation`__CFRUNLOOP_IS_CALLING_OUT_TO_A_SOURCE0_PERFORM_FUNCTION__ + 24
    frame #9: 0x0000000180360720 CoreFoundation`__CFRunLoopDoSource0 + 204
    ​```
    
    ​```objective-c
    //下面这个方法同样产生source0
    [self performSelector:@selector(performEvent) onThread:self.innerThread withObject:nil waitUntilDone:YES];
    - (void)performEvent {
        NSLog(@"处理Perform事件");
    }
    ​```

  - source1  **包含了一个 mach_port 和一个回调（函数指针）**Runloop的睡眠和唤醒

    - 基于mach_Port的, 系统内核或者其他进程或线程的事件 可以主动唤醒休眠中的RunLoop

    - mach_port大家就理解成进程间相互发送消息的一种机制就好,对应**port的事件源**

    - 简单举个例子：一个APP在前台静止着，此时，手指点击屏幕，首先产生的是一个系统事件，通过source1来接受捕捉,source1唤醒RunLoop, 然后将事件Event由**Springboard**程序包装成`source0`

      分发给app处理

      ![08A3DA44-AECD-4A18-AA59-F995D4280760](https://tva1.sinaimg.cn/large/e6c9d24egy1h4d6qprbhpj20xg0gsgmg.jpg)

    ![3A8D0CFC-DF04-4890-8F0A-81B7BEDE4A3C](https://tva1.sinaimg.cn/large/e6c9d24egy1h4d6csd37rj21f20gc0tx.jpg)

- timer source

  - **CFRunLoopTimerRef**  
  - 定时器事件 
  - [performSelector: withObject: afterDelay:]

#### observer

- Runloop的状态切换时，都会被`observer`监听到
- ​

### 休眠的实现

> 用户状态切换到内核态 当前线程不干事情 等待source1唤醒

- mach_msg()内核


### 线程保活

> 主线程不要关闭子线程的runloop

- waitUntilDone设置为`NO` 表示子线程执行任务时不用等待 其他主线程也可以同时进行  YES表示必须等待执行完成



## 多线程

### GCD

- sync  当前线程 也就是在主线程执行 不能并发 不能开新线程

- async  子线程  异步能开新线程 不是每一个子线程都开新的线程 多个任务能共用同一条线程

  ```objective-c
  //100个异步任务 可能只会开30条线程
  dispatch_queue_t q = dispatch_queue_create( "dantesx" , DISPATCH_QUEUE_CONCURRENT );
  for  ( int  i = 0; i<100; i++) {
    dispatch_async(q, ^{
      NSLog (@ "%@ %d" , [ NSThread  currentThread], i);
    });
  }
  ```

  ​

#### 死锁

- 队列FIFO 原则
- **使用sync函数往当前串行队列添加任务会死锁** 串行必须一步一步往下执行

```objective-c
//主线程互相等待死锁
- (void)viewDidLoad {
    [super viewDidLoad];
    NSLog(@"0");
    // 等
    dispatch_sync(dispatch_get_main_queue(), ^{
        NSLog(@"1");
    });
    NSLog(@"2");
//   dispatch_sync(dispatch_get_main_queue(), ^任务块需要 1完成打印才能返回(这里与打印没有任何关系 是dispatch_sync这个队列与block块之间的相互等待)  打印1又在任务块的后面 同步又是按顺序来执行 所以相互等待
}
```

#### 异步  不会阻塞调用线程 

```objective-c
//异步并发队列中 只打印13  afterDelay是NStimer 在runloop中 异步生成子线程 runloop默认没有开启
-(void)touchesBegan:(NSSet<UITouch *> *)touches withEvent:(UIEvent *)event{
    dispatch_queue_t queue2 = dispatch_queue_create("myqueu2", DISPATCH_QUEUE_CONCURRENT);
    dispatch_async(queue2, ^{
        NSLog(@"1");
        [self performSelector:@selector(test2) withObject:nil afterDelay:.0];
        NSLog(@"3");
    });
}
-(void)test2{
    NSLog(@"2");
}
//异步并发 打印顺序 16 2345 2345顺序固定 只有一条子线程 队列遵循先进先出原则 所以2345按顺序打印
    NSLog(@"1");
    dispatch_queue_t queue = dispatch_get_global_queue(0, 0);
    //A线程
    dispatch_async(self.queue, ^{
        NSLog(@"2");
        NSLog(@"3");
        NSLog(@"4");
        NSLog(@"5");
    });
  NSLog(@"6");
//如果在开一条线程B A和B的打印顺序随机 每条线程都有自己的队列 每一个任务都在它自己的线程上执行
```

- 异步主队列

  ```objective-c
  //所有任务都在主队列 FIFO原则 for循环是排在任务最后面 所以最后打印
  dispatch_queue_t q = dispatch_get_main_queue();
  // 2. 安排一个任务
  for  ( int  i = 0; i<10; i++) {
       dispatch_async(q, ^{
           NSLog (@ "%@ %d" , [ NSThread  currentThread], i);
       });
  }
  NSLog (@ "睡会" );
  [ NSThread  sleepForTimeInterval:2.0];
  NSLog (@ "come here" );
  ```

  ​

### 全局与并发

- 全局队列：没有名称 本质是一个并发队列
- 并发队列：有名字，和 NSThread 的 name 属性作用类似  dispatch_barrier必须使用自定义

### group

```objective-c
dispatch_group_t group = dispatch_group_create();
dispatch_group_enter(group);// 进入组
dispatch_group_async()
dispatch_group_notify

```



### 优缺点

- 多个线程访问同一个资源 ，会引发数据错乱和数据安全

```objective-c
- (void)ticketTest{
    self.ticketsCount = 10;
    dispatch_queue_t queue = dispatch_get_global_queue(0, 0);
    dispatch_async(queue, ^{
        for (int i = 0; i < 5; i++) {
            [self saleTicket];
        }
    });
    dispatch_async(queue, ^{
        for (int i = 0; i < 5; i++) {
            [self saleTicket];
        }
    });
}
- (void)saleTicket{
    int oldTicketsCount = self.ticketsCount;
    oldTicketsCount--;
    self.ticketsCount = oldTicketsCount;
    NSLog(@"还剩%d张票 - %@", oldTicketsCount, [NSThread currentThread]);
}

```



- 解决方案：使用线程同步技术

### 线程同步 Lock  

> 防止多个线程访问同一个变量  同一时间只能做一件事情 同时读取不用加锁 同时改需要

- 需求：两个任务虽然是异步的，但仍需要同步执行


- 互斥锁和自旋锁
  - 假设有一个线程加锁成功，其他线程加锁自然会失败，失败线程的处理方式如下
    - **互斥锁**加锁失败后，线程释放CPU，给其他线程
    - **自旋锁**加锁失败后，线程会忙等待，直到它拿到锁
  - 自旋锁使用  cpu一直占用资源
    - 其他线程等待时间比较短
    - 多核处理器


- osspinlock  自旋锁  利用cpu周期 一直自旋 直到锁可用

  ```objective-c
  //1.先执行task2，拿到spin_lock，由于任务比较耗时，未执行完，继续持有锁。 2.紧接着执行task1，等待锁释放。由于优先级高， task1线程也尝试获得这个锁，它会处于spin lock 的忙等状态，占用大量CPU, 此时low priority线程无法与high priority线程争夺CPU时间，从而导致任务无法正常完成，以及释放锁
  task1：high priority，要使用资源data
  task2：low priority，要使用资源data
  spin_lock：一把自旋锁
  ```

  ​

- os_unfair_lock **推荐**

- pthread_mutex 递归锁 ，互斥锁 等待锁的线程会处于休眠状态

  - 允许同一个线程对一把锁进行重复加锁

- dispatch_semaphore **推荐**   [ˈseməfɔːr]

  - 当红灯0 说明此时T2不能进去 当 T1出去之后  dispatch_semaphore_wait执行-1

     dispatch_semaphore_signal +1 灯为1 变绿灯 T2进来 （临界区）

  ![3970D83E-6F46-4CCF-9D17-76A848FD6682](https://tva1.sinaimg.cn/large/e6c9d24egy1h4fk49cl94j21d40o4aef.jpg)

- NSLock 互斥锁

- DISPATCH_QUEUE_SERIAL 串行队列保证线程同步

- dispatch_semaphore_t

  - 初始值 控制线程并发访问的最大值

  ```objective-c
  // 根据信号量的值，判断是休眠还是继续执行代码 设置初始值之后 >0 后面每次进来就-1 直到=0 就开始休眠
      dispatch_semaphore_wait(self.semaphore, DISPATCH_TIME_FOREVER);
  // 信号量的值加一
      dispatch_semaphore_signal(self.semaphore);

  ```

  ​


### 读写安全

- atomic 原子性 不可分割

  ```objective-c
   // Atomic retain release world
      spinlock_t& slotlock = PropertyLocks[slot];
      slotlock.lock();
      id value = objc_retain(*slot);
      slotlock.unlock();
  @property(atomic,strong)NSMutableArray *arr;
  self.arr = [NSMutableArray array];//这句操作set方法 加锁了 线程安全
  [self.arr addObject:@"1"];//这句不是线程安全 addObject和set方法没关系了
  ```

  - 保证属性的setter和getter内部线程同步 原子性操作

- nonatomic

- **多读单写**同一时间 只能一个线程写  允许多个线程读 不能既有写的操作 又有读的操作 

  - pthread rwlock
  - dispatch_barrier_(a)sync 解决数据竞争问题 同步函数无需栅栏函数
    - 栅栏函数仅与并发队列（DISPATCH_QUEUE_CONCURRENT）有效
    - 异步栅栏挡住后面的任务执行 必须自己里面的执行完 但是可以和前面的任务并行

### 建立线程监控预警体系

- 通过线程的销毁和创建来记录线程数量

## 内存管理

### 循环引用

- NSTime

  ```objective-c
  //timer内部target强引用 不能释放
  self.timer = [NSTimer scheduledTimerWithTimeInterval:1 target:self selector:@selector(timerTest) userInfo:nil repeats:YES];
  //解决方案1
  __weak typeof(self)weakSelf = self;
      self.timer = [NSTimer scheduledTimerWithTimeInterval:1 repeats:YES block:^(NSTimer * _Nonnull timer) {
          [weakSelf timerTest];
      }];
  //利用中间对象去解决 NSProxy消息转发实现target方法
  ```

- NSProxy 转发消息的场所 实现了NSObject协议，类似于 NSObject 的根类

  - 必须要实现它的 forwardInvocation: 和 methodSignatureForSelector: 方法

  - NSObject 寻找方法顺序：本类 -> 父类 -> 动态方法解析 -> 备用对象 -> 消息转发；

     NSproxy 寻找方法顺序：本类-> 消息转发；

### GCD定时器

- dispatch_source_create + dispatch_semaphore_wait解决  semaphore保证对字典操作同一块数据安全

### 内存分区

- void*指针 64位占8个字节 isa也是 函数指针也是8字节

####栈区(Stack) FILO

- 地址空间在iOS中`通常以0x7开头`
- 栈区一般是`由编译器来自动分配和释放的`，主要用来存储
  - `局部变量`
  - `函数的参数`，例如函数的隐藏参数（id self，SEL_cmd）
- 栈的内存较小（iOS主线程栈大小1MB，其它线程512KB）
- 存储数据不灵活（存储内容基本固定，由编译器分配）

####堆区 (Heap) FIFO

- 堆的地址空间在iOS中`通常以0x6开头`
- 堆区一般是`由开发者自己分配和释放的`，同时系统也会在必要的时候对堆区存储的内容进行回收和释放（`系统检测属性或者对象引用计数为零时，进行回收`）
  - OC使用alloc、new、block或者使用copy创建的对象都会存在这里
- 访问堆区内存时，一般是先通过对象读取到对象所在的栈区的指针地址，然后通过指针地址访问堆区

####全局区

- 在iOS一般是`以0x1开头` 在程序`运行的过程中该数据就一直存在`，程序`结束后再由系统释放`
- 未初始的全局变量
- 未初始化静态变量

####常量区

- `初始化过得全局变量`
- `初始化过得静态变量`

####代码区

- 编译之后的代码 00110111汇编这种

### Tagged Pointer

- Tag + Data

### 深浅拷贝

- 针对不可变对象使用copy修饰，针对可变对象使用strong修饰 
  - 对于可变对象类型，如NSMutableString、NSMutableArray等则不可以使用copy修饰
    - copy修饰可变返回的也是不可变对象 不能使用可变对象的方法
  - strong 可以用来修饰可变和不可变 所以strong对象用copy操作时 是深拷贝 内容不一样 strong的对象可以修改 copy出来的不能修改


- 根据有没产生新对象判断 （开辟新内存） 和指针没啥关系

- copy去拷贝可变对象时 因为原对象可变 拷贝出来的不可变 所以不能共用同一块内存 因此深拷贝(此变量是声明在局部区间 不是属性声明)

  ![761CA8A0-E661-4F3E-9C3A-FA7FEEBEBA1E](https://tva1.sinaimg.cn/large/e6c9d24egy1h4fu1a5yjlj20t00f475u.jpg)

### copy 

1. 当拷贝对象为不可变时 （因为拷贝目的就是不修改原来的值 所以都指向一块内存）

- **只要调用了copy 返回的都是不可变**

- 拷贝的只是指针 和原对象指向同一块内存地址 

  ```objective-c
  //copy修饰的对象都是 使用NSMutableString也不会影响copy返回值不可变
  @property(nonatomic,copy)NSMutableString *name;
  NSLog(@"%p %p", self.name, str);
  //不管左边什么类型 都是不可变对象
  NSMutableString *str  = [self.name copy];
  ```

2. 当拷贝对象为可变时  深拷贝

   ```objective-c
   //程序会崩溃  s2为可变对象 但是copy之后所有的对象为不可变 所以 s3不可能和s2指向同一块内存 s3 不管什么对象修饰 都是不可变  但是 这个拷贝为深拷贝
   NSMutableString *s2 = [NSMutableString stringWithFormat:@"asdaasfffafd"];
   NSMutableString *s3 = [s2 copy];
   [s3 appendString:@"ass"];
   NSLog(@"s3 %p %p", s2, s3);
   ```

###mutableCopy  

- 开辟新的内存  内容拷贝 深拷贝

###引用计数

- 存储在isa union 位域 extra_rc 19位里 不够的话存储到散列表

### weak

> 首先weak修饰会生成弱引用hash表 （用来维护指向对象的所有weak指针）根据对象地址的值作为key 去weak表里面查找 弱引用指针（referentes）及其所指向的对象（referent）（key value）

- Runtime所有的弱引用都由StripedMap内的64个全局SideTable（散列表）进行管理 
- 利用哈希算法根据对象地址找一个SideTable。SideTable又包含一个专门用来管理弱引用的weak_table


- weak_entry_t *weak_entries = weak_table->weak_entries; 


### AutoreleasePool

![760B269E-A461-4EA0-8846-3FFAD67C2208](https://tva1.sinaimg.cn/large/e6c9d24egy1h4gkqhndp6j217a0oq77u.jpg)

- 每个AutoreleasePoolPage对象占用4096字节内存  所有的AutoreleasePoolPage对象通过双向链表的形式连接在一起
- App 启动后，苹果在主线程 RunLoop 里注册了两个 Observer 其回调都是 _wrapRunLoopWithAutoreleasePoolHandler() 第一个 Observer 监视的事件是 Entry (即将进入 Loop )
- 第二个 Observer 监视了两个事件: BeforeWaiting (准备进入休眠) 时调用 `_objc_autoreleasePoolPop()` 和 `_objc_autoreleasePoolPush()`释放旧的池并创建 新池; Exit (即将退出 Loop ) 时调用`_objc_autoreleasePoolPop()`来释放自动释放

## 性能优化

### 帧 FPS

- 显卡渲染  60FPS -> 1s画了60张图
- 左上角逐行扫描到右下角

### 卡顿

> vsync到来之前 GPU还没有渲染完成  60FPS的话 一帧就是16ms  16ms之内完成渲染 1s = 1000ms 16*60=960ms

![BDB4B357-5BFD-45B0-9633-6EB5467E69DC](https://tva1.sinaimg.cn/large/e6c9d24egy1h4gm373oenj20vg084dg3.jpg)

- CPU：负责对象的创建和销毁、对象属性的调整、布局计算、文本的计算和排版、图片的格式转换和解码、图像的绘制(Core Graphics)
- GPU: 负责纹理的渲染(将数据渲染到屏幕)

#### 解决方案

cpu

1. 用不到事件处理的地方 可以calayer替换uiview
2. 控制并发数量  
3. 耗时操作放在子线程
   - 文本绘制
   - 图片处理 （解码 绘制）

gpu

- 避免段时间大量图片显示
- 避免离屏渲染
  - 正常渲染都是在帧缓冲区
  - 需要专门开辟一块离屏缓冲区造成存储量开销，在离屏缓冲区和帧缓冲区中切换上下文造成性能开销
  - **光栅化（shouldRasterize）**

### 耗电

- 少用定时器
- 优化I/O操作
  - 避免频繁写入小数据 最好批量一次性写入
- 网络优化
  - 减少 压缩网络数据
  - 断点续传
- 定位优化

### 启动优化

冷启动 Cold launch  Mach[mɑːkˌmæk] 

![C5574D5B-3E87-42AE-B68B-CF6631B21CBA](https://tva1.sinaimg.cn/large/e6c9d24egy1h4gpr03l2vj21ig0fcgnc.jpg)

- 对于pre-main阶段，在 Xcode 中 Edit scheme -> Run -> Auguments 将环境变量DYLD_PRINT_STATISTICS 设为1 
- dyld
  - 动态链接库 Mach-O文件
  - rebase指针调整(在镜像内部调整指针的指向)和bind符号绑定 (将指针指向镜像外部的内容)
- runtime
  - ObjC类，方法越多 启动越慢
  - ObjC的+load越多，启动越慢
    - 延迟调用 不必要的可以等到冷启动结束调用
  - C++静态对象越多，启动越慢
- main

main函数之后

- 即从main()开始，到appDelegate的didFinishLaunchingWithOptions方法执行完毕
- 推迟减少I/O操作
- 优化串行操作
- 闪屏页使用

热启动

- app已经在内存中

### 打包体积

资源

- 无损压缩
- 去除没有用到的资源

可执行文件

- 编译器优化
- 编写LLVM检测

link map

## 架构与设计

### mvc

model-view-controller

- View和Model实现了分离，但是View与Controller仍是紧耦合

### mvp

model-presenter-view

### mvvm

model-view-viewmodel

### 分层结构

界面层 -》业务层-〉网络层

### 设计模式

> 提高代码可读性、可扩展性、可维护性和可测性的最佳实践

- 策略模式

  - 有点类似 ios 协议 其他类遵守协议去实现各自的方法

- 单例模式

  - 扩展困难   单一职责过重
  - 系统内存中只存在一个对象，因此可以节约系统资源
  - 适用一些需要频繁创建和销毁的对象

  ​



