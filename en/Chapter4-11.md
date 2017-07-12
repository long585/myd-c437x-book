###4.11 USB Host Test

This example demonstrates how to use USB host to mount mass stroage device and verify the 
driver of USB host.  

**Hardware Preparation:**    
  * One MYD-C437X-PRU development board     
  * One USB to TTL converter used to connect J25 of MYD-C437X-PRU development board and host PC, set the baudrate of serial port on  host PC to 115200-8-n-1
  * One USB disk

**Software Preparation:**    
  * Linux Kernel 4.1.18   
  * mount and umount commands  

**Test Steps:**  
  * Plug the USB disk in the USB host interface of MYD-C437X-PRU development board, use `mount` or `umount` command to load and unload USB disk.
When users plug in  the USB disk, Linux kernel dumps the message as below:  
  
```
# [13752.969569] usb 1-1: new high-speed USB device number 2 using xhci-hcd
[13753.114361] usb 1-1: New USB device found, idVendor=0930, idProduct=6545
[13753.121504] usb 1-1: New USB device strings: Mfr=1, Product=2, SerialNumber=3
[13753.129056] usb 1-1: Product: DT 101 G2
[13753.133580] usb 1-1: Manufacturer: Kingston
[13753.138021] usb 1-1: SerialNumber: 001D6095CA1EEC2146A90004
[13753.179033] usb-storage 1-1:1.0: USB Mass Storage device detected
[13753.189488] scsi host0: usb-storage 1-1:1.0
[13753.197187] usbcore: registered new interface driver usb-storage
[13754.266687] scsi 0:0:0:0: Direct-Access  Kingston DT 101 G2  PMAP PQ: 0 ANSI: 0 CCS
[13755.539278] sd 0:0:0:0: [sda] 15240576 512-byte logical blocks: (7.80 GB/7.27 GiB)
[13755.547768] sd 0:0:0:0: [sda] Write Protect is off
[13755.553880] sd 0:0:0:0: [sda] No Caching mode page found
[13755.559903] sd 0:0:0:0: [sda] Assuming drive cache: write through
[13755.591585]  sda: sda1
[13755.602727] sd 0:0:0:0: [sda] Attached SCSI removable disk
```  

  * it shows USB host works well and USB disk is detected, users can mount it to /mnt directory of
the embedded Linux system as below:  
  
```   
#
# mount /dev/sda1 /mnt
[ 1301.201855] FAT-fs (sdb1): Volume was not properly unmounted. Some data may be corrupt. 
Please run fsck.

# ls /mnt
u-boot.img  		MLO             helloworld      

```  

  * Plug out the USB disk, Linux kernel dumps the message as below:  
  
```
#
# [14018.109698] usb 1-1: USB disconnect, device number 2

```
