### 4.15 HDMI

本例程演示通过linux的libdrm库提供的接口来访问内核DRM显示框架，实现HDMI接口屏的显示。

**测试硬件环境：**

* MYD-C437X-EVM 开发板一块
* HDMI连接线一根，HDMI接口的显示器一台

* USB转TTL调试串口一根，连接MYD-C437X-EVM J16和PC, PC端波特率设置115200-8-n-1

**测试软件环境：**

* Linux Kernel 4.1.18

* modetest应用程序

**测试过程：**

* 编译modetest应用程序



