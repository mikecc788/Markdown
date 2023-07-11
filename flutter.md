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

```
// com.example.feellife1
com.lfs.ibreathe.ble


```

## google账号申请

- 注册 Google 账号
- 进入开发者链接（<https://play.google.com/apps/publish/signup>），登录 Google 账号。
- 付费 25 美元。需要 VISA 或 Master 等信用卡

### 本地网络配置

- flutter ios Local Network Privacy Permissions

### 生成iOS文件夹

- ```
  flutter create -i swift .
  ```

- 国际化报错 （未解决）


### iOS 启动页

- view里面设置启动图片
- 设置view的背景色 默认白色 而且比较早


## FireBase

- 安装 CLI https://firebase.google.com/docs/cli?authuser=0&hl=zh#sign-in-test-cli

- 查看版本  `firebase --version` 

- 登录`firebase login`

- ID 98913

- 如果登录失败 编辑bash.profile. export http_proxy=[http://127.0.0.1:7890;export](http://127.0.0.1:1087;export/) https_proxy=http://127.0.0.1:7890

- xcode 运行报错的话 可以在flutter项目ios文件夹下面单独命令行安装pod install  Podfile文件夹 platform :ios, '10.0'注释打开  

- 终端执行命令 不能再flutter项目里面执行无效

  ```
  dart pub global activate flutterfire_cli
  flutterfire configure --project=airsmart-35823
  ```

  ​

### add android app

- [intorduce doc](https://console.firebase.google.com/project/instagram-test-c9877/overview?hl=zh-cn)


- /Users/feellife/Desktop/instagram_demo/android/app/build.gradle   defaultConfig找到id

- android/build.gradle dependencies添加依赖项 'com.google.gms:google-services:4.3.12'

- /android/app/build.gradle defaultConfig 添加一行 multiDexEnabled true

- 上传出错 修改规则 有效

  ```
  service firebase.storage {
    match /b/{bucket}/o {
      match /{allPaths=**} {
        allow read, write: if true;
      }
    }
  }
  ```

  ​

## android 运行

### 审核

- //本"吸哈"应用"体验-保健-任意文章下的往期推荐”页面存在定向广告但不涉及广告内容的个性化推送,因此不做显著标识。推荐广告内容向所有用户展示，非个性化推荐/定向推送

### AS常用快捷键

- opt + cmd + ←  回退到上一次编辑

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

  - [下载wx tool](https://blog.csdn.net/qq2276031/article/details/126123912)

  ```
  cd android  
  ./gradlew signingReport  //好像有问题

  7503756F5778729790781A517D5A0C01
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

- 蓝牙反复通知的问题 断开的时候监听那里取消通知[refer](https://segmentfault.com/a/1190000037495356?sort=votes)

- Resolve send duplicate notify when reconnected [issue525](https://github.com/pauldemarco/flutter_blue/issues/525#issuecomment-734281294)

- timeout有问题

  ```dart
  error
  // flutterBlue.startScan(timeout: scanTimeout); 
  timeout写在外面 用系统自带的
  flutterBlue.startScan().timeout(scanTimeout);  
  ```

- release版本蓝牙搜索不到 混淆代码忽略了 

  ```java
  minifyEnabled false //删除无用代码
  useProguard false    //代码压缩设置
  shrinkResources false //删除无用资源
  ```

  [release issue](https://github.com/pauldemarco/flutter_blue/issues/768)

- 设置MTU

  https://github.com/pauldemarco/flutter_blue/issues/902

- 解决3.11.4问题

  ```
  https://github.com/protocolbuffers/protobuf/issues/8062
  要用安卓运行  才能看到flutter_blue的gradle 
  ```

  ​

### 代码混淆

1. you will need to create the proguards rules file at  android/app/proguard-rules.pro

   - In this file add the proguard rules as  

     ```dart
     -keep class com.pauldemarco.flutter_blue.Protos* { *; }

     //-keep class com.pauldemarco.flutter_blue.* { *; }
     ```

2. Go into the `android/app/build.gradle` file and add the `proguardFiles`

   ```Fav
   android {
       release {
           proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
       }
   }
   ```

   ​

### 解决flutter镜像问题

### 解决m1芯片插件问题

- https://blog.csdn.net/m0_37780940/article/details/116646620

  ​

-  put get : 获取依赖
-  删除了 GradleException 前面的“new”
-   `File -> Project Structure -> Modules -> Select your project -> apply your API`


### 打包apk

1. feellife@apps-iMac feellife_1 % keytool -genkey -v -keystore ~/sign.jks -keyalg RSA -keysize 2048 -validity 10000 -alias sign
2. 上面指令可能报错 下面这条
   - feellife@apps-iMac feellife_1 % keytool -genkey -v -keystore ~/feellife-keystore.jks -keyalg RSA -keysize 2048 -validity 10000 -alias feellife -storetype JKS
     -  -alias feellife -storetype JKS  feellife为 keyAlias名称
3. flutter build apk --release
4. flutter build apk --release --no-sound-null-safety //如果没有适配空安全就打没有空安全的包

### 权限请求报错

main.xml 加上xmlns:tools="http://schemas.android.com/tools"

### 清除打印信息

```
flutter run | grep -v "D/ViewRootImpl" 
flutter logs //可行
```

- logcat

  ```dart
  package:mine level:DEBUG 过滤Debug可以输入
  package:mine    -tag:<D/FlutterBluePlugin>  level:DEBUG 
  ```

  ​

## Dart库

### 添加代码模版

[Live Templates添加代码模版](https://www.jianshu.com/p/dc5cd0c40f93)

![7F8CCD4A-5223-45F7-A2A8-92A373185B5C](https://tva1.sinaimg.cn/large/e6c9d24egy1h4rcor0vuaj21760u0781.jpg)

### flutter_launcher_icons

- 配置yaml文件 添加一张1024*1024 图片 名字对上就可以 

  ```
  flutter_icons:
    image_path: "images/ic_launcher.png"
    android: "launcher_icon" # can specify file name here e.g. "ic_launcher"
    ios: false
  ```

- 输入命令 flutter pub run flutter_launcher_icons:main 生成icon文件

- 报错问题  Cannot not find minSdk from android/app/build.gradle

  ```
  added flutter.minSdkVersion to android/local.properties.
  flutter.minSdkVersion=30 // I added here
  ```

  ​

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

- list 转str

  ```
   lungData = data.join('');
  ```

  ​

- 去重 转换为 Set 然后反转为 List

  ```
  final myNumbers = [1, 2, 3, 3, 4, 5, 1, 1];
    final uniqueNumbers = myNumbers.toSet().toList();
  ```

- 计算平均值

  [stack](https://stackoverflow.com/questions/54441296/flutter-how-do-calculate-average-the-data-in-list)

####多项式

- equations: ^4.1.0 

https://pub.dev/documentation/scidart/latest/numdart/PolyFit-class.html

### 去除空安全打印警告

https://blog.csdn.net/AllThePain/article/details/125658530

#### map

- 从数组新增元素 
  ```dart
  // Map map1 = Map();
  // for (int i = 0; i < preFixArr.length; i++) {
  // map1.putIfAbsent(preFixArr[i], () => deviceName[i]);
  // }
  ```

  ​

- 类型转换 `snap.data() as map<String,dynamic>`

### StreamBuilder

- 异步编程方式 ===通过 Stream 实现每秒钟局部更新数据

### tips  final const

- const 在编译的时候值都必须是确定的  final是在运行的时候才赋值

### @immutable

- 被@immutable注解标明的类或者子类都必须是不可变的
- 定义到Widget中的数据一定是不可变的，需要用final来修饰

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
  //bytedata 
  https://api.dart.dev/stable/1.10.1/dart-typed_data/ByteData-class.html
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

## UI study

- [Flutter_deer ](https://github.com/simplezhli/flutter_deer)
- ​

## 组件Widget

- 快捷键 command + option + t  ：快速调出环绕代码
- option + enter  ：将 widget 自动包裹一层 widget 或删除这层 widget


- ​	**向下传递约束 向上传递尺寸**
- 跳转页面的时候去掉 **MaterialApp** 不然没有返回箭头
- listview切圆角要和背景图片一起设置

### 下载地址

- https://appgallery.huawei.com/app/C106428933 华为应用商店 吸哈
- ​

### 常用组件

- FittedBox 放大缩小字体 配合Text使用
- LayoutBuilder 得到父级控件的大小

### navigator

- 处理系统事件

[使用1](https://developer.aliyun.com/article/918784)



#### 命名路由

> 这种方式原生并不支持直接解析路由参数

```dart
 // 路由表  
routes: {
  '/': (context) => HomeScreen(),
  '/details': (context) => DetailScreen(),
},
//跳转
Navigator.pushNamed(
  context,
  '/details',
);

//报错 Flutter : Could not find a generator for route RouteSettings("/HomePage", null) in the _WidgetsAppState
使用 Navigator.of(context, rootNavigator: true).pushNamed("/route");
而不是Navigator.pushNamed("/route");
rootNavigator 应用场景嵌套导航器
```

- #### 结合onGenerateRoute使用

#### 2.0路由

### bottom_nav_bar

- 常用库 persistent_bottom_nav_bar_v2

  ```dart
  NavBarStyle _navBarStyle = NavBarStyle.style15; 设置样式
    onItemSelected: (index){}//设置点击事件
  主页面不要设置appbar 每个页面单独设置
  ```

  ​

### Draw

- How to make flutter drawer above bottom navigation  

### snackbar

- another_flushbar: ^1.10.29
- flash: ^2.0.3+3

### ChoiceChip

### Overlay

> `Overlay`是一个可以管理的堆栈,通过将一个Widget插入这个堆栈中，这样就可以让此`Widget`浮在其他的`Widget`之上，从而实现悬浮窗效果

- OverlayEntry对象的配置来管理Overlay的层级关系
- ​

### PopupMenu

- 弹出菜单栏

### flexible

- 把屏幕剩余空间按比例分割 比如 SizedBox(width: 100,), 就是减去100再去按比例分割

### Spacer

- `Spacer()` 相当于弹簧的效果,使两个控件之间的距离达到最大值. (在页面不可滑动时才有效果)
- ​

### showSearch

- 搜索框

### Stepper

- 步骤列表

### context

- context无效
  - 可以在调用之前提前定义好需要context的控件

### slivers

- SliverGrid  SliverToBoxAdapter SliverList搭配使用

- spacer（）和

  ​

### TextField

```dart
//居中  设置  isCollapsed: true,以及Center
```



### IndexedStack

- 可以在几个页面切换的页面使用  设置currentIndex

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

### 底部tabbar

- 第一次参考 [Marshal-S](https://github.com/Marshal-S)/flutter_list_demo

### 动画

- 空安全可以使用late初始化 不用写在initstate

```dart
late AnimationController _controller = AnimationController(vsync: this);
```

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

### Column 里面放一个横向滚动的列表

- Fun1  ：利用横向row实现
- fun2： stack+listviewbuild实现  stack先用一个widget确定宽高设置透明（这个用来向上传递尺寸的作用 不做显示） 其余的利用positon覆盖（positon可以利用最先的widget的大小）

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

- 跟随父列表滚动  

  ```dart
   physics: NeverScrollableScrollPhysics(),跟随父列表滚动
  physics: ScrollPhysics(),滚动
  ```

  ​

#### 多选实现

- https://github.com/ritsat/listview_multiselection/blob/master/lib/main.dart

### ExpansionTile

- 改变leading间距 tilePadding: EdgeInsets.zero,

- 去掉分割线 

  ```
  Theme(
    data: Theme.of(context).copyWith(dividerColor: Colors.transparent),
        child: ExpansionTile(
  ```

  ​

### SingleChildScrollView

- 解决 Expand 布局页面 键盘弹出溢出问题 

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

  ```dart
    decoration: const BoxDecoration(
            borderRadius: BorderRadius.all(Radius.circular(18)),
            gradient: LinearGradient(
              colors: [
                Color(0xff2c274c),
                Color(0xff46426c),
              ],
              begin: Alignment.bottomCenter,
              end: Alignment.topCenter,
            ),
          ),
  ```

  ​


### Listener

- 监听点击 触摸事件

### Button

- Textbutton:  MaterialStateProperty.all()设置属性
- Shape 设置圆角

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

  ![WeChatbe739d91a5da1989da3fa7d2fcc3fc08](https://tva1.sinaimg.cn/large/e6c9d24egy1h3a6p97d9jj20rg0gawfx.jpg)


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

refer [登陆配置](https://blog.csdn.net/haoxuhong/article/details/117956586)

#### ios端

- 证书页面 需要打开 Associated Domains

- [ ] 

### 启动页

- iOS 启动页白屏 添加1x 2x 3x图片 设置leading  traing top和bottom注意父视图设置成view 也可以解决顶部留白问题

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


### 底部导航栏沉浸

```
main() {
  runApp(MyApp());
  
  if (Platform.isAndroid) {
    SystemUiOverlayStyle systemUiOverlayStyle = SystemUiOverlayStyle(
        statusBarColor: Colors.transparent,
        systemNavigationBarColor: "#2196f3".toColor); 
    SystemChrome.setSystemUIOverlayStyle(systemUiOverlayStyle);
    SystemChrome.setEnabledSystemUIMode(SystemUiMode.edgeToEdge);
  }
}
```

## 输入框等弹出键盘超出范围

- 使用SingleChildScrollView包裹部件


## 点击弹出菜单栏

- flutter_portal: ^1.1.4


## sqflite update数据需要上传默认id

## Navigator反向页面传值刷新页面.pop(true) then接收



https://t66y.com/thread0806.php?fid=7

//https://www.cnki.net/

[^1]: 原生解析 