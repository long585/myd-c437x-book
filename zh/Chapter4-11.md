### 4.11 USB Host 测试

本例程演示通过相关命令来验证USB Host驱动的可行性。

**测试硬件环境：**

* MYD-C437X-EVM 开发板一块  
* U盘一个  
* USB转TTL调试串口一根，连接MYD-C437X-PRU J25和PC, PC端波特率设置115200-8-n-1

**测试软件环境：**

* Linux Kernel 4.1.18   
* mount, umount 应用程序  

**测试过程：**

* 将U盘连接到开发板USB Host接口，并且执行mount, umount，读写，插拔等操作。插入U盘至USB Host接口J7时内核提示信息如下：  

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

* 将U盘挂载到文件系统/mnt目录，并执行基本的查看，读写操作  

```
#
# mount /dev/sda1 /mnt
[ 1301.201855] FAT-fs (sdb1): Volume was not properly unmounted. Some data may be corrupt. 
Please run fsck.

# ls /mnt
u-boot.img          MLO             helloworld
```

* 拔下U盘，内核提示信息   

```
#
# [14018.109698] usb 1-1: USB disconnect, device number 2
```

MYIR AM437X系列其它板型USB Host接口测试情况类似。

