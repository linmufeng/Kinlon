# 前言

昨天懵逼了一整天，以前一直都是很好装的Ubuntu16.04，昨天几乎用了一整天也没有安装好，终于熬了个夜安好了，现在安装了基本的软件后，就开始把过程记录下来。

## 问题描述

具体情况是，在安装完Ubuntu16.04后，无法进入系统，报错信息为：
1. nouveau 0000:01:00.0: bus: MMIO read of 00000000 FAULT at 022554
2. nmi watchdog bug soft lockup

从Ubuntu16.04的高级选项中偶尔可以进入系统，但是不可重现，重装了10余次，强制了无数次，很绝望。

## 参考资料

查看了大多数资料，终于弄明白了，是因为Ubuntu系统中第三方的开源显卡的原因，解决方案就是禁用第三方开源显卡nouveau，然后重新安装NVIDIA的闭源显卡驱动。参考资料主要是以下三篇，均有写小瑕疵，具体步骤可以参考后面的具体的操作。
1. [Ubuntu 16.04 + Nvidia 显卡驱动 + Cuda 8.0 （问题总结 + 解决方案）](http://blog.csdn.net/zafir_410/article/details/73188228?utm_source=itdadao&utm_medium=referral)
2. [Ubuntu 16.04 安装英伟达（Nvidia）显卡驱动](https://gist.github.com/dangbiao1991/7825db1d17df9231f4101f034ecd5a2b)
3. [Ubuntu 16.04 禁用 nouveau 安装 nvidia显卡驱动](https://silenceu.me/ubuntu/disablenoveau.html)
 
# 具体步骤
## 1.驱动安装文件下载
找一台可用的机器，从 Nvidia 官网下载显卡对应的驱动安装文件
