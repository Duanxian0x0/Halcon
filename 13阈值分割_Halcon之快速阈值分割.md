## Halcon之快速阈值分割

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/e5ff2ce0ec4d464a83648bc6e255848d.png)

```csharp
read_image (Image, 'printer_chip/printer_chip_01')
fast_threshold (Image, Region, 0, 50, 120)


* 对应参数分别为输入图像、输出区域、阈值下限、阈值上限、处理参数（MinSize）
* 在理解MinSize的时候，可以直接理解为就是一个加速参数，把图像分成两块来分别处理，MinSize决定了两块的比例；
* MinSize越大，速度越快
```

```csharp
* 快速阈值分割 加速

read_image (Image, 'printer_chip/printer_chip_01')

* 输入图像
* 输出分割区域
* 阈值下限
* 阈值上限
* minSize 越大，我们速度越快
fast_threshold (Image, Region, 128, 255, 2)

* 16

* 40  24

* 50 10

* 59 9

* 75 16

* 85 10

* 113 28

* 131 18 
```

