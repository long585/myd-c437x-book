### 4.7 CAN Bus 测试

本例程演示使用Linux API让开发板上的CAN实现点对点发送和接收数据，详情请参考源码。

**测试硬件环境：**

* MYD-C437X-EVM开发板两块  
* 数据线连接两块板的J15，CANH0&lt;-&gt;CANH0，CANL0&lt;-&gt;CANL0，GND&lt;-&gt;GND  
* USB转TTL调试串口线两根，分别连接两块板的J25和PC的USB host接口, PC端波特率设置115200-8-n-1。

**测试软件环境：**

* Linux Kernel 4.1.18   
* can\_test 应用程序  
* Buildroot自带can\_utils应用程序包
* ip link应用程序

**测试过程：**

* 将目录`<WORKDIR>/Examples/can`中的可执行程序can\_test拷贝至开发板/usr/bin/, 分别将两块开发板上的can0通信波特率都设置位50Kbps，并使能can0设备, 如下所示:  

```
# can_test --help
Usage: can_test [options]

Version 1.0
Options:
-d | --device name   can device name: can0
-b | --baudrate baudrate   set baudrate, default baudrate:50000
-l | --loop              operate circularly, default not operate circularly!
-w | --write  frame     frame string with format ID#MESSAGE. such as: 123#112233445566
-h | --help                Print this message
```

```
# ip link set can0 down
# ip link set can0 type can bitrate 50000 triple-sampling on
# ip link set can0 up
```

* 上述设置波特率的命令在can\_test程序初始化时已经执行，不需再次手动执行。两块开发板一个作为发送端另一个作为接收端，先在一块开发板执行以下命令发送数据：

```
# chmod 777 /usr/bin/can_test
# can_test -d can0 -w 123#112233445566
[ 6862.997962] c_can_platform 481cc000.can can0: setting BTR=1c1d BRPE=0000
====== write  frame: ======
 frame_id = 0x123
 frame_len = 6
 frame_data = 0x11 0x22 0x33 0x44 0x55 0x66
===========================
```

* 再在另一开发板执行以下命令接收数据：

```
# chmod 777 /usr/bin/can_test
# can_test -d can0 -l
[ 6484.014811] c_can_platform 481cc000.can can0: setting BTR=1c1d BRPE=0000
  can0  0x123  [6]  0x11 0x22 0x33 0x44 0x55 0x66
```

* 如果使用-l参数，则会循环执行读写操作，两块板互换一下角色，结果一样，不再赘述。

注意, 如果程序运行过程中出现下述错误，可以修改tx\_queue\_len， 如下：

```
# can_test -d can0 -w 123#112233445566
  can raw socket write: No buffer space available

# echo 1000 > /sys/class/net/can0/tx_queue_len
```

* 用同样的方法测试两块板子的can1接口，结果一样，不再赘述。

MYIR AM437X系列其它板型CAN BUS测试情况类似。

