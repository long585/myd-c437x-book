### 4.8 KeyPad测试

本例程演示使用Linux User API读取按键信息，详情请参考源码。

**测试硬件环境：**

* MYD-C437X-EVM 开发板一块  
* USB转TTL调试串口一根，连接MYD-C437X-EVM J16和PC, PC端波特率设置115200-8-n-1

**测试软件环境：**

* Linux Kernel 4.1.18   
* hexdump或keypad\_test应用程序  

**测试过程:**

* 将目录`<WORKDIR>/Examples/rootfs/usr/bin/`中的可执行程序keypad\_test拷贝至开发板/usr/bin/目录下, 执行该程序进行测试如下：

```
$ chmod 777 /usr/bin/keypad_test
$ keypad_test -h 
Usage: keypad_test [options]

Version 1.0
Options:
-d | --device name   keypad device name, default: /dev/input/event0
-h | --help          Print this message
```

* 查看按键设备对应的设备节点，可以发现S3,S4两个按键对应/dev/input/event0， 而S1对应/dev/input/event1

```
# ls /dev/input/
by-path  event0   event1   event2   mice     mouse0

# cat /sys/class/input/event0/device/name 
gpio_key_pads@0

# cat /sys/class/input/event1/device/name 
ti-tsc

# cat /sys/class/input/event2/device/name 
tps65218_pwrbutton
```

* 测试S3和S4  

```
# keypad_test  -d /dev/input/event0
Event: Code = 102, Type = 1, Value=1         --按下SW3
Event: Code = 0, Type = 0, Value=0
Event: Code = 102, Type = 1, Value=0         --释放SW3
Event: Code = 0, Type = 0, Value=0
Event: Code = 158, Type = 1, Value=1         --按下SW4
Event: Code = 0, Type = 0, Value=0
Event: Code = 158, Type = 1, Value=0         --释放SW4
Event: Code = 0, Type = 0, Value=0
```

* 测试S1  

```
# kepad_test  -d /dev/input/event2
Event: Code = 116, Type = 1, Value=1                -- 按下SW1
Event: Code = 0, Type = 0, Value=0
Event: Code = 116, Type = 1, Value=0                -- 释放SW1
Event: Code = 0, Type = 0, Value=0
```

以上测试结果中返回的Event Code即为按键键值，和内核设备树中定义的键值必须一致。  
用户也可以通过Buildroot文件系统中自带的hexdump程序进行按键的测试，然后分别按SW3,SW4这两个按键。 其中输出信息中第7列中的0073和0072即为设备树中定义的按键键值115和114，其它信息含义请参阅hexdump说明。

```
# hexdump /dev/input/event0
0000000 925c 5956 7aff 000c 0001 0066 0001 0000
0000010 925c 5956 7aff 000c 0000 0000 0000 0000
0000020 925c 5956 de71 000e 0001 0066 0000 0000
0000030 925c 5956 de71 000e 0000 0000 0000 0000

0000040 925e 5956 ac85 000a 0001 009e 0001 0000
0000050 925e 5956 ac85 000a 0000 0000 0000 0000
0000060 925e 5956 f915 000c 0001 009e 0000 0000
0000070 925e 5956 f915 000c 0000 0000 0000 0000
```

测试S1按键方法类似，如下：

```
# hexdump /dev/input/event2
0000000 937b 5956 879e 000b 0001 0074 0001 0000
0000010 937b 5956 879e 000b 0000 0000 0000 0000
0000020 937b 5956 89c0 000e 0001 0074 0000 0000
0000030 937b 5956 89c0 000e 0000 0000 0000 0000
```

MYIR AM437X系列其它板型的KeyPad测试情况类似。

