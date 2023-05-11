ble 服务特征

- air right 
  - 服务 1000
- air pro
  - 服务 1000





### 微信登录

- 模拟器解决方案

  - https://developers.weixin.qq.com/community/develop/doc/000c6275b68bf014265b30dc256800

    ​


- opened 唯一值
- ​

**ouYhT6oGiemtN3dI9t7l1tfjWOnU**



- https://developers.weixin.qq.com/community/develop/doc/000c6275b68bf014265b30dc256800?_at=1634607968131 [^1]

  ​




### iOS 健康

- HealthKit  [健康应用](https://www.jianshu.com/p/020e62b581ad)
- 健康隐私协议 app描述信息需要说明HealthKit
- [掘金文章](https://juejin.cn/post/7154177634707406855)
- [健康里面的名词解释很详细](https://www.jianshu.com/p/6e49cff7a0b6)

【HealthKit】将呼吸数据添加到HealthKit，当您在呼吸天使APP进行呼吸训练时，相应的数据将会同步到HealthKit 

有关您的隐私和HealthKit数据的重要说明:

由于您的健康隐私性非常重要，我们只允许请求写入部分数据的权限，您可以在设置->健康关闭APP写入数据权限



[baidu](www.baidu.com)
![picture](http://mouapp.com/Mou_128.png)





FeelLife

英文名称

iBreathe-The 3rd Way Of Dosing

应用描述

来福士智能蓝牙雾化器管理端

英文描述

The 3rd Way Of Dosing

应用官网

https://cn.feellife.com/

应用图标

![应用小图标](https://mmbiz.qlogo.cn/mmbiz_png/ibLHRNNxD8va9uNqhnRfUaOLs2aWmF86hy8jaiaVCb7gmOeEnP9K1IQTM35jbSK01KDnohteFmAMnEicpXmZtopZQ/0?wx_fmt=png) ![应用图标](https://mmbiz.qlogo.cn/mmbiz_png/ibLHRNNxD8va9uNqhnRfUaOLs2aWmF86hovJNEeic7SKzibtFzgLrELIYhiaRWgtE2nt6pjhateuQV4H4Ur1HkO39g/0?wx_fmt=png)

已上架应用下载链接

https://apps.apple.com/cn/app/1623298115

应用状态

未上架

应用类目

IT科技-硬件与设备

APP运行流程图

申请/修改应用说明

https://www.feellife.com/clients.html 网站都很明确显示app名字 请仔细看一下 我们官网没有放下载链接 但是我给你的网站文字明确提到了 app都在苹果商城呼吸天使上架了 

备注

每次迭代，页面展示最新审核通过的信息，OP审核后台保留相关数据

应用平台

#### iOS平台

iPhone

应用下载地址：https://apps.apple.com/cn/app/1623298115

Bundle ID：com.lfsmedical.FeelLife

测试版本Bundle ID：com.lfsmedical.FeelLife

Universal Links：https://www.feellife.com/

iPad 无

#### Android平台

应用下载地址：未填写

应用签名：f160f982a6e5e1bd0b77f96acbc2f706

包名：com.example.feellife_1

## 修改包名

- android目录下 app->build.gradle的applicationId :"com.example.feellife_1"
- main的AndroidManifest的package
- profile的AndroidManifest的package
- src下的debug的AndroidManifest的package
- 修改google-services.json包名
- [参考网址](https://www.flutterbeads.com/change-package-name-in-flutter/ )

## 打包aab出错

-  /Users/feellife/AndroidStudioProjects/feellife_1/build/app/intermediates/javac/release/classes/com/feellife/iBreathe/MainActivity.class

-  /Users/feellife/AndroidStudioProjects/feellife_1/build/app/tmp/kotlin-classes/release/com/feellife/iBreathe/MainActivity.class

- ```
  Execution failed for task ':app:minifyReleaseWithR8'.
  加入下面代码不行
  buildscript {

      repositories {
          maven {
              url 'https://storage.googleapis.com/r8-releases/raw'
          }
      }

      dependencies {
          classpath 'com.android.tools:r8:2.1.80'          // Must be before the Gradle Plugin for Android.
          classpath 'com.android.tools.build:gradle:X.Y.Z' // Your current AGP version.
       }
  }
  ```

- ​

## AirSmart

- 雷达图 参考 WBRadarAnimationDemo  AMDiscoverDeviceModule

- button文字图片排布  YLButton-master

- UITextField参考  ACFloatingTextField

- 选择器 参考 [BRPickerView](https://github.com/91renb/BRPickerView)

- UitableView的textfield无法点击 重写UitableView的touchbegan事件

- https://github.com/QMUI/QMUIDemo_iOS 腾讯UI

- 空白占位图使用的是 DZNEmptyDataSet

- 去除navgationbar分割线  self.navigationController.navigationBar.translucent = NO;

- 折线图  [AAChartView](https://github.com/AAChartModel/AAChartKit)

  ​

  ```

  -(UIImageView *)findHairlineImageViewUnder:(UIView *)view {
      if ([view isKindOfClass:UIImageView.class] && view.bounds.size.height <= 1.0) {
          return (UIImageView *)view;
      }
      for (UIView *subview in view.subviews) {
          UIImageView *imageView = [self findHairlineImageViewUnder:subview];
          if (imageView) {
              return imageView;
          }
      }
      return nil;
  }
  -(void)viewWillDisappear:(BOOL)animated{
      self.navigationController.navigationBar.translucent = YES;
  }
  //调用
  UIImageView *navigationImageView = [self findHairlineImageViewUnder:self.navigationController.navigationBar];
   navigationImageView.hidden = YES;
   self.navigationController.navigationBar.translucent = NO;
  ```

  ​

- 粒子动画 CAEmitterLayer 参考 EmitterAnimation

- k线年月日  参考HyChartsDemo

- uitableviewcell 里面for循环创建的视图取值 

  ```
  for (int i = 0; i<6; i++) {
    UIView *view = [cell.contentView viewWithTag:i+100];
  } 

  ```

- ​
  ```
  直接在 AAChartView 的 .m 文件中 注释掉这段代码即可:

  #if DEBUG
      [AAJsonConverter printPrettyPrintedJsonStringWithJsonObject:dic];
  #endif
  ```

  ​

  ```objective-c

  -(UIImageView *)findHairlineImageViewUnder:(UIView *)view {
      if ([view isKindOfClass:UIImageView.class] && view.bounds.size.height <= 1.0) {
          return (UIImageView *)view;
      }
      for (UIView *subview in view.subviews) {
          UIImageView *imageView = [self findHairlineImageViewUnder:subview];
          if (imageView) {
              return imageView;
          }
      }
      return nil;
  }
  -(void)viewWillDisappear:(BOOL)animated{
      self.navigationController.navigationBar.translucent = YES;
  }
  //调用
  UIImageView *navigationImageView = [self findHairlineImageViewUnder:self.navigationController.navigationBar];
   navigationImageView.hidden = YES;
   self.navigationController.navigationBar.translucent = NO;
  ```

  ​

- 粒子动画 CAEmitterLayer 参考 EmitterAnimation

- k线年月日  参考HyChartsDemo

- uitableviewcell 里面for循环创建的视图取值 

  ```swift
  for (int i = 0; i<6; i++) {
    UIView *view = [cell.contentView viewWithTag:i+100];
  } 

  ```

- ​

### delegate为nil 一般 类被提前释放 强引用属性保存

### question1 ：蓝牙断开 需要全局响应时 怎么不用注册多个通知 写在基类也不行，只会响应当前所在控制器

------

------

| 表头   | 表头   | 表头   |
| ---- | ---- | ---- |
| 内容   | 内容   | 内容   |
| 内容   | 内容   | 内容   |

| Title1    | Title2 | Title3 |
| --------- | ------ | ------ |
| Cell(2,1) | Cell2  | Cell3  |
| Cell(3,1) | Cell2  | Cell3  |

(```)  代码...  代码...  代码...(```)

`this is code`

```
    def func1(){
        print('hello')
    }

```

[1]   微信sdk 模拟器报错原因 使用pod导入

### delegate为nil 一般 类被提前释放 强引用属性保存

### question1 ：蓝牙断开 需要全局响应时 怎么不用注册多个通知 写在基类也不行，只会响应当前所在控制器 





----
****

| 表头   |  表头  |   表头 |
| ---- | :--: | ---: |
| 内容   |  内容  |   内容 |
| 内容   |  内容  |   内容 |




| Title1    | Title2 | Title3 |
| --------- | :----: | :----: |
| Cell(2,1) | Cell2  | Cell3  |
| Cell(3,1) | Cell2  | Cell3  |



(```)
  代码...
  代码...
  代码...
(```)





`this is code`

```python
    def func1(){
        print('hello')
    }

```
[^1]: 微信sdk 模拟器报错原因 使用pod导入