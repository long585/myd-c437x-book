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

从以上查询结果可知，电容触摸对应的设备节点为/dev/input/event2, 测试步骤如下：

```
# export TSLIB_TSDEVICE=/dev/input/event2
# ts_calibrate
xres = 800, yres = 480
Took 3 samples...
Top left : X =   54 Y =   55
Took 3 samples...
Top right : X =  740 Y =   56
Took 3 samples...
Bot right : X =  737 Y =  419
Took 4 samples...
Bot left : X =   44 Y =  425
Took 4 samples...
Center : X =  395 Y =  243
-4.342529 1.015266 0.018063
-9.879883 0.003775 1.036696
Calibration constants: -284592 66536 1183 -647488 247 67940 65536
```

MYIR AM437X系列其它板型Touch Screen测试情况类似。

