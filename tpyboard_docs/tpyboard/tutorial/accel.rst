加速度传感器
=================

通过本篇教程你将学到如何读取加速度计的信号并能通过左右倾斜开发板改变LED灯的状态。

加速度传感器的使用
-----------------------

开发板上有一个能够检测角度和运动状态的加速度传感器（小封装小模块）。
X ,Y, Z 轴上各有不同的传感器。
通过创建一个 pyb.Accel()对象和调用 x() 方法可以获取加速度传感器的数值。
::
    >>> accel = pyb.Accel()
    >>> accel.x()
    7

上述例子返回-30 到 30 之间的带符号的角度值。
注意其测量结果不算精准，这意味着即使保持 TPYBoard 的完全静止不动依旧会有测量数据出现。
因此，x() 方法得到的数据不能当成精确值使用，而应视其为一定精度的范围值。

倾斜开发板，通过加速度传感器点亮 LED 灯的 代码如下所示：
::
    accel = pyb.Accel()
    light = pyb.LED(3)
    SENSITIVITY = 3

    while True:
        x = accel.x()
        if abs(x) > SENSITIVITY:
            light.on()
        else:
            light.off()

        pyb.delay(100)

上述代码中我们创建了 Accel 和 LED 两个对象，然后直接获得加速度传感器在 X 方向上的数值。
如果 x 值的大小比定值 SENSITIVITY 大，LED 灯将被点亮，否则将被熄灭。
循环中调用了 pyb.delay() 函数，否则当 x 的值接近 SENSITIVITY 时LED灯将闪烁得十分频繁。
尝试在 TPYBoard 开发板上运行该程序，直到左右倾斜开发板使 LED 灯亮或灭。

**练习：改变上述脚本使得倾斜的角度越大蓝色的LED灯越亮。**
**提示： 你需要重新调整数值，其大小在 0 到 255 之间**

制作水平仪
---------------------

上面的例子只使用了 x 方向上的角度值，然而我们可以通过 y()函数的值和更多的LED灯将 TPYBoard 开发板打造成一个水平仪。
::
    xlights = (pyb.LED(2), pyb.LED(3))
    ylights = (pyb.LED(1), pyb.LED(4))

    accel = pyb.Accel()
    SENSITIVITY = 3

    while True:
        x = accel.x()
        if x > SENSITIVITY:
            xlights[0].on()
            xlights[1].off()
        elif x < -SENSITIVITY:
            xlights[1].on()
            xlights[0].off()
        else:
            xlights[0].off()
            xlights[1].off()

        y = accel.y()
        if y > SENSITIVITY:
            ylights[0].on()
            ylights[1].off()
        elif y < -SENSITIVITY:
            ylights[1].on()
            ylights[0].off()
        else:
            ylights[0].off()
            ylights[1].off()

        pyb.delay(100)

一开始我们创建了 一个包含 x 和 y 方向上的 LED 对象的元组。
python 语言中元组是不可更改的对象，这意味着一旦创建后就不能被改变。
然后我们像上个例程开始的那样，但当 x 的值为正或为负时分别点亮不同的 LED 灯。
y 方向上也是同样的原理。 这看起来不算很复杂但确实实现了水平仪的功能。
在你的TPYBoard 板上运行这个程序，现象为：向不同方向倾斜开发板点亮不同的 LED 灯。
