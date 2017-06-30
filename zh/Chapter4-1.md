### 4.1 GPIO测试

本例程演示如何使用Linux API配置开发板上的GPIO，详情请参考源码。

**测试硬件环境：**

* MYD-C437X-EVM开发板一块  
* USB转TTL调试串口一根，连接MYD-C437X-EVM J16和PC, PC端波特率设置115200-8-n-1

**测试软件环境：**

* Linux Kernel 4.1.18   
* gpio\_test 应用程序  

**测试过程：**

* 将目录`<WORKDIR>/Examples/gpio`中的可执行程序gpio\_test拷贝至开发板/usr/bin/目录下, 执行该程序进行测试如下：

```\`
# gpio_test -h
Usage: gpio_test [options]

Version 1.0
Options:
-n | -- number gpio      gpio number.
-g | -- get            get gpio level.
-s | -- set  level     set gpio level. 0: low; 1: high
-h | --help                  Print this message
```

* GPIO3\_7作为EEPROM的写保护控制脚，输出高则使能写保护，输出低则禁用写保护，下面通过gpio\_test进行测试：  

```
# gpio_test -n 103 -s 1
==gpio3_7  direction is out
==gpio3_7  level is high
Set gpio3_7 level high success!
# gpio_test -n 103 -s 0
==gpio3_7  direction is out
==gpio3_7  level is low
Set gpio3_7 level low success!
```

用户也可以通过shell脚本，使用echo和cat命令来实现对/sys/class/gpio下文件的访问。例如:/usr/bin/set\_eeprom.sh

```
#!/bin/bash

EEPROM_WP_GPIO_PIN=103

wait_gpio() {
        sleep 1
}

wp_init() {
        if [ ! -d "/sys/class/gpio/gpio$EEPROM_WP_GPIO_PIN" ]; then
                echo "$EEPROM_WP_GPIO_PIN" > /sys/class/gpio/export; wait_gpio
        fi

        echo "out" > /sys/class/gpio/gpio$EEPROM_WP_GPIO_PIN/direction; wait_gpio
}

up() {
        echo "0" > /sys/class/gpio/gpio$EEPROM_WP_GPIO_PIN/value; wait_gpio
}

down() {
        echo "1" > /sys/class/gpio/gpio$EEPROM_WP_GPIO_PIN/value; wait_gpio
}

if [ "$1" = "1" ]; then
        wp_init
        up
fi

if [ "$1" = "0" ]; then
        wp_init
        down
        echo "$EEPROM_WP_GPIO_PIN" > /sys/class/gpio/unexport; wait_gpio
fi
```

* 通过执行set\_eeprom.sh脚本也可以实现对GPIO的控制。  
  ```
  # chmod 777 /usr/bin/set_eeprom.sh
  # set_eeprom.sh 1                            -- 使能写保护
  # set_eeprom.sh 0                            -- 禁止写保护
  ```

MYIR AM437X系列其它板型GPIO测试情况类似。

