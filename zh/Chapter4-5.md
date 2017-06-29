###4.5 RS232测试
 
本例程演示如何使用Linux API配置开发板上的RS232并使其发送和接收数据，详情请参考源码。  

**测试硬件环境：**  
  * MYD-C437X-PRU 开发板两块  
  * 数据线连接两块板的J12，GND<->GND，TXD<->RXD，RXD<->TXD, CTS<->RTS， RTS<->CTS。  
  * USB转TTL调试串口线两根，分别连接两块板的J25和PC的USB host接口, PC端波特率设置115200-8-n-1

**测试软件环境：**  
  * Linux Kernel 4.1.18   
  * tty_test 应用程序  

**测试过程：**  
  * 将目录`<WORKDIR>/Examples/tty`中的可执行程序tty_test拷贝至开发板/usr/bin/目录下，执行`tty_test -h`, 查看测试程序帮助信息，如下： 

````
# tty_test -h
Usage: tty_test [options]
Version 1.0
Options:
-d | --device name   tty device name, default: /dev/tty0
-m | --mode  mode   operate mode. 0: RS232, 1: RS485  default mode: 0 
-f | --flow  	 			 flow control 
-b | --baudrate baudrate  set baudrate, default baudrate: 115200 
-l | --loop   	  	 operate circularly 
-w | --write  frame	 frame string. such as: 0123456789 
-h | --help          Print this message 
```  
  * MYD-C437X-PRU开发板的J12为一个带硬件流控的RS232串口，对应的设备节点/dev/ttyO3, 两块开发板一个作为发送端另一个作为接收端，先在一块开发板执行以下命令发送数据：  

```
# tty_test -d /dev/ttyO3 -b 9600 -m 0  -w 0123456789 -f -l  
 SEND:0123456789
 SEND:0123456789
 SEND:0123456789
```  
  * 再在另一开发板执行以下命令接收数据：  

```
# tty_test -d /dev/ttyO3 -b 9600 -m 0 -f -l
 RECV:0123456789, total:10
 RECV:0123456789, total:10
 RECV:0123456789, total:10

```
  * 两块板互换一下角色，结果一样，也可以通过短接某一块开发板的RTS和CTS，TXD和RXD, 进行硬件回环测试，具体情况不再赘述。           
                                 
MYIR AM437X系列其它板型RS232测试情况类似。  
