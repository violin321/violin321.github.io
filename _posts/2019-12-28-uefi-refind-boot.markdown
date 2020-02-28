---
layout: post
title: rEFInd一个好用又好看的UEFI启动管理器
date: 2019-12-28 20:00:00 +0800
img: /assets/img/rEFInd.jpg
tags: [MAC,野生技术协会]
categories: 分享
---

使用rEFInd引导多系统启动 Windows 10、macOS 10.15.2（基于Open Core）、其他

> 软件版本：refind-bin-0.11.4
>
> 官网：http://www.rodsbooks.com/refind/
>
> 下载地址：https://sourceforge.net/projects/refind/
>
> 官方安装教程：http://www.rodsbooks.com/refind/installing.html 
>
>主题：<a href="https://evanpurkhiser.com/rEFInd-minimal/" target="_blank">rEFInd Minimal</a>  、<a href="https://github.com/kgoettler/ursamajor-rEFInd" target="_blank">ursamajor-rEFInd</a>
> 
> 主题标签： https://github.com/topics/refind-theme
>
> 项目地址：https://github.com/techysy/rEFInd

+ **EFI** 文件夹结构图

![EFI]({{site.baseurl}}/assets/img/refind/EFI.jpg) 

### 项目文件说明

+ EFI     配置好的rEFInd 自用引导OC和WIN 使用minimal主题

+ refind-bin-0.11.4   官方版本 驱动和安装脚本

+ Screenshot   我的EFI 文件目录结构图

+ Themes    主题文件夹可自行放置在rEFInd目录下   

### 安装rEFInd

![refind]({{site.baseurl}}/assets/img/refind/refind.jpg)

+ 自动安装

    使用终端执行 **refind-install** 系统自动把文件拷贝到EFI文件夹下 并建立启动项，如果不认盘可能需要手动复制驱动文件 **drivers_x64** 如果存在多个EFI 引导盘 不确定回安装在哪一个

![drivers]({{site.baseurl}}/assets/img/refind/drivers.jpg)
+ 手动安装

    建议使用手动安装的方式，拷贝refind-bin-0.11.4下的refind到EFI目录（可使用Hackintool工具、mount命令挂载）下删掉多余的驱动比如aa64 arm处理器和其他32位处理器的驱动 只保留 **drivers_x64** 。最后使用BCD、EasyUEFI工具添加UEFI启动项。

![uefi]({{site.baseurl}}/assets/img/refind/UEFI.jpg)    

### 配置 refind.conf

#### 注释或者删掉之前的代码，参考refind.conf-sample 去调整 

#### 我的配置：

    #选择等待时间
    
    timeout 3

    #屏蔽自动搜索Windows启动项和其他启动项文件夹
     
    dont_scan_files \EFI\Microsoft\Boot\bootmgfw.efi 
        
    dont_scan_dirs /EFI/BOOT,/EFI/Microsoft,/EFI/OC

    #手动添加启动项并设置图标

    menuentry "Windows 10" {
         
    volume "Windows 10"
         
    icon \EFI\refind\themes\rEFInd-minimal\icons\os_win.png
         
    loader \EFI\Microsoft\Boot\bootmgfw.efi 
         
    }
         
    menuentry "macOS 10.15.2" {
         
    volume "macOS boot"
         
    icon \EFI\refind\themes\rEFInd-minimal\icons\os_mac.png
         
    loader \EFI\OC\OpenCore.efi 
    }

    #设置主题 我使用的minimal，也可以在GitHub找到其他的主题
    
    #放在 EFI/rEFInd/themes目录下 并指向主题配置文件

    include themes\rEFInd-minimal\theme.conf 

    #include themes\ursamajor-rEFInd\theme.conf


