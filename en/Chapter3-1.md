###3.1 Bootloader  

* Enter the directory of bootloader, uncompress the source code package as shown below：

```
$ cd <WORKDIR>/Bootloader
$ tar -zxvf myir-u-boot.tar.gz
$ cd myir-u-boot
```  

* Compile U-Boot：  

The configuration of U-boot for MYD-C437X-PRU is located at *myir-u-boot/configs/*, the corresponding configuration file name and output files are shown below:  

| Board| Configuration File Name | Output |
|---------|-------------|------|
| MYD-C437X-PRU | myd_c437x_defconfig | MLO and u-boot.img |
 
Compile U-boot for MYD-C437X-PRU development board：
```
$ make distclean
$ make myd_c437x_defconfig
$ make
```  

After compiling U-boot, MLO and u-boot.img will be generated for mmc boot mode, They can be used for booting from TF Card(mmc0) and EMMC(mmc1). 
Users can interrupt the running of U-boot by pressing `SPACE` key in the debug terminal on host PC to enter the console of U-boot. Please input 
`help` command in the U-boot console to get the usage of U-boot as shown below:  

```
># help			-- Display help for U-boot
># echo			-- View a U-boot environment variable
```


