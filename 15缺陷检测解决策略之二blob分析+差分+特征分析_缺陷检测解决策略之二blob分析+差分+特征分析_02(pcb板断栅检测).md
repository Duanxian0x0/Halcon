## 缺陷检测解决策略之二blob分析+差分+特征分析_02(pcb板断栅检测)

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/7e1bbaafa70b45e5b10de6f071aae273.png)

```csharp
read_image (Image, 'pcb')

* 差分 ：带缺陷的图像 - 不带缺陷的图像 得到缺陷

* 灰度图像的膨胀和腐蚀

* 对原图进行灰度的开操作(把缺陷区域合并起来了)
gray_opening_shape (Image, ImageOpening, 7, 7, 'octagon')

* 灰度开操作
gray_closing_shape (Image, ImageClosing, 7, 7, 'octagon')

* 相减操作

dyn_threshold (ImageOpening, ImageClosing, RegionDynThresh, 80, 'dark')

area_center (RegionDynThresh, Area, Row, Column)

if(Area>200)
    dev_get_window (WindowHandle)
    disp_message (WindowHandle, 'NG', 'window', 12, 12, 'red', 'true')
endif
```

```csharp
* 读取图像
read_image (Image, 'pcb')
* 窗口设置
dev_close_window ()
* 获取图像大小
get_image_size (Image, Width, Height)
* 窗口显示设置
dev_open_window (0, 0, Width, Height, 'black', WindowHandle)
dev_display (Image)

* 灰度开运算：暗的像素点变多
gray_opening_shape (Image, ImageOpening, 7, 7, 'octagon')
* 灰度闭运算：亮的像素点变多
gray_closing_shape (Image, ImageClosing, 7, 7, 'octagon')
* 差分：动态阈值分割
dyn_threshold (ImageOpening, ImageClosing, RegionDynThresh, 75, 'not_equal')
* 结果显示
dev_display (Image)
dev_set_color ('red')
dev_set_draw ('margin')
dev_display (RegionDynThresh)

```

