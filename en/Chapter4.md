##4. Linux Application Development    
------------------------------------

This section focuses on application development based on embedded Linux, the following examples provided by myirtech demonstrate
how to take the control of some commonly used peripheral devices through Linux applications. The source code of these examples is 
located at *04-Linux_Source\Examples* of our release package. Please follow the instructions provided in the readme file to set the 
environment variables, compile the source code and install the binary files into MYD-C437X-PRU development board.  


```
$ cd <WORKDIR>/Examples/
```   
Make sure the following environment variables are right.   
```
$ export ARCH=arm
$ export CROSS_COMPILE=arm-linux-gnueabihf-
$ export PATH=$PATH:<WORKDIR>/ToolChain/gcc-linaro-5.3-2016.02-x86_64_arm-linux-gnueabihf/bin  
```    
After building Buildroot, a cross compile toolchain has been created at *myir-buildroot/output/host/usr/bin*, it can be used here by setting 
the environment variables instead of the above.  

```
$ export CROSS_COMPILE=arm-linux-myir-gnueabihf-
$ export PATH=$PATH:<WORKDIR>/Filesystem/myir-buildroot/output/host/usr/bin
```   
For building PRU test application, a PRU toolchain should be instaled as show below, it has been provided in our release package at path *03-Tools/ti_cgt_pru_2.1.3_linux_installer_x86.bin*.    

```
$ cd <WORKDIR>/ToolChain/
$ ./ti_cgt_pru_2.1.3_linux_installer_x86.bin 
$ export PRU_CGT=<WORKDIR>/ToolChain/ti-cgt-pru_2.1.3/
```  
Customers could build examples all in one step:   

```
$ cd <WORKDIR>/Examples/
$ make  
```  
or build the examples respectively:   
```
$ cd <WORKDIR>/Examples/<APP_DIR>
$ make
```  

If the binary files have no permission to run, please assign the running permission to them by chmod：  

```
# chmod +x *
```  
