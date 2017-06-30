### 4.4 RTC 测试

本例程演示如何使用Linux API对开发板上的RTC进行时间设置与读取，详情请参考源码。  
用户也可以使用Buildroot文件系统自带的date和hwclock配合使用对RTC进行测试。

**测试硬件环境：**

* MYD-C437X-EVM 开发板一块  
* RTC所用CR2032 3V纽扣电池一块  
* USB转TTL调试串口一根，连接MYD-C437X-EVM J16和PC, PC端波特率设置115200-8-n-1

**测试软件环境：**

* Linux Kernel 4.1.18   
* date, hwclock或rtc\_test应用程序  

**测试过程:**

* 将目录`<WORKDIR>/Examples/rootfs/usr/bin/`中的可执行程序rtc\_test拷贝至开发板/usr/bin/，执行以下命令将rtc当前时间设置为2016/11/18  17:58:18 并读取当前时间：  

```
# chmod 777 /usr/bin/rtc_test
# rtc_test -h 
Usage: rtc_test [options]

Version 1.0
Options:
-d | --device name   rtc device name, default: /dev/rtc0
-w | --write  time     time string with format MMDDhhmm[CCYY][.ss]. such as: 111817582016.18 
-h | --help          Print this message

# rtc_test -d /dev/rtc1 -w 111817582016.18

  date/time is updated to:  18-11-2016, 17:58:18.
```

* 将开发板断电，经过一段时间之后，重新上电开机，再次通过rtc\_test读出rtc的时间，如下：  

```
# rtc_test -d /dev/rtc

  Current RTC date/time is 18-11-2016, 17:59:12.
```

* 用户也可以使用date, hwclock配合，进行RTC的测试  

```
# date  081518002016.30                            -- 设置系统时间为2016年8月15日18:00:30
Mon Aug 15 18:00:30 UTC 2016
# date
Mon Aug 15 18:00:38 UTC 2016
# hwclock -w /dev/rtc1                            -- 将系统时间写入到rtc1
```

* 将开发板断电，经过一段时间之后，重新上电开机，再次通过hwclock读出rtc的时间，如下：  

```
# hwclock -r /dev/rtc                            -- 
Mon Aug 15 18:11:08 2016  0.000000 seconds
```

MYIR AM437X系列其它板型的RTC测试情况类似。

