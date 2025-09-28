## Halcon之动态阈值分割

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/29a19a60eae74ee0a2a3afeeede7dda3.png)


```csharp
read_image (Image, 'printer_chip/printer_chip_01')

* 均值滤波后的图像
mean_image (Image, ImageMean, 100, 100)

* 获取比滤波图（+5）更亮的区域
dyn_threshold (Image, ImageMean, RegionDynThresh, 5, 'light')

* 获取比滤波图（-5）更黑的区域
dyn_threshold (Image, ImageMean, RegionDynThresh1, 5, 'dark')

* 获取在滤波图（-5 +5）中间的区域
dyn_threshold (Image, ImageMean, RegionDynThresh2, 5, 'equal')

* 获取在滤波图（-25 +25）非中间的区域（同时获取，更亮和更黑的区域）
dyn_threshold (Image, ImageMean, RegionDynThresh2, 35, 'not_equal')
```

