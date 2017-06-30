## 4. Linux应用开发

---

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

编译完Buildroot之后，在_myir-buildroot/output/host/usr/bin_目录下也生成了一个交叉编译工具链，这里可以使用这个工具链替换上面linaro的工具链：

```
$ export CROSS_COMPILE=arm-linux-myir-gnueabihf-
$ export PATH=$PATH:<WORKDIR>/Filesystem/myir-buildroot/output/host/usr/bin
```

Camera和Audio例程的Makefile需要指定依赖库的目录和头文件的目录,下面以Camera的例程为例子：

```
$ cd <WORKDIR>/Examples/camera
$ cat Makefile
CC = $(CROSS_COMPILE)gcc
CFLAGS ?=-I /<WORKDIR>/Filesystem/myir-buildroot/output/host/usr/arm-myir-linux-gnueabihf/sysroot/usr/include/libdrm/ \
                 -I <WORKDIR>/Filesystem/myir-buildroot/output/host/usr/arm-myir-linux-gnueabihf/sysroot/usr/include \
                 -I <WORKDIR>/Filesystem/myir-buildroot/output/host/usr/arm-myir-linux-gnueabihf/sysroot/usr/include/omap
LDFLAGS ?=  -lpthread  -ljpeg -ldrm -ldrm_omap -L <WORKDIR>/Filesystem/myir-buildroot/output/host/usr/arm-myir-linux-gnueabihf/sysroot/usr/lib
TARGET = $(notdir $(CURDIR))_test
SRC =  $(shell ls *.c)
OBJS = $(patsubst %.c ,%.o ,$(SRC))
.PHONY: all
all: $(TARGET)
$(TARGET) : $(OBJS)
        $(CC) $(CFLAGS) $(LDFLAGS) -o $@ $^
%.o : %.c
        $(CC) $(CFLAGS) -c $< -o $@ 
clean:
        $(RM) *.o $(TARGET)
```

用户可以一次完成所有示例的编译，如下:

```
$ cd <WORKDIR>/Examples/
$ make OPTION=MYD-C437X-EVM clean
$ make OPTION=MYD-C437X-EVM
$ make OPTION=MYD-C437X-EVM install
```

安装路径由Makefile里面的PREFIX变量指定默认为`<WORKDIR>/Examples/rootfs`

也可以一个一个的单独编译，例如:

```
$ cd <WORKDIR>/Examples/<APP_DIR>
$ make
```

在开发板上运行程序时需要注意权限，如果无法执行请使用如下命令：

```
# chmod +x *
```



