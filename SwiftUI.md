[TOC]

### 快捷键

- command+鼠标左键 包裹控件

###组件

#### HStack  VStack ZStack

- 水平垂直布局

#### List

- ForEach创建报错使用 id

  ```
  ForEach(postList.list ,id:\.id)
  ```

####  Rectangle

- 创建矩形方框

#### Group

> 添加10个以上视图

- vstack和hstack 用if判断切换


#### overlay

- 设置圆角边框

#### GeometryReader

- 获取控件的size大小


#### edgesIgnoringSafeArea

- 忽略安全区域高度

#### @state @Binding@

- @State 值传递 子视图修改无法传递到父视图   加上@Binding页面间传递


- 刷新数据 + 绑定数据传值
- binding绑定的数据用.constant()赋值

##### ObservableObject

> 修饰的属性用@Published

- 对实例进行监听（必须是对象）

##### environmentObject

> 将数据放到环境中，更适用在多视图中

#### @discardableResult

- 加在函数前面 忽略返回值

#### Combine

- 更新绑定数据信息

#### UIViewRepresentable

#### Form

- 创建设置页面 可以设置头文字

```swift
Form{
  Section(header: Text("User Info")) {
  mineMessageView
  }
}
```







### 语法







