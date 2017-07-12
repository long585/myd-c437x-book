###4.12 USB DEVICE Test  

This example demonstrates how to use USB device interface and verify the driver of USB client.  
The MYD-C437X-PRU development board works as a TF card reader, it is connected to the USB host of PC with
a USB mini B to USB A cable.  

**Hardware Preparation：**  
  * One MYD-C437X-PRU development board   
  * One TF card    
  * One USB mini B to USB A cable  
  * One USB to TTL converter used to connect J25 of MYD-C437X-PRU development board and host PC, set the baudrate of serial port on  host PC to 115200-8-n-1

**Software Preparation:**    
  * Linux Kernel 4.1.18   
  * lsmod, rmmod, modprobe command    

**Test Steps:**     
 
  * After the MYD-C437X-PRU development is booted, connect it to the USB host interface of PC with a USB mini B to USB A cable, plugin a TF card to J19.
Load the mass storage gadget driver as below:  

```
# modprobe g_mass_storage stall=0 file=/dev/mmcblk0p1 removable=1
[15151.225218] Mass Storage Function, version: 2009/09/11
[15151.231350] LUN: removable file: (no medium)
[15151.236837] LUN: removable file: /dev/mmcblk0p1
[15151.242148] Number of LUNs=1
[15151.245236] Number of LUNs=1
[15151.248511] g_mass_storage gadget: Mass Storage Gadget, version: 2009/09/11
[15151.256739] g_mass_storage gadget: userspace failed to provide iSerialNumber
[15151.264318] g_mass_storage gadget: g_mass_storage ready
[15151.270155] dwc3 48390000.usb: otg: gadget gadget registered
# [15152.172523] g_mass_storage gadget: high-speed config #1: Linux File-Backed Storage
```  
  * After g_mass_storage driver is loaded, a removable disk will be detected on PC. The content of this removable disk is just the same with the TF card in J19. 

> Note: Beyond that, users can load different gadget modules to achieve different functions. such as g_ether is used to make a RNDIS network interface,
g_serial is used to make a serial port.  


