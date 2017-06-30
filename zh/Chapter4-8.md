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
#  ls /dev/input/
by-path  event0   event1   event2   event3   mice     mouse0   mouse1

# cat /sys/class/input/event0/device/name
volume_keys@0

# cat /sys/class/input/event1/device/name
tps65218_pwrbutton

# cat /sys/class/input/event2/device/name
ti-tsc

# cat /sys/class/input/event3/device/name
ft5x06_ts
```

* 测试S3和S4  

```
# keypad_test  -d /dev/input/event0
Event: Code = 115, Type = 1, Value=1                -- 按下S3
Event: Code = 0, Type = 0, Value=0
Event: Code = 115, Type = 1, Value=2
Event: Code = 0, Type = 0, Value=1
Event: Code = 115, Type = 1, Value=0                -- 释放S3            
Event: Code = 0, Type = 0, Value=0
Event: Code = 114, Type = 1, Value=1                -- 按下S4
Event: Code = 0, Type = 0, Value=0
Event: Code = 114, Type = 1, Value=0                -- 释放S4
Event: Code = 0, Type = 0, Value=0
```

* 测试S1  

```
# kepad_test  -d /dev/input/event1
Event: Code = 116, Type = 1, Value=1                -- 按下S1
Event: Code = 0, Type = 0, Value=0
Event: Code = 116, Type = 1, Value=0                -- 释放S1
Event: Code = 0, Type = 0, Value=0
```

以上测试结果中返回的Event Code即为按键键值，和内核设备树中定义的键值必须一致。  
用户也可以通过Buildroot文件系统中自带的hexdump程序进行按键的测试，然后分别按S3,S4这两个按键。 其中输出信息中第7列中的0073和0072即为设备树中定义的按键键值115和114，其它信息含义请参阅hexdump说明。

```
# hexdump /dev/input/event0
0000000 3912 57b2 cc9e 0007 0001 0073 0001 0000
0000010 3912 57b2 cc9e 0007 0000 0000 0000 0000
0000020 3912 57b2 056d 000b 0001 0073 0000 0000
0000030 3912 57b2 056d 000b 0000 0000 0000 0000

0000040 3915 57b2 1e47 0004 0001 0072 0001 0000
0000050 3915 57b2 1e47 0004 0000 0000 0000 0000
0000060 3915 57b2 6c14 0007 0001 0072 0000 0000
0000070 3915 57b2 6c14 0007 0000 0000 0000 0000
```

测试S1按键方法类似，如下：

```
# hexdump /dev/input/event1
0000000 3a0a 57b2 93a2 0009 0001 0074 0001 0000
0000010 3a0a 57b2 93a2 0009 0000 0000 0000 0000
0000020 3a0a 57b2 348b 000d 0001 0074 0000 0000
0000030 3a0a 57b2 348b 000d 0000 0000 0000 0000
```

MYIR AM437X系列其它板型的KeyPad测试情况类似。

