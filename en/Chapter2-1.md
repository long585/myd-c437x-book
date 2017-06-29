###2.1 Install Tools  

There are some essential tools and libraries needed to be installed  during developing an embedded Linux system, such as `build-essential`, `zip`, `unzip` and so on.
For the sake of convenience we install all the tools and libraries before as shown below:

```c
$ sudo apt-get install build-essential git-core libncurses5-dev
$ sudo apt-get install flex bison texinfo zip unzip zlib1g-dev gettext
$ sudo apt-get install gperf libsdl-dev libesd0-dev libwxgtk2.6-dev
$ sudo apt-get install uboot-mkimage
$ sudo apt-get install g++ xz-utils
```  

On 64bit Ubuntu OS, some 32bit runtime libraries should be installed as shown below:  

```
$sudo apt-get install libc6-i386 lib32stdc++6 lib32z1
```
