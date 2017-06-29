##4. Linux应用开发  
------------------------------------

本节主要介绍设备驱动功能验证的一些方法，并提供一些基本的应用开发示例实现对设备的操作。

MYD-AM437X系列提供了常用外设的演示程序，程序以及源码都位于`<WORKDIR>/Examples/`, 请根据目录内的Makefile或README文件进行编译：

```
$ cd <WORKDIR>/Examples/
```   
确保环境变量按下面的示例设置完成:   
``` 
$ export ARCH=arm
$ export CROSS_COMPILE=arm-linux-gnueabihf-
$ export PATH=$PATH:<WORKDIR>/ToolChain/gcc-linaro-5.3-2016.02-x86_64_arm-linux-gnueabihf/bin  
```    
编译完Buildroot之后，在*myir-buildroot/output/host/usr/bin*目录下也生成了一个交叉编译工具链，这里可以使用这个工具链替换上面linaro的工具链：  

```
$ export CROSS_COMPILE=arm-linux-myir-gnueabihf-
$ export PATH=$PATH:<WORKDIR>/Filesystem/myir-buildroot/output/host/usr/bin
```   

为了编译PRU的测试程序，还需要安装一个PRU工具链。 这个工具可以在附带的发布包*03-Tools/ti_cgt_pru_2.1.3_linux_installer_x86.bin*路径下找到。 
下面是安装和设置环境变量的步骤:  
```
$ cd <WORKDIR>/ToolChain/
$ ./ti_cgt_pru_2.1.3_linux_installer_x86.bin 
$ export PRU_CGT=<WORKDIR>/ToolChain/ti-cgt-pru_2.1.3/
```  
用户可以一次完成所有示例的编译，如下:  

```
$ cd <WORKDIR>/Examples/
$ make  
```  
也可以一个一个的单独编译，例如:     
```
$ cd <WORKDIR>/Examples/<APP_DIR>
$ make
```  
在开发板上运行程序时需要注意权限，如果无法执行请使用如下命令：

```
# chmod +x *
```  
