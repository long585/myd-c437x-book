### 4.3 Touch Screen 测试

本例程主要使用buildroot制作的文件系统自带TS\_CALIBRATE测试程序，进行触摸屏的校准测试。

**测试硬件环境：**

* MYD-C437X-EVM 开发板一块  
* 一块MY-TFT070CV2 连接MYD-C437X-EVM J8  
* 或者一块MY-TFT070RV2 连接MYD-C437X-EVM J8  
* USB转TTL调试串口一根，连接MYD-C437X-EVM J16和PC, PC端波特率设置115200-8-n-1

**测试软件环境：**

* Linux Kernel 4.1.18   
* TS\_CALIBRATE应用程序  

**测试过程:**

* 断电，连接电容触摸模组，启动开发板，查看触摸设备对应的设备节点。  

```
# ls /dev/input/
by-path  event0   event1   event2   event3   mice     mouse0   mouse1

# cat /sys/class/input/event0/device/name 
gpio_key_pads@0

# cat /sys/class/input/event1/device/name 
tps65218_pwrbutton

# cat /sys/class/input/event2/device/name 
ti-tsc

# cat /sys/class/input/event3/device/name 
ft5x06_ts
```

从以上查询结果可知，电阻触摸对应的设备节点为/dev/input/event2, 电容触摸对应的设备节点为/dev/input/event3。测试步骤如下：

```
# export TSLIB_TSDEVICE=/dev/input/event3
# ts_calibrate
xres = 800, yres = 480
Took 4 samples...
Top left : X =   54 Y =   46
Took 4 samples...
Top right : X =  745 Y =   58
Took 4 samples...
Bot right : X =  745 Y =  421
Took 3 samples...
Bot left : X =   68 Y =  429
Took 3 samples...
Center : X =  394 Y =  245
-5.867981 1.023202 -0.019352
-2.867676 -0.003020 1.017846
Calibration constants: -384564 67056 -1268 -187936 -197 66705 65536
```

* 断电，连接电阻触摸模组，启动开发板，查看触摸设备对应的设备节点。  

```
# cat /sys/class/input/
event0/ event1/ event2/ input0/ input1/ input2/ mice/   mouse0/ 

# cat /sys/class/input/event0/device/name 
gpio_key_pads@0

# cat /sys/class/input/event1/device/name 
ti-tsc

# cat /sys/class/input/event2/device/name 
tps65218_pwrbutton
```

从以上查询结果可知，电容触摸对应的设备节点为/dev/input/event1, 测试步骤如下：

```
# export TSLIB_TSDEVICE=/dev/input/event1
# ts_calibrate
xres = 800, yres = 480
Took 60 samples...
Top left : X =  244 Y =  592
Took 72 samples...
Top right : X = 3674 Y =  604
Took 84 samples...
Bot right : X = 3695 Y = 3404
Took 2 samples...
Bot left : X =  275 Y = 3295
Took 11 samples...
Center : X = 2040 Y = 1881
-2.257812 0.204344 -0.001784
-25.022156 -0.002373 0.137957
Calibration constants: -147968 13391 -116 -1639852 -155 9041 65536 
```

MYIR AM437X系列其它板型Touch Screen测试情况类似。

