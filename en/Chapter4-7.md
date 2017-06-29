###4.7 CAN Bus Test 
 
This example demonstrates how to use Linux APIs to send and receive data from CAN bus, please refer to the source code for detail. 

**Hardware Preparation: **  
  * Two MYD-C437X-PRU development boards  
  * Two data cables to connect J10 of the two boards: CANH<->CANH，CANL<->CANL，GND<->GND
  * Two USB to TTL converters, each connects J25 of MYD-C437X-PRU development board and a USB host of PC respectively, set the baudrate of serial port on  host PC to 115200-8-n-1

**Software Preparation: **  
  * Linux Kernel 4.1.18   
  * can_test application 
  * ip link applicatoin 
  * can_utils tools built with Buildroot  
  
**Test Steps:**  
  * Copy cross compiled `<WORKDIR>/Examples/can/can_test` to `/usr/bin` directory of the MYD-C437X-PRU development board, run `can_test` application as below:

```
# can_test --help
Usage: can_test [options]

Version 1.0
Options:
-d | --device name   can device name: can0
-b | --baudrate baudrate   set baudrate, default baudrate:50000
-l | --loop              operate circularly, default not operate circularly!
-w | --write  frame     frame string with format ID#MESSAGE. such as: 123#112233445566
-h | --help                Print this message
```  
  * Set the baudrate for can interface as below:  
   
```
# ip link set can0 down
# ip link set can0 type can bitrate 50000 triple-sampling on
# ip link set can0 up
```

  * The previous processes are no need to be executed manually. During running `can_test`, it will be set automatically.  
One board is used as sender, the other is used as receiver, they communicate with can_test application as below: 

```
# chmod 777 /usr/bin/can_test
# can_test -d can0 -w 123#112233445566
[ 6862.997962] c_can_platform 481cc000.can can0: setting BTR=1c1d BRPE=0000
====== write  frame: ======
 frame_id = 0x123
 frame_len = 6
 frame_data = 0x11 0x22 0x33 0x44 0x55 0x66
===========================

```

  * Execute the following command at other board to receive data as below:  

```
# chmod 777 /usr/bin/can_test
# can_test -d can0 -l
[ 6484.014811] c_can_platform 481cc000.can can0: setting BTR=1c1d BRPE=0000
  can0  0x123  [6]  0x11 0x22 0x33 0x44 0x55 0x66
```  

> Note: -l option is used for operating circularly.
> Note: In case of the following error, please modify the value of "tx_queue_len" as below:  
 
```
# can_test -d can0 -w 123#112233445566
  can raw socket write: No buffer space available
 
# echo 1000 > /sys/class/net/can0/tx_queue_len
```  
  * Exchange roles of the two boards, the result is the same.  

