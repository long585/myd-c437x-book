###4.3 Touch Screen Test

This example demonstrates  how to test touch screen by ts_calibrate application built with Buildroot.  

**Hardware preparation:**  
  * One MYD-C437X-PRU development board  
  * One USB to TTL converter used to connect J25 of MYD-C437X-PRU development board and host PC, set the baudrate of serial port on  host PC to 115200-8-n-1  
  * One MY-TFT070CV2 module connects to J21 of the MYD-C437X-PRU development board    
  * Or one MY-TFT070RV2 module connects to J21 of the MYD-C437X-PRU development board 

**Software Preparation:**  
  * Linux Kernel 4.1.18   
  * TS_CALIBRATE application    
 
**Test Steps:**  
  * Connect MY-TFT070CV2 module to J21 of the MYD-C437X-PRU development board, power on the board and view the device node in /dev/input directory    

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

The result above shows the resistive touch screen is corresponding to `/dev/input/event2`;
The capacitive touch screen is corresponding to `/dev/input/event3, so test capactive touch screen as below:  

```
# export TSLIB_TSDEVICE=/dev/input/event3
# ts_calibrate
xres = 800, yres = 480
Took 4 samples...
Top left : X =   54 Y =   46
Took 4 samples...
Top right : X =  745 Y =   58
Took 4 samples...
Bot right : X =  745 Y =  421
Took 3 samples...
Bot left : X =   68 Y =  429
Took 3 samples...
Center : X =  394 Y =  245
-5.867981 1.023202 -0.019352
-2.867676 -0.003020 1.017846
Calibration constants: -384564 67056 -1268 -187936 -197 66705 65536
```    

  * Power off the MYD-C437X-PRU development board，connect MY-TFT070RV2 module to J21 of the MYD-C437X-PRU development board, power on the board and view the device node in /dev/input directory    

```
#  ls /dev/input/
by-path  event0   event1   event2   mice     mouse0   mouse1

# cat /sys/class/input/event0/device/name
volume_keys@0

# cat /sys/class/input/event1/device/name
tps65218_pwrbutton

# cat /sys/class/input/event2/device/name
ti-tsc

```  

The result above shows the resistive touch screen is corresponding to `/dev/input/event2`, so test resistive touch screen as below:  

```
# export TSLIB_TSDEVICE=/dev/input/event2
# ts_calibrate
xres = 800, yres = 480
Took 3 samples...
Top left : X =   54 Y =   55
Took 3 samples...
Top right : X =  740 Y =   56
Took 3 samples...
Bot right : X =  737 Y =  419
Took 4 samples...
Bot left : X =   44 Y =  425
Took 4 samples...
Center : X =  395 Y =  243
-4.342529 1.015266 0.018063
-9.879883 0.003775 1.036696
Calibration constants: -284592 66536 1183 -647488 247 67940 65536
```    
