###4.6 RS485 Test
 
This example demonstrates how to use Linux APIs to send and receive data from RS485, please refer to the source code for detail. 

**Hardware Preparation: **  
  * Two MYD-C437X-PRU development boards  
  * Two data cables to connect J10 of the two boards: 485A<->485A，485B<->485B，GND<->GND  
  * Two USB to TTL converters, each connects J25 of MYD-C437X-PRU development board and a USB host of PC respectively, set the baudrate of serial port on  host PC to 115200-8-n-1

**Software Preparation: **  
  * Linux Kernel 4.1.18   
  * tty_test application 

**Test Steps:**  
  * Copy cross compiled `<WORKDIR>/Examples/tty/tty_test` to `/usr/bin` directory of the MYD-C437X-PRU development board, run `tty_test` application as below:

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
  * J10 interface on the MYD-C437X-PRU development board is a RS485 serial port. It is corresponding to /dev/ttyO5 
on a embedded Linux system. One board is used as sender, the other is used as receiver, they communicate with
tty_test application as below:  

```
# tty_test -d /dev/ttyO5 -b 9600 -m 1  -w 0123456789 -l  
 SEND:0123456789
 SEND:0123456789
 SEND:0123456789
```  
  * Execute the following command at other board to receive data as below:  

```
# tty_test -d /dev/ttyO5 -b 9600 -m 1  -l
 RECV:0123456789, total:10
 RECV:0123456789, total:10
 RECV:0123456789, total:10

```
  * Exchange roles of the two boards, the result is the same.  
