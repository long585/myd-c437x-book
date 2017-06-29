###4.10 EEPROM Test  

This example demonstrates how to use Linux API to read and write the EEPROM of MYD-C437X-PRU development board, please refer the source code for detail.  


**Hardware Preparation:**  
  * One MYD-C437X-PRU development board     
  * One USB to TTL converter used to connect J25 of MYD-C437X-PRU development board and host PC, set the baudrate of serial port on  host PC to 115200-8-n-1
  * Make sure a EEPROM IC 24256E is soldered on the MYD-C437X-PRU development board  

**Software Preparation:**    
  * Linux Kernel 4.1.18   
  * eeprom_test application  
 
**Test Steps:**    
  * Copy cross compiled `<WORKDIR>/Examples/eeprom/eepromc_test` to `/usr/bin` directory of the MYD-C437X-PRU development board, run `eepromc_test` application as below:

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
   
  * Before testing write on EEPROM, write-protect should be disabled by outputing low level on GPIO3_7. During running of eeprom_test, it set write-protect to be disabled automatically, users do not need to handle manually. 
  
```
# echo 103 > /sys/class/gpio/export
# echo "out" > /sys/class/gpio/gpio103/direction
# echo 0 > /sys/class/gpio/gpio103/value
```    
  * Read and write EEPROM as below:    

```
# eeprom_test -d /dev/i2c-0 -a 0x50 -w "hello world!"
WRITE:hello world!
WRITE SUCCESS!


# eeprom_test -d /dev/i2c-0 -a 0x50 -r 12
READ:hello world!
TOTAL 12 BYTES.

```