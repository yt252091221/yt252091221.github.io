---
layout: post
title: TEST
header-img: "img/tag-bg.jpg"
tag:
- 王政
description: "A test post -- Taking the least squares code as an example"
---

这是一个测试用例，包括图片使用，哔哩哔哩视频嵌入


<blockquote>
这是一个引用标签
</blockquote>
<blockquote>
今天高考结束了
</blockquote>

<blockquote>
Latex公式嵌入 → $$W (s_{i}leftarrow s_{j})=frac{1}{1+exp[ (p_{i}-p_{j})/K]}$$
</blockquote>

## 这是一个表格

<table>
    <tr>
        <th bgcolor="#3879e7">表头1</th>
        <th bgcolor="#3879e7">表头2</th>
        <th bgcolor="#3879e7">表头3</th>
        <th bgcolor="#3879e7">表头4</th>
    </tr>
    <tr>
        <td bgcolor="white">11</td>
        <td >21</td>
        <td>31</td>
        <td >41</td>
    </tr>
    <tr>
        <td bgcolor="lightblue">22</td>
        <td bgcolor="lightblue">32</td>
        <td bgcolor="lightblue">42</td>
        <td bgcolor=" #add8e6">46</td>
    </tr>
    <tr>
        <td bgcolor="white">11</td>
        <td >21</td>
        <td>31</td>
        <td >41</td>
    </tr>
    <tr>
        <td bgcolor="lightblue">22</td>
        <td bgcolor="lightblue">32</td>
        <td bgcolor="lightblue">42</td>
        <td bgcolor=" #add8e6">46</td>
    </tr>
</table>


## Welcome to GitHub Pages

### Markdown

```cpp
Syntax highlighted code block

# 这是一个标题
# 最小二乘法
import numpy as np   #科学计算库
import matplotlib.pyplot as plt  #绘图库
##样本数据(Xi,Yi)，需要转换成数组(列表)形式
X=np.array([8.88,9.1,2.3,4.44,12.1,11.11,8.0,7.7,1.98,10.5])
Y=np.array([5.25,2.83,6.41,6.71,5.1,4.23,5.05,1.98,10.5,6.3])

# 初始化赋值
s1 = 0
s2 = 0
s3 = 0
s4 = 0
n = 10

# 循环累加
for i in range(n):
    s1 = s1 + X[i] * Y[i]
    s2 = s2 + X[i]
    s3 = s3 + Y[i]
    s4 = s4 + X[i] * X[i]

# 计算斜率和截距
# w2 = (s1-(s2*s3)/n)/(s4-(s2*s2)/n)
w = (n*s1-s2*s3)/(n*s4-s2*s2)
b = (s3 - w * s2) / n

print(w,b)

plt.scatter(X,Y)
x=np.linspace(0,12,100)
y=w*x+b ##函数式
plt.plot(x,y)
plt.show()
```

### A vedio from Bilibili
<iframe height="400" width="600" src="//player.bilibili.com/player.html?aid=974361420&bvid=BV1b44y1m7sT&cid=374406761&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>

<iframe src="//player.bilibili.com/player.html?aid=444505923&bvid=BV1tL411i7cC&cid=1157052210&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>
