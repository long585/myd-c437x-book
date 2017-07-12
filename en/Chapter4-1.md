###4.1 GPIO Test  
 
This example demonstrates how to use Linux API to control GPIO of MYD-C437X-PRU development board, please refer to the source code for detail.  

**Hardware Preparation：**    
  * One MYD-C437X-PRU development board     
  * One USB to TTL converter used to connect J25 of MYD-C437X-PRU development board and host PC, set the baudrate of serial port on  host PC to 115200-8-n-1

**Software Preparation:**    
  * Linux Kernel 4.1.18   
  * gpio_test application   

**Test Steps:**   
  * Copy cross compiled `<WORKDIR>/Examples/gpio/gpio_test` to `/usr/bin` directory of the MYD-C437X-PRU development board, run `gpio_test` application as below:  

````
# gpio_test -h
Usage: gpio_test [options]

Version 1.0
Options:
-n | -- number gpio    gpio number.
-g | -- get            get gpio level.
-s | -- set  level     set gpio level. 0: low; 1: high
-h | -- help            Print this message

```  
  * GPIO3_7 is used as the write-protect pin, write-protect is enabled when GPIO3_7 outputs high, disabled when GPIO3_7 outputs low.  
Test with gpio_test application as below:     

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

Users can also control GPIO by executing `echo` and `cat` commands to write and read `/sys/class/gpio` in a shell script, 
For example, there is a bash script named as `/usr/bin/set_eeprom.sh` used for controlling GPIO3_7, the content of file is shown below:    
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
  * Run `/usr/bin/set_eeprom.sh` to control the write-protect pin of EEPROM：  

```
# chmod 777 /usr/bin/set_eeprom.sh
# set_eeprom.sh 1							-- Enable write-protect
# set_eeprom.sh 0							-- Disable write-protect
```

