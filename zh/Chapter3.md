## 3. 构建系统

---

Linux平台上有许多开源的嵌入式linux系统构建框架，这些框架极大的方便了开发者进行嵌入式系统的定制化构建，目前比较常见的有[openWRT](https://openwrt.org/), [Buildroot](https://buildroot.org/), [Yocto](https://www.yoctoproject.org/), [Arago](http://arago-project.org/)等等。其中Buildroot功能强大，使用简单，而且采用了类似于linux kernel的配置和编译框架，所以受到广大嵌入式开发人员的欢迎。

本节重点介绍使用Buildroot构建文件系统和u-boot, kernel镜像的方法，并从这三个部分入手，描述如何使用Buildroot构建一个适合MYD-C437X-EVM开发板的嵌入式Linux系统。  
在构建文件系统时，还简要介绍了如何通过Buildroot将QT5图形系统集成到文件系统中, 方便用户后续开发QT5的应用程序。

TI 官方提供了使用Arago构建系统的方法和相应的文件系统镜像文件，也可以在这款开发板上运行。

