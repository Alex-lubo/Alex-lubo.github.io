---
title: CentOS7 upgrade glibc
date: 2017-11-16 22:53:34
tags:
---
最近电脑更新时老是更新失败，看提示信息说是升级需要安装glibc-2.17.196**之类的提示，查询之后发现需要升级个glibc。
通过以下命令查询本机已经安装版本：
`rpm -qa | grep glibc`
可以看到本机安装版本为：
>glibc-2.17-196.el7_3.1.x86_64
>glibc-devel-2.17-196.el7_3.1.x86_64
>glibc-headers-2.17-196.el7_3.1.x86_64
>glibc-common-2.17-196.el7_3.1.x86_64

得知系统中安装了 x86_64 的 glibc、glibc-common、glibc-devel、glibc-headers，并且版本是 2.17-192.el7_3，所以升级时我们要下载新版本的以上 4 种 RPM 包。接下来，可以在 ftp.redsleeve.org、rpmfind.net(x86_64) 或百毒云下载相应版本的 RPM 包，注意只能用在 el7 发行版中.
然后将安装包放置在某文件夹下，输入以下命令：
`sudo rpm -Uvh glibc-*`
即可完成更新，然后update下系统，就可以更新系统了。