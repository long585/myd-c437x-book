### 4.2 LCD测试

本例程通过对Linux的FrameBuffer操作，实现LCD的彩色画点，画线，以及区域填充的测试。 也可以使用Buildroot文件系统自带的fbv程序显示图片。

**测试硬件环境：**

* MYD-C437X-EVM 开发板一块  
* MY-TFT070CV2 连接MYD-C437X-EVM J8 LCD16Bit 
* USB转TTL调试串口一根，连接MYD-C437X-EVM J16和PC, PC端波特率设置115200-8-n-1

**测试软件环境：**

* Linux Kernel 4.1.18   
* framebuffer\_test 应用程序  
* fbv应用程序

**测试过程：**

* 编译并拷贝`<WORKDIR>/Examples/rootfs/usr/bin/`目录下的测试程序framebuffer\_test至开发板/usr/bin目录。在Linux终端输入如下命令：  

```
# chmod 777 /usr/bin/framebuffer_test
# framebuffer_test -h
Usage: framebuffer_test [options]

Version 1.0
Available options:
-d | --device name   framebuffer device name, default: /dev/fb0
-h | --help          Print this message

# framebuffer_test -d /dev/fb0
xres:800 >>> yres:480 >>> bpp:32>>>
```

运行程序后，终端显示屏幕信息，LCD屏幕会先后出现多种背景色，然后进行彩色画点，画线，以及区域填充的测试。

* 将一个32位颜色深度，分辨率800X480的BMP图像拷贝到开发板/media/1.bmp, 用fbv测试图片显示：  

```
# fbv
Usage: fbv [options] image1 image2 image3 ...

Available options:
 --help        | -h : Show this help
 --alpha       | -a : Use the alpha channel (if applicable)
 --dontclear   | -c : Do not clear the screen before and after displaying the image
 --donthide    | -u : Do not hide the cursor before and after displaying the image
 --noinfo      | -i : Supress image information
 --stretch     | -f : Strech (using a simple resizing routine) the image to fit onto
                                           screen if necessary
 --colorstretch| -k : Strech (using a 'color average' resizing routine) the image to 
                                             fit onto screen if necessary
 --enlarge     | -e : Enlarge the image to fit the whole screen if necessary
 --ignore-aspect| -r : Ignore the image aspect while resizing
 --delay <d>   | -s <delay> : Slideshow, 'delay' is the slideshow delay in tenths of seconds.

Keys:
 r            : Redraw the image
 a, d, w, x   : Pan the image
 f            : Toggle resizing on/off
 k            : Toggle resizing quality
 e            : Toggle enlarging on/off
 i            : Toggle respecting the image aspect on/off
 n            : Rotate the image 90 degrees left
 m            : Rotate the image 90 degrees right
 p            : Disable all transformations
Copyright (C) 2000 - 2004 Mateusz Golicz, Tomasz Sterna.
Error: Required argument missing.

# fbv /meida/1.bmp
fbv - The Framebuffer Viewer
/media/1.bmp
800 x 480
```

程序执行完毕，图片完整显示在LCD上。

MYIR AM437X系列其它板型的LCD测试情况类似。

