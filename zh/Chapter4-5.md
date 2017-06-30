### 4.5 RS232测试

本例程演示如何使用Linux API配置开发板上的RS232并使其发送和接收数据，详情请参考源码。

**测试硬件环境：**

* MYD-C437X-EVM 开发板

* USB转TTL调试串口线两根，一根连接开发板的J16和PC的USB host接口, PC端波特率设置115200-8-n-1；

  另一根连开发板J17和PC的USBhost接口，PC端波特率设置115200-8-n-1，此串口为测试串口

**测试软件环境：**

* Linux Kernel 4.1.18   
* tty\_test 应用程序  

**测试过程：**

* 将目录`<WORKDIR>/Examples/rootfs/usr/bin/`中的可执行程序tty\_test拷贝至开发板/usr/bin/目录下，执行`tty_test -h`, 查看测试程序帮助信息，如下： 

```\`
# tty_test -h
Usage: tty_test [options]
Version 1.0
Options:
-d | --device name   tty device name, default: /dev/tty0
-m | --mode  mode   operate mode. 0: RS232, 1: RS485  default mode: 0 
-f | --flow                    flow control 
-b | --baudrate baudrate  set baudrate, default baudrate: 115200 
-l | --loop              operate circularly 
-w | --write  frame     frame string. such as: 0123456789 
-h | --help          Print this message
```

* MYD-C437X-EVM开发板的J17为一个带硬件流控的RS232串口，对应的设备节点/dev/ttyO3, 先在PC通过putty打开J16对应的串口终端，执行以下命令发送数据：  

```
# tty_test -d /dev/ttyO3 -b 9600 -m 0  -w 0123456789 -f -l  
 SEND:0123456789
 SEND:0123456789
 SEND:0123456789
```

* 再在PC通过putty打开J17对应的串口终端，即可看到接收数据：  

`012345678901234567890123456789`

* 两个串口终端端互换角色，J16作为接收，J17作为数据发送。

 先在J16的串口终端执行以下命令接收数据：

\#`tty_test -d /dev/ttyO3 -b 9600 -m 0 -f -l`





MYIR AM437X系列其它板型RS232测试情况类似。

