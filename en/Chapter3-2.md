###3.2 Build Linux Kernel  

* Enter the work directory and uncompress the Linux Kernel source code package：  

```
$ cd <WORKDIR>/
$ tar -zxvf myir-kernel.tar.gz
$ cd myir-kernel
```

* Compile Kernel：  

The configuration of Kernel for MYD-C437X-PRU is located at *myir-kernel/arch/arm/configs/*, the corresponding configuration file name is `myd_c437x_idk_defconfig`.
Customers can compile Kernel as shown below:  

```
$ make mrproper
$ make myd_c437x_idk_defconfig
$ make zImage
$ make dtbs
```

After compiling, the Kernel image file *myir-kernel/arch/arm/boot/zImage* is generated for MYD-C437X-PRU. 
At the same time, there are two device tree binary files generated at *myir-kernel/arch/arm/boot/dts/*, 
they are named as `myd_c437x_idk.dtb` and `myd_c437x_idk_lcd.dtb`. 
 
As the LCD and PRU Ethernet are hard multiplexed on MYD-C437X-PRU development board, the two dtb files are designed to work in different mode.
`myd_c437x_idk.dtb` is designed for industrial ethernet, and `myd_c437x_idk_lcd.dtb` is designed for LCD, please refer to the device tree source code of 
these two device tree binary files.

The two work mode of MYD-C437X-PRU are controlled by GPIO4-19 and GPIO4-21. When GPIO4-19 and GPIO4-21 output low level, PRU Ethernet and CAMERA0 are choosed,
and LCD does not work, so the device node `&dss` for LCD should be set to `disabled`, the device node `&vpfe0` for camera0 and device node `pruss1_eth` for PRU Ethernets
should be set to `okay`, as shown in `myd_c437x_idk.dts` file as shown below:  

```
/*
 * File: myd_c437x_idk.dts
 */

......

	gpio4_pins: gpio4_pins_default {
		pinctrl-single,pins = <
		0x1fc ( PIN_OUTPUT_PULLUP | MUX_MODE7 ) /* (AE23) cam1_data5.gpio4[19] */
		0x204 ( PIN_OUTPUT_PULLUP | MUX_MODE7 ) /* (AE24) cam1_data7.gpio4[21] */
		>;
	};
	
......

&gpio4 {
	pinctrl-names = "default";
	pinctrl-0 = <&gpio4_pins>;
	status = "okay";

	p19 {
		gpio-hog;
		gpios = <19 GPIO_ACTIVE_HIGH>;
		output-low;
		line-name = "SelPRUorLCDEN";
	};
	p21 {
		gpio-hog;
		gpios = <21 GPIO_ACTIVE_HIGH>;
		output-low;
		line-name = "SelPRUorLCDSEL";
	};
	/* SelPRUorLCDEN enable selects between PRU_ETH and LCD :
	 * P19(OE)	| P21 (SEL)	| FUNCTION 
	 *----------|-----------|----------
	 * LOW		  |  LOW	    |  PRU_ETH + CAMERA0
	 * LOW		  |	 HIGHT	  |	 LCD
	 * HIGH		  |	 ANY		  |	 NONE
	 * 
	 * When changing this line make sure the newly
	 * selected device node is enabled and the previously
	 * selected device node is disabled.
	 */
};

......

&dss {
	status = "disabled";
......
};

&vpfe0 {
	status = "okay";
......
};

&pruss1 {
	pruss1_mdio: mdio@54432400 {
		pinctrl-0 = <&pruss1_mdio_default>;
		pinctrl-names = "default";
		reset-gpios = <&gpio4 20 GPIO_ACTIVE_LOW>;
		status = "okay";

		pruss1_eth0_phy: ethernet-phy@0 {
			reg = <0>;
		};

		pruss1_eth1_phy: ethernet-phy@1 {
			reg = <1>;
		};
	};

	/* Dual mac ethernet application node on icss1 */
	pruss1_eth {
		compatible = "ti,am4372-prueth";
		pruss = <&pruss1>;
		sram = <&ocmcram_nocache>;
		status = "ok";

		pinctrl-0 = <&pruss1_eth_default>;
		pinctrl-names = "default";

		pruss1_emac0: ethernet-mii0 {
			phy-handle = <&pruss1_eth0_phy>;
			phy-mode = "mii";
			sysevent-rx = <20>;	/* PRU_ARM_EVENT0 */
			/* Filled in by bootloader */
			local-mac-address = [00 00 00 00 00 00];
		};

		pruss1_emac1: ethernet-mii1 {
			phy-handle = <&pruss1_eth1_phy>;
			phy-mode = "mii";
			sysevent-rx = <21>;	/* PRU_ARM_EVENT1 */
			/* Filled in by bootloader */
			local-mac-address = [00 00 00 00 00 00];
		};
	};
};

```  

Otherwise, when GPIO4-19 outputs low level, GPIO4-21 outputs high level, the device node `&dss` for LCD should be set to `okay`, the device node `&vpfe0` for camera0 and device node `pruss1_eth` for PRU Ethernets
should be set to `disabled`, please refer to `myd_c437x_idk.dts` file for detail.   

Another example of device tree is relative to the SGX feature of AM437X. The three processors, AM4372, AM4376 and AM4377 have no SGX feature, so 
customers should set the device node `&sgx` to `disabled`.  

```
&sgx {
        status = "disabled";
};

```  
Otherwise, for AM4378 and AM4379, these two processors have SGX feature, the device node `&sgx` should be set to `okay` as shown below:  

```
&sgx {
        status = "okay";
};

```   
> Note: After modifying source code of Kernel or U-boot, Buildroot can not update and build it automatically. 
Customers should commit it to the master branch of their local git repo manually.   
 
> Note: If the source code of Kernel is updated, before building Buildroot again, customers should remove the package "myir-buildroot/dl/linux-master.tar.gz" and 
the "myir-buildroot/output/build/linux-master" and "myir-buildroot\output\build\linux-headers-master" directories manually. The same to rebuilding of U-boot.




