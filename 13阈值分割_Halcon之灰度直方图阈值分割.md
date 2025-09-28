## Halcon之灰度直方图阈值分割

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/e2e80e9d1edd4d298d035318ab686bba.png)

```csharp
read_image (Image, 'printer_chip/printer_chip_01')
* 自动分割出所有区域（多个区域）
auto_threshold (Image, Regions, 30)

* 对应参数分别为输入图像、输出区域、高斯参数
* 利用直方图确定的阈值分割图像，通过多阈值分割单通道图像，会返回多个区域；
* 首先输入图像的绝对直方图的灰度值是确定的，然后高斯平滑（Sigma）后从直方图中提取相关的最小值

* Sigma越大，提取的区域就越小（高斯系数，越大，分割出区域种类越少）
```

