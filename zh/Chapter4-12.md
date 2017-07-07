### 4.12 USB DEVICE测试

本实例演示通过相关命令来验证USB DEVICE驱动的可行性。使用USB数据线连接开发板的miniUSB接口与电脑端的USB接口，开发板实现一个读卡器功能。

**测试硬件环境：**

* MYD-C437X-EVM 开发板一块  
* TF卡一个  
* USB miniB to mini A 线一根  
* USB转TTL调试串口一根，连接MYD-C437X-EVM J16和PC, PC端波特率设置115200-8-n-1

**测试软件环境：**

* Linux Kernel 4.1.18   
* modprobe 应用程序  

**测试过程：**

* 开发板上的系统起来后，使用USB mini B to USB A转接线连接开发板（J14接口）与电脑端，其中USB mini B接口连接开发板，USB A接口连接电脑端，插入TF卡（J19）,在开发板上运行如下命令：  

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

* g\_mass\_storage 驱动加载成功之后， PC端会识别到一个可移动磁盘，磁盘内容正是TF上的内容，至此就实现了读卡器的功能  

除此之外，通过加载不同的gadget模块驱动，开发板可以实现不同的功能，如g\_ether实现RNDIS网卡的功能，g\_serial实现串口的功能，等等。这里不一一列举。  
MYIR AM437X系列其它板型USB Device接口测试情况类似。

