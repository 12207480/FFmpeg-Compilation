# FFmpeg 编译

大家都知道，要播放多种格式MP4,MOV, 视频流的rtmp,rtsp,还有直播格式的m3u8，是一件好蛋疼的事情，幸好:
-	我们有FFmpeg:http://ffmpeg.org/
-	有FFmpeg的封装版KxMovie:https://github.com/kolyvan/kxmovie


##步骤一般是这样的
- 1.编译FFmpeg，生成适合arm7,arm7s,arm64等架构的静态库跟源文件
- 2.把编译好的文件放到项目里面
- 3.项目是基于KXMovie的直接用OC使用，直接使用的用FFmpeg的C语言API
	
	
##但是第一步编译FFmpeg都是好复杂好容易崩溃的事情:
现在好了，有yasm，FFmpeg-iOS-build-script这些大神弄出来的东西帮我们简化工作，直接搞到播放视频，播放直播的任务:


PS:编译后的包大小有189M，太大了，大家按着步骤走也可以运行Demo的


#方法A
###用脚本形式来生成ffmpeg所需要的静态库跟源文件
- git clone git://github.com/kolyvan/kxtorrent.git kxmovie 或者下载 :kxmovie:https://github.com/kolyvan/kxmovie
- 安装装yasm，指令：sudo curl http://www.tortall.net/projects/yasm/releases/yasm-1.2.0.tar.gz >yasm.tar.gz
- 下载https://github.com/libav/gas-preprocessor到/usr/bin 
  （`可能出现这个错误GNU assembler not found, install/update gas-preprocessor,其实第三步会自动下载gas-preprocessor,第二步可以去掉,网上的教程都是旧的`）
- 下载脚本文件kewlbear/FFmpeg-iOS-build-script 运行 `./build-ffmpeg.sh
`,将会把所有架构所需的文件都编译打包完整,如图。

![](http://7xo1qe.com1.z0.glb.clouddn.com/git%2Fkx_thin.png)


- 把FFmpeg-iOS这个包含所有架构的文件添加到kxmovie里，并添加必要的框架


![FFmpeg-iOS1](http://7xo1qe.com1.z0.glb.clouddn.com/git%2Fkx_total.png)

-	下面是项目需要的框架,不用加少了，特别VideoTool那些

![FFmpeg-iOS2](http://7xo1qe.com1.z0.glb.clouddn.com/git%2Fkx_frameowork_s.png)

- 编译运行Demo

  -	蜗壳是本地的，普通网络的
  
![Demo](http://7xo1qe.com1.z0.glb.clouddn.com/git%2Fkx_video.png)

  -	HLS,m3u8结尾的，是凤凰卫视，视频直播
  
![HLS](http://7xo1qe.com1.z0.glb.clouddn.com/git%2Fkx_m3u8.png)


##可能出现的错误:
- GNU assembler not found, install/update gas-preprocessor:
https://github.com/kewlbear/FFmpeg-iOS-build-script/issues/20

- XCode7.1: "Implicit declaration of function 'avpicture_deinterlace' is invalid in C99'":（答案是我写的)
https://github.com/kolyvan/kxmovie/issues/125



#方法B

###官方方法
 -  git clone git://github.com/kolyvan/kxtorrent.git kxmovie

- 配置编译ffmpeg
  -  cd kxmovie
  -  git submodule update --init
  -  rake
  - `git submodule update --init`超级无敌慢

#参考链接：

- kxmovie：https://github.com/kolyvan/kxmovie
- FFmpeg-iOS-build-script：https://github.com/kewlbear/FFmpeg-iOS-build-script
- 简易教程：http://blog.csdn.net/oqqquzi1234567/article/details/43152689
- FFmpeg的编译,非kxmoive:http://www.cocoachina.com/ios/20150514/11827.html
- 基于使用AsyncSocket实现RTSP协议(用RTSP+RTP来实现摄像头的实时监控):http://www.1000phone.net/thread-7784-1-5.html
- CocoaAsyncSocket基础教程:http://www.1000phone.net/thread-7781-1-1.html






