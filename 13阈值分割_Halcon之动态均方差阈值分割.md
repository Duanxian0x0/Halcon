## Halcon之动态均方差阈值分割

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/08ccba7cb0a44362ab7c17632a6dc11c.png)

```csharp
read_image (Image, 'printer_chip/printer_chip_01')


var_threshold (Image, Region, 100, 100, 0.2, 55, 'light')
* var_threshold (Image, Region2, 100, 100, 0.2, 55, 'dark')
* var_threshold (Image, Region3, 100, 100, 0.2, 55, 'equal')
* var_threshold (Image, Region4, 100, 100, 0.2, 55, 'not_equal')

* var_threshold算子和dyn_threshold算子极为类似。不同的是var_threshold集成度更高，并且加入了“标准差×标准差因子”这一变量。


read_image (Image1, 'printer_chip/printer_chip_01')
mean_image (Image1, ImageMean, 100, 100)
dyn_threshold (Image1, ImageMean, RegionDynThresh, 55, 'light')
```

