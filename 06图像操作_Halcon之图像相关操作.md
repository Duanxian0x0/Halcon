## Halcon之图像相关操作

> 图像数组的索引是从1开始的
> 元组数组的索引是从0开始的

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/809f72b4b1a9449ca87842d3d8c04982.png)


```
* 打开图像选择弹窗
dev_open_file_dialog ('read_image', 'default', 'default', Selection)

* 根据路径读取图像
read_image (Image, Selection)

* rgb转灰度图
rgb1_to_gray (Image, GrayImage)

* 分解三通道
decompose3 (Image, R, G, B)

* RGB转HSV
trans_from_rgb (R,G, B, H, S, V, 'hsv')


* 获取图像宽
get_image_size (GrayImage, Width, Height)


* 生成单通道空图像
gen_image_const (Image1, 'byte', Width, Height)

*生成某固定值的单通道图像
gen_image_proto (Image1, ImageOut, 125)
gen_image_proto (Image1, ImageOut2, 255)


* 获取灰度图的灰度值
get_grayval (GrayImage, [0,1], [0,1], Grayval)

* 修改灰度值(单像素修改)
set_grayval (ImageOut, 1, 1, 255)
```

