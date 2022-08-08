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



## Masonry框架

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

## HttpS

SSL

- 证书机构

TSL: **Transport Layer Security** 传输安全层





