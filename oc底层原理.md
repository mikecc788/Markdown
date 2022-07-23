---
typora-copy-images-to: ipic
---

[TOC]

### 任务列表 

- [x] eat

- [x] heat


 - [x] [check this url](https://www.baidu.com)
 - [ ] run

## 标注

Test1[^1]

Demo[^2]

[^2]: demo2

`print("hello")`

## 上标和下标

h~2~o

x^2^

h~2~o

x^2^

## 公式

$$sum_{i=1}^n a_i=0$$

$$sum$$

## C++

```shell
feellife@apps-iMac isa指针01 % xcrun -sdk iphoneos clang -arch arm64 -rewrite-objc main.m 
```



## 字节数据

> __LP64__  表示在64位系统下

1. 0xff 1111 负数在计算机中用补码 -1 ->原码 1001 反码 1110  补码 1111即0xff

## 内存对齐

- .收尾工作:结构体的总大小，也就是sizeof的结果，**必须是其内部最大成员的整数倍**，不足的要补齐

1. ``` c
    struct DRStruct1 {    
    double a;    char   b;    int c;    char d; 
    }MyStruct;
       分析：- a占8个字节 
     -b占用一个字节 根据内存对齐规则 起始位置（第八个位置）为所占空间的整数倍（b所占空间为1，所以b不需要调整 ，
     - c占4个字节 但是c的起始位置是9，根据对齐规则起始位置应该为4的倍数 只能是4*2 4*3，所以从第12个位置开始 
     - d 1个字节 且位置16 符合规则  
     
    ```
   ```

   ![image.png](https://tva1.sinaimg.cn/large/e6c9d24egy1h32e8ind0mj20xc06z74p.jpg)

   2. ```c
      typedef struct DRStruct1 {
          char   b;
          double c;
          char d;
      }MyStruct;
      sizeof(MyStruct) = 24
      
      struct DRStruct2 {
          char d;
          char   b;
          MyStruct c;
      }MyStruct1;
      sizeof(MyStruct1) = 32
        
       分析 -d 起始位置为0，占用位置为0
      	- b占用1个字节，起始位置为1，占用位置为1
        - c MyStruct占用24字节，起始位置为2，根据对其规则，其中最大的为double类型8个字节，故后延查找8的整数倍为8，由此得出从起始位置为8，占用位置为（8-31），共占用31个字节，为8的整数倍，符合对齐规则，得出MyStruct1的sizeof为32.
   ```

   ![image.png](https://tva1.sinaimg.cn/large/e6c9d24egy1h32eqft5ycj20xc05k3z8.jpg)

## 对象的分类

- 生成main.cpp文件

  ```
  feellife@apps-iMac isa指针01 % xcrun -sdk iphoneos clang -arch arm64 -rewrite-objc main.m 

  ```

### instance

- 存储的信息包括
  - isa 指针 8个字节 作用 调用父类方法
  - 其他成员变量
  - isa的地址在对象的最前面 所以也就是对象的地址
- 不存储**实例对象方法**

### class对象

- class的superclass指针

  > 调用实例方法 

  - student 调用init方法时 init属于NSObject 先通过isa找到student的class对象 在通过superclass找到 person的class ==》person的superclass找到 NSObject的class

  ![74AC9D7A-7C45-4F9C-929F-90C8F632AD57](https://tva1.sinaimg.cn/large/e6c9d24egy1h3i4hg50aqj21sk0tegqz.jpg)


- 存储**实例对象方法**

![img](https://upload-images.jianshu.io/upload_images/6545546-81240313e9a503dc.png?imageMogr2/auto-orient/strip|imageView2/2/format/webp)

- isa指针
- super_class指针

### meta-class

- **存储类对象**


- 元类是一个类对象的类  （也是一个对象，所以也必须有类指针）

- 使用super_class 指针来指向 superclass

- 调用类方法流程

  > 调用类方法时 通过isa找到student的meta-class 通过meta-class里面的superclass找到person的meta-class --》〉


### isa superclass

- **instance的isa指向class  class的isa指向meta-class meta-class的isa指向 root-class的meta-class**


- subclass 子类 Student  Superclass Person  rootclass NSObject 
  - Student继承自Person类

- [x] ​

------

![img](https://upload-images.jianshu.io/upload_images/3044508-18e0ce1fedc91a37.jpeg?imageMogr2/auto-orient/strip|imageView2/2/format/webp)

- subclass查找实例方法流程

  ![1105A5B6-2F5E-4336-A376-C1D900042385](https://tva1.sinaimg.cn/large/e6c9d24egy1h3j5bppdpnj20oi10qae1.jpg)

- subclass查找类方法

  ![6E274132-9F0F-441D-B25B-6312FC2644C5](https://tva1.sinaimg.cn/large/e6c9d24egy1h3j64l749wj20v80u0aer.jpg)

### class和meta-class inside结构

```
struct objc_object {
    Class _Nonnull isa;
};

struct objc_class : objc_object {
    Class superclass;
    cache_t cache;
    class_data_bits_t bits;    // 用户获取类的具体信息
}
```



## KVO

```
[self.person addObserver:self forKeyPath:@"age" options:options context:nil]; 
//context 上下文传递
```

- 添加监听的属性 会使用runtime动态生成派生类-NSKVONotifying_person ，并且让instance的isa指向全新的类，是Person的子类 里面的superlcass指针指向Person  通过foundation底层方法改变值

- 没有监听属性的对像isa 打印依然是Person  object_getClassName 打印

  ```objective-c
  (lldb) p object_getClassName(self.person)
  (const char * _Nonnull) $0 = 0x0000600002063360 "NSKVONotifying_Person"
  //  person2是没有添加观察的类 isa指向的是class对象 按照之前的图
    (lldb) p object_getClassName(self.person2)
  (const char * _Nonnull) $1 = 0x00000001026572de "Person"
  ```

- 打印地址值

  ```objective-c
   NSLog(@"%p --%p",[self.person methodForSelector:@selector(setAge:)],[self.person2 methodForSelector:@selector(setAge:)]);
  (lldb) p (IMP) 0x100381d94
  (IMP) $0 = 0x0000000100381d94 (KVO`-[Person setAge:] at Person.h:13)
  (lldb) p (IMP) 0x1807ab9bc
  (IMP) $1 = 0x00000001807ab9bc (Foundation`_NSSetIntValueAndNotify)
  ```

### NSKVONotifying_person实现

![img](https://upload-images.jianshu.io/upload_images/3333296-36bcadffb5a684bd.png?imageMogr2/auto-orient/strip|imageView2/2/format/webp)

- 调用Foundation _NSSetIntValueAndNotify方法 实现

  ```objective-c
  [self willChangeValueForKey:@"age"];调用自己的方法
  [super setAge:age]; 调用原类Person的set方法 修改属性值
  [self didChangeValueForKey:@"age"];再回来调用自己的方法
  ```

- Foundation封闭 可以通过重写父类方法也就是Person类添加 willchange 和didchange 方法验证 



### 应用场景

- 看instance是否有set方法

- 直接修改属性不会触发 

  ```
  通过暴露属性去修改不会触发
  {
    @public
    int _age;
  }
  ```

  ​

## KVC

-  person类里定义cat对象 修改cat成员的值

  ```
   //cat对象必须要存在
      [self.person setValue:@49 forKeyPath:@"cat.weight"];
  ```

- KVC会触发KVO监听 不管有没有set方法都会触发

  [取值表]()

  ![img](https://upload-images.jianshu.io/upload_images/4130989-3b4c3c7cc11c1b8c.png?imageMogr2/auto-orient/strip|imageView2/2/format/webp)


## Category

- 一个类永远只有一个类对象。不存在分类类对象  多个分类通过runtime动态合并到类信息中(类对象 元类)。存到类对象的方法列表， 最终都是通过instance对象的isa到类对象中寻找，然后调用
- 所有分类中的类方法都会合并到一个元类对象 存储到元类的方法列表


- 属性和成员变量两个概念 属性可以通过点语法访问 成员变量是带_这种

  - clang查看

    ```objective-c
    feellife@apps-iMac ~ % clang -rewrite-objc Person+Eat.m -o category.cpp

    ```

- 添加属性访问

  ```objective-c
  @interface NSObject (test)
  @property (nonatomic,strong) NSString *name;
  @end
  NSString *_name;
  @implementation NSObject (test)
  - (NSString *)name {
  return _name;
  }
  - (void)setName:(NSString *)name{
  _name = name;
  }
  ```

  ​

- 同样的方法会优先调用分类，分类方法是最后添加到方法数组 数组取值通过while-- 最后面参与编译的放在前面 原来的方法列表是最前面  **while（--）顺序调用**

  ​

  - 可以修改编译顺序  Person+Eat在最后面 同一个方法 会最先调用Eat里面的

  ![D624D448-2CDA-4027-9453-077E2307D680](https://tva1.sinaimg.cn/large/e6c9d24egy1h3umcizivdj210q09yq3i.jpg)

```objective-c
struct category_t {
    const char *name;// 类名
    classref_t cls;// 类
    WrappedPtr<method_list_t, PtrauthStrip> instanceMethods; // 实例方法列表
    WrappedPtr<method_list_t, PtrauthStrip> classMethods; // 类方法列表
    struct protocol_list_t *protocols;// 协议列表
    struct property_list_t *instanceProperties;// 属性列表 这个是最后一个成员
  //分类所定义的实例属性，不过我们一般在分类中添加属性都是通过关联对象来实现的
    // Fields below this point are not always present on disk.
    struct property_list_t *_classProperties; //这个可以当作不存在
// 如果是元类,就返回类方法列表;否则返回实例方法列表
    method_list_t *methodsForMeta(bool isMeta) {
        if (isMeta) return classMethods;
        else return instanceMethods;
    }

    property_list_t *propertiesForMeta(bool isMeta, struct header_info *hi);
    
    protocol_list_t *protocolsForMeta(bool isMeta) {
        if (isMeta) return nullptr;
        else return protocols;
    }
};
```

### load

- 先调用父类的方法 按编译顺序调用 子类后调用 最后在调用分类（按编译顺序）


- 不用导入头文件就直接被调用 

  ```objective-c
   //源码 +load 方法时 与其他类方法不同 先调用类方法 在调用分类的  通过函数指针指向内存地址值直接拿到方法所以不同 其他的都是通过objc_msgsend 通过isa 拿方法
   while (loadable_classes_used > 0) {
              call_class_loads();
          }

          // 2. Call category +loads ONCE
    more_categories = call_category_loads();
  ```

  ​

- 主动调用**[Class load]**就是 objc_msgsend 就会按照消息发送机制，通过`isa`指针找到类、元类，之后在方法列表中进行查找

###  initialize

- 类第一次接受到消息调用 **[person alloc]**  父类的`+initialize`方法可能会被调用多次 只调用一次 内部会先判断父类有没初始化 在判断子类有没初始化

  ```objective-c
  if ((behavior & LOOKUP_INITIALIZE)  &&  !cls->isInitialized())
  ```

  ​

- 先调用父类 在调用子类  有一个子类就是会调用两次（父类未实现先调用父类 在判断子类有没实现 没实现调用一次）  依此类推 两个子类就会调用三次（发送三条消息）

### Associated

- .m文件声明int _age属性重写set get方法可以实现 缺点 全局属性 多个对象时 取值错误  通过字典包装key 也是全局存在 内存问题

- get set 

  ```objective-c
  const void *NameKey = &NameKey; extern外部可以访问  加上static防止
  objc_setAssociatedObject(self, @selector(name), name, OBJC_ASSOCIATION_COPY_NONATOMIC);
  return  objc_getAssociatedObject(self, _cmd);
  //消除关联对象 
  person.name = @""; person.name = nil;
  相当于 objc_setAssociatedObject(person, @"temp", nil, OBJC_ASSOCIATION_ASSIGN);
  ```

  ​

## block

>clang -rewrite-objc main.m -o block.cpp

- 有结构体 isa指针 本质上就是oc对象 里面封装了函数

- clang编译一下 feellife@apps-iMac ~ % clang -rewrite-objc main.m -o block.cpp

  ```objective-c
  struct __block_impl {
    void *isa;
    int Flags;
    int Reserved; 
    void *FuncPtr; //block代码块放在一个函数里面 这个就是指向函数地址的指针
  };
  struct __main_block_impl_0 {
    struct __block_impl impl;
    struct __main_block_desc_0* Desc; //描述信息 block内存大小

    //构造函数 *fp 函数地址
    __main_block_impl_0(void *fp, struct __main_block_desc_0 *desc, int flags=0) {
      impl.isa = &_NSConcreteStackBlock;
      impl.Flags = flags;
      impl.FuncPtr = fp;
      Desc = desc;
    }
  };
  ```

### 变量捕获 capture

- 局部变量  捕获到block struct内部 新增一个成员或指针来存储外面的变量 

  - auto int a =10 离开作用域就销毁  **值传递**  离开block再去修改值 block里面的新增的成员值不会变化

  - static int b =20 **指针传递** block内部的值会跟着改变

    ```objective-c
      struct __block_impl impl;
      struct __main_block_desc_0* Desc;
      int c;//auto修饰的变量
      int *d; //static指针修饰的变量
    ```

  - self 也是局部变量 当被传递的时候 作为参数

- 全局变量  已经存到内存了 不需要额外再存到block结构体

  - 不需要捕获 直接访问变量
  - 函数外面定义的变量称作全局变量 

### block类型

｜block 强引用 block不死 引用对象不会死

- 所有block都继承自**NSBlock**  最终类型以运行时为准

- **__NSGlobalBlock__**

  - 没有访问auto变量  static 全局不属于auto 所以都是全局 block  放在数据段

  - 调用copy 什么都不会发生 依然在global

    ```objective-c
     void(^block1)(void) = ^{
            NSLog(@"HT_Block");
     };
     NSLog(@"%@",block1); // 打印： <__NSGlobalBlock__: 0x102239040>
    ```

- **NSStackBlock**   和全局block对立 完全相反 



  - 存储在栈区 作用域消失后数据可能会错乱 内存被回收了 随时可能被销毁
  - 访问了auto变量 
  - 调用copy 从栈拷贝到堆区 产生堆内存的block


  ```objective-c
  //block没有任何参数返回所以不会自动copy 依然是stackblock
  NSLog(@"type5=  %@ %@",[^{NSLog(@"1111=%d",c);}  class],[[^{NSLog(@"1111=%d",c);} class] superclass]);
  ```

  ​

- **__NSMallocBlock__** 存储在堆区  

  - ARC下我们使用block的时候，系统很多情况下都会自动帮我们复制一份栈block到堆区

    - blcok作为函数参数返回

      ```objective-c
      //这个是stack block 但是blcok作为函数参数返回 ARC下系统自动复制了，所以看不到copy操作 变成堆区Block
       int aaa =10; 
          void(^cblock)(void)= ^(){       
           NSLog(@"this is cblock %d",aaa);     
        };
       NSLog(@"type3=  %@ %@",[cblock class],[[cblock class] superclass]);

      ```

    - __strong

    - GCD会自动copy


  - stack copy操作

  - 堆区 copy操作 只会引用计数增加

    ```objective-c
    auto int c = 30;
    [[^{NSLog(@"1111=%d",c);} copy] class] //不加copy就是stackblock 加了就是malloc

    ```


### 对象类型使用block

> 注意block 作用域   栈上的block 随时会被销毁 都不会强引用对象

```objective-c
  Person *per = [[Person alloc]init];
  per.age = 20;
  AABlock aaBlock = ^{
      NSLog(@"age====%d",per.age);
  };

struct __main_block_impl_0 {
  struct __block_impl impl;
  struct __main_block_desc_0* Desc;
  Person *per; // block是会生成一个person强指针的 block在 person就在 per指针指向外面的person对象
};
//block销毁 per才会被销毁  
```

- __weak 弱引用访问外面的变量会被销毁 block不会对person强引用

  ```objective-c
  struct __main_block_impl_0 {
    struct __block_impl impl;
    struct __main_block_desc_0* Desc;
    Person *__weak per;//这里是弱引用
  };
  ```

  ​

### __block

>当block在栈上的时候，是不会对__block变量进行强引用的
>
>当block在堆上的时候.  看内部auto变量进行强引用还是弱引用

- _ _block 不管修饰oc对象还是局部变量都会生成结构体

  ```objective-c
  // 结构体的内存地址：0x000000010073c240，也就是结构体首成员isa的内存地址
  struct __Block_byref_age_0 {
    void *__isa; // 0x000000010073c240
   struct __Block_byref_age_0 *__forwarding; // 0x000000010073c240 + 8 = 0x000000010073c248
   int __flags; // 0x000000010073c248 + 8 = 0x000000010073c250
   int __size; // 0x000000010073c250 + 4 = 0x000000010073c254
   int age; // 0x000000010073c254 + 4 = 0x000000010073c258
  };
  ```

  - 不能修饰static变量

    ​

- 变成全局也可以修改 传递的是指针地址

### 内存管理

> 堆上才有 栈会自动销毁不需要内存管理 

基本数据类型，内部是值存储，不涉及对象管理 。static变量和全局变量：内存放在数据段，由程序统一管理，可全局访问，长期持有而且不会销毁，所以不需要内存管理

1. 对象类型的auto变量
   - `_Block_object_assign`根据__对象类型的auto变量是__strong/__weak，决定强/弱引用
2. 引用了 __block 修饰符的变量
   - __block变量都是强引用

### 循环引用

> block持有的weakSelf 和局部变量strongSelf释放时机不一样(生命周期不一样)
>
> __strong并没有使self变成强引用，block依然只是持有了一个weakSelf，__strong 只是定义了一个 returnCount+1的strongSel

- weakSelf  block 捕获到了一个self 的弱引用，block在结构体内会弱持有这个self， 所以block和self 之间不会形成循环引用

- **strongSelf是一个局部变量，block执行完即会被释放，weakSelf则是保存在block结构体中的变量，是随着block进行释放的**

  ```objective-c
  __weak __typeof(self)weakSelf = self;
  //block里加了一个strong 这样的话又使得 Self 的returnCount + 1
  __strong __typeof(weakSelf)strongSelf = weakSelf;

  ```

  ​

## runtime

### isa

> 打印isa地址 p/x person->isa

- 取值 设值

  ```objective-c
  &运算 想取哪位把某一位置1 其余位为0
  0b00000100 
    00000100 // 1<<0 1<<1 1<<2 这种写法也行
  //这样取第三位1 
  ｜运算  取值
  ```

- 位域

  ```objective-c
  //BOOL left : 1  用了位域 ：1只占一个字节 bool这个时候只表示数据类型  
  //不用位域一起占四个字节 一个bool就是一个字节
  struct HPDirection {
      BOOL left : 1;
      BOOL right : 1;
      BOOL front : 1;
      BOOL back : 1;
  }; //结构体只占用一个字节
  ```

- 共用体 共用一块内存 后面的值会覆盖前面的值 

  ```objective-c
  union Date{
      int year;
      int month;
      int day;
  };//占4个字节 day month year都放在一个公用字节里 取最大成员占据的大小 就是一个int
  //char和int 都存在的时候 取int 就是四个字节
  union Date{
      int year;
      char month;
      char day;
  };
  // shiftcls存储着类信息和元类信息 所以最后三位永远都是0 ISA_MASK 33位 可以取出33位isa指针信息 &33位1  0x0000000ffffffff8ULL的后三位是0 
  # if __arm64__
  // ARM64 simulators have a larger address space, so use the ARM64e
  // scheme even when simulators build for ARM64-not-e.
  #     define ISA_MASK        0x0000000ffffffff8ULL
  #     define ISA_MAGIC_MASK  0x000003f000000001ULL
  #     define ISA_MAGIC_VALUE 0x000001a000000001ULL
                                  \
          uintptr_t shiftcls          : 33; /*MACH_VM_MAX_ADDRESS 0x1000000000*/ \

  ```

  64位系统 分布 shiftcls 存储类指针 `isa_t`占用`8`字节=>64位

  > 通过配置`OBJC_DISABLE_NONPOINTER_ISA`为`YES`可以设置`nonpointer`为`0`

  ![A3477353-62AF-4CFB-9B11-4CE26350DDE3](https://tva1.sinaimg.cn/large/e6c9d24egy1h43zca1n6ij21xo0fk76b.jpg)

### class结构

- class_rw_t  

  二维数组 涉及到分类 动态添加 修改 ，运行时才出现 将ro_t数据复制进去

  ```
  rw->set_ro(ro);
  ```

- class_ro_t

   一维数组 类的初始内容 程序刚运行 不能修改

  ```objective-c
  struct method_t {
      SEL name; //函数名  @selector()和sel_registerName()获得 底层结构跟char *类似(@selector(test) 类似于 "test")
      const char *types; //编码（返回值类型、参数类型）
      IMP imp;//指向函数的指针（函数地址）  代表函数的具体实现,本质就是一个"*"
  };
  ```

- Type Encoding @encode  `NSLog(@"%s",@encode(int));`

- cache_t 方法缓存

  - 散列表缓存 第一次找到元类里面的方法 可以缓存到类方法 下次去调用
    - 与key通过&运算 存取

  ```c
  struct cache_t {
      bucket_t *_buckets; // 用于存储缓存方法的哈希表(散列表)
      mask_t _mask;// 哈希表的长度 -1
      mask_t _occupied; // 已经缓存的方法数量
  };
  struct bucket_t {
      // SEL作为key，SEL也就是方法名
      cache_key_t _key;
      // 函数的内存地址，也就是指向函数的指针
      IMP _imp;
  }
  ```


###  消息发送

- 消息机制  给reveiver(方法调用者 Person)发送消息（方法名 alloc）

  ```objective-c
  Person *per = ((Person *(*)(id, SEL))(void *)objc_msgSend)((id)objc_getClass("Person"), sel_registerName("alloc"));

  首先判断receiver是否为空 为空直接退出 汇编里面消息发送第一部会判断
    // 如果是类方法就是从元类对象的缓存方法查找 rw_t的方法数组查找
  ```

![6E05F0E9-5B89-446C-9D91-F4ED7C0426E2](https://tva1.sinaimg.cn/large/e6c9d24egy1h45axwc6q7j21ik0u0n25.jpg)

### 动态方法解析 method resolver

- (BOOL)resolveInstanceMethod:(SEL)sel （实例方法调用这个）
- (BOOL)resolveClassMethod:(SEL)sel(类方法调用这个)

![54EC98CA-6C84-439D-9563-81747C33FD7B](https://tva1.sinaimg.cn/large/e6c9d24egy1h46edkittjj221w0iigok.jpg)

### 消息转发

前面的两个阶段都没有实现，就会继续进行第三步：消息转发 

![8C37DA1D-527F-4329-A88E-62AB83E7994C](https://tva1.sinaimg.cn/large/e6c9d24egy1h46efiv3q6j21dr0u0n0m.jpg)

​

1. `forwardingTargetForSelector：`方法是指把响应这个方法的对象转发给其他的对象，其他对象要实现一个同名的方法才不会nil 那么消息接受者就发生了变化，会重新调用一遍`objc_MsgSend(消息接受者，SEL)`流程

2. 方法签名  函数编码

3. invocation

   ```
   -(void)forwardInvocation:(NSInvocation *)anInvocation{
    //最后一个方法 里面可以随便操作   
    可以直接打印 NSLog('-----') 
   }
   ```



#### 应用 

- 统一拦截找不到的方法 防止崩溃 某个类或者NSObject拦截

### super

- [super run] 方法接收者（receiver）还是当前类（子类） 只是从父类的方法开始查找
- [self run]  从当前类开始查找方法
- [super class] 消息接收者还是子类

```
super的参数第一个传的是结构体
struct objc_super {
    /// Specifies an instance of a class.
    __unsafe_unretained _Nonnull id receiver;
    __unsafe_unretained _Nonnull Class super_class;
};
//按照上面的可以得出 [super viewdidLoad] 结构
objc_msgSendSuper{
  self，
  [UiviewController class] 
},@selector(message)
//汇编底层
objc_msgSendSuper2 {
  
}
```

### 动态添加

- 类


- 成员变量
  - ivar_getName 获取私有成员名称 kvc再去修改
  - class_copyIvarList //获取成员变量列表

- 方法 

- 参数类型[^1]

  ```objective-c
  class_addMethod([self class], sel, class_getMethodImplementation(self,@selector(startEngine:)), "v@:@"); 
  ```

  - 判断方法已实现就调用原来方法 未实现就交换

    ```objective-c
    + (void)load{
        static dispatch_once_t onceToken;
        dispatch_once(&onceToken, ^{
        
          Method method1 = class_getInstanceMethod(self, @selector(eat));
          Method method2 = class_getInstanceMethod(self, @selector(anotherEat));
            //把原始方法名传到要改的方法
            BOOL didAddMethod =  class_addMethod(self, @selector(eat), method_getImplementation(method2), method_getTypeEncoding(method2));
            if (didAddMethod) {
                //用新的方法替换原来的方法  而并没有改变Father类中eat方法的具体实现 
                    class_replaceMethod(self,
                    @selector(anotherEat),
                    method_getImplementation(method1),
                    method_getTypeEncoding(method1));
                }else{
                    method_exchangeImplementations(method1, method2);
                }
        });
    }
    //判断当前类是否是son 从而可以和父类区分开来 一个方法两种实现
    - (void)anotherEat{
      NSLog(@"self:%@", self);
        if ([self isKindOfClass:[son class]]) {
            NSLog(@"-----son");
           NSLog(@"替换之后的吃的方法...");
        }else{
            [self anotherEat];
        }
    }
    ```

    ​

  - ​

[^1]: ![F047D17B-3F1B-4F21-B60D-33BDE34AEC4C](/var/folders/29/9z71rmy546j6xy105qtf2z3c0000gn/T/abnerworks.Typora/F047D17B-3F1B-4F21-B60D-33BDE34AEC4C.png)