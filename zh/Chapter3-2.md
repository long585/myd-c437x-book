### 3.2 编译Linux内核

* 进入Kernel目录，解压内核源码：  

```
$ cd <WORKDIR>/Kernel
$ tar -zxvf myir-kernel.tar.gz
$ cd myir-kernel
```

* 开始编译Kernel：  

不同的开发板对应不同的配置文件，配置文件位于_myir-kernel/arch/arm/configs/_目录

| 开发板 | 内核配置 |
| --- | --- |
| MYD-C437x-EVM | myd\_c437x\_evm\_defconfig |

下面以MYD-C437X-EVM开发板为例，说明kernel的编译过程：

```
$ make mrproper
$ make myd_c437x_evm_defconfig
$ make zImage
$ make dtbs
```

编译完成后，在arch/arm/boot目录下生成zImage文件, 在arch/arm/boot/dts目录下生成设备树的二进制.dtb文件, 同一块开发板，适当修改DTS文件可以适应于不同的硬件配置。  
例如AM4372,AM4376和AM4377三种型号的处理器，不具备SGX图形加速的功能，所以需要在设备树中禁用SGX，如下：

```
&sgx {
        status = "disabled";
};
```

反之，如果选择AM4378或AM4379这两款支持SGX图形加速的处理器，则需要使能SGX，如下：

```
&sgx {
        status = "okay";
};
```

> 注意: 当用户修改了U-boot或Kernel的代码之后，Buildroot不会自动更新，必须手动提交到相应的GIT仓库。
>
> 注意: 当用户更新了Kernel代码之后，再重新快速编译Buildroot时，需要手动删除"myir-buildroot/dl/linux-master.tar.gz"文件以及  
> "myir-buildroot/output/build/linux-master" 和"myir-buildroot\output\build\linux-headers-master"这两个目录。U-boot的更新也类似。



