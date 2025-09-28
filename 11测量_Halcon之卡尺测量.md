## Halcon之卡尺测量

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/682090d92bb142149165fb4470a669f1.png)


```csharp
read_image (Image, 'C:/Users/Jumy/Desktop/Halcon-AOI教学资料/Halcon-AOI教学资料/程序与图像/07测量/一维测量/矩形.png')

* 获取图像大小
get_image_size (Image, Width, Height)

dev_get_window (WindowHandle)
* 卡尺1（找点）
draw_rectangle2 (WindowHandle, Row, Column, Phi, Length1, Length2)
* 显示化的操作
gen_rectangle2 (Rectangle, Row, Column, Phi, Length1, Length2)

* 卡尺2（找点）
draw_rectangle2 (WindowHandle, Row1, Column1, Phi1, Length11, Length21)
gen_rectangle2 (Rectangle1, Row1, Column1, Phi1, Length11, Length21)

* 创建测量卡尺
gen_measure_rectangle2 (Row, Column, Phi, Length1, Length2, Width, Height, 'nearest_neighbor', MeasureHandle)

* 卡点
* 平滑系数
* 阈值（重要参数）
* 极性（黑到白，由白到黑，都找）negative
* 第几个（第一个或者最后一个）
* 输出：卡出点的 Y X 变换35
measure_pos (Image, MeasureHandle, 1, 50, 'all', 'first', RowEdge, ColumnEdge, Amplitude, Distance)


gen_cross_contour_xld (Cross, RowEdge, ColumnEdge, 60, Phi1)

* 测量卡尺
gen_measure_rectangle2 ( Row1, Column1, Phi1, Length11, Length21, Width, Height, 'nearest_neighbor', MeasureHandle1)

* 卡点
measure_pos (Image, MeasureHandle1, 1, 30, 'all', 'first', RowEdge1, ColumnEdge1, Amplitude1, Distance1)

gen_cross_contour_xld (Cross1, RowEdge1, ColumnEdge1, 60, Phi1)

* 显示
disp_line (WindowHandle, RowEdge, ColumnEdge,RowEdge1, ColumnEdge1)

distance_pp ( RowEdge, ColumnEdge, RowEdge1, ColumnEdge1, Distance2)
```

