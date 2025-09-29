---
title: x200刷coreboot从入门到入土
date: 2025-09-29 20:37:09
---
### 前言
- Thinkpad X200(7458E69型)在多篇文章已经拥有记载，不过文章统一性不一样，在一部分文章中，我将一些刷机，购买，编译等步骤进行了整理，希望能对大家有所帮助。
### 刷机前准备
> [!CAUTION]
> 刷机前请对机器价值衡量，如机器对你的价值很高这篇文章你就可以关闭了，因为可能会产生无法预料的后果。
- 准备工作：
- 购买SOP-16转DIP-8夹，CH341A土豪金编程器（3.3v版），一台Linux电脑（尽量不要使用自己的thinkpad电脑，因为编译一个组件作者就已经花费了大量时间，而且会导致电脑卡顿）一台Thinkpad X200 优良的网络环境（可以访问Google，Github等）
### 链接编程器
- X200的BIOS芯片是25型，拥有16根引脚，可以直接通过SOP-16夹具来连接编程器。夹具和编程器都可在淘宝购买到。我手上的X200为高配版本，芯片容量为8.0 MB。
#### 1）卸下键盘与掌托
X200的BIOS隐藏在掌托下，需要卸下键盘与掌托才能看到BIOS芯片。

- 卸掉笔记本底部除硬盘仓外所有的螺丝（还有一颗螺丝藏在电池仓，需要先卸下电池），然后小心沿着边缘拆下掌托，即可同时将键盘和掌托卸下。注意不要弄坏键盘底部的排线，有的X200是带有指纹传感器的（高配版），它与主板的接口是一块可拆卸的芯片，轻轻将它从主板取出。
#### 2）准备编程器
- 编程器右侧有一个黑色的连接器。先把连接器右侧的手柄向上推，解锁母座，将夹具的8个针脚插入连接器左半边的8个孔位。注意区分方向，不要接反，最后，将手柄向下压，即可将夹具固定住，这样编程器与夹具就连接上了。
#### 3）夹上BIOS芯片
- X200的BIOS芯片位于主板下沿、Intel南桥芯片左侧，上面应写着25L开头的字样。将夹具夹在在BIOS芯片上，先让连着红色导线的夹板立在芯片右侧（对着南桥芯片那一侧），仔细对准芯片的针脚。接着缓缓将夹子夹在芯片上，确保芯片两侧的针脚都被夹住。一旦夹牢靠，就不容易松动。
接好之后，将编程器插在电脑的USB接口上，确保红灯亮起。
#### 4）备份bios
- 为了防止出现意外，最好备份一下原来的bios，而且coreboot也需要BIOS文件才能正常编译。
- 首先运行以下命令，尝试读取BIOS芯片，检测型号，借此检查连接情况：
```
sudo flashrom -p ch341a_spi
```
若你夹紧并且没有反的话，执行结果如下。可以看到，编程器已经识别出了芯片：
```
flashrom v1.2 on Linux 5.15.65-1-MANJARO (x86_64)
flashrom is free software, get the source code at https://flashrom.org
Using clock_gettime for delay loops (clk_id: 1, resolution: 1ns).
Found Macronix flash chip "MX25L6405" (8192 kB, SPI) on ch341a_spi.
Found Macronix flash chip "MX25L6405D" (8192 kB, SPI) on ch341a_spi.
Found Macronix flash chip "MX25L6406E/MX25L6408E" (8192 kB, SPI) on ch341a_spi.
Found Macronix flash chip "MX25L6436E/MX25L6445E/MX25L6465E/MX25L6473E/MX25L6473F" (8192 kB, SPI) on ch341a_spi.

Multiple flash chip definitions match the detected chip(s): "MX25L6405", "MX25L6405D", "MX25L6406E/MX25L6408E", "MX25L6436E/MX25L6445E/MX25L6465E/MX25L6473E/MX25L6473F"
Please specify which chip definition to use with the -c <chipname> option.
```
- 备份bios文件：
```
sudo flashrom -p ch341a_spi -r x200_bios.rom
```
- 若备份成功，会在当前目录下生成名为x200_bios.rom的BIOS文件。
### 刷机
- 额，由于时间问题，其实这篇文章没有写完，之后会再次更新。