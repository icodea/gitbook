---
title: 小米Pro笔记本 Hackintosh MacOS Mojave 10.14.4 安装教程
copyright: true
date: 2019-04-10 22:34:28
categories: 
 - 黑苹果
 - MacOS Mojave 10.14.4安装教程
tags:
 - 小米Pro
 - MacOS
 - Tools
 - Hackintosh
 - macOS Mojave 10.14.4
top: 149
---



### **前言**

**Mi Pro Hackintosh MacOS Mojave 10.14.4 **

![](https://raw.githubusercontent.com/WJingC/pichome/master/img/XiaoMiPro_hack.jpg)

 本文仅针对小米笔记本Pro，其他机型可作参考，需要找到对应的EFI替换。本文部分内容引用：[macOS安装教程兼小米Pro安装过程记录](https://blog.daliansky.net/MacOS-installation-tutorial-XiaoMi-Pro-installation-process-records.html) 此教程大部分准备工作在win下进行已尽量通俗易懂了。

让你的小米笔记本Pro吃上黑苹果，体验MacBook Pro 95%的功能

<!--more-->

### **电脑配置**

| 规格     | 详细信息                                      |
| -------- | --------------------------------------------- |
| 电脑型号 | 小米笔记本电脑Pro 15.6''(MX150/GTX)           |
| 处理器   | 英特尔 酷睿 i5-8250U/i7-8550U 处理器          |
| 内存     | 8GB/16GB 三星 DDR4 2400MHz                    |
| 硬盘     | 三星 NVMe固态硬盘 PM961 256GB                 |
| 集成显卡 | 英特尔 UHD 图形620                            |
| 显示器   | 京东方 NV156FHM-N61 FHD 1920x1080 (15.6 英寸) |
| 声卡     | 瑞昱 ALC298 (节点:30/99)                      |
| 网卡     | 英特尔 无线 8265                              |
| 读卡器   | 瑞昱 RTS5129                                  |



### **问题**

#### **哪些不能正常工作**

- 独立显卡，因为macOS不支持Optimus技术
  - 使用了SSDT-DDGPU来禁用它以节省电量

指纹传感器

- 使用了USBPorts来禁用它以节省电量

英特尔蓝牙只有在从Windows热重启后有效

- 阅读蓝牙解决方案

英特尔无线网卡

- 购买USB网卡或者支持的内置网卡

瑞昱USB SD读卡器

- 使用了USBPorts来禁用它以节省电量

其他都工作正常

- 在PM981上发生内核错误，无法安装
  - 目前[PM981在macOS 10.13.3+上无解](http://bbs.pcbeta.com/viewthread-1774117-1-1.html)。你可以安装macOS到另一块硬盘上。
  - PM981` 硬盘序列号以 `MZVLB` 开头，`PM961` 硬盘序列号以 `MZVLW` 开头。

#### **哪些可以工作得更好**

- 使用ALCPlugFix来修复耳机重新插拔后无声
- 使用DVMT_and_0xE2_fix来把帧缓存设为64mb并解锁CFG
- 使用one-key-hidpi来提升系统UI质量
- 使用one-key-cpufriend来提升CPU性能 



### **下载文件及工具**

- 一个容量8G及以上的U盘
- MacOS镜像：
  - 最新镜像：[【黑果小兵】MacOS Mojave 10.14.4 18E226 正式版 with Clover 4903原版镜像](https://blog.daliansky.net/macOS-Mojave-10.14.4-18E226-official-version-with-Clover-4903-original-image.html)
- Windows下工具：
  - [BalenaEtcher](https://www.balena.io/etcher/):把MacOS镜像写进U盘
  - [WinMD5](http://www.winmd5.com/):验证镜像文件的MD5值。
  - DiskGenius：对硬盘分区修改
  - [Bootice](https://bootice.en.softonic.com/)：添加UEFI引导选项
  - [微PE](http://www.wepe.com.cn/download.html)：用于硬盘ESP分区扩容 


- Win工具下载地址：
  - 百度云：



- **小米Pro专用EFI持续长期更新**

  - https://github.com/daliansky/XiaoMi-Pro

    

### **开始**

#### **制作安装镜像**

- ###### 镜像制作：

  - 使用WinMd5验证下载的MacOS的MD5值，

  - MD5 (macOS Mojave 10.14.4(18E226) Installer with Clover 4903.dmg) = ee923768b29194efc704bcf34d7f9fd8

  - 下载[etcher](https://etcher.io/)，打开镜像，选择U盘，点击Flash即可。

    [![etcher](https://7.daliansky.net/etcher.png)](https://7.daliansky.net/etcher.png)

#### **复制EFI**

- **将镜像里的EFI复制到USB安装盘的EFI分区下**
  - 解压下载的EFI文件，选择EFI用快捷键ctrl+c 复制 ![img](http://attach.bbs.miui.com/forum/201801/27/204216mmfpk4r4l04mbk0v.png)

- 打开DiskGenius分区软件删除U盘的EFI目录

![img](http://attach.bbs.miui.com/forum/201801/27/204507zdggagiusdgtii06.png)

- 在DiskGenius分区软件下用快捷键ctrl+v 把新的EFI复制进去

![img](http://attach.bbs.miui.com/forum/201801/27/204740dccqyfqmmyyda2cc.png)

- **复制EFI到硬盘的EFI分区**

![img](http://attach.bbs.miui.com/forum/201801/28/190106s4di3zdc4wdxn0yy.png)

- 操作完成后目录结构应与下图一致，此处注意Microsoft为windows引导.如何想安装Win10和MacOS双系统，里面文件不要动，否则windows无法引导。

![img](http://attach.bbs.miui.com/forum/201801/28/190200e8t8y1jbhy1udt26.png)



#### **制作PE**

- 在DiskGenius分区软件下操作，右键点击U盘空闲区域建立新分区

![img](http://attach.bbs.miui.com/forum/201801/30/224138jf0a0ff3l0ut7cyl.png)

- 分区格式为FAT32，大小为500M，确定并保存设置

  ![img](http://attach.bbs.miui.com/forum/201801/30/224138s0jp1mttufjcjuts.png)

- ***注意要安装PE不影响之前做好的macOS安装盘需要把U盘其他分区的盘符隐藏，仅保留刚才创建的500M分区盘符*** ![img](https://attach.bbs.miui.com/forum/201803/11/140020o5mkstd8vqk2n4h6.png.thumb.jpg)

![img](https://attach.bbs.miui.com/forum/201803/11/140020g6yq5ot6xqt67t67.png.thumb.jpg) 

- 打开微PE工具箱安装程序选择安装方式为安装到U盘

![img](https://attach.bbs.miui.com/forum/201801/30/224139ayuoweg7zg0xwwhe.png.thumb.jpg)

- 如下图安装方式选择**方式三UEFI全能单分区**，**取消勾选格式化**，然后点击安装进U盘

![img](https://attach.bbs.miui.com/forum/201801/30/224223u0t8ogv8a99q01rv.png.thumb.jpg)



#### **添加UEFI引导选项**

- 使用工具:BOOTICE

- 操作过程:
  - 打开BOOTICE软件,选择物理磁盘,选择欲操作的目标磁盘,点击分区管理,弹出分区管理的窗口,点击分配盘符,为ESP分区分配一个盘符,点击确定 

    ![img](https://attach.bbs.miui.com/forum/201801/27/205346z63bgb3l6395t9l9.jpg.thumb.jpg)

  - 选择UEFI,点击修改启动序列,点击添加按钮,菜单标题填写:CLOVER,选择启动文件,在打开的窗口里选择ESP分区下的目录\EFI\CLOVER\CLOVERX64.EFI,选中CLOVER上移到最上面，点击保存当前启动项设置

  ![img](https://attach.bbs.miui.com/forum/201801/27/205347wxgog7z54t01g74x.jpg.thumb.jpg)

	​	

#### **小米BIOS设置**

​    ***小米笔记本的BIOS默认开启了安全认证,UEFI引导需要关闭安全启动Secure Boot Mode方式,否则无法加载UEFI引导设备,比如刚制作好的macOS安装USB盘***

- 操作方法:

  - 开机按F2进入BIOS设置,光标移动到Security,点击**Set Supervisor Password**设置一个**BIOS密码**,输入两次相同的密码,点击YES保存。`***这个BIOS密码非常重要，一定要设置成常用的密码并且备份，我现在就是忘了BIOS密码进不去BIOS，走售后只能换主板，价格3000+。 欲哭无泪中。。千万记得备份密码***。`

    ![img](https://attach.bbs.miui.com/forum/201801/27/211600vkepe765lkee5t3r.png.thumb.jpg)

  - 关闭安全启动，点击Secure Boot Mode,设置为Disabled关闭安全启动

    ![img](https://attach.bbs.miui.com/forum/201801/27/211601iqzpjqp9wp2zvtj5.png.thumb.jpg)

  - 按F10保存设置并退出BIOS

  

  #### **PE下扩容ESP(EFI)分区**

  MacOS系统要求EFI为**200M**以上，否则安装时无法抹盘 

- **操作方法**1：插上U盘开机按F12键进入Boot Manager引导管理,选择 EFI USB Device1,回车 进入PE ![img](https://attach.bbs.miui.com/forum/201801/30/230915mwprblesk5mpf9pz.png.thumb.jpg)

- 使用分区助手（无损）删除MSR分区

  ![img](https://attach.bbs.miui.com/forum/201801/30/230916qb0zxava2zcfuxyl.png.thumb.jpg)

- 选中ESP(EFI)分区 右键选择调整移动分区

  ![img](https://attach.bbs.miui.com/forum/201801/30/230918kjojjp2gm3junjqp.png.thumb.jpg)

- 如图拉到最右边，扩容efi分区，确定并提交更改。

  ![img](https://attach.bbs.miui.com/forum/201801/30/230919dqshbiynduf0awi0.png.thumb.jpg)



#### **安装macOS**

- 开机按F12键进入Boot Manager引导管理,选择 EFI USB Device,回车进入macOS安装程序


![XiaoMi-Bios5](https://7.daliansky.net/XiaoMi-Bios5.png)	

![XiaoMiCloverboot](https://7.daliansky.net/XiaoMiCloverboot.png)



- 移动光标到`Boot OS X Install from XiaoMiPro 10131`回车

##### 安装第一阶段

###### 开始引导macOS系统

![ParallelsPicture](https://7.daliansky.net/ParallelsPicture.png)

​	这个过程需要1-2分钟,耐心等待进入安装程序,出现语言选择界面.

###### 语言选择

- 选择简体中文

![ParallelsPicture1](https://7.daliansky.net/ParallelsPicture1.png)

- 出现`macOS实用工具`界面,选择磁盘工具

###### 磁盘工具

![ParallelsPicture0](https://7.daliansky.net/ParallelsPicture0.png) 

- 选择`显示所有设备`:

![ParallelsPicture2](https://7.daliansky.net/ParallelsPicture2.png) 

- 选择`SSD Media`,点击`抹掉`按钮,选择默认的`Mac OS扩展(日志型)`,将名称修改为`Macintosh HD`,点击`抹掉`按钮.(PS:会抹掉磁盘上所有内容，之前记得备份)

![ParallelsPicture6](https://7.daliansky.net/ParallelsPicture6.png)

- 抹盘成功后,它会自动生成一个200MB的EFI分区,这样做的好处是不会出现安装过程中的由于EFI分区尺寸小于200MB而引起的无法安装的错误.当然前提是你的磁盘中没有重要的数据,或者您的数据已经提前备份过了.

![ParallelsPicture7](https://7.daliansky.net/ParallelsPicture7.png) 

- 到这里,磁盘工具的动作就已经结束了.按左上角x ,退出磁盘工具进入安装界面,进行系统的安装了.

###### 安装macOS

![ParallelsPicture8](https://7.daliansky.net/ParallelsPicture8.png) 

- 进入安装界面

![ParallelsPicture11](https://7.daliansky.net/ParallelsPicture11.png) 

- 选择继续，同意

![ParallelsPicture12](https://7.daliansky.net/ParallelsPicture12.png) 

 ![ParallelsPicture13](https://7.daliansky.net/ParallelsPicture13.png) 

- 选择刚刚抹掉的分区，安装

![img](https://attach.bbs.miui.com/forum/201801/27/213036j6czeg36qi67d6wc.png.thumb.jpg)

 ![img](https://attach.bbs.miui.com/forum/201801/27/213037kns73php6dllyjjp.png.thumb.jpg)

- 会自动重启数次。

![ParallelsPicture16](https://7.daliansky.net/ParallelsPicture16.png) 



##### **安装第二阶段**

- 第二阶段的安装会有两种界面,一种是不进安装界面直接安装,另一种是先进入安装界面直接安装,需要注意的是,无论是哪一种界面下,安装的过程中全程是禁用鼠标和键盘的,需要你做的只是耐心等待它安装完成



- 安装速度取决于你的磁盘速度,第二阶段的安装已经与USB安装盘没什么关系了.macOS 10.13的安装完全区别于10.12,它不能免二次安装,甚至还需要重启多次,这些都是正常现象,请不要大惊小怪的.
- 系统安装完成后,重启进入系统设置向导



##### **设置向导**

###### 国家选择

- 首先让你选择国家,这里我选择中国

![Installer3](https://7.daliansky.net/Installer3.png) 

 ![Installer4](https://7.daliansky.net/Installer4.png) 

- 点击`继续`,设置键盘。

###### 设置键盘

![Installer5](https://7.daliansky.net/Installer5.png) 

- 这里询问你`是否传输信息到这台Mac`

###### 数据恢复

- 可以从Mac或者Time Machine备份恢复

![Installer6](https://7.daliansky.net/Installer6.png) 

- 我选择`现在不传输任何信息`,点击`继续`

###### Apple ID登录

![Installer7](https://7.daliansky.net/Installer7.png) 

- 选择是否`使用您的Apple ID登录`,如何想现在就登录到`Apple ID`,请输入`Apple ID`和密码登录,否则选择`不登录`,进入系统后也可以设置登录到`iCloud`,点击`继续`

![Installer8](https://7.daliansky.net/Installer8.png) 

- 阅读条款与条件,选择`同意`继续

![Installer9](https://7.daliansky.net/Installer9.png) 



- 出现`创建电脑用户`的窗口,输入用户名和密码,点击`继续`

###### 创建电脑用户

![Installer10](https://7.daliansky.net/Installer10.png) 

- 用户创建成功后,弹出`iCloud`的`正在设置用户`的窗口

![Installer12](https://7.daliansky.net/Installer12.png) 

- 紧接着弹出设置`iClound钥匙串`的设置窗口,选择`稍候设置`,点击`继续`

- 出现`正在设置您的Mac`,请稍候完成设置向导

###### 设置向导完成

![Installer18](https://7.daliansky.net/Installer18.png) 

- 出现桌面后,整个的安装向导就完成了. 

![Installer19](https://7.daliansky.net/Installer19.png)



##### 安装后的系统设置

- 系统安装后,你可以先喝杯咖啡兴奋会儿,马上还有更艰巨的任务在等着你呢

###### MacOS:教你将U盘上的EFI复制到磁盘的EFI分区,脱离USB运行

- `diskutil`命令的基本用法：
  - 查看磁盘分区表

```
diskutil list
```

​	/dev/disk0(internal, physical):

| #:   | TYPE                  | NAME  | SIZE     | IDENTIFIER |
| ---- | --------------------- | ----- | -------- | ---------- |
| 0:   | GUID_partition_scheme |       | 256 GB   | disk0      |
| 1:   | EFI                   | EFI   | 200 MB   | disk0s1    |
| 2:   | Apple_HFS             | MAC   | 128 GB   | disk0s2    |
| 3:   | Microsoft Basic Data  | WIN10 | 127.7 GB | disk0s3    |

​	/dev/disk1(internal, physical):

| #:   | TYPE                  | NAME                 | SIZE    | IDENTIFIER |
| ---- | --------------------- | -------------------- | ------- | ---------- |
| 0:   | GUID_partition_scheme |                      | 16 GB   | disk1      |
| 1:   | EFI                   | EFI                  | 200 MB  | disk1s1    |
| 2:   | Apple_HFS             | Install macOS Sierra | 15.8 GB | disk1s2    |

- 挂载磁盘EFI分区

```
sudo diskutil mount disk0s1
```

- 挂载U盘EFI分区

```
sudo diskutil mount disk1s1
```

- 打开Finder，注意后面有个`.`

```
open .
```

- 左侧会显示挂载了两个EFI分区，将U盘EFI目录全部复制到磁盘的EFI分区即可。



###### 完善驱动

USB网卡推荐：  `COMFAST CF-WU810N`

驱动下载：[COMFAST CF-WU810N 网卡驱动.zip](http://www.miui.com/forum.php?mod=attachment&aid=Mjk4OTEwNTB8OTVhNmMxYWN8MTU1NTQyMjAxOXwyMjQ5MzY2NTA4fDExMzYzNjcy) 



###### 重建缓存

-  [重建缓存 Kext Utility.zip](http://www.miui.com/forum.php?mod=attachment&aid=Mjk4OTEwNTJ8MDYzM2E5ODZ8MTU1NTQyMjAxOXwyMjQ5MzY2NTA4fDExMzYzNjcy) 
- 使用Kext Utility 重建缓存然后重启就可以正常使用了。



### 参考链接

- [macOS安装教程兼小米Pro安装过程记录](https://blog.daliansky.net/MacOS-installation-tutorial-XiaoMi-Pro-installation-process-records.html)


- [【老司机引路】小米笔记本pro Win10+黑苹果macOS 10.13.6双系统](http://www.miui.com/thread-11363672-1-1.html)	 


- [Hackintosh Your Xiaomi NoteBook Pro](https://github.com/daliansky/XiaoMi-Pro/wiki/%E4%B8%BB%E9%A1%B5)

- [XiaoMi NoteBook Pro for macOS Mojave & High Sierra](https://github.com/daliansky/XiaoMi-Pro/blob/master/README.md)

 