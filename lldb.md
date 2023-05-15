---
typora-copy-images-to: ipic
---

[TOC]

## LLVM

![BAC59436-4539-4924-875F-64D75CB0B6F6](https://tva1.sinaimg.cn/large/e6c9d24egy1h4h0i0se2wj217e0h6jt0.jpg)

> oc->中间代码->汇编  is a compliler [kəmˈpaɪlər]



source code - > lexical Analyzer  [ˈleksɪkl] 词汇 -> Syntax Analyzer  [ˈsɪntæks]语法分析-> semantic Analyzer  [si'mæntik] ->AST

**源码下载**

```objective-c
//新版下载
git clone https://github.com/llvm/llvm-project.git
//安装cmake
$ brew install cmake
$ brew install ninja
ninja如果安装失败，可以直接从github获取release版放入【/usr/local/bin】中
https://github.com/ninja-build/ninja/releases

//old
$ git clone https://git.llvm.org/git/llvm.git/ 
下载clang
$ cd llvm/tools
$ git clone https://git.llvm.org/git/clang.git/
备注：clang是llvm的子项目，但是它们的源码是分开的，我们需要将clang放在llvm/tools目录下
```



### clang

#### 常用指令

命令行查看编译的过程:$ clang -ccc-print-phases main.m

```objective-c
             +- 0: input, "main.m", objective-c
            +- 1: preprocessor, {0}, objective-c-cpp-output //预处理器
         +- 2: compiler, {1}, ir
      +- 3: backend, {2}, assembler
   +- 4: assembler, {3}, object
+- 5: linker, {4}, image
6: bind-arch, "x86_64", {5}, image
```

**词法分析**，生成Token: $ clang -fmodules -E -Xclang -dump-tokens main.m

- 将代码分成一个个小单元（token） > (  , )  + - 空格 都会生成一个小单元

生成语法树 AST  $ clang -fmodules -fsyntax-only -Xclang -ast-dump main.m

便于阅读的文本格式，类似于汇编语言，拓展名.ll， $ clang -S -emit-llvm main.m

**IR基本语法**

[官方语法参考](https://llvm.org/docs/LangRef.html)

- Clang是LLVM的框架的一部分，是它的一个C/C++的前端

## 常用指令

- `x/4gx`: 读内存段, 以16进制读取4个内存片段
- `p/x`: 以16进制形式打印, 可用作打印地址
- `po`: 读出/输出值, 也可以用作打印对象
- **p (Class)0x00000001f0762d19** 打印类
- x/4g 打印八个字节 四个

### 堆栈信息

- thread backtrace 可以查看调用栈

## lldb

- -n 跳过整段函数 进入下一步 
- -s 进入函数里面
- ni  si 汇编级别调试
- finish 直接执行完当前函数 跳出去



## Xcode指令

```shell
xcrun -sdk iphonesimulator clang -rewrite-objc main.m
//模拟器生成arm64 汇编代码
feellife@apps-iMac arm64 % xcrun -sdk iphonesimulator clang -arch arm64 -S CTest.c
```

