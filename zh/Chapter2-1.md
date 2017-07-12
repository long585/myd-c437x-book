### 2.1 安装工具

Linux编译过程中需要用到一些常用的开发工具或库，比如build-essential（提供编译程序必须软件包的列表信息）zip unzip xz-utils解压缩工具等，这里先进行安装。后续编译过程中发现缺少某些工具或库文件，也可以用类似的方法安装。

```
$ sudo apt-get install build-essential git-core libncurses5-dev
$ sudo apt-get install flex bison texinfo zip unzip zlib1g-dev gettext
$ sudo apt-get install gperf libsdl-dev libesd0-dev libwxgtk2.6-dev
$ sudo apt-get install uboot-mkimage
$ sudo apt-get install g++ xz-utils
```

在64位的Ubuntu系统中，还需要安装一些32位的运行时库，如下所示:

```
$sudo apt-get install libc6-i386 lib32stdc++6 lib32z1
```



