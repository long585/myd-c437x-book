### 3.1 Bootloader

* 进入Bootloader目录，解压U-boot源码：

```
$ cd <WORKDIR>/Bootloader
$ tar -zxvf myir-u-boot.tar.gz
$ cd myir-u-boot
```

* 开始编译U-Boot：  

不同的开发板对应不同的配置文件，配置文件位于myir-u-boot/configs/目录

| 开发板 | 编译选项 | 输出文件 |
| --- | --- | --- |
| MYD-C437X-EVM | myd\_c437x\_evm\_defconfig | MLO和u-boot.img |

下面以MYD-C437X-PRU开发板为例，说明u-boot的编译过程：

```
$ make distclean
$ make myd_c437x_defconfig
$ make
```

编译完成之后生成MLO和u-boot.img可以用于TF卡启动和EMMC启动，优先选择从TF卡启动，启动过程中，通过按空格键可以进入U-Boot命令控制台执行需要的命令。例如：

```
># help            -- 获取帮助
># echo            -- 查看变量
```



