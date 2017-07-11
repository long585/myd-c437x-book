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

* 编译modetest应用程序，进入到Buildroot所在目录，执行`make menuconfig`进入如所示的配置界面

选中`Install test programs` 保存配置退出。

```
 /home/qinlh/builtroot/myir-buildroot/.config - Buildroot 2017.02-git-g92c215a C
o> Target packages > Libraries > Graphics > libdrm ----------------------------
  +-------------------------------- libdrm ---------------------------------+
  |  Arrow keys navigate the menu.  <Enter> selects submenus ---> (or empty |  
  |  submenus ----).  Highlighted letters are hotkeys.  Pressing <Y>        |  
  |  selectes a feature, while <N> will exclude a feature.  Press           |  
  |  <Esc><Esc> to exit, <?> for Help, </> for Search.  Legend: [*] feature |  
  | +---------------------------------------------------------------------+ |  
  | |    --- libdrm                                                       | |  
  | |    [ ]   radeon                                                     | |  
  | |    [ ]   amdgpu                                                     | |  
  | |    [ ]   nouveau                                                    | |  
  | |    -*-   omap (experimental)                                        | |  
  | |    [ ]   etnaviv (experimental)                                     | |  
  | |    [ ]   exynos (experimental)                                      | |  
  | |    [ ]   freedreno                                                  | |  
  | |    [ ]   tegra (experimental)                                       | |  
  | |    [ ]   vc4                                                        | |  
  | |    [*]   Install test programs                                      | |  
  | +---------------------------------------------------------------------+ |  
  +-------------------------------------------------------------------------+  
  |        <Select>    < Exit >    < Help >    < Save >    < Load >         |  
  +-------------------------------------------------------------------------+
  
```

执行make命令等待一段时间，编译好的modetest应用程序位于./output/build/libdrm-2.4.74/tests/modetest/modetest



