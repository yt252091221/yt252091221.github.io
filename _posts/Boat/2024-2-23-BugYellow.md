---
layout: post
title: 大黄使用手册
header-img: "img/bigyellow/back.png"
tag:
- 无人船
- 船舶控制
description: "操作手册"
---

# 1. 文件说明：

mdk526.exe【编译器 keil MDK 5.26 版本】

keygen_new(2032).zip【MDK 破解软件 2032 内置文件会被识别为病毒，请允许在设备上】

peazip-8.1.0.WIN64.exe【压缩/解压缩软件】

Keil.STM32F1xx_DFP.1.1.0.pack【MDK 控制器底层文件】

Keil.STM32F4xx_DFP.2.2.0.pack【MDK 控制器底层文件】

ANO_TC 匿名上位机 V6.72.exe【A 版控制器(红色)上位机软件】

匿名上位机 V7.exe【B 版控制器(白色)对应上位机软件】

JLink_Windows_V688b.exe【下载器驱动】

编队.zip【含定向箭形编队信息表格以及对应的十艘无人艇各自独立的程序】

编队避障.zip【含巡航箭形编队避障详细信息表格及对应的十艘无人艇各自独立的程序】

传感器校准程序.zip【含 A&B 版控制器在校准传感器之前需要下载的程序，校准后需恢复】

电机配置文件.zip【含十艘无人艇各自的电机驱动文件，根据调试需要和硬件改动实时更新】

匿名拓空者 Pro 飞控手册 20181114.pdf【A 版控制器供应商官方说明书】

匿名--拓空者 P2 手册 V1.02.pdf【B 版控制器供应商官方说明书】

GISEMI_ConfigTool 2.1.12.exe【Lora 通讯设备配置软件】

CP210x_Universal_Windows_Driver.zip【CP2102 驱动，用于 USB 转 TTL 模块】

u-center_v20.10.exe【GNSS 模块配置软件，用于配置互联基站】

匿名传感器校准.exe【B 版控制器传感器校准程序】

# 2. 安装压缩软件：

双击 peazip-8.1.0.WIN64.exe 安装压缩软件

安装过程有修改部分截图——

![1](/img/bigyellow/P1.png)

# 3. 文件解压步骤：

点击待解压文件，右键后执行下图——

![1](/img/bigyellow/P2.png)

# 4. 安装编译软件：

双击 mdk526.exe

安装过程有修改部分截图——

![1](/img/bigyellow/P3.png)

返回桌面管理员方式打开 Keil uVision5

菜单栏 file-License Management；复制 CID

解压 keygen_new(2032).zip

![1](/img/bigyellow/P4.png)

打开文件夹 keygen_new(2032)，双击 keygen_new2032.exe，设置如下

![1](/img/bigyellow/P5.png)

粘贴 CID 至对应位置

点击 Generate，复制生成的序列号

返回 Keil uVision5 界面，粘贴至 LIC，点击 Add LIC

双击 Keil.STM32F1xx_DFP.1.1.0.pack

双击 Keil.STM32F4xx_DFP.2.2.0.pack

# 5. 安装下载器驱动：

双击 JLink_Windows_V688b.exe

# 6. 安装 CP2102 驱动：

解压缩 CP210x_Universal_Windows_Driver.zip

双击 CP210xVCPInstaller_x64.exe

# 7. 安装 GNSS 模块配置软件：

双击 u-center_v20.10.exe

固定任务栏

# 8. 控制系统软件介绍：

某特定实验目的的程序文件夹中，*号船的控制程序压缩包命名为 num * rtk 版 v5.zip，
ANO_Pioneer.uvprojx 为程序入口，其他关键性功能文件在 SRC 文件夹中，也可进入程序界
面左侧点击查看/修改，如下图

![1](/img/bigyellow/P6.png)

自上至下分别为路径栏、菜单栏、功能区 1、功能区 2、程序目录&编辑窗口、编译&查找结
果；功能区 2 中，左 2 功能键为快速编译，左 3 功能键为完全编译，左 6 功能键为下载；原有编译基础上进行单文件的简单修改可使用快速编译，其余情况使用完全编译；编译成功后
需在保证控制器供电、仿真器连接至控制软件开发平台 USB 端且控制器和仿真器已相互连
接的情况下点击下载功能键将程序烧录进控制器中

# 9. 遥控器接收机介绍：

用于无线接收遥控器的指令信号并通过 S.Bus 协议转发给控制器，同时作为辅助的 5V 电源
为其他设备或模块供电或实现共地

![1](/img/bigyellow/P7.png)

黄色框——接收机指示灯，绿色常亮-遥控器信号正常，红色慢闪-遥控器无信号或弱信号，应检查遥控器与接收机距离、空间遮挡、电源电压等情况，确保遥控器正常工作

红色框——S.Bus 功能端，自上至下分别为信号端、电源端、地线端同一排的端口功能类型相同，在其他任意列分别选取电源和地线可辅助作为 5V 电源

# 10. 接收机数据线介绍：

用于连接控制器和遥控器接收机，通过接收机向控制器发送控制器的指令信号

![1](/img/bigyellow/P8.png)

黄色框——3-Pin 防反接母口端子线

红色框——3-Pin 母口杜邦线

# 11. 螺旋天线介绍：

用于接收GNSS信号，竖直固定在船体左后方，据船体中心水平偏移为长轴42cm，短轴13cm，由天线延长线连接至 RTK 模块对应功能端

# 12. 棒状天线介绍：

用于 433 基站 Lora 的天线棒，向临近的其他 433Lora 广播发送基站数据

# 13. 遥控器介绍：

用于设置指令信号通过无线传输给接收机进而发送给控制器

![1](/img/bigyellow/P9.png)

红色框——电源开关，关闭状态下长按至屏幕亮起后开启遥控器，开启状态下长按至屏幕熄灭后关闭遥控器

黄色框——界面退出键，室外屏幕自动弱光下难以看清，该键可用于开启屏幕以辨识遥控器是否开启，并检查电池电压状态、RSSI 状态

蓝色框——三档辅助开关 AUX2，动作【中上 1s 中】校准坐标系东向，动作【中下 1s 中】校准坐标系原点

绿色框——三档辅助开关 AUX3，仅影响自动控制行为，动作【中】定点于航线起始点，标准航向为巡航方向，动作【上】启动巡航至终点定点且标准航向为巡航方向，动作【下】启动返航，从终点至起始点定点且标准航向为返航方向

灰色框——三档辅助开关 AUX4，动作【中】电机无动作，动作【上】进入手动控制模式，动作【下】进入自动控制模式

红黄复合框——控制摇杆，仅影响手动控制模式，上/下动作控制前/反向推进，左/右动作控制逆/顺时针转向速度由两节 18650 锂电池供电，出现松动情况需重新安装电池
