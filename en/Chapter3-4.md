###3.4 Build QT

QT5 is included in Buildroot as a target package, we have provided a config file with QT5 for MYD-C437X-PRU development board, so it is easy to build filesystem images with QT5 
shown as below.
```
$ cd <WORKDIR>/Filesystem/myir-buildroot
$ make myd_c437x_idk_qt5_defconfig
$ make
```
Build Buildroot：  

The following table shows the difference between the two config files. 

| Config File| Description |
|---------|------|
| myd_c437x_idk_defconfig | Buildroot configuration without QT5 for MYD-C437X-PRU development board |
| myd_c437x_idk_qt5_defconfig | Buildroot configuration with QT5 for MYD-C437X-PRU development board |

In `myd_c437x_idk_qt5_defconfig`, the following items are different with `myd_c437x_idk_defconfig` have been choosen:  
```
BR2_PACKAGE_QT5=y
BR2_PACKAGE_QT5BASE_LICENSE_APPROVED=y
BR2_PACKAGE_QT5BASE_EXAMPLES=y
BR2_PACKAGE_QT5BASE_WIDGETS=y
BR2_PACKAGE_QT5BASE_LINUXFB=y
BR2_PACKAGE_QT5BASE_EGLFS=y
BR2_PACKAGE_QT5QUICKCONTROLS=y
BR2_PACKAGE_TI_SGX_DEMOS=y
BR2_PACKAGE_TI_SGX_KM=y
BR2_PACKAGE_TI_SGX_UM=y
```   

After compiling, the filesystem includes eglfs, linuxfb, minimal, offscreen platform plugins for running QT5 applications, the default platform plugin is eglfs.
If the processor is one of AM4372, AM4376 and AM4377 which has no SGX feature, the device node `&sgx` should be disabled in the device tree source file, thus the eglfs platform plugin
does not work in the filesystems. Another platform plugin `linuxfb` should be choosed for running QT5 applications here and now as shown below:
```
$ ./helloqt5 --platform linuxfb:fb=/dev/fb0
```

After compiling with the config file `myd_c437x_idk_qt5_defconfig`, all the target images are generated at path `myir-buildroot/output/images`.
Beyond that, a cross compiler and a qmake tools for building QT5 applications are generated at path `myir-buildroot/output/host`. 
These will be described in detail in the subsequent sections.


