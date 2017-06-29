###2.2 Setup GCC Toolchain  

Before compiling U-boot or Kernel, we should set some environment variables on Ubuntu. 
The path of the cross compile toolchain should be added to `PATH` environment variable, and `ARCH` environment variable should be set to `arm`, `CROSS_COMPILE` environment variable should be set to `arm-linux-gnueabihf-`.

```c
$ cd <WORKDIR>/ToolChain
$ tar Jxvf  gcc-linaro-5.3-2016.02-x86_64_arm-linux-gnueabihf.tar.xz
$ export PATH=$PATH:<WORKDIR>/ToolChain/gcc-linaro-5.3-2016.02-x86_64_arm-linux-gnueabihf/bin
$ export ARCH=arm
$ export CROSS_COMPILE=arm-linux-gnueabihf-
```

All of the above processes are only effective in the current shell.
In order to make the environment variables effective to the current user, customers should set the profile configuration of the current user by adding or modifing
`~/.profile` in the home directory of the current user. It is shown as below:

```c
vi ~/.profile
```
Add or modify `~./profile` at the end of the file:  

```c
export ARCH=arm
export CROSS_COMPILE=arm-linux-gnueabihf-
export PATH=$PATH:<WORKDIR>/ToolChain/gcc-linaro-5.3-2016.02-x86_64_arm-linux-gnueabihf/bin
```

**Verify the Enviroment Variables：**    

```
$source ~/.profile
$echo $ARCH
arm
$echo $CROSS_COMPILE
arm-linux-gnueabihf-
```  

**Verify the Cross Compiler：**  
```
$ arm-linux-gnueabihf-gcc -v
Using built-in specs.
COLLECT_GCC=arm-linux-gnueabihf-gcc
......
Thread model: posix
gcc version 5.3.1 20160113 (Linaro GCC 5.3-2016.02)
```  

