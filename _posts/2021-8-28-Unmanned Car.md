---
layout: post
title: Unmanned Car
header-img: "img/Car.png"
tag:
- Learning
- Car
description: "学习心得 / 摘要笔记"
---

# 视频网站
## 相机测试
<blockquote>
  【1】<a href="http://www.qishunwang.net/news_show_98775.aspx" target="_blank">树莓派SCI相机调试和预览</a>
</blockquote>
云台测试
<blockquote>
  【2】<a href="https://v.qq.com/x/page/r0980pdhsf3.html" target="_blank">https://v.qq.com/x/page/r0980pdhsf3.html</a>
</blockquote>
<blockquote>
  【3】<a href="https://v.qq.com/x/page/u3078hssi5t.html" target="_blank">https://v.qq.com/x/page/u3078hssi5t.html</a>
</blockquote>
<blockquote>
  【4】<a href="https://v.qq.com/x/page/d3078xilzcl.html" target="_blank">https://v.qq.com/x/page/d3078xilzcl.html</a>
</blockquote>
<blockquote>
  【5】<a href="https://v.qq.com/x/page/k30785ohvei.html" target="_blank">https://v.qq.com/x/page/k30785ohvei.html</a>
</blockquote>

Jetson Nano相关教程
<blockquote>
  【1】<a href="https://v.qq.com/x/page/r0976psa45v.html" target="_blank">Python基础</a>
</blockquote>
<blockquote>
  【2】<a href="https://v.qq.com/x/page/h0976jywrnp.html" target="_blank">Python例子</a>
</blockquote>
<blockquote>
  【3】<a href="https://v.qq.com/x/page/o0976lp7flp.html" target="_blank">MORE例子</a>
</blockquote>
<blockquote>
  【4】<a href="https://v.qq.com/x/page/g0976fq6dbu.html" target="_blank">安装Python IDE环境:Visual Studio Code</a>
</blockquote>
<blockquote>
  【5】<a href="https://v.qq.com/x/page/y0976gsafqv.html" target="_blank">使用Matplotlib，Pyplot和Numpy</a>
</blockquote>
<blockquote>
  【6】<a href="https://v.qq.com/x/page/a0976qism1a.html" target="_blank">在python3上安装openCV 3.3.1</a>
</blockquote>
<blockquote>
  【7】<a href="https://v.qq.com/x/page/n09771eog0u.html" target="_blank">在openCV中运行树莓派相机SCI</a>
</blockquote>

# 教程
## 云台搭建
### 【1】连线
- 掏出祖传PCA9685驱动模块
-  <table>
      <tr>
          <th bgcolor="lightblue">GND → GND</th>
          <th bgcolor="lightblue">V+ → 5v</th>
          <th bgcolor="lightblue">VCC → 3v3</th>
          <th bgcolor="lightblue">SDA → PIN3</th>
          <th bgcolor="lightblue">SCL → PIN5</th>
      </tr>
- 舵机接线按照颜色连接
- 图例
- ![2df88331ab43f385256fcabf0299bfb](https://user-images.githubusercontent.com/61528011/133183040-69301710-968d-47a5-9d78-89730e1ba98b.jpg)
- ![f1af1a0157996b8f7538b30eead5be3](https://user-images.githubusercontent.com/61528011/133183076-43f703d8-d47c-4a3d-b4fb-9ae029c866c0.jpg)
### 【2】环境配置

- ```cpp
sudo apt-get install python3-pip
sudo pip3 install adafruit-circuitpython-servokit
#这里开始没啥大用
sudo usermod -aG i2c pjm
sudo groupadd -f -r gpio
sudo usermod -a -G gpio pjm
#一般都装好了，可以略过
sudo cp /opt/nivida/jetson-gpio/etc/99-gpio.rules /etc/udev/rules.d/
sudo udevadm control --reload-rules && sudo udevadm trigger
sudo reboot now
```

### 【3】代码测试
 ```cpp
import cv2
print(cv2.__version__)
import numpy as np
from adafruit_servokit import ServoKit
kit=ServoKit(channels=16)

pan=45
tilt=135
kit.servo[0].angle=pan
kit.servo[1].angle=tilt

def nothing(x):
    pass

cv2.namedWindow('Trackbars')
cv2.moveWindow('Trackbars',980,0)

cv2.createTrackbar('hueLower', 'Trackbars',96,179,nothing)
cv2.createTrackbar('hueUpper', 'Trackbars',120,179,nothing)

cv2.createTrackbar('hue2Lower', 'Trackbars',50,179,nothing)
cv2.createTrackbar('hue2Upper', 'Trackbars',0,179,nothing)

cv2.createTrackbar('satLow', 'Trackbars',157,255,nothing)
cv2.createTrackbar('satHigh', 'Trackbars',255,255,nothing)
cv2.createTrackbar('valLow','Trackbars',100,255,nothing)
cv2.createTrackbar('valHigh','Trackbars',255,255,nothing)


dispW=1280
dispH=960
flip=2
#Uncomment These next Two Line for Pi Camera
#camSet='nvarguscamerasrc !  video/x-raw(memory:NVMM), width=3264, height=2464, format=NV12, framerate=21/1 ! nvvidconv flip-method='+str(flip)+' ! video/x-raw, width='+str(dispW)+', height='+str(dispH)+', format=BGRx ! videoconvert ! video/x-raw, format=BGR ! appsink'
#cam= cv2.VideoCapture(camSet)

#Or, if you have a WEB cam, uncomment the next line
#(If it does not work, try setting to '1' instead of '0')
cam = cv2.VideoCapture(0)
width=cam.get(cv2.CAP_PROP_FRAME_WIDTH)
height=cam.get(cv2.CAP_PROP_FRAME_HEIGHT)
print('width:',width,'height:',height)
while True:   
    ret, frame = cam.read()
    #frame=cv2.imread('smarties.png')
    hsv=cv2.cvtColor(frame,cv2.COLOR_BGR2HSV)

    hueLow=cv2.getTrackbarPos('hueLower', 'Trackbars')
    hueUp=cv2.getTrackbarPos('hueUpper', 'Trackbars')

    hue2Low=cv2.getTrackbarPos('hue2Lower', 'Trackbars')
    hue2Up=cv2.getTrackbarPos('hue2Upper', 'Trackbars')

    Ls=cv2.getTrackbarPos('satLow', 'Trackbars')
    Us=cv2.getTrackbarPos('satHigh', 'Trackbars')

    Lv=cv2.getTrackbarPos('valLow', 'Trackbars')
    Uv=cv2.getTrackbarPos('valHigh', 'Trackbars')

    l_b=np.array([hueLow,Ls,Lv])
    u_b=np.array([hueUp,Us,Uv])

    l_b2=np.array([hue2Low,Ls,Lv])
    u_b2=np.array([hue2Up,Us,Uv])

    FGmask=cv2.inRange(hsv,l_b,u_b)
    FGmask2=cv2.inRange(hsv,l_b2,u_b2)
    FGmaskComp=cv2.add(FGmask,FGmask2)
    cv2.imshow('FGmaskComp',FGmaskComp)
    cv2.moveWindow('FGmaskComp',0,530)

    contours,_=cv2.findContours(FGmaskComp,cv2.RETR_EXTERNAL,cv2.CHAIN_APPROX_SIMPLE)
    contours=sorted(contours,key=lambda x:cv2.contourArea(x),reverse=True)
    for cnt in contours:
        area=cv2.contourArea(cnt)
        (x,y,w,h)=cv2.boundingRect(cnt)
        if area>=50:
            #cv2.drawContours(frame,[cnt],0,(255,0,0),3)
            cv2.rectangle(frame,(x,y),(x+w,y+h),(255,0,0),3)
            objX=x+w/2
            objY=y+h/2
            errorPan=objX-width/2
            errorTilt=objY-height/2
            if abs(errorPan)>15:
                pan=pan-errorPan/75
            if abs(errorTilt)>15:
                tilt=tilt-errorTilt/75


            if pan>180:
                pan=180
                print("Pan Out of  Range")   
            if pan<0:
                pan=0
                print("Pan Out of  Range")
            if tilt>180:
                tilt=180
                print("Tilt Out of  Range")
            if tilt<0:
                tilt=0
                print("Tilt Out of  Range")                 

            kit.servo[0].angle=pan
            kit.servo[1].angle=tilt
            break        

    cv2.imshow('nanoCam',frame)
    cv2.moveWindow('nanoCam',0,0)


    if cv2.waitKey(1)==ord('q'):
        break
cam.release()
cv2.destroyAllWindows()
```
