### 媒体元素

在HTML5问世之前，要在网络上展示视频，音频，动画，除了使用第三方自主开发的
播放器之外，使用得最多的工具就是Flash,但是需要在浏览器上安装各种插件，并且有时
速度很慢。HTML5新增了两个与媒体相关的标签，让开发人员不必依赖任何插件就能在
网页中嵌入跨浏览器的音频和视频内容，这两个标签是 <audio> <video>。

#### 在页面中嵌入

```html
<!-- 通过 src 指定一个视频地址 -->
<video src="resource/demo.mp4" type="video/mp4" controls autoplay>
</video>
<!-- 通过 source 元素为同一个媒体数据指定多个播放格式与编码方式，确保浏览
器可以从中选择一种自己支持的播放格式 -->
<video autoplay controls>
<source src="resource/demo_1.mp4" type="video/mp4">
<source src="resource/demo_2.mp4" type="video/mp4">
</video>
```

#### 基本属性

- width 视频的宽度，以像素为单位
- height 视频的高度，以像素为单位
- src 视频的地址
- controls 控制条
- autoplay 设置自动播放
- poster 当 视频不可用时，向用户展示一副替代用的图片
  - 填充图片大小`video{object-fit: fill}`
- loop 是否循环
- volume 音量，取值为 0~1
- muted 是否处于静音状态
- type 媒体类型 MIME mp4- video/mp4
- defaultPlaybackRate 默认播放速率
- playbackRate 当前播放 速率 >  1 快放 放 < 1 慢放
- currentTime/duration 当前播放位置、总的播放时间

#### 只读属性

- currentSrc 读取播放中的媒体数据的 URL 地址
- readyState 准备状态
  0  没有获取到媒体的任何信息
  1  获取媒体数据无效，无法播放
  2  当前帧已获得，但还未获取到下一帧数据
  3  当前帧已获得，也获取到下一帧数据
  4  当前帧已获得，也获取到了让播放器前进的数据
- played 、 paused 、 ended
  已经播放的时间段、是否暂停、是否播放完毕

#### 共有方法

- play() 播放媒体，自动将元素的paused属性的值变为false
- pause() 暂停播放，自动将元素的paused属性变为true
- load() 重新载入媒体进行播放，自动将元素的playbackRate属性的值变为defaultPlaybackRate属性的值，自动将元素的error值变为null
- canPlayType() 用来测试浏览器是否支持指定的媒体类型

#### 事件

- loadstart 浏览器 开始在网上寻找媒体数据
- progress  浏览器 正在获取媒体数据
- suspend  浏览器 暂停获取媒体数据
- abort  浏览器 在下载完全部媒体数据之前中止获取媒体数据，非错误引起
- error  获取 媒体数据过程中出错
- emptied  所 在网络变未初始化状态
- stalled  浏览器 舱室获取媒体数据失败
- play  即将 开始播放
- paused  播放 暂停
- loadedmetadata 浏览器 已经获取完毕媒体时间长和字节数
- loadeddata 浏览器 已加载完毕当前播放位置的媒体数据，准备播放
- waiting  播放过程中由于得不到下一帧而停止播放
- playing  正在播放
- canplay 正在播放，并且以当前播放速率能够将媒体播放完毕，需缓存
- canplaythrough 浏览器可以播放媒体，并且以当前播放速率能够将媒体播放完毕，无需缓存
- seeking  请求数据， seeking 属性为 true
- seeked 停止请求数据， seeking 属性为 false
- timeupdate 当前播放位置改变
- ended  播放结束
- ratechange 播放速率改变
- durationchange 播放时长改变
- volumechange 音量改变

#### 获取本地视频流

- 1. 需要一个接口：提供当前的方法调用的一些功能，navigator：提供视频流注册的一些活动
  2. 目的：获取相机权限，开启摄像头
  3. mediaDevices：提供访问链接媒体输入的设备（相机，麦克风）
  4. 得到用户的设备，使用户开启设备权限 getUserMedia()
  5. 得到用户的媒体流输出到video

