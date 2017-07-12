###4.4 RTC Test  

This example demonstrates how to use Linux API to read and write real time on RTC，please refer to the source code for detail. 
Users can also test the RTC with `date` and `hwclock` command built with Buildroot.  

**Hardware Preparation:**  
  * One MYD-C437X-PRU development board     
  * One USB to TTL converter used to connect J25 of MYD-C437X-PRU development board and host PC, set the baudrate of serial port on  host PC to 115200-8-n-1
  * One CR2032 button cell  
	
**Software Preparation: **  
  * Linux Kernel 4.1.18   
  * date, hwclock command 
  * rtc_test application    
 
**Test Steps: **  
  * Copy cross compiled `<WORKDIR>/Examples/rtc/rtc_test` to `/usr/bin` directory of the MYD-C437X-PRU development board, run `rtc_test` application as below:

```
# chmod 777 /usr/bin/rtc_test
# rtc_test -h 
Usage: rtc_test [options]

Version 1.0
Options:
-d | --device name   rtc device name, default: /dev/rtc0
-w | --write  time	 time string with format MMDDhhmm[CCYY][.ss]. such as: 111817582016.18 
-h | --help          Print this message

# rtc_test -d /dev/rtc1 -w 111817582016.18

  date/time is updated to:  18-11-2016, 17:58:18.
```  
  
  * Power off the MYD-C437X-PRU development board, wait for a while, power on again and read the rtc time by `rtc_test` as below:

```
# rtc_test -d /dev/rtc1

  Current RTC date/time is 18-11-2016, 17:59:12.
```
  * Users can also use date and hwclock command to test RTC as below:  

```
# date  081518002016.30							-- Set system time to 2016/8/15 18:00:30
Mon Aug 15 18:00:30 UTC 2016
# date
Mon Aug 15 18:00:38 UTC 2016
# hwclock -w /dev/rtc1							-- Write system time to rtc1
```  
  * Power off the MYD-C437X-PRU development board, wait for a while, power on again and read the rtc time by hwclock as below:  

```
# hwclock -r /dev/rtc1							-- 
Mon Aug 15 18:11:08 2016  0.000000 seconds

```  

