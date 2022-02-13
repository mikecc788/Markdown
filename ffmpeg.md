[toc]

# ffmpeg

​	**ffmpeg -i 1.mp4 demo.avoi**  视频转换

- 输入`ffmpeg -f avfoundation -list_devices true -i ""`

- *`AVFoundation video devices`下是视频设备源头，分别是摄像头和屏幕捕捉*

- `AVFoundation audio devices`下是音频设备源，第一个是刚刚安装的Soundflower，第二个是麦克风。这里我们选屏幕捕捉和Soundflower，记下对应序号2和3

​	![image-20210918171956330](/Users/janson/Desktop/Markdown/image-20210918171956330.png)

**start**

1. `ffmpeg -y -f avfoundation -i 1:0 -framerate 60 -c:v libx264 -r 60 -pix_fmt yuv420p -preset 0 -crf 19 -c:a aac -b:a 192k "$HOME/Screen Record $(date "+%Y-%m-%d %H-%M-%S").mp4"`
    注意`-i`后面的参数就是刚才需要记下的两个序号，录像需要停止的话按`q`即可

## 设备枚举

1. 查看设备列表

   - ffmpeg -hide_banner -devices

2. 设备采样举例

   - ffmpeg -f avfoundation -list_devices true -i ""

3. 采集内置摄像头

   - ffmpeg -f avfoundation -video_size 1280x720 -framerate 30 -video_device_index 1 -i "FaceTime高清摄像头" out.mp4 

     - > *1280x720@[30.000030 30.000030]fps* 支持的framerate

   - 引号来代替  ffmpeg -f avfoundation -i 1 out.mp4

   - 所以上面的命令也可以使用 ffmpeg -f avfoundation -video_size 1280x720 -framerate 30  -i "1:1" out.mp4 

4. 播放视频

   - ffplay out.mp4

## 采集桌面

- 采集桌面  ffmpeg -f avfoundation -i 2 -r:v 30 screen.mp4
  - 2 代表引号



## 安装nginx

 1. brew install nginx(这种安装有问题) 通过$ brew tap denji/homebrew-nginx 然后$ brew install nginx-full --with-rtmp-module

    - > `--with-rtmp-module`，一定要加上rtmp模块

 2. 启动nginx

    - sudo nginx

	3. 关闭nginx

    - sudo nginx -s stop

	4.  重新加载nginx

    - sudo nginx -s reload

	5. 配置 （默认端口8080）

    - brew info nginx 查看conf文件位置 进去修改

    - ```tex
      在http节点后面加上rtmp配置：
      rtmp {
        ``server {
          ``listen 1935;
          ``application live1 {
            ``live ``on``;
            ``record off;
          ``}
        ``}
      }
      ```



6. 保存文件后 重新加载

7. 推流 ffmpeg -re -i 本地视频路径 -vcodec libx264 -acodec aac -strict -2 -f flv rtmp://localhost:1935/live/room

8. 推流本地视频 sudo ffmpeg -re -i /Users/janson/Desktop/obs/3.mp4 -vcodec copy -f flv rtmp:localhost:1935/live1/room1

9. vlc播放

10. 分享桌面 ffmpeg -f avfoundation -i "2" -vcodec libx264 -preset ultrafast -acodec libfaac -f flv rtmp:localhost:1935/live1/room1

11. 桌面+麦克风 ffmpeg -f avfoundation -i "2:3" -vcodec libx264 -preset ultrafast -acodec libmp3lame -ar 44100 -ac 1 -f flv rtmp:localhost:1935/live1/room1

    - ffmpeg -f avfoundation -framerate 30 -video_size 1280x720 -i "2:3" -vcodec libx264 -preset ultrafast -acodec libmp3lame -ar 44100 -ac 1 -f flv rtmp:localhost:1935/live1/room1（方法二）

    - ffmpeg -f avfoundation -framerate 30 -video_size 1280x720 -i "2:3" -vsync 2 -vcodec libx264 -preset ultrafast -acodec libmp3lame -ar 44100 -ac 1 -b:v 1M -b:a 128K -f flv rtmp:localhost:1935/live1/room1（方法三）

    12. 推流麦克风 ffmpeg -f avfoundation -i ":2" -vcodec libx264 -preset ultrafast -acodec libmp3lame -ar 44100 -ac 1 -f flv rtmp:localhost:1935/live1/room1
    13. 推流摄像头 ffmpeg -f avfoundation -framerate 30 -video_size 1280x720 -i  "0"  -vcodec libx264 -acodec libfaac -f flv  rtmp://192.168.10.61:1935/zbcs/room
    14. 推流桌面 + 只有桌面内容
        - ffmpeg -f avfoundation -i "1" -vcodec libx264 -preset ultrafast -acodec libfaac -f flv rtmp://192.168.10.61:1935/zbcs/room
    15. **桌面录屏** **ffmpeg -f avfoundation -pixel_format uyvy422 -i "2" -f flv rtmp:localhost:1935/live1/room1**（相对可靠一点）

## 安装soundflower
