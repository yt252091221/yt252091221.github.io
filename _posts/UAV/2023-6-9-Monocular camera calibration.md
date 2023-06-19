---
layout: post
title: 单目相机标定
header-img: "img/camera.png"
tag:
- Calibration
- Car
- UAV
- 单目相机
- 王政
description: "单目相机标定"
---

# 快速更改快门速度和亮度

## 【1】文件下载

- 链接：https://pan.baidu.com/s/1BNWxhJADlhBflLgCeudbvQ
  提取码：8888

## 【2】步骤

- 下载程序![1](/post_img/1.jpg )

- 烧录程序![1](/post_img/2.jpg )

- 验证成功![1](/post_img/3.jpg)

# 相机标定

## 【1】安装USB相机驱动

- 需提前有一个工作空间

```cpp
cd ~/catkin_ws/src
git clone git://github.com/bosch-ros-pkg/usb_cam.git usb_cam
cd ..
catkin_make -DCATKIN_WHITELIST_PACKAGES=usb_cam
```

## 【2】更改USB摄像头编号

- 修改```<param name="video_device" value="/dev/video0" />```的参数
- 使用```ls /dev/video*```查看摄像头端口

```cpp
cd ~/catkin_ws/src/usb_cam/launch
sudo gedit usb_cam-test.launch
```

## 【3】启动摄像头

```cpp
roslaunch usb_cam usb_cam-test.launch
```

## 【4】相机内参标定

- 安装camera_calibration包

```cpp
sudo apt install ros-melodic-camera-calibration
```

- 通过```rostopic list```确认```/usb_cam/image_raw```已经发布

- 运行标定程序

```cpp
rosrun camera_calibration cameracalibrator.py --size 8x8 --square 0.025 image:=/usb_cam/image_raw camer:=/usb_cam
```
<blockquote>
此命令运行标定结点的python脚本，其中 ： <br>
（1）--size 8x8 为当前标定板的大小(内部角点，棋盘格是9x9的）<br>
（2）--square 0.025为每个棋盘格的边长 <br>
（3）image:=/usb_cam/image_raw标定当前订阅图像来源自名为/usb_cam/image_raw的topic <br>
（4）camera:=/usb_cam为摄像机名 <br>
</blockquote>

- 开始标定

<blockquote>
为了达到良好的标定效果，需要在摄像机周围移动标定板，并完成以下基本需求：<br>
（1）移动标定板到画面的最左、右，最上、下方 <br>
（2）移动标定板到视野的最近和最远处 <br>
（3）移动标定板使其充满整个画面 <br>
（4）保持标定板倾斜状态并使其移动到画面的最左、右，最上、下方 <br>
</blockquote>

- 保存并获取标定结果

当```calibration```按钮亮起时，代表已经有足够的数据进行摄像头的标定，此时请按下```calibration```并等待一分钟左右。点击```save```储存标定结果。通过给出路径查看结果。
