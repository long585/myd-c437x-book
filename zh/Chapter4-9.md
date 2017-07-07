### 4.9 LED测试

Linux系统下，LED的控制主要是通过访问sysfs文件系统下文件节点实现的，本例程演示如何使用echo命令和led\_test应用对LED进行控制，验证开发板上的LED驱动程序。

**测试硬件环境：**

* MYD-C437X-EVM 开发板一块  
* USB转TTL调试串口一根，连接MYD-C437X-EVM J16和PC, PC端波特率设置115200-8-n-1

**测试软件环境：**

* Linux Kernel 4.1.18   
* echo, led\_test 应用程序  

**测试过程：**

MYD-C437X-EVM由核心板和底板组成，底板上面有四个LED:D34、D35、D36、D39，核心板有两个LED:D1、D36，其中D1和D39不由软件控制，其他四个LED在软件上的命名通过myc和myd前缀来区分。

* 查看led设备的设备节点  

```
# ls /sys/class/leds/
myc_d36  myd_d34  myd_d35  myd_d36
```

* 用'echo'命令对LED进行控制  

```
#echo "0" > /sys/class/leds/myc:blue:cpu0/brightness
#echo "1" > /sys/class/leds/myc:blue:cpu0/brightness
#echo "0" > /sys/class/leds/myc:blue:cpu0/brightness

#echo "1" > /sys/class/leds/myd:blue:mmc1/brightness
#echo "0" > /sys/class/leds/myd:blue:mmc1/brightness

#echo "1" > /sys/class/leds/myd:blue:heartbeat/brightness
#echo "0" > /sys/class/leds/myd:blue:heartbeat/brightness

#echo "1" > /sys/class/leds/myd:blue:usr3/brightness
#echo "0" > /sys/class/leds/myd:blue:usr3/brightness
```

* 将目录`<WORKDIR>/Examples/rootfs/usr/bin/`中的可执行程序led\_test拷贝至开发板/usr/bin/目录下用'led\_test'应用程序对LED进行控制   

```
# led_test -h
Usage: led_test [options]

Version 1.0
Options:
-d | --device name   led name myc:blue:cpu0
-l | --light brightness   led brightness. 0~255 0: off.
-h | --help                Print this message
# led_test -d myc:blue:cpu0 -l 0
Set led myc:blue:cpu0 off, brightness = 0
# led_test -d myc:blue:cpu0 -l 1
Set led myc:blue:cpu0 off, brightness = 1
```

* 观察LED的变化  

> 注意：由于myc:blue:cpu0设置了触发器为cpu0, 所以不能直接控制，需要先往/sys/class/leds/myc:blue:cpu0/brightness文件节点写"0", 使触发器失效之后才能像其它LED一样正常控制。

MYIR AM437X系列其它板型LED测试情况类似。

