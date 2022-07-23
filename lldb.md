---
typora-copy-images-to: ipic
---

[TOC]

## LLVM

![BAC59436-4539-4924-875F-64D75CB0B6F6](https://tva1.sinaimg.cn/large/e6c9d24egy1h4h0i0se2wj217e0h6jt0.jpg)

> oc->中间代码->汇编  is a compliler [kəmˈpaɪlər]



source code - > lexical Analyzer  [ˈleksɪkl] 词汇 -> Syntax Analyzer  [ˈsɪntæks]语法分析-> semantic Analyzer  [si'mæntik] ->AST

### clang

- Clang是LLVM的框架的一部分，是它的一个C/C++的前端

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

