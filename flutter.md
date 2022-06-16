---
typora-copy-images-to: ipic
---

[TOC]

## 下载sdk



vim  ~/**.dash_Profile**

**export PATH=/Users/feellite/flutter/bin:$PATH** 

- /Users/feellite/flutter/ flutter的安装路径

 **export PUB_HOSTED_URL=https://pub.flutter-io.cn**

  **export FLUTTER_STORAGE_BASE_URL=https://storage.flutter-io.cn**



**source ~/.bash_profile**

**flutter doctor** 

- 升级 flutter upgrade --force

### mac操作

- 打开bash
  - feellife@apps-iMac feellife_1 % cd ~
    feellife@apps-iMac ~ %  open -e .bash_profile
- ​

## iOS 运行

### 生成iOS文件夹

- ```
  flutter create -i swift .
  ```

- 国际化报错 （未解决）

## android 运行

### 配置启动页

- 修改AndroidManifest.xml下的

  ```java
  <meta-data
          android:name="io.flutter.embedding.android.SplashScreenDrawable"
          android:resource="@drawable/launch_background"
   />
  ```

- 修改res-》drawable-〉backgroud

  ```java
   <!--    android:gravity="fill" -->  centre
   <item>
          <bitmap
              android:src="@mipmap/launch_image" />
      </item>
  ```

- Mipmap-xxxhdpi添加启动页图片 launch_image.png

### 配置adb

- 打开.bash_profile 文件  vim  ~/**.bash_Profile**

- /Users/feellife/Library/Android/sdk

  ```shell
  export ANDROID_HOME=/Users/feellife/Library/Android/sdk
  export PATH=${PATH}:${ANDROID_HOME}/tools
  export PATH=${PATH}:${ANDROID_HOME}/platform-tools
  ```

-  source ~/.bash_profile

- 配置端口 adb tcpip 7890 

- 连上手机 feellife@apps-iMac ~ % adb connect 192.168.2.2:7890

### 获取MD5 flutter默认不支持

- 获取md5

  ```
  cd android  
  ./gradlew signingReport 
  ```

  ​

- 获取sha1 sha256 

  ```
  feellife@apps-iMac key % keytool -list -v -keystore sign.jks
  cd 到sign.jks所在文件夹 执行命令
  ```

  ​

### flutter_blue

- Resolve 安卓12 权限 https://github.com/boskokg/flutter_blue_plus/issues/7
- ​

### 解决flutter镜像问题

### 解决m1芯片插件问题

- https://github.com/protocolbuffers/protobuf/issues/8062


- https://blog.csdn.net/m0_37780940/article/details/116646620

  ​

-  put get : 获取依赖
-  删除了 GradleException 前面的“new”
-   `File -> Project Structure -> Modules -> Select your project -> apply your API`


### 打包apk

1. feellife@apps-iMac feellife_1 % keytool -genkey -v -keystore ~/sign.jks -keyalg RSA -keysize 2048 -validity 10000 -alias sign
2. flutter build apk --release
3. flutter build apk --release --no-sound-null-safety //如果没有适配空安全就打没有空安全的包

## Dart库

### JSON解析

- https://www.jianshu.com/p/a61e0ff22248[^1]

  #### response.data 类型错误 string=>Map

  - 只能解析由json 解码而成的键值对存储的Map<String,dynamic>数据，不能直接解析String， Map<String, dynamic> responseData =  jsonDecode(response.data);

- ResponseType.plain 类型解析

### 语法

- 整除 num ~/ 3   num % 3//取模

#### List

- **List.generate 快速生产 Flutter 中的 Widget**

  - children: List.generate(L.length, (index){
    ​    return Text("$index");
     })

  - ```dart
    children: List.generate(l1.length, (index){
        return Text('${l1[index]}');
    }), /// List.generate 返回的是[] 所以children不需要:[]
    ```

#### map

- 从数组新增元素 
  ```dart
  // Map map1 = Map();
  // for (int i = 0; i < preFixArr.length; i++) {
  // map1.putIfAbsent(preFixArr[i], () => deviceName[i]);
  // }
  ```

  ​

### StreamBuilder

- 异步编程方式 ===通过 Stream 实现每秒钟局部更新数据

### tips  final const

- const 在编译的时候值都必须是确定的  final是在运行的时候才赋值

### 异步编程Future、Stream

#### Future

- 打印的时候 Instance of 'Future<String>'  用then


- Future需要配合[async](https://so.csdn.net/so/search?q=async&spm=1001.2101.3001.7020)和await一起使用 不会阻塞在此之后的代码

- 想在异步函数返回结果后再做其他操作，可以使用`then()`方法来监听

- Event Loop

  - Microtask Queue 级别高


  - Event Queue不停添加队列

  - 配合await 获取值 用法如下

    - ```dart
      void getStr() async{
        String userName = await AppSharedPreferences.getString('test1');
        setState(() {
          noteValue  = userName;
        });
      }
      ```

- 利用**FutureBuilder**加载状态  connectionState  可以用在不用setState的地方

  ![WeChat9f063712445688ae4a55b0170c554cf1](/Users/feellife/Library/Containers/com.tencent.xinWeChat/Data/Library/Caches/com.tencent.xinWeChat/2.0b4.0.9/933108715dd5b6c1f17d530ec406de36/dragImgTmp/WeChat9f063712445688ae4a55b0170c554cf1.png)

#### stream

- async* 返回`Stream`


- StreamBuilder 和 setState可以互换 setState会加载整个页面数据 
- 当只负责监听的时候 传递一个stream就行。需要负责修改数据 传递 Streamcontroller
- 用来处理连续的异步操作


- Stream可以发送执行多个事件 ,一系列异步事件的序列
- stream教程 [stream](https://www.youtube.com/watch?v=sZJu7qcUps0&list=PLDD3xNHFJjoob3GCF1JqaDxwrOTmpGGbe&index=4)
- 同时被多个监听 broadcast

### 字节流转换

- ```dart
      //dart中的字节流为int数组
  // 转成int数组
      List<int> bytes = utf8.encode(writeHex);
      print("bytes:$bytes");
  // 转回来
      String r = utf8.decode(bytes);
  ```


### with、implements、extends

- extends：能[重写](https://so.csdn.net/so/search?q=%E9%87%8D%E5%86%99&spm=1001.2101.3001.7020)父类属性或者方法，同时能通过super引用父类field  继承Class 什么都不用干能直接调用父类方法
- implements：必须重写所有属性和方法，且无法调用父类field  实现Class
- with：无需super关键字能直接调用父类方法或者属性  混入Class

### Abstract

- 用Abstract修饰的class是无法被实例化的
- Abstract classes 常被用于定义interfaces（接口）

#### 传参

```dart
List<Snowflake>? snowflake; //接收参数 
MyPainter(this.snowflake);
```

## 组件Widget

- 快捷键 command + option + t  ：快速调出环绕代码
- option + enter  ：将 widget 自动包裹一层 widget 或删除这层 widget


- ​	**向下传递约束 向上传递尺寸**
- 跳转页面的时候去掉 **MaterialApp** 不然没有返回箭头
- listview切圆角要和背景图片一起设置

### Spacer

- `Spacer()` 相当于弹簧的效果,使两个控件之间的距离达到最大值. (在页面不可滑动时才有效果)
- ​

### context

- context无效
  - 可以在调用之前提前定义好需要context的控件

### slivers

- SliverGrid  SliverToBoxAdapter SliverList搭配使用

### IndexedStack

- 可以在几个页面切换的页面使用  设置currentIndex

### Router

类似放盘子

- push 入栈  
- pop 出 

#### 命名路由

routes:{	

​	'/home':(context)=>Home(),

}

### 页面保活

- AutomaticKeepAliveClientMixin

### Container 

- 占满屏幕 设置宽高无效 用一个 row 或者column包住

### shared_preferences

- SharedPreferences.setMockInitialValues({}); 加上这句不能持久化数据
- ​

### CustomClipper

- 贝塞尔曲线
- ​

### InkWell

- InkWell有的叫溅墨效果，有的叫水波纹效果
- 可以添加 onTap点击方法

### 动画

- `Animation`的使用需要配合`AnimationController`  `AnimationController`需要一个`TickerProvider`


- Ticker就是一个帧定时器


- `CustomPainter`, `CustomPaint`, `Canvas`

- SingleTickerProviderStateMixin 只能加一个AnimationController 多个用TickerProviderStateMixin 

- ```dart
  ..forward() 往前播放一次
    forward(from: 0.0) 重新开始
  ```


#### 显示动画

- ScaleTransition _controller!.drive(Tween(begin: 0.5,end: 1.0)), 动画驱动tween
- AnimatedBuilder 循环一直播放

#### 隐示动画

- AnimatedContainer
- AnimatedSwitcher 
  - 两个控件之间切换动画
- TweenAnimationBuilder 
  - Transform.scale
- CurveTween 交错动画


#### hero动画

- Tag 需要定义一样

#### CustomPaint

> **自由绘制的一个widget**

- painter CustomPainter类： 提供了一个paint绘图方法供我们绘制图形




### row 里面图片 文字 排布

- AspectRatio 设置宽高比  可以用来替换Expand

### Column Row

- mainAxisSize: MainAxisSize.min,  // 这一行是关键所在   改变控件起始 默认起始位置在上边，排列方向为从上至下
  - **MainAxisAlignment.spaceAround** 均分
  - mainAxisAlignment的默认值就是MainAxisAlignment.start
  - **MainAxisAlignment.center** 子控件放在主轴的中间位置
  - **MainAxisAlignment.spaceEvenly**主轴空白区域均分，使各个子控件间距相等
- crossAxisAlignment: CrossAxisAlignment.stretch, 水平的，默认起始位置在中间
  - **CrossAxisAlignment.stretch** 使子控件填满交叉轴
- ​

### GridView

- ```dart
  shrinkWrap: true 设置这个属性才显示 自适应高度
  physics: NeverScrollableScrollPhysics() 滚动设置
  ```


### ListView

- ```dark
  shrinkWrap: true,自适应高度
  ```

- 传值  final Function(int) onClick; ListViewDemo({Key? key, required this.onClick(int)}) : super(key: key); 外面函数接收

- 上下都有缓冲区 预先加载

- 分割线 index%2==0 ？ Divider() 不推荐

#### 多选实现

- https://github.com/ritsat/listview_multiselection/blob/master/lib/main.dart

### SingleChildScrollView



### showModalBottomSheet

- custom

- ```dart
   _showModalSheet1(context,List<String> options){
    return showModalBottomSheet(
        isScrollControlled: true,
        shape: RoundedRectangleBorder(
          borderRadius: BorderRadius.only(topLeft: Radius.circular(10),topRight: Radius.circular(10),)
        ),
        backgroundColor: MyColors.bar_color,
        context: context,
        constraints: BoxConstraints(
            maxHeight: MediaQuery.of(context).size.height - MediaQuery.of(context).viewPadding.top),

        builder: (BuildContext context){
          return sheetView(options);
      }
    );
     }
   ```
  ```

- ​

### Flex 

- 弹性布局

### Stack

- 叠放组件

### 加载本地json

​```dart
child: FutureBuilder(
  future: DefaultAssetBundle.of(contex).loadString('images/health_data.json'),
	builder: (contex,snapshot){
  var new_data = json.decode(snapshot.data.toString());
LogD(new_data);
}
)


  ```

### 页面传值

### BoxDecoration 切圆角

- 设置背景颜色要在切圆角里面


### Listener

- 监听点击 触摸事件

### Button

- Textbutton:  MaterialStateProperty.all()设置属性

### Localization

- 国际化设置

  ```dart
   #国际化
    flutter_localizations:
      sdk: flutter
    最底部添加 
    flutter_intl:
    enabled: true
  ```

  ![WeChat6c0edf204d73798366053b07850497ef](/Users/feellife/Library/Containers/com.tencent.xinWeChat/Data/Library/Caches/com.tencent.xinWeChat/2.0b4.0.9/4100b5b68703b908e35e8fd25bba4804/dragImgTmp/WeChat6c0edf204d73798366053b07850497ef.png)


- #### 占位符传参

  有时候文案中的某些部分最开始是不确定的，在运行的时候才能确定。譬如文案中有价格，但是这个价格不是固定的，这时候就需要先用一个占位符占位，然后在运行的时候用真实的数据替换掉这个占位符。

  我们案例中的`button`文案我们替换为为`button {seq}`, `לַחְצָן{seq}`和`按钮 {seq}`。

  在使用的时候我们可以改为`Text("${S.of(context).button(index + 1)}")))`，这样的效果和前面的一样

  ​

### initState 

- 里面改变方法的时候 热重载没用 只走一次  重新编译生效




### Provider

- ```dart
  Provider.of<ShoppingCart>(context,listen: false).add(model.list[index]);  
  //listen: false 添加方法
  // listen: true 读取变量
  ```

#### flutter_riverpod

- 所有Provider都有一个 "ref "作为参数
  - 获得一个Provider的值并监听变化，这样，当这个值发生变化时，这将重建订阅该值的Widget或Provider。这是通过ref.watch完成的
  - 在一个Provider上添加一个监听器，以执行一个action，如导航到一个新的页面或在该Provider发生变化时执行一些操作。这是通过 ref.listen 完成的
  - 获取一个Provider的值，同时忽略它的变化。当我们在一个事件中需要一个Provider的值时，这很有用，比如 "点击操作"。这是通过ref.read完成的


- ```dart
  // Provider 不能实时更新
  final shopProvider = Provider((ref) => ShoppingCart());
  //这个命名可以实时更新
  final shopProvider = ChangeNotifierProvider<ShoppingCart>((ref) => ShoppingCart());

  ```

  StateProvider  定义一个全局常量StateProvider  内部实现 

  ```dart
  class StateProvider<State> extends AlwaysAliveProviderBase<State>
      with
          StateProviderOverrideMixin<State>,
          OverrideWithProviderMixin<StateController<State>,
              StateProvider<State>> 
                
  //定义一个全局常量StateProvider
  final StateProvider<int> numProvider = StateProvider((_) => 0);
   final state = ref.watch(numProvider.notifier).state++;//修改值
  ref.read(numProvider.notifier).state;//读取
  ```

- stream和future监听 

  ```
  Stream<User> user = ref.watch(userProvider.stream);
  Future<User> user = ref.watch(userProvider.future);//该Future以最新发出的值进行解析
  ```

  ​

### BLOC



### WeChat login

- [x] fluwx
      - no_pay版本
- [ ] ​

### app名称国际化

- **android\app\src\main\res**下面新建strings.xml文件

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <resources>
      <string name="app_name">你的应用名字</string>
  </resources>

  android:label="@string/app_name"
  ```

- iOS 名字国际化 只能新建**InfoPlist.strings**文件 其他名字会报错

  - 添加"CFBundleName" = "flutter demo";
  - 删掉info.plist文件的 <key>CFBundleDisplayName</key><string>Feellife 1</string>

🐛 lfs :: value=[230, 23, 0, 0, 0, 0, 0, 0, 0, 75, 0, 0, 0, 0, 85, 1, 242, 160, 8, 143]

I/flutter ( 2185): │ 🐛 lfs :: 我是蓝牙返回数据 - [0xe6, 0x17, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x4b, 0x00, 0x00, 0x00, 0x00, 0x55, 0x01, 0xf2, 0xa0, 0x08, 0x8f]

https://t66y.com/thread0806.php?fid=7

[^1]: 原生解析 