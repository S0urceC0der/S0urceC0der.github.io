
title:  Mac下Wine运行心灵终结Mental Omega 3.3
date: 2017-10-21 17:53:12
tags:
---

---

# Mac下Wine运行心灵终结Mental Omega 3.3

## 简介

红色警戒(Command & Conquer: Red Alert 2)是经典的游戏，心灵终结是基于尤里的复仇的一个Mod，被称为"几乎完美的尤里的复仇"，2016年底该Mod发布3.3版本。

## 安装Wine

请安装开发版本的Wine以便获得更好的兼容性

```
brew install wine --devel
```

## 安装Mental Omega 3.3

请安装3.3.2版本，最早的MentalOmega 3.3.0版本的clientxna.exe无法正常启动，需要后续更新才行，该问题可以参考(https://github.com/CnCNet/xna-cncnet-client/issues/29)

选择一个心灵终结的懒人包，解压之后运行

```
winetricks xna40
```

会自动安装依赖.net40和xna40，安装完毕后如果直接运行`wine MentalOmegaClient.exe`仍然会提示缺少.NET组件，但是可以通过`wine Resources/clientxna.exe`来开启程序绕过检查。

![Wine](/img/wine-MentalOmega.png)

## 其他优化

关于Wine的字体优化，可以参考[WINE界面与字体美化全攻略，及我的常用WINE程序截图展示](https://www.lulinux.com/archives/362)

## 目前遇到的Bug

- 1.会经常性的在命令行窗口弹出错误信息：

>  winedevice.exe(18227,0xb0004000) malloc: *** error for object 0x40203bb2: pointer being freed was not allocated

- 2.在长时间运行红警后，会出现闪退现象