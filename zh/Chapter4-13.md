### 4.13 CAMERA

本例程演示在linux下的摄像头使用。

**测试硬件环境：**

* MYD-C437X-EVM 开发板一块
* 
* USB转TTL调试串口一根，连接MYD-C437X-PRU J25和PC, PC端波特率设置115200-8-n-1

**测试软件环境：**

* Linux Kernel 4.1.18
* ifconfig, ping等命令     
* pru\_led\_test 应用程序  

**测试过程：**

* 测试PRUSS1扩展工业以太网接口  
  MYD-C437X-PRU开发板默认出厂固件为PRU Ethernet工作模式，实现一个千兆以太网接口J6和两个PRUSS1扩展的百兆工业以太网接口\(J26和J27\)。
  三个网口可以独立工作。将三个网口连接到交换机和PC一起组成一个小型网络，然后测试如下：  

```
# ifconfig -a
can0      Link encap:UNSPEC  HWaddr 00-00-00-00-00-00-00-00-00-00-00-00-00-00-00
                    NOARP  MTU:16  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:10
          RX bytes:0 (0.0 B)  TX bytes:0 (0.0 B)
          Interrupt:159

eth0      Link encap:Ethernet  HWaddr C4:BE:84:CB:13:AF
          inet addr:192.168.30.116  Bcast:192.168.30.255  Mask:255.255.255.0
          inet6 addr: fe80::c6be:84ff:fecb:13af/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:1992 errors:0 dropped:0 overruns:0 frame:0
          TX packets:11 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:138373 (135.1 KiB)  TX bytes:1392 (1.3 KiB)
          Interrupt:143

eth1      Link encap:Ethernet  HWaddr 4E:18:13:C8:60:0D
          BROADCAST MULTICAST  MTU:1500  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:1730283589 txqueuelen:1000
          RX bytes:0 (0.0 B)  TX bytes:0 (0.0 B)

eth2      Link encap:Ethernet  HWaddr 72:90:07:2B:6C:10
          BROADCAST MULTICAST  MTU:1500  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:958943060 txqueuelen:1000
          RX bytes:0 (0.0 B)  TX bytes:0 (0.0 B)
```

系统默认只使能了eth0, eth1和eth2可以通过如下命令手动设置IP地址并使能，如下：

```
# ifconfig eth1 192.168.30.117 netmask 255.255.255.0 up
[ 1361.576798]  remoteproc1: powering up 54434000.pru0
[ 1361.582309]  remoteproc1: Booting fw image ti-pruss/am437x-pru0-prueth-fw.elf, 
size 3893
[ 1361.591546] pruss-rproc 54400000.pruss: configured system_events = 0x0000060000500000 
intr_channels = 0x00000095 host_intr = 0x00000115
[ 1361.604692]  remoteproc1: remote processor 54434000.pru0 is now up
[ 1361.612466] net eth1: started
[ 1361.615797] IPv6: ADDRCONF(NETDEV_UP): eth1: link is not ready
# [ 1364.079947] eth1: Link is Up - 100Mbps/Full - flow control rx/tx
[ 1364.086377] IPv6: ADDRCONF(NETDEV_CHANGE): eth1: link becomes ready

# ifconfig eth2 192.168.30.118 netmask 255.255.255.0 up
[ 1442.906605]  remoteproc2: powering up 54438000.pru1
[ 1442.912123]  remoteproc2: Booting fw image ti-pruss/am437x-pru1-prueth-fw.elf, 
size 3893
[ 1442.921365] pruss-rproc 54400000.pruss: configured system_events = 0x0060000000a00000 
intr_channels = 0x0000012a host_intr = 0x0000022a
[ 1442.934397]  remoteproc2: remote processor 54438000.pru1 is now up
[ 1442.942090] net eth2: started
[ 1442.945432] IPv6: ADDRCONF(NETDEV_UP): eth2: link is not ready
# [ 1445.159906] eth2: Link is Up - 100Mbps/Full - flow control rx/tx
[ 1445.166334] IPv6: ADDRCONF(NETDEV_CHANGE): eth2: link becomes ready
```

以上Log信息显示PRU Ethernet连接正常，接下来可以在PC上使用ping命令和开发板上的网口依次进行连通测试，也可以进一步使用iperf工具进行测试，这里不详细介绍了。

* 测试PRUSS0控制GPIO LED   

定制内核和设备树, 由于MYD-C437X-PRU默认发布的代码使用的是PRUSS1以实现工业以太网接口的扩展，而板上的LED是通过PRUSS0来控制的。  
所以需要给内核打上相应的补丁进行切换。补丁打上之后，重新编译zImage和dtbs。补丁内容参见出厂附带资料04-Linux\_sources/patches/0001-Enable-PRUSS0-instead-of-PRUSS1-on-the-MYD\_C437x\_PRU.  
由于补丁内容较长，这里仅摘取关于pinmux配置的部分加以说明，如下：

```
// 修改pinmux模式，将AD21, AE22, AD22三个GPIO由arm控制改为由pru控制。
@@ -168,9 +148,10 @@
    leds_pins: leds_pins {
        pinctrl-single,pins = <
            0x244 ( PIN_OUTPUT_PULLUP | MUX_MODE7 ) /* (F23) gpio5_11.gpio5[11] */
-            0x1f0 ( PIN_OUTPUT_PULLUP | MUX_MODE7 ) /* (AD21) cam1_data2.gpio4[16] */
-            0x1f4 ( PIN_OUTPUT_PULLUP | MUX_MODE7 ) /* (AE22) cam1_data3.gpio4[17] */
-            0x1f8 ( PIN_OUTPUT_PULLUP | MUX_MODE7 ) /* (AD22) cam1_data4.gpio4[18] */
+            0x1f0 ( PIN_OUTPUT_PULLUP | MUX_MODE4 ) /* (AD21) cam1_data2.pr0_pru1_gpo[10] */
+            0x1f4 ( PIN_OUTPUT_PULLUP | MUX_MODE4 ) /* (AE22) cam1_data3.pr0_pru1_gpo[11] */
+            0x1f8 ( PIN_OUTPUT_PULLUP | MUX_MODE4 ) /* (AD22) cam1_data4.pr0_pru1_gpo[12] */
+
```

* 编译PRU端的firmware。PRU端的firmware使用ti-cgt-pru\_2.1.3编译器进行编译，对应的代码参见04-Linux\_Source\Examples\pru\_led目录下的代码PRU\_RPMsg\_LED0\_1。
  编译之后得到PRU\_RPMsg\_LED0\_1.out，将其拷贝到linux文件系统/lib/firmware/目录下，并重命名为am437x-pru0\_1-fw  

```
$ cd <WORKDIR>/ToolChain/
$ ./ti_cgt_pru_2.1.3_linux_installer_x86.bin 
$ export PRU_CGT=<WORKDIR>/ToolChain/ti-cgt-pru_2.1.3/
$ make

************************************************************
Building project: PRU_RPMsg_LED0_1

Finished building project: PRU_RPMsg_LED0_1
************************************************************
```

* 重新启动系统，如果Linux系统下出现/dev/rpmsg\_pru31设备节点，则说明PRU工作正常，且正确加载了firmware，才可以进行以下测试。
  在zImage和dtbs不变的情况下，也可以通过重新加载pru\_rproc.ko实现PRU firmware的加载  

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

* pru\_led\_test程序对LED进行控制  

```
# pru_led_test -h
Usage: pru_led_test [options]

Version 1.0
Options:
-d | --device name   pru rpmsg char device name /dev/rpmsg_pru31
-l | --loop          operate circularly, default not operate circularly!
-n | --number num    number send to pru! must be 0~7.
-h | --help          Print this message


# pru_led_test -d /dev/rpmsg_pru31 -n 7
Opened /dev/rpmsg_pru31, sending 1 messages

1 - Sent to PRU: 7
1 - Received from PRU:7

Received 1 messages, closing /dev/rpmsg_pru31
```

* 观察LED的变化, 会发现三个LED随着-n后面参数变化而变化。变化的规律就是数字转化为二进制之后，对应位为1的灯点亮，为0的熄灭。例如3的二进制位011b,则对应D18熄灭,D19,D20两个LED灯亮

MYIR AM437X系列其它板型PRU测试可以参照本例程进行。

