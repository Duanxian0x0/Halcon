## Halcon之阈值分割-Threshold


![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/f14db4751abd450baf590cf659efae14.png)

```csharp

read_image (Image1, 'printer_chip/printer_chip_01')
* 输入：Image1,  128 ,255
* 输出：region
threshold (Image1, Region, 128, 255)
```

