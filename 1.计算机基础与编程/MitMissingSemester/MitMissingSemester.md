# Shell

1. 终端是能显示shell的窗口



## 常用命令

```bash
date

echo hello
echo "Hello world"
echo Hello\ world # 使用转义字符
echo $PATH

which echo

pwd

cd /home # change dir
cd ~
cd -

ls
ls .. 
ls -a # flag, 标志
ls -C=WHEN # option, 选项

mv oldPath newPath # 移动或重命名

cp from to

rm 
rm -f # 递归删除

rmdir # 删除空目录

mkdir

man ls

# Ctrl + L == clear

sudo cmd

# tee 命令将输入内容写入一个文件, 同时将它的输出到标准输出
echo 20000 | sudo tee brightness

# 以合适的程序打开文件
xdg-open test.md
```



## 环境变量

1. shell通过环境变量确定程序的位置: PATH, 用冒号分隔的列表
2. 环境变量是启动shell时设置的东西



## 路径

1. 路径是一种描述计算机上文件位置的方式。
2. linux上所有内容都属于（挂载）根命名空间; 所有绝对路径都以 `/` 开头；在windows上，每个分区都有一个根目录（每个驱动器都有一个单独的路径结构）。 
3. 所有相对路径都是相对于当前工作目录而言的。
4. `.` / `..`: 表示当前目录和上一层目录
5. 运行程序时，默认会在当前工作目录下进行操作。



## 权限

```bash
ls -alh
# 类型
#      文件所有者权限
#                   同用户组的用户权限 
#                                   其他人权限表
# r:读/w:写/x:执行/-:没有权限
```

- 读：
    1. 查看文件内容
    2. 列出目录所包含的内容
- 写：
    1. 修改文件或保存
    2. 移动/删除目录所拥有的文件
- 执行：
    1. 目录：搜索目录所包含的文件，搜索权限，是否被允许进入该目录



## 流

1. 每个程序都有两个主要的流：输入流和输出流
2. 默认情况下，输入流来自于键盘（终端），输出流也是指向终端
3. shell提供了重定向这些流的方法，以改变程序的输入/输出指向。
    1. `<`: 表示将这个程序的输入重定向为这个文件的内容
    2. `>`: 表示将前面程序的输出重定向到这个文件中

```sh
# < file > file
echo hello > hello.txt
cat < hello.txt
cat < hello.txt > hello2.txt # 将hello.txt作为cat的输入,并输出到hello2.txt
```



4. `>>`: 追加
5. `|`: 管道符，将左边程序的输出作为右边程序的输入

```sh
ls -l | tail -n1
ls -l | tail -n1 > ls.txt

# 访问http头
% curl --head --silent google.com
HTTP/1.1 301 Moved Permanently
Location: http://www.google.com/
Content-Type: text/html; charset=UTF-8
Content-Security-Policy-Report-Only: object-src 'none';base-uri 'self';script-src 'nonce-hXdtc1db_5M9CRvQRhBVxg' 'strict-dynamic' 'report-sample' 'unsafe-eval' 'unsafe-inline' https: http:;report-uri https://csp.withgoogle.com/csp/gws/other-hp
Date: Wed, 26 Nov 2025 13:23:25 GMT
Expires: Fri, 26 Dec 2025 13:23:25 GMT
Cache-Control: public, max-age=2592000
Server: gws
Content-Length: 219
X-XSS-Protection: 0
X-Frame-Options: SAMEORIGIN

% curl --head --silent google.com | grep -i content-length
Content-Length: 219

# 设置空格为分隔符，并打印第二个字符
% curl --head --silent google.com | grep -i content-length | cut --delimiter=' ' -f2
219
```



## 内核

1. /sys 的文件实际上不是计算机上的文件，而是各种内核参数
2. 可以通过文件来访问各种内核参数
    1. /sys/class 表示有许多不同的类型的设备可以与之交互，或者可以访问各种队列或各种内部奇怪的旋钮
    2. /sys/class/backlight 可以配置电脑的背光灯

```sh
ls -alh /sys
总计 0
dr-xr-xr-x  13 root root   0 11月19日 20:54 .
drwxr-xr-x   1 root root 122 10月20日 19:53 ..
drwxr-xr-x   2 root root   0 11月19日 20:54 block
drwxr-xr-x  48 root root   0 11月19日 20:54 bus
drwxr-xr-x  75 root root   0 11月19日 20:54 class
drwxr-xr-x   4 root root   0 11月19日 20:54 dev
drwxr-xr-x  29 root root   0 11月19日 20:54 devices
drwxr-xr-x   6 root root   0 11月19日 20:54 firmware
drwxr-xr-x  10 root root   0 11月19日 20:54 fs
drwxr-xr-x   2 root root   0 11月26日 21:06 hypervisor
drwxr-xr-x  18 root root   0 11月19日 20:54 kernel
drwxr-xr-x 294 root root   0 11月19日 20:54 module
drwxr-xr-x   3 root root   0 11月19日 20:54 power

ls  /sys/class                                       
accel       devcoredump     firmware       leds      nvme-generic      rc           spi_slave     wakeup
ata_device  devfreq         graphics       lirc      nvme-subsystem    regulator    thermal       watchdog
ata_link    devfreq-event   hidraw         mei       pci_bus           remoteproc   tpm           wmi_bus
ata_port    devlink         hwmon          mem       phy               rfkill       tpmrm
backlight   dma             i2c-dev        misc      platform-profile  rtc          tty
bdi         dma_heap        ieee80211      mmc_host  powercap          scsi_device  usbmisc
block       dmi             input          msr       power_supply      scsi_disk    vc
bluetooth   drm             intel_pmt      mtd       pps               scsi_host    video4linux
bsg         drm_dp_aux_dev  intel_scu_ipc  net       ptp               sound        virtio-ports
cpuid       extcon          iommu          nvme      pwm               spi_master   vtconsole



```



```
                                       
accel       devcoredump     firmware       leds      nvme-generic      rc           spi_slave     wakeup
ata_device  devfreq         graphics       lirc      nvme-subsystem    regulator    thermal       watchdog
ata_link    devfreq-event   hidraw         mei       pci_bus           remoteproc   tpm           wmi_bus
ata_port    devlink         hwmon          mem       phy               rfkill       tpmrm
backlight   dma             i2c-dev        misc      platform-profile  rtc          tty
bdi         dma_heap        ieee80211      mmc_host  powercap          scsi_device  usbmisc
block       dmi             input          msr       power_supply      scsi_disk    vc
bluetooth   drm             intel_pmt      mtd       pps               scsi_host    video4linux
bsg         drm_dp_aux_dev  intel_scu_ipc  net       ptp               sound        virtio-ports
cpuid       extcon          iommu          nvme      pwm               spi_master   vtconsole

```

