### 3.4 编译QT

QT的编译我们推荐使用Buildroot，Buildroot可以很容易地编译Qt5图形库，并且整合在文件系统中。同时，我们提供了配置文件，可以更方便地编译Qt5。

进入上一节中解压好的myir-buildroot目录：

```
$ cd <WORKDIR>/Filesystem/myir-buildroot
```

开始编译Buildroot：

| 配置项 | 含义 |
| --- | --- |
| myd\_c437x\_evm\_defconfig | MYD-C437X-EVM不带qt5运行环境的的Buildroot配置 |
| myd\_c437x\_evm\_qt5\_defconfig | MYD-C437X-EVM带qt5运行环境的的Buildroot配置 |

两种配置之间的差异如下:

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

编译完成之后的QT支持eglfs, linuxfb, minimal, offscreen等几种platform插件，默认使用的插件是eglfs。  
如果我们使用的SOC为AM4372，AM4376或AM4377这几个没有SGX图形加速功能的型号之一，则linux内核设备树中，需要禁用sgx驱动，eglfs插件也就不能工作了。这时只能选择另外一种platform插件linuxfb。  
在运行QT5应用程序的时候，通过--platform参数选择插件。例如：

```
$ ./helloqt5 --platform linuxfb:fb=/dev/fb0
```

`MYD-C473X-PRU`开发板支持QT5的系统编译：

```
$ make clean
$ make myd_c437x_evm_qt5_defconfig
$ make
```

等待编译完成后，Qt5会打包在位于`<WORKDIR>/Filesystem/myir-buildroot/output/images`目录之下的文件系统镜像中。`<WORKDIR>/Filesystem/myir-buildroot/output/host`同样可作为Qt开  
发的SDK目录，其中包含了交叉编译工具, QT5应用程序构建工具qmake等，在后续章节中会详细介绍。

