---
typora-copy-images-to: ipic
---

[TOC]

## class

- **isKindOfClass** i**sMemberOrClass** 都是拿当前对象的类对象去判断
  - 实例对象是类方法 类对象是元类
- 如何让一个类不走消息发送
  - 获取到方法的函数指针 通过`NSObject`的`methodForSelector:`方法可以获取到函数指针 发起调用


## TCP/IP

- model 图  cables 电缆  12也可以合并称为link层  5也可以分为三层 会话层 表示层 应用层

  ![A4DB6F58-BF9E-4719-8301-248DA4D86C02](https://tva1.sinaimg.cn/large/e6c9d24egy1h4nr6t138ij216q0u0n3m.jpg)

- socket位于传输层

- 为什么不是两次握手

  - a第一次发送syn信号 有可能延迟 然后又发了一次syn 到b  b先接收到了第二个syn 回了一条syn+ack 但是有可能 a第一次发送的信号过了很久发成功了 所以三次握手保证 a回一个ack信号

  - 丢包问题

    - 发送缓冲 

    ![49B0E7C3-CB50-4C00-99EB-9EA13FA42CD4](https://tva1.sinaimg.cn/large/e6c9d24egy1h4vz30y3c3j21de0qe0x5.jpg)

- 四次挥手 发送四个包

  > 因为tcp提供了连接的一端结束它的发送后 还能接收来自另一端数据 所以需要两次才能彻底关闭 也就是2、3步

  主动关闭（客户端发起关闭请求） 被动关闭（服务端发起）

  1. 发送FIN报文 报文指定一个序号seq 并停止在发送数据 主动关闭TCP连接 进入等待
  2. 服务端收到FIN之后 回一个ACK 服务此时进入CLOSE_WAIT状态
  3. 这次是服务端主动想要断开连接 发送一个FIN报文 且指定一个序列号 与客户端发来的seq不一样
  4. 客户端收到FIN之后 发送一个ACK应答

## AFN

- **AFURLSessionManager/AFHTTPSessionManager**这里就是AF代码的核心了，主要负责网络请求的发起，回调处理，是在系统网络相关API上的一层封装

- **AFURLRequestSerialization/AFURLResponseSerialization**这两兄弟主要处理一些参数序列化相关的工作

  - 比如自定义的header，一些post或者get参数等等

- **AFSecurityPolicy**处理https相关的公钥和验证逻辑

  > 避免攻击

  - SSL Pinning的原理就是需要将服务器的公钥打包到客户端中 tls验证时，会将服务器的证书和本地的证书做一个对比，一致的话才允许验证通过
  - ​

- **AFNetworkReachabilityManager** 网络状态

- **UIKit+AFNetworking**这里主要是通过Category来提供了一下UIkit的便利方法

### 处理回调数据

- 2.0x 采用线程保活 runloop 
- 现在采用 sessionWithConfiguration

## **SDWebImage**

![CE39D708-C956-4FCC-A68E-1834AC307CAB](https://tva1.sinaimg.cn/large/e6c9d24egy1h4um9ofcptj212q0qi7a4.jpg)

- 通过字节数据前几位拿到图片格式 getBytes

###  SDImageCache 

- SDDiskCache
  - 继承NSObject
- SDMemoryCache
  - 继承NSCache
  - 内存中存储我们要缓存的数据

### SDWebImageDownloader

- 异步图片下载管理 

### SDWebImageManager

- 主要对缓存管理 + 下载管理进行了封装

## **Alamofire**

## RxSwift

## SnapKit

## SQLite.swift

## Codable

>  Encodable 和 Decodable 两个协议的组合  把 JSON 这种弱类型数据转换成代码中使用的强类型数据



## Masonry

- 为什么不会强引用
  - 内部没有self调用  只是调用了一个block方法 block(constraintMaker);

## CocoaAsyncSocket

> 长连接的情况下使用socket  类似音视频通话

socket 

- ip
- 端口
  - 区分应用
- 建立心跳连接


## Cocoapods

Podfile.lock

- 解决依赖库版本不一致的问题

## CommonSuper

```objective-c
-(Class)classA:(Class)clsA andClassB:(Class)clsB{
    NSArray *aa1  = [self findSuperViews:clsA];
    NSArray *aa2  = [self findSuperViews:clsB];
    NSInteger arrCount =  aa1.count <  aa2.count ? aa1.count:aa2.count;
    Class result;
    for (NSInteger i =0;i < arrCount;i++) {
        Class claA = aa1[i];
        Class claB =  aa2[i];
        NSLog(@"A=%@====B=%@",claA,claB);
        if ( claA == claB ) {
            result = claA;
            break;
        }
    }
    return  result;
}
- (NSArray*)findSuperViews:(Class)class {
    if (class == nil) {
        return  @[];
    }
    NSMutableArray *result = [NSMutableArray array];
    while (class != nil) {
        [result addObject:class];
        class = [class superclass];
    }
    return [result copy];
}
```



## Response Chain

Note:UIControl会截断处理

- 处理视图之外的响应

  - B是A的子视图 红框超出部分不能点击 可以修改A的事件hit-Testing 把坐标转移到B上

    ```objective-c
    //A view code
    - (instancetype)initWithCoder:(NSCoder *)coder{
        self = [super initWithCoder:coder];
        if (self) {
            for (id subView in self.subviews) {
                if ([subView isKindOfClass:[BView class]]) {
                    _BView = subView;
                    break;
                }
            }
        }
        return self;
    }
    -(BOOL)pointInside:(CGPoint)point withEvent:(UIEvent *)event{
        //将触摸点坐标转换到在B上的坐标
        CGPoint pointTemp = [self convertPoint:point toView:_BView];
        if ([_BView pointInside:pointTemp withEvent:event]) {
            return YES;
        }
        //否则返回默认的操作
        return [super pointInside:point withEvent:event];
    }
    ```

    ​

  ![528558DD-C728-4990-8D87-174E2B622579](https://tva1.sinaimg.cn/large/e6c9d24egy1h52p2vdqqmj20fm0bqjrl.jpg)

- 事件的传递顺序

  - UITextField ——> UIView ——> UIView ——> UIViewController ——> UIWindow ——> UIApplication ——> UIApplicationDelegation

- ​

## HttpS

[TLS](https://zhuanlan.zhihu.com/p/36981565)

对称加密 

> 一对1

- 共同拥有一个密钥  双方加解密

非对称加密

> 场景 1对多的情况  

- 使用两个密钥 公钥和私钥 RSA算法 
- 利用public key在网络上随便传递
- 本地保留私钥加密数据 利用对方的公钥解密

中间人攻击 截获 AB公钥 发送假消息  保证公钥不被截获 需要CA验证公钥真实性

1. 保证A、B和CA之间的通信不被截获
   - CA直接继承在操作系统和浏览器里面  AB不用网络即可获取

流程就是 A->Public key-> CA  <-public key<-B

SSL

- 证书机构

TSL: **Transport Layer Security** 传输安全层

- private 
  - 私钥仅仅签名身份认证使用
- public
  - 利用对端的公钥加密数据

get

> 从服务器检索数据。这是一种只读方法(安全). 每次发出多个相同的请求都必须产生相同的结果

- 只被用于获取数据 因为读取数据，所以可以用来做缓存
- 长度受限于url 实际

post

> 调用两个相同的 POST 请求将导致两个不同的资源包含相同的信息

- 发送表单信息（可写 所以不安全）
- 带body传输数据 相对安全一点 实际安全度由https决定






value=[英文, 1] msg=[F2, 33, 00, 1c, aa, 3c, 03, 01, 02, 4b, 01, 01, 01, 00, 66, 0f, 00, 00, 00, 4F]

value=[关闭, 1] msg=[F2, 33, 00, 1c, aa, 3c, 03, 01, 02, 4b, 01, 00, 01, 00, 66, 0f, 00, 00, 00, 4F]


