## 1. 软件资源介绍

---

MYD AM437X系列开发板出厂附带嵌入式Linux系统开发所需要的交叉编译工具链，U-boot源代码，Linux 内核和各驱动模块的源代码，以及各种配套的开发调试工具，应用开发样例等。  
本节内容以表格形式描述MYD-C437X-PRU的软件资源。  
表 1-1 软件资源列表

| 类别 | 名称 | 描述 | 源码 |
| --- | --- | --- | --- |
| 引导程序 | U-boot201605 | 一，二级引导程序SPL和U-Boot，负责系统初始化和引导内核 | YES |
| 内核 | Linux 4.1.18 | 专为MYD-AM437X系列开发板的硬件制定的Linux内核 | YES |
| 驱动 | USB Host | USB Host驱动，支持OHCI和EHCI两种传输模式 | YES |
| 驱动 | USB Device | USB Device驱动（Gadget） | YES |
| 驱动 | Ethernet | 以太网驱动 | YES |
| 驱动 | PRU Ethernet | 工业以太网驱动 | YES |
| 驱动 | MMC/SD | MMC/SD卡驱动 | YES |
| 驱动 | EMMC | EMMC驱动 | YES |
| 驱动 | I2C | I2C驱动 | YES |
| 驱动 | SPI | SPI驱动 | YES |
| 驱动 | LCD | LCD屏驱动，默认7寸800\*480液晶屏 | YES |
| 驱动 | RTC | 内置RTC时钟驱动 | YES |
| 驱动 | RX-8025T | 外置RTC时钟驱动 | YES |
| 驱动 | RS485 | RS485驱动，提供源码 | YES |
| 驱动 | ADC | ADC驱动，提供源码 | YES |
| 驱动 | Resistive TouchScreen | 4线电阻触摸屏驱动 | YES |
| 驱动 | Capacitive TouchScreen | FT5X06电容触摸 | YES |
| 驱动 | UART | 串口驱动 | YES |
| 驱动 | CAN | CAN驱动 | YES |
| 驱动 | PMU | 电源管理驱动 | YES |
| 驱动 | LED | LED驱动，包括GPIO LED和PWM LED驱动 | YES |
| 驱动 | Button | GPIO Button 驱动 | YES |
| 驱动 | Camera | 摄像头驱动 | YES |
| 文件系统 | RAMDISK | 基于Buildroot定制的文件系统 | YES |
| 文件系统 | UBIFS | 基于Buildroot定制的Qt文件系统 | YES |
| 应用程序 | CAN | CAN 测试程序 | YES |
| 应用程序 | EEPROM | EEPROM测试程序 | YES |
| 应用程序 | FrameBuffer | 显示设备测试程序 | YES |
| 应用程序 | Keypad | 按键测试程序 | YES |
| 应用程序 | LED | LED测试程序 | YES |
| 应用程序 | RTC | RTC时钟测试实验 | YES |
| 应用程序 | GPIO | GPIO测试程序 | YES |
| 应用程序 | RS232 | RS232测试程序 | YES |
| 应用程序 | RS485 | RS485测试程序 | YES |
| 应用程序 | PRU | PRU LED测试程序 | YES |
| 应用程序 | Qt | Qt环境以及演示程序 | YES |
| 工具 | Cross Compiler | Linaro GCC 5.3 | BIN |
| 工具 | Win32DiskImager | TF/SD镜像烧写工具 | EXE |
| 工具 | Format Tools | TF/SD格式化工具 | EXE |



