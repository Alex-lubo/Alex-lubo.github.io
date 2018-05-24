---
title: windows下MD5计算
date: 2018-02-21 22:22:22
tags:
---

linux下计算文件的ｍｄ５值很方便，直接命令行内输入: md5sum　file　即可，那么windows下呢．其实windows下也可以直接利用自带的小工具来进行md5值的计算 - certutil.exe

certutil工具是一个命令行工具,可以用这个小工具来存储\展示CA配置信息, 配置证书服务, 备份和还原CA组件,并且合一验证证书\秘钥对\和证书链.

计算md5值的方法:
```
certutil -hashfile file MD5
certutil -hashfile file SHA1
certutil -hashfile file SHA256
```