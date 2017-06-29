###4.13 PRU Test

This example demonstrates how to communicate between ARM and PRU with `rpmsg` and `remoteproc` feature of Linux kernel. 
The PRU test application writes a number from 0 to 7 to PRU, and the PRU set the three LEDs on MYD-C437X-PRU development board according to this number.    

**Hardware Preparation：**  
  * One MYD-C437X-PRU development board     
  * One USB to TTL converter used to connect J25 of MYD-C437X-PRU development board and host PC, set the baudrate of serial port on  host PC to 115200-8-n-1

**Software Preparation：**  
  * Linux Kernel 4.1.18   
  * pru_led_test application  

**Test Steps：**  
   
  * Apply a patch to enable PRUSS0 and disable PRUSS1, because the three LEDs are controlled by PRUSS0 instead of PRUSS1  

After appling the patch located at *04-Linux_sources/patches/0001-Enable-PRUSS0-instead-of-PRUSS1-on-the-MYD_C437x_PRU*, customers should rebuild 
the Kernel and device tree source to generate `zImage` and device tree binary files again. 
  
```  
// set pinmux for AD21, AE22, AD22 to be controlled by PRUSS0
@@ -168,9 +148,10 @@
	leds_pins: leds_pins {
		pinctrl-single,pins = <
			0x244 ( PIN_OUTPUT_PULLUP | MUX_MODE7 ) /* (F23) gpio5_11.gpio5[11] */
-			0x1f0 ( PIN_OUTPUT_PULLUP | MUX_MODE7 ) /* (AD21) cam1_data2.gpio4[16] */
-			0x1f4 ( PIN_OUTPUT_PULLUP | MUX_MODE7 ) /* (AE22) cam1_data3.gpio4[17] */
-			0x1f8 ( PIN_OUTPUT_PULLUP | MUX_MODE7 ) /* (AD22) cam1_data4.gpio4[18] */
+			0x1f0 ( PIN_OUTPUT_PULLUP | MUX_MODE4 ) /* (AD21) cam1_data2.pr0_pru1_gpo[10] */
+			0x1f4 ( PIN_OUTPUT_PULLUP | MUX_MODE4 ) /* (AE22) cam1_data3.pr0_pru1_gpo[11] */
+			0x1f8 ( PIN_OUTPUT_PULLUP | MUX_MODE4 ) /* (AD22) cam1_data4.pr0_pru1_gpo[12] */
+			

```   
  * Compile the firmwaer running on PRU  

The source code of PRU firmware is located at *04-Linux_Source\Examples\pru_led\PRU_RPMsg_LED0_1*, it should be compiled by ti_cgt_pru toolchain. 
After compiling is complete, the firmware for PRU `PRU_RPMsg_LED0_1.out` is generated, please rename it to `am437x-pru0_1-fw` and copy to /lib/firmware/ of the embedded Linux 
root file system. If ti_cgt_pru is not installed, please install it and compile `pru_led` test application as below:  

```
$ cd <WORKDIR>/ToolChain/
$ ./ti_cgt_pru_2.1.3_linux_installer_x86.bin 
$ export PRU_CGT=<WORKDIR>/ToolChain/ti-cgt-pru_2.1.3/
$ cd <WORKDIR>/04-Linux_Source\Examples\pru_led
$ make

************************************************************
Building project: PRU_RPMsg_LED0_1

Finished building project: PRU_RPMsg_LED0_1
************************************************************

```     
  * Restart the embedded Linux system rebuilt with PRUSS0 enabled  
   
A device node `/dev/rpmsg_pru31` generated after booting shows PRU working well, then users can move on to the following steps. 
Users can use `rmmod` or `modprobe` to unload or reload PRU remoteproc driver and firmware for debugging as below:  
  
```
# rmmod pru_rproc -f
[ 5702.650841] pru-rproc 54478000.pru1: pru_rproc_remove: removing rproc 54478000.pru1
[ 5702.660895] pruss-rproc 54440000.pruss: unconfigured system_events = 0x0800000000000000 
host_intr = 0x00000002
[ 5702.672138]  remoteproc2: stopped remote processor 54478000.pru1
[ 5702.678938]  remoteproc2: releasing 54478000.pru1
[ 5702.684928] pru-rproc 54474000.pru0: pru_rproc_remove: removing rproc 54474000.pru0
[ 5702.694806] pruss-rproc 54440000.pruss: unconfigured system_events = 0x1000000000000000 
host_intr = 0x00000001
[ 5702.706273]  remoteproc1: stopped remote processor 54474000.pru0
[ 5702.713233]  remoteproc1: releasing 54474000.pru0
# modprobe pru_rproc
[ 5707.305735]  remoteproc1: 54474000.pru0 is available
[ 5707.314208]  remoteproc1: Note: remoteproc is still under development and considered 
experimental.
[ 5707.324384]  remoteproc1: THE BINARY FORMAT IS NOT YET FINALIZED, and backward 
compatibility isn't yet guaranteed.
[ 5707.342644]  remoteproc1: powering up 54474000.pru0
[ 5707.347860]  remoteproc1: Booting fw image am437x-pru0_0-fw, size 84928
[ 5707.355459] pruss-rproc 54440000.pruss: configured system_events = 0x1000000000000000 
intr_channels = 0x00000001 host_intr = 0x00000001
[ 5707.368458]  remoteproc1: remote processor 54474000.pru0 is now up
[ 5707.375567] virtio_rpmsg_bus virtio0: rpmsg host is online
[ 5707.381484]  remoteproc1: registered virtio0 (type 7)
[ 5707.386895] pru-rproc 54474000.pru0: PRU rproc node /ocp/pruss@54440000/pru0@54474000 
probed successfully
[ 5707.397446] virtio_rpmsg_bus virtio0: creating channel rpmsg-pru addr 0x1e
[ 5707.405191]  remoteproc2: 54478000.pru1 is available
[ 5707.415107] rpmsg_pru rpmsg4: new rpmsg_pru device: /dev/rpmsg_pru30
[ 5707.421988]  remoteproc2: Note: remoteproc is still under development and considered 
experimental.
[ 5707.432231]  remoteproc2: THE BINARY FORMAT IS NOT YET FINALIZED, and backward
compatibility isn't yet guaranteed.
[ 5707.448155]  remoteproc2: powering up 54478000.pru1
[ 5707.453860]  remoteproc2: Booting fw image am437x-pru0_1-fw, size 84360
[ 5707.461062] pruss-rproc 54440000.pruss: configured system_events = 0x0800000000000000
 intr_channels = 0x00000002 host_intr = 0x00000002
[ 5707.474342]  remoteproc2: remote processor 54478000.pru1 is now up
[ 5707.481129] virtio_rpmsg_bus virtio1: rpmsg host is online
[ 5707.487010]  remoteproc2: registered virtio1 (type 7)
[ 5707.492891] pru-rproc 54478000.pru1: PRU rproc node /ocp/pruss@54440000/pru1@54438000 
probed successfully
[ 5707.503186] virtio_rpmsg_bus virtio1: creating channel rpmsg-pru addr 0x1f
# [ 5707.517016] rpmsg_pru rpmsg5: new rpmsg_pru device: /dev/rpmsg_pru31

```
  * Use pru_led_test application to control LEDs as below:    
    
```  
# pru_led_test -h
Usage: pru_led_test [options]

Version 1.0
Options:
-d | --device name   pru rpmsg char device name /dev/rpmsg_pru31
-l | --loop          operate circularly, default not operate circularly!
-n | --number num    number send to pru! must be 0~7.
-h | --help                Print this message


# pru_led_test -d /dev/rpmsg_pru31 -n 7
Opened /dev/rpmsg_pru31, sending 1 messages

1 - Sent to PRU: 7
1 - Received from PRU:7

Received 1 messages, closing /dev/rpmsg_pru31

```  
  * Observe the state of the three LEDs on the motherboard of MYD-C437X-PRU development board.  
  
They are turned on and off depending on the number along with `-n`. The variation regularity of the LEDs is relative to the binary value of the number.
For example, if customers set the number to 3(011b), then D18 will be turned off, D19 and D20 will be turned on.   