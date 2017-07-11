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

执行make命令等待一段时间，编译好的modetest应用程序位于`<WORKDIR>/Filesystem/myir-buildroot/output/build/libdrm-2.4.74/tests/modetest/modetest`

* 修改启动要加载的设备树为myd\_c437x\_evm\_hdmi.dtb：

  **SD卡启动**：编辑位于SD中的uEnv\_ramdisk.txt的内容，编辑完成后将文件名改为uEnv.txt。内容改为如下所示：

```
# This uEnv.txt file can contain additional environment settings that you
# want to set in U-Boot at boot time.  This can be simple variables such
# as the serverip or custom variables.  The format of this file is:
#    variable=value
# NOTE: This file will be evaluated after the bootcmd is run and the
#       bootcmd must be set to load this file if it exists (this is the
#       default on all newer U-Boot images.  This also means that some
#       variables such as bootdelay cannot be changed by this file since
#       it is not evaluated until the bootcmd is run.
#optargs=video=HDMI-A-1:800x600

# Uncomment the following line to enable HDMI display and disable LCD display.
#fdtfile=myd_c437x_evm.dtb
fdtfile=myd_c437x_evm_hdmi.dtb
devtype=mmc
devnum=0
bootdir=/
bootpart=0:1
uenvcmd=if run loadimage; then run loadfdt; run loadramdisk; echo Booting from mmc${mmcdev} ...; run ramargs; print bootargs; bootz ${loadaddr} ${rdaddr} ${fdtaddr}; fi
```

**EMMC启动和NFS启动**：uboot启动阶段进入控制台，设置环境变量

```
MYIR># setenv fdtfile myd_c437x_evm_hdmi.dtb
MYIR># saveenv
Saving Environment to FAT...
Card did not respond to voltage select!
** Bad device mmc 0 **
MYIR># reset
```

* 上述启动方式的设备树文件修改完毕后，上电进入到开发板，将之前编译好的modetest应用程序拷贝到开发板的/usr/bin目录下

* 把HDMI连接线一端插入到开发板的J9 \(HDMI\)接口，另一端接入到显示器的HDMI接口，执行应用程序



