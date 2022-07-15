[TOC]

## class

- **isKindOfClass** i**sMemberOrClass** 都是拿当前对象的类对象去判断
  - 实例对象是类方法 类对象是元类
- 如何让一个类不走消息发送
  - 获取到方法的函数指针 通过`NSObject`的`methodForSelector:`方法可以获取到函数指针 发起调用



