## Halcon之二值化阈值分割
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/fe32f2774e5a4748a92c47ee00d53e94.png)

```csharp
read_image (Image, 'printer_chip/printer_chip_01')

binary_threshold (Image, Region1, 'max_separability', 'dark', UsedThreshold)

binary_threshold (Image, Region2, 'max_separability', 'light', UsedThreshold)
*对应参数分别为输入图像、输出区域、使用方法、选择前景还是背景、输出使用的阈值

*使用二值阈值分割图像，binary_threshold使用自动确定的全局阈值分割单通道图像，并在region中返回分割后的区域，分割所使用的阈值由method中给出的方法确定，非常适用于在均匀照明的背景下分割字符；
*目前，method总共提供两种方法：'max_separability' 和 'smooth_histo'，这两种方法都只能用于具有双峰直方图的图像：
*'smooth_histo' 方法和bin_threshold的功能一致，可以参考bin_threshold的分割过程；
*'max_separability' 方法是靠UsedThreshold来决定最小阈值的，速度比'smooth_histo'要快，对于干扰也不是很敏感；


```

