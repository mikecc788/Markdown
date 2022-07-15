[TOC]

## LLVM

> oc->中间代码->汇编

## 常用指令

- `x/4gx`: 读内存段, 以16进制读取4个内存片段
- `p/x`: 以16进制形式打印, 可用作打印地址
- `po`: 读出/输出值, 也可以用作打印对象
- **p (Class)0x00000001f0762d19** 打印类
- x/4g 打印八个字节 四个

## Xcode指令

```shell
xcrun -sdk iphonesimulator clang -rewrite-objc main.m
```

