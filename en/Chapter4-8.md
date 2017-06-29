###4.8 KeyPad Test  

This example demonstrates how to read the keypad event information by Linux user APIs, please refer to the source code for detail.

**Hardware Preparation:**  
  * One MYD-C437X-PRU development board     
  * One USB to TTL converter used to connect J25 of MYD-C437X-PRU development board and host PC, set the baudrate of serial port on  host PC to 115200-8-n-1
	
**Software Preparation: **  
  * Linux Kernel 4.1.18   
  * hexdump command 
  * keypad_test application    
 
**Test Steps: **  
  * Copy cross compiled `<WORKDIR>/Examples/keypad/keypad_test` to `/usr/bin` directory of the MYD-C437X-PRU development board, run `keypad_test` application as below:

``` 
$ chmod 777 /usr/bin/keypad_test
$ keypad_test -h 
Usage: keypad_test [options]

Version 1.0
Options:
-d | --device name   keypad device name, default: /dev/input/event0
-h | --help          Print this message 
```    
  * View the device nodes of keypad, the following information shows `S3` and `S4` keypads
  are corresponding to `/dev/input/event0`，`S1` keypad is corresponding to `/dev/input/event1`.  

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
  * Test `S3` and `S4` keypads as below:  
  
```
# keypad_test  -d /dev/input/event0
Event: Code = 115, Type = 1, Value=1				-- press S3
Event: Code = 0, Type = 0, Value=0
Event: Code = 115, Type = 1, Value=2
Event: Code = 0, Type = 0, Value=1
Event: Code = 115, Type = 1, Value=0				-- release S3			
Event: Code = 0, Type = 0, Value=0
Event: Code = 114, Type = 1, Value=1				-- press S4
Event: Code = 0, Type = 0, Value=0
Event: Code = 114, Type = 1, Value=0				-- release S4
Event: Code = 0, Type = 0, Value=0

```

  * Test `S1` keypad as below:  
  
```
# kepad_test  -d /dev/input/event1
Event: Code = 116, Type = 1, Value=1				-- press S1
Event: Code = 0, Type = 0, Value=0
Event: Code = 116, Type = 1, Value=0				-- release S1
Event: Code = 0, Type = 0, Value=0

```   
Each keypad has a event code as show above. the code should be consistent with the value in device tree source. 
Users can use `hexdump` command to test keypads as below:  

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

Test `S1` keypad with hexdump as below:  

```  
# hexdump /dev/input/event1
0000000 3a0a 57b2 93a2 0009 0001 0074 0001 0000
0000010 3a0a 57b2 93a2 0009 0000 0000 0000 0000
0000020 3a0a 57b2 348b 000d 0001 0074 0000 0000
0000030 3a0a 57b2 348b 000d 0000 0000 0000 0000

```



