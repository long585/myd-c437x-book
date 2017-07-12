### 2.2 设置交叉编译工具

设置交叉编译工具主要是设置PATH， ARCH和CROSS\_COMPILE三个环境变量，下面介绍具体设置方法。

```
$ cd <WORKDIR>/ToolChain
$ tar Jxvf  gcc-linaro-5.3-2016.02-x86_64_arm-linux-gnueabihf.tar.xz
$ export PATH=$PATH:<WORKDIR>/ToolChain/gcc-linaro-5.3-2016.02-x86_64_arm-linux-gnueabihf/bin
$ export ARCH=arm
$ export CROSS_COMPILE=arm-linux-gnueabihf-
```

执行完“export”命令后，该设置只对当前终端有效，如需永久修改，请修改用户配置文件, Ubuntu系统下，修改如下：

```c
vi ~/.profile
```

在行尾添加或修改：

```
export ARCH=arm
export CROSS_COMPILE=arm-linux-gnueabihf-
export PATH=$PATH:<WORKDIR>/ToolChain/gcc-linaro-5.3-2016.02-x86_64_arm-linux-gnueabihf/bin
```

**测试环境变量：**

```
$source ~/.profile
$echo $ARCH
arm
$echo $CROSS_COMPILE
arm-linux-gnueabihf-
```

**测试交叉编译器：**

```
$ arm-linux-gnueabihf-gcc -v
Using built-in specs.
COLLECT_GCC=arm-linux-gnueabihf-gcc
......
Thread model: posix
gcc version 5.3.1 20160113 (Linaro GCC 5.3-2016.02)
```



