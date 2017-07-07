### 4.13 CAMERA

本例程演示在linux下的通过V4L2 API接口实现双摄像的显示，详情请参考源码。

**测试硬件环境：**

* MYD-C437X-EVM 开发板一块
* MY-CAM011B摄像头模块两个

* MY-TFT070-K显示屏一块

* USB转TTL调试串口一根，连接MYD-C437X-EVM J16和PC, PC端波特率设置115200-8-n-1

**测试软件环境：**

* Linux Kernel 4.1.18

* camera\_test应用程序

**测试过程：**

* 开发板关机，将两个个摄像头模块MY-CAM011B分别接入到J21、J23摄像头接口中，MY-TFT070-K显示屏接入J8显示接口，上电开机。通过命令查看摄像头驱动是否记载成功并生成设备：

```
# ls /dev/v*
/dev/vcs          /dev/vcsa         /dev/vga_arbiter  /dev/video1
/dev/vcs1         /dev/vcsa1        /dev/video0

/dev/v4l:
by-path
```

可以看到驱动加载正常并成功找到两个摄像头设备video0、video1

* 将目录`<WORKDIR>/Examples/rootfs/usr/bin/`中的可执行程序camera\_test拷贝至开发板/usr/bin/,执行下面命令：

```
#chmod +x /usr/bin/camera_test
# camera_test -h
Usage: camera_test [options]

Version 1.0 
Options:
-h | --help                Print this message
```

可以看到本例程不需要其他参数，直接运行：

```
# camera_test 
xres:800 >>> yres:480 >>> bpp:32>>>
CRTCs size: 800x480

Capture 0: Opened Channel

Capture 0: Capable of streaming

Capture 0: Init done successfully

Exported buffer fd = 7

Exported buffer fd = 9

Exported buffer fd = 11


Capture 1: Opened Channel

Capture 1: Capable of streaming

Capture 1: Init done successfully

Exported buffer fd = 14

Exported buffer fd = 16

Exported buffer fd = 18
```

运行成功后即可在7寸LCD显示屏上看到两个摄像头的画面

* MYIR AM437X系列其它板型PRU测试可以参照本例程进行。



