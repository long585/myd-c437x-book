### 4.14 AUDIO

本例程演示通过linux下ALSA音频框架及其alsa-utils工具集来实现音频播放和录音的功能。

**测试硬件环境：**

* MYD-C437X-EVM 开发板一块
* 双头耳机一副

* USB转TTL调试串口一根，连接MYD-C437X-EVM J16和PC, PC端波特率设置115200-8-n-1

**测试软件环境：**

* Linux Kernel 4.1.18

* alsa-utils工具集，audio\_test应用程序

**测试过程：**

* 开发板开机，将双头耳机的耳机头插入开发板J19 \(LINE\_OUT\),将耳机的麦克风头插入到开发板J18 \(LINE\_IN\).
* 测试音频播放，将准备好的WAV格式的音频文件拷贝到开发板`/media/test.wav`目录下，用alsa-utils工具集里面的`aplay`来测试音频播放：

```
# cd media/
# ls
test.wav
# aplay test.wav 
Playing WAVE 'test.wav' : Signed 16 bit Little Endian, Rate 44100 Hz, Stereo
```

此时通过耳机即可听到正在播放的音乐文件，如需调节音量可通过alsa-utils工具集里面的`alsamixer`来调节，这个命令执行然后就会出现字符形式的图形界面，使用四个方向键就可以进行调节：

```
+------------------------------ AlsaMixer v1.1.2 ------------------------------+
| Card: MYD-C437X-EVM                                  F1:  Help               |
| Chip:                                                F2:  System information |
| View: F3:[Playback] F4: Capture  F5: All             F6:  Select sound card  |
| Item: Headphone [dB gain: -4.00, -4.00]              Esc: Exit               |
|                                                                              |
|     +--+                       +--+     +--+                                 |
|     |  |                       |aa|     |  |                                 |
|     |  |                       |aa|     |  |                                 |
|     |  |                       |aa|     |  |                                 |
|     |  |                       |aa|     |  |                                 |
|     |  |                       |aa|     |  |                                 |
|     |  |                       |aa|     |  |                                 |
|     |aa|                       |aa|     |  |                                 |
|     |aa|                       |aa|     |  |                                 |
|     |aa|                       |aa|     |  |                                 |
|     |aa|                       |aa|     |  |                                 |
|     |aa|                       |aa|     |  |                                 |
|     +--+     DAC      +--+     +--+     +--+     +--+    MIC_IN    +--+      |
|                       |OO|                       |MM|              |OO|      |
|                       +--+                       +--+              +--+      |
|    50<>50                    100<>100    0                                   |
|  <Headphon>Headphon Headphon   PCM      Mic    Capture  Capture  Capture     |
+------------------------------------------------------------------------------+
```

通过方向键左右切换项目，`Item: Headphone [dB gain: -4.50, -4.50]`项目为音量调节，通过方向键上下调节所需的音量大小。

* 测试音频输入，使用alsa-utils工具集里面的`arecord`来测试：

```
# arecord -c 2 -r 44100 -f S16_LE record.wav
Recording WAVE 'record.wav' : Signed 16 bit Little Endian, Rate 44100 Hz, Stereo
```

结束录音，通过`aplay`来播放刚才的录音文件：

```
# aplay record.wav 
Playing WAVE 'record.wav' : Signed 16 bit Little Endian, Rate 44100 Hz, Stereo
```

用户也可通过audio\_test应用程序来测试音频，此应用的功能为同步播放录音的音频

* 将目录`<WORKDIR>/Examples/rootfs/usr/bin/`中的可执行程序audio\_test拷贝至开发板/usr/bin/,执行下面命令：

```
# audio_test 
rate set to 21999, expected 22000
Init capture successfully, rate: 21999, period_size: 128
rate set to 21998, expected 21999
Period size: 128 frames, buffer size: 256 bytes
^C38 frames
```

此时通过麦克风说话，耳机端也能同步听到声音。

* MYIR AM437X系列其它板型audio测试可以参照本例程进行。



