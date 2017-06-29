##3. Build System  
---------------------
There are many open source tools for building an embedded Linux system, they are more convenient for embedded software engineers to build bootloader, kernel, filesystem all in one step. Currently, [OpenWRT](https://openwrt.org), [Buildroot](http://buildroot.org) , [Yocoto](https://www.yoctoproject.org) are more commonly used.

Buildroot is a simple, efficient and easy-to-use tool to generate embedded Linux systems through cross-compilation,thanks to its kernel-like menuconfig, gconfig and xconfig configuration interfaces, building a basic system with Buildroot is easy, so it's very popular among embedded software engineers.  

The following sections will explain U-boot, kernel, filesystem respectively, in the part of filesystem, we build a filesystem with QT5 included, customers can develop QT5 application based on this filesystem easily. 

