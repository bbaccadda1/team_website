---
layout: post
comments: true
title:  大疆机甲大师教育机器人Python API中文化之四五：云台灯效，指定序号
description: 
date:   2019-10-23 00:00:00 -0700
categories: Python 机甲大师
---

续上文[大疆机甲大师教育机器人Python API中文化之三：底盘灯效](https://zhuanlan.zhihu.com/p/87999734)。
### 视频演示

（友情提示：有背景音请关音箱）
[在线视频演示](https://v.qq.com/x/page/v3018qp331c.html)

这个API只能指定云台一边或两边的LED按照某种模式进行亮暗。
### 例程主体

完整代码仍[开源在此](https://github.com/program-in-chinese/robomaster-python-samples-zh/blob/master/Python%20API%E8%A7%86%E9%A2%91%E6%BC%94%E7%A4%BA%E4%B8%8E%E4%BE%8B%E7%A8%8B/LED%E7%81%AF/%E4%BA%91%E5%8F%B0%E7%81%AF.py)，这里仅贴出主要部分。
```python
def start():
    云台灯(常量.云台所有, 白色, 常量.效果常亮)
    时间.睡眠(2)
    云台灯(常量.云台所有, 黑色, 常量.效果熄灭)
    时间.睡眠(1)
    云台灯(常量.云台左, 绿色, 常量.效果呼吸)
    云台灯(常量.云台右, 红色, 常量.效果闪烁)
    时间.睡眠(4)
    云台灯(常量.云台左, 黄色, 常量.效果走马灯)
    云台灯(常量.云台右, 蓝色, 常量.效果走马灯)
    时间.睡眠(4)

def 云台灯(位置, 颜色, 灯效):
    LED灯.云台(位置, 颜色['红'], 颜色['绿'], 颜色['蓝'], 灯效)
```
### 视频演示

这个API可以指定云台两侧可独立控制的 8 颗 LED 灯中的一个或多个进行亮灭。
[独立控制LED](https://v.qq.com/x/page/l3018kyi13z.html)
### 例程主体

视频中仅拍了左侧，1，3，5，7处LED依次点亮，然后熄灭，改为偶数序号点亮。
```python
def start():
    云台灯(常量.云台所有, 黄色, 常量.效果常亮)
    时间.睡眠(2)
    云台灯(常量.云台左, 绿色, 常量.效果熄灭)
    云台灯(常量.云台右, 红色, 常量.效果熄灭)
    for 序号 in range(1, 5):
        云台单灯(常量.云台左, 序号 * 2 - 1, 常量.效果常亮)
        云台单灯(常量.云台右, 序号 * 2, 常量.效果常亮)
        时间.睡眠(1)
    云台单灯(常量.云台左, 偶数, 常量.效果常亮)
    云台单灯(常量.云台右, 奇数, 常量.效果常亮)
    云台单灯(常量.云台左, 奇数, 常量.效果熄灭)
    云台单灯(常量.云台右, 偶数, 常量.效果熄灭)
    时间.睡眠(2)

def 云台单灯(位置, 序号, 灯效):
    LED灯.云台单灯(位置, 序号, 灯效)

def 云台灯(位置, 颜色, 灯效):
    LED灯.云台(位置, 颜色['红'], 颜色['绿'], 颜色['蓝'], 灯效)

奇数 = [1, 3, 5, 7]
偶数 = [2, 4, 6, 8]
```
### 关于API命名

之前的灯效常量命名添加了”效果“前缀。也将之前的”装甲左顶“改为了”云台左“。

“云台单灯”其实有些不妥，因为它的中间参数可以是列表用于指定多个灯。也许“云台指定灯”？
### 关于API设计

前一个API中，“闪烁”和“走马灯”效果应该可以由后一个API结合“常亮”和“熄灭”效果完成。感觉API的层次有些交错。