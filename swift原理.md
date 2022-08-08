

##常用指令

- 生成语法树 swiftc -dump-ast main.swift
- 中间代码   swiftc -emit-sil main.swift

## 代码格式检查工具

[swiftLint](https://github.com/realm/SwiftLint)



##语法

### def

> 当前作用域结束之前执行

- 使用场景 
  - do-catch

### 属性 

- 存储属性

- 计算属性 不占用内存

  > 通过setter、getter方法来取值或赋值

  - 本质就是方法 所以可以在enum定义
  - rawValue也是计算属性

- 延迟存储属性 lazy 

  - 与var配套 let常量 不延迟


  - 网络请求 初始化过程比较复杂

- 属性观察器 preperty observer

  - willSet 
  - didSet

###inout

- inout修改外部变量
- 本质地址传递

### enum

- 关联枚举  内存占比大 外部传进来的变量 内存动态

  ```swift
  enum Score{
      case points(Int)
      case grader(Character)
  }
  var score = Score.points(5)
  score = .grader("A")
  switch score {
      case let .points(i):
          print(i,"point")
      case let .grader(i):
          print(i,"grader")
  }
  //内存分配
  enum Password {
      case number(Int,Int,Int,Int)
    	case test2(Int,Int)
      case other
    //case score(Int) 加了这个之后 大小依然不变 33 32个字节足够存储  case number(Int,Int,Int,Int，int)除非这样 内存就变成41 分配48
  }
  var pwd = Password.number(5, 6, 4, 9)
  pwd = .test2(2, 3)
  pwd = .other
  MemoryLayout.size(ofValue: pwd) //33 = 32+1 1用来判断当前成员是 other还是number 
  MemoryLayout<Password>.stride //40
  MemoryLayout<Password>.alignment // 8
  //内存分布
   05 00 00 00 00 00 00 00
   06 00 00 00 00 00 00 00
   04 00 00 00 00 00 00 00
   09 00 00 00 00 00 00 00
   00 00 00 00 00 00 00 00
   
  //test(2,3)内存分布 01表示枚举内部的序号 占用一个字节 由于内存对齐 需要用8个字节 当前取值是第几个 
   02 00 00 00 00 00 00 00
   03 00 00 00 00 00 00 00
   00 00 00 00 00 00 00 00
   00 00 00 00 00 00 00 00
   01 00 00 00 00 00 00 00
  ```

- Raw values  内存写死 占比小 不能自定义

  ```swift
  // 虽然定义了Int 但是 枚举内部已经默认给值了 所以占比都是1
  enum Season:Int{
      case spring = 1,summer,autumn,winter
    //case test1
    //case test2  
    //不管这里写多少个 内存占比都是1 因为这里面是判断语句 只是返回一个值 所以只用占一个空间大小
  }
  var s = Season.spring
  var s1 = Season.summer
  MemoryLayout<Season>.stride //1
  MemoryLayout<Season>.size //1
  MemoryLayout<Season>.alignment//1
  //内存分布 只占用一个字节
  00 //只有第一个字节会变化 00 01 02  这里记录的是标号 具体的值通过rawValue显示
  ```

  ​

  - 不是存储到枚举内存单元  

- 隐式枚举值

  - 指定类型会自动分配值

### MemoryLayout

> Oc sizeof

- String 16个字节  oc里面八个字节


- Int = Int64 64位系统 = long类型

```swift
// Int = Int64 
MemoryLayout<Int>.size //8
MemoryLayout<Int>.stride //8
MemoryLayout.size(ofValue: age)
```

### 可选项

- var age:Int? //默认nil

###强制解包

- nil 类似空盒子
- 对nil强制解包 运行时错误

### 空合并

- a??b   返回类型取决于b  a??b??3 看最右边类型 3 所以返回不可选类型
  - a是可选项
  - b是不可选项时 ，返回a会自动解包

### guard

- 适合提前退出 

```
guard 条件 else{
    //退出作用域
    return
}
```

### 小端模式 高位在前 读内存从右到左

## typealias

- 别名

##闭包本质

> 闭包是一个函数和它所捕获的变量/常量环境组合起来,称作闭包

```swift
//这个称为闭包表达式 不能等同于闭包
{(参数列表) 返回值 in
	Return 
}

//getFn() 里面 有捕获的变量和函数 构成了闭包
typealias Fn = (_:Int)->Int
func getFn()-> Fn{
    var num = 10
    func fn(_ i:Int)->Int{
       num+=i
       return num
    }
    return fn
}
var fn1 = getFn() //每调用一次getFn函数，就会重新分配一段新的堆空间来处理num的值 num会作为堆空间临时变量被释放  fn1是随着函数的释放而释放
print(fn1(1))
var fn2 = getFn()
print(fn2(5))
```

- 尾随闭包

  - 闭包作为函数的最后一个参数

    ```swift
    exec (v1:10 v2: 20, fn:{})
    ```

    ​

##struct

> 值类型 枚举也是 深拷贝

- 使用某个类（struct或者class）进行网络请求，并将请求结果赋值给该类的一个属性

- Bool Double String 等都是结构体

- 初始化器 自动生成

- 如果自定义了 （init）系统不会自动生成了 汇编可以查看 不写init也会生成

  ```swift
  struct Point {
          var x : Int = 10
          var y: Int = 20
      }
  var p1 = Point()
  print(Mems.memStr(ofVal: &p1)) 
  //    print(MemoryLayout<Point>.size) //16
  print(MemoryLayout<Point>.stride) //16
  print(MemoryLayout.size(ofValue: p1))//16
  print(MemoryLayout<Point>.alignment)//8

  struct Point {
          var x : Int = 10
          var y: Int = 20
    		var b = true
      }
  var p1 = Point()
  print(Mems.memStr(ofVal: &p1)) 
  //    print(MemoryLayout<Point>.size) //17
  print(MemoryLayout<Point>.stride) //24
  print(MemoryLayout.size(ofValue: p1))//17
  print(MemoryLayout<Point>.alignment)//8
  ```


## class

```swift
> 引用类型（指针类型 栈空间） 指针指向的内存在堆空间 内容释放后 指针回到sp（寄存器）位置 

- 不会自动生成自动化器 
- 向堆申请空间 汇编查看 
-  Size.__allocating_init()->swift_allocObject->swift_slowAlloc->malloc_zone_malloc

​```swift
class Size {
        var width : Int = 1
        var height: Int = 2
    }
    //无论Size里面写多少变量 打印都是8个字节 这里打印的都是size指针的大小 并不能显示内容大小
    print(MemoryLayout<Size>.stride) //8
    print(MemoryLayout<Size>.alignment)//8
    print(MemoryLayout<Size>.size)//8

class Size {
        var width : Int = 1
        var height: Int = 2
        var x : Int = 3
        var y: Int = 4
    }
struct HeapObject {
  HeapMetadata const * metadata;
  InlineRefCounts refCounts   // 占8字节
} 
内存大小是 48 默认分配了16 其他根据变量增加
```



##Array

- forEach

```swift
for (num,index) in numbers.enumerated()
```

- makeIterator()
- numbers.indices

## 删除

- ArraySlice

