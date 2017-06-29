##1. Software Resources   
------------------------  
Along with the MYD-C437X-PRU development board, we provide a SDK for developing an embedded Linux system on MYD-AM437X-PRU development board, it includes the fundamental cross compile toolchains，source code of U-boot, Kernel, drivers and test applications of all the peripheral modules.
 
The content of SDK for Linux is in the directory *05-RTOS_Source* of our release package. It is shown as a table below.    

Table 1-1 Software Resouces List    

 Category | Name | Description | source  
 ---- | ---- | ---- | ----   
Bootloader | U-boot201605 | First and second stage bootloader SPL and U-Boot，responsible for system initialization and boot Linux kernel | YES
Kernel | Linux 4.1.18 | Kernel designed for MYD-C437X-PRU | YES
Driver | USB Host | USB host driver| YES 
Driver| USB Device | USB device driver（Gadget） | YES
Driver| Ethernet | Ethernet driver | YES
Driver| PRU Ethernet| Industrial ethernet driver | YES
Driver| MMC/SD | MMC/SD card driver | YES
Driver| EMMC | EMMC driver | YES
Driver| I2C | I2C driver | YES
Driver| SPI | SPI driver | YES 
Driver| LCD | LCD framebuffer driver, use 7 inches 800*480 LCD as default | YES 
Driver| RTC | Internal RTC driver | YES 
Driver| RX-8025T | External RTC driver | YES 
Driver| RS485 | RS485 driver | YES 
Driver| ADC | ADC driver | YES 
Driver| Resistive TouchScreen | Risistor touchscreen driver | YES 
Driver| Capacitive TouchScreen | Capacitive touchscreen driver with FT5X06 IC  | YES
Driver| UART | Uart driver | YES 
Driver| CAN | CAN driver| YES 
Driver| PMU | Power manager unit driver | YES 
Driver| LED | LED driver| YES 
Driver| Button | GPIO Button driver | YES
Driver| Camera | camera driver with ov2659 IC| YES 
Filesystem | RAMDISK | Ramdisk filesystem built with Buildroot| YES 
Filesystem| UBIFS | UBIFS filesystem built with Buildroot | YES 
Application| CAN | CAN test application| YES 
Application| EEPROM | EEPROM test application| YES 
Application| FrameBuffer | Framebuffer test applicaton| YES 
Application| Keypad | Keypad test application | YES 
Application| RTC | RTC test application| YES 
Application| GPIO | GPIO test application| YES
Application| LED | LED test application| YES 
Application| RS232 | RS232 test application| YES 
Application| RS485 | RS485 test application | YES 
Application| PRU | PRU led test application| YES 
Application| Qt | Qt Hello World demo application| YES 
Tools| Cross Compiler| Linaro GCC 5.3 | BIN 
Tools| Win32DiskImager| TF/SD image write | EXE 
Tools| Format Tools | TF/SD format tools | EXE




