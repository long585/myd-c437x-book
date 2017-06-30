### 4.6 RS485测试

本例程演示如何使用Linux API配置开发板上的RS485并使其发送和接收数据，详情请参考源码。

**测试硬件环境：**

* MYD-C437X-EVM开发板两块  
* 数据线连接两块板的J10，485A&lt;-&gt;485A，485B&lt;-&gt;485B，GND&lt;-&gt;GND  
* USB转TTL调试串口线两根，分别连接两块板的J25和PC的USB host接口, PC端波特率设置115200-8-n-1

**测试软件环境：**

* Linux Kernel 4.1.18   
* tty\_test 应用程序  

**测试过程：**

* 将目录`<WORKDIR>/Examples/tty`中的可执行程序tty\_test拷贝至开发板/usr/bin/目录下，执行`tty_test -h`, 查看测试程序帮助信息，如下：    

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

* 两块开发板一个作为发送端另一个作为接收端，先在一块开发板执行以下命令发送数据：  

```
# tty_test -d /dev/ttyO5 -b 9600 -m 1  -w 0123456789 -l  
 SEND:0123456789
 SEND:0123456789
 SEND:0123456789
```

* 再在另一开发板执行以下命令接收数据：  

```
# tty_test -d /dev/ttyO5 -b 9600 -m 1  -l
 RECV:0123456789, total:10
 RECV:0123456789, total:10
 RECV:0123456789, total:10
```

* 两块板互换一下角色，结果一样，不再赘述。           

MYIR AM437X系列其它板型RS485测试情况类似。

