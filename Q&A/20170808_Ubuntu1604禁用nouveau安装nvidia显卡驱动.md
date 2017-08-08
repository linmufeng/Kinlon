# 前言

昨天懵逼了一整天，以前一直都是很好装的Ubuntu16.04，昨天几乎用了一整天也没有安装好，终于熬了个夜安好了，现在安装了基本的软件后，就开始把过程记录下来。

## 问题描述

具体情况是，在安装完Ubuntu16.04后，无法进入系统，报错信息为：
1. nouveau 0000:01:00.0: bus: MMIO read of 00000000 FAULT at 022554
2. nmi watchdog bug soft lockup

从Ubuntu16.04的高级选项中偶尔可以进入系统，但是不可重现，重装了10余次，强制了无数次，很绝望。

## 参考资料

查看了大多数资料，终于弄明白了，是因为Ubuntu系统中第三方的开源显卡的原因，解决方案就是禁用第三方开源显卡nouveau，然后重新安装NVIDIA的闭源显卡驱动。参考资料主要是以下四篇，均有写小瑕疵，具体步骤可以参考后面的具体的操作。
1. [Ubuntu 16.04 + Nvidia 显卡驱动 + Cuda 8.0 （问题总结 + 解决方案）](http://blog.csdn.net/zafir_410/article/details/73188228?utm_source=itdadao&utm_medium=referral)
2. [Ubuntu 16.04 安装英伟达（Nvidia）显卡驱动](https://gist.github.com/dangbiao1991/7825db1d17df9231f4101f034ecd5a2b)
3. [Ubuntu 16.04 禁用 nouveau 安装 nvidia显卡驱动](https://silenceu.me/ubuntu/disablenoveau.html)
4. [Ubuntu 16.04安装Nvidia显卡驱动](http://www.2cto.com/kf/201702/594221.html)
 
## 错误方法：
打开系统设置->选择软件&更新->选择额外驱动->将N卡驱动设置为某个官方驱动：
![img](http://www.2cto.com/uploadfile/Collfiles/20170128/201701282225121284.png)

如上图所示，在这里直接更改驱动会出现很严重的问题。所以不推荐这种方法。下面介绍正确的方法。
 

# 具体步骤

## 1.查看合适的显卡驱动版本
最重要的就是你首先知道你该装那个版本的驱动，不然即便你的方法对了，那么也是徒劳的，方式如下：
```
sudo apt-cache search nvidia*
```
结果如下：
![查看显卡版本](http://img.blog.csdn.net/20170613154319917?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvWmFmaXJfNDEw/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
可以看到我的电脑最高支持nvidia-375.66的驱动。有了版本就好办了。

第一种方式，我下载了NVIDIA-Linux-x86_64-375.66.run （根据你的版本号下载）

## 2.驱动安装文件下载
找一台可用的机器，从 [NVIDIA官网](http://www.nvidia.com/Download/index.aspx?lang=cn)下载显卡对应的驱动安装文件
```
  NVIDIA-Linux-x86_64-375.66.run
```
将下载到的 *.run 文件拷贝到待安装驱动的主机

## 3.准备工作
在待安装驱动的主机上打开一个终端(Ctrl+Alt+T)，或者直接切换到终端界面(Ctrl+Alt+F1),进行如下操作
* 卸载可能存在的旧版本 nvidia 驱动（对没有安装过 nvidia 驱动的主机，这步可以省略，但推荐执行，无害）
```
$sudo apt-get remove --purge nvidia*
```
* 安装驱动可能需要的依赖(可选)
```
 $sudo apt-get update

 $sudo apt-get install dkms build-essential linux-headers-generic
```
* 把 nouveau 驱动加入黑名单(重要！！！)
```
 $sudo nano /etc/modprobe.d/blacklist-nouveau.conf

  在文件 blacklist-nouveau.conf 中加入如下内容：
  blacklist nouveau
  blacklist lbm-nouveau
  options nouveau modeset=0
  alias nouveau off
  alias lbm-nouveau off
```
* 禁用 nouveau 内核模块
```
  $echo options nouveau modeset=0 | sudo tee -a /etc/modprobe.d/nouveau-kms.conf

  $sudo update-initramfs -u
```

* 重启

## 4.运行驱动安装文件

* 检测nouveau是否成功被禁止了
在终端执行命令：
```
lsmod | grep nouveau
```
查看nouveau模块是否被加载。如果什么都没输出，则执行下一步。 

* 关闭图形系统
```
$sudo service lightdm stop
```

* 给驱动run文件赋予执行权限
```
sudo chmod a+x NVIDIA-Linux-x86_64-375.66.run
```
* Ctrl + Alt + F1 进入tty1（Ctrl + Alt + F7是回到桌面系统）出现上面的问题，根本原因在于 参数： –no-opengl-files
```
sudo /etc/init.d/lightdm stop
sudo ./NVIDIA-Linux-x86_64-375.20.run --no-opengl-files
sudo /etc/init.d/lightdm start
```
–no-opengl-files也就是不安装opengl，这里的原因有很多种，可能是因为ubuntu桌面系统是采用3D渲染的，可能是驱动冲突问题。当然还有其他几个参数，都值得你去试一试：

在安装驱动的时候，有一布问你”Would you like to run the nvidia-xconfig utility to automatically update your X configuration file…”什么的，选择 No。
```
sudo ./NVIDIA-Linux-x86_64-375.20.run --no-opengl-files –no-x-check –no-nouvea
```
–no-x-check 安装驱动时不检查X服务
–no-nouveau-check 安装驱动时不检查nouveau

* 安装完成后，启动图形系统
```
$sudo service lightdm start
```

上面的命令执行后会自动转到图形界面，因为之前Ubuntu系统集成的显卡驱动程序nouveau被禁用了，这时候可能无法显示图形界面，此时再按下Ctrl+Alt+F1进入命令行模式，输入reboot 重启计算机即可。

上述步骤做完之后，即可正常使用独立显卡了。现在可以根据自己要求打开NVIDIA X Server Settings进行设置。

* 重启电脑，没有问题，输入命令：
```
nvidia-smi
```
![nvidia-smi](http://img.blog.csdn.net/20170613161644275?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvWmFmaXJfNDEw/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

# 后记
现在是2017/08/09 04:08 am， 弄了这么久终于搞定了，非常高兴，接下来继续配环境，然后在5点半左右出去，去天安门看日出。如果对于这篇日志有任何建议和问题，欢迎和我讨论，linmufeng (at) yeah.net
