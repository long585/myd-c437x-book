###3.2 编译Linux内核  

* 进入Kernel目录，解压内核源码：  

```
$ cd <WORKDIR>/Kernel
$ tar -zxvf myir-kernel.tar.gz
$ cd myir-kernel
```

* 开始编译Kernel：  

不同的开发板对应不同的配置文件，配置文件位于*myir-kernel/arch/arm/configs/*目录

| 开发板 | 内核配置 |
|---------|------|
| MYD-C437x-PRU | myd_c437x_idk_defconfig |

下面以MYD-C437X-PRU开发板为例，说明kernel的编译过程：

```
$ make mrproper
$ make myd_c437x_idk_defconfig
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

`MYD-C437X-PRU`开发板中LCD和PRU Ethernet复用，也可以修改设备树中GPIO4的配置，通过GPIO4-19和GPIO4-21来选择。 当GPIO4-19和GPIO4-21都为低时，选择PRU Ethernet和CAMERA0可用，LCD不能工作，
所以同时还要设置&dss节点status="disabled", &vpfe0节点status="okay", pruss1_eth节点status="okay"，如下所示：
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

反之，当GPIO4-19为低,GPIO4-21为高时，相应的节点设置也要反过来，具体参照myd_c437x_idk_lcd.dts。  
  
> 注意: 当用户修改了U-boot或Kernel的代码之后，Buildroot不会自动更新，必须手动提交到相应的GIT仓库。  

> 注意: 当用户更新了Kernel代码之后，再重新快速编译Buildroot时，需要手动删除"myir-buildroot/dl/linux-master.tar.gz"文件以及
"myir-buildroot/output/build/linux-master" 和"myir-buildroot\output\build\linux-headers-master"这两个目录。U-boot的更新也类似。  



