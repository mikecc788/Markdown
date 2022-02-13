[toc]

Analogue:模拟 [ˈænəlɔːɡ]

Voltage:电压

Electrical:[ɪˈlektrɪkl]  用电的

PCM:采样率 44.1kHZ

## Audio Queue Services

- 1，给buffer填充数据，并把buffer放入就绪的buffer queue；
- 2，应用通知队列开始播放；
- 3、队列播放第一个填充的buffer；
- 4、队列返回已经播放完毕的buffer，并开始播放下面一个填充好的buffer；
- 5、队列调用之前设置的回调函数，填充播放完毕的buffer；
- 6、回调函数中把buffer填充完毕，并放入buffer queue中

## 音频基础

**音频数据的三个层级，buffer, packet, frame**

数据缓冲 buffer , 装音频包 packet，

音频包 packet，装音频帧 frame

- 固定码率 CBR, 平均采样，对应原始文件，pcm ( 未压缩文件 )，可变码率 VBR，对应压缩文件，例如: mp3

AudioStreamBasicDescription：一些配置信息，包含通道数、采样率、位深...

### 音频 data >> 音频包 Packet

- **建立音频的处理通道, 注册解析回调方法**
- **传递数据进来，开始解析**

### **音频包 packet >> 音频缓冲 buffer**

### AudioQueueBufferRef

- AudioQueue录制音频原始帧PCM数据

- AudioQueue播放PCM音频文件

- ### 通过回调函数与CoreAudio交互

  - CoreAudio会需要向App请求一些音频数据，App通过从文件系统中读取音频数据，然后通过callback函数传递给CoreAudio。（播放时候）

  ```objective-c
  typedef void (*AudioQueuePropertyListenerProc) (
                  void *                  inUserData,
                  AudioQueueRef           inAQ,
                  AudioQueuePropertyID    inID
              );
  ```

#### Audio Data Packets

- 将一个或者多个frames称为一个packet

  
  
  

# AudioEngine

- `AVAudioMixerNode` - 混淆多个输入到一个输出

  

# buffer

- buffer 大，声卡去消费的时间长，时间延迟 latency 大一些

  buffer 小，声卡去消费快，时延小

  #### 采样数据，转 buffer

  

# service

- Audio File Services, 处理本地音频资源文件

# CMTime

- value表示当前第几帧，timescale表示每秒钟多少帧，播放时间为value/timescale，例如创建一个代表5s的CMTime表达式有下面几种不同的方式

```swift
let time1 = CMTimeMake(value: 5, timescale: 1)
```

