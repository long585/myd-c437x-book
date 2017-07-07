### 4.10 EEPROM测试

本例程演示使用Linux User API对开发板上EEPROM进行读写测试，详情请参考源码。

**测试硬件环境：**

* MYD-C437X-EVM 开发板一块  
* 请确认板上带有EEPROM芯片，MYD-C437X-PRU核心板带有EEPROM芯片24256E  
* USB转TTL调试串口一根，连接MYD-C437X-PRU J25和PC, PC端波特率设置115200-8-n-1

**测试软件环境：**

* Linux Kernel 4.1.18   
* eeprom\_test应用程序  

**测试过程:**

* 将目录`<WORKDIR>/Examples/eeprom`中的可执行程序eeprom\_test拷贝至开发板/usr/bin/目录下, 执行改程序进行测试如下：  

```
# chmod 777 /usr/bin/eeprom_test
# eeprom_test -h 
Usage: eeprom_test [options]

Version 1.0
Options:
-d | --device name   i2c device name: /dev/i2c-0
-a | --address addr     eeprom i2c address, default 0x50
-s | --start addr       start offset to read/write
-r | --read  count    read byte count
-w | --write frame      write frame string. such as: 0123456789
-h | --help                Print this message
```

* 测试读写之前，需要先控制GPIO3\_7，禁用EEPROM写保护。eeprom\_test执行时已经禁用了EEPROM写保护，用户不需要再手动执行

```
# echo 103 > /sys/class/gpio/export
# echo "out" > /sys/class/gpio/gpio103/direction
# echo 0 > /sys/class/gpio/gpio103/value
```

* EEPROM读写测试  

```
# eeprom_test -d /dev/i2c-0 -a 0x50 -w "hello world!"
WRITE:hello world!
WRITE SUCCESS!


# eeprom_test -d /dev/i2c-0 -a 0x50 -r 12
READ:hello world!
TOTAL 12 BYTES.
```

MYIR AM437X系列其它板型的EEPROM测试情况类似。

