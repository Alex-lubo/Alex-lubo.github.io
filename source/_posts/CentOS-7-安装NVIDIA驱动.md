---
title: CentOS 7 安装NVIDIA驱动
date: 2017-11-04 22:33:08
tags: linux
---

*centOS 7.1上安装NVIDIA显卡驱动，在网上资料很多是centOS7.0或是7.0以上的操作系统，信息过时，很难成功，后在网上资料的基础上（主要是思路），结合自己电脑上的实际，总结形成文档以记录，以便后面重装时能复用。以下是安装步骤：*

### <i class="icon-file"></i> 从ELRepo源中安装驱动

- 添加ELRepo源

    >`# rpm --import https://www.elrepo.org/RPM-GPG-KEY-elrepo.org`
    >`# rpm -Uvh http://www.elrepo.org/elrepo-release-7.0-2.el7.elrepo.noarch.rpm`
- 安装显卡检测程序
    >`# yum install nvidia-detect`
- 检测显卡型号，并选择对应的驱动
    
    >`$ nvidia-detect -v`
    >
    >*Probing for supported NVIDIA devices...[10de:06dd]
    NVIDIA Corporation GF100GL [Quadro 4000]
    This device requires the current 346.47 NVIDIA driver kmod-nvidia*
    
- 安装显卡驱动
    根据nvidia-detect的输出信息，可以知道显卡的型号，以及要使用的驱动版本
    若驱动版本为304.xx，则安装304xx版本:
    >`# yum install nvidia-x11-drv-304xx nvidia-x11-drv-304xx-32bit`

- 对于大多数比较新的显卡来说，直接安装最新版的驱动即可:
    >`# yum install nvidia-x11-drv nvidia-x11-drv-32bit`
    安装过程中可能会给出软件冲突的警告，需要卸载以下软件包及其依赖:
    >`# yum remove xorg-x11-glamor`
- 重启系统；


### <i class="icon-file"></i>手动编译显卡驱动

- 下载对应驱动，http://www.nvidia.com
- 更新系统：
    
    >`# yum update`
    >`# yum install kernel-devel kernel-headers gcc make`
    >`# yum -y upgrade kernel kernel-devel`
- 修改Blacklist，屏蔽Nouveau 

    >`# vi /lib/modprobe.d/dist-blacklist.conf`
    注释掉nvidia 
    `# nvidiab`
    加入以下文字：
    `blacklist nouveau` 
    `options nouveau modeset=0 `
    备份initramfs文件
    `# mv /boot/initramfs-$(uname -r).img/boot/initramfs-$(uname-r).img.bak`
    重建initramfs文件
    `# sudo dracut -v /boot/initramfs-$(uname -r).img $(uname -r)`
    关机重启。
    由于此时nouveau驱动已经被禁用，桌面的显示效果非常差。

- 安装
    >`# init 3`
    >`# chmod +x <Nvidia file>.run`
    >`# ./<Nvidia file>.run `
    >`# init 5`
    ok,启动成功

### 更新kernel后重装驱动
手动编译显卡驱动的一个麻烦之处在于，每次更新kernel之后，都会出现无法进入图形界面的情况，此时需要重新安装显卡驱动。
更新kernel，重启，发现无法进入图形界面； 
1. 按下Alt+F2，进入文本界面，以root身份登陆； 
2. 备份initramfs文件 
    >`# mv /boot/initramfs-$(uname -r).img/boot/initramfs-$(uname-r).img.bak`
3. 重建initramfs文件 
    >`# sudo dracut -v /boot/initramfs-$(uname -r).img $(uname -r)`
4. 重启，按下Alt+F2，进入文本界面，以root身份登陆； 
5. 执行第七步；再重启； 
6. 卸载显卡驱动
    >`#sh ./NVIDIA-Linux-xxxxx --uninstall`