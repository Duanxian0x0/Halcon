## Halcon之卡尺进行弧形测量

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/34a24aafbf5e45098dc518273860a29f.png)

```csharp
read_image (Image, 'zeiss1')

* 获取图像的大小

get_image_size (Image, Width, Height)

* 创建
dev_get_window (WindowHandle)
draw_circle (WindowHandle, Row, Column, Radius)

* 圆心 
Y := 275
X :=335
* 半径
r := 107
* 开始角度
startAngle :=rad(55)
* 角度范围
AngleExt :=rad(170)

** 显示
* 获取椭圆的一点
get_points_ellipse (startAngle+AngleExt, Y, X, 0, Radius, Radius, RowPoint, ColPoint)

disp_arc (WindowHandle, Y, X, AngleExt, RowPoint, ColPoint)


* 测量句柄
gen_measure_arc (Y, X, r, startAngle, AngleExt, 10, Width, Height, 'nearest_neighbor', MeasureHandle)


* 测量
measure_pos (Image, MeasureHandle, 1, 10, 'all', 'all', RowEdge, ColumnEdge, Amplitude, Distance)

gen_cross_contour_xld (Cross, RowEdge, ColumnEdge, 6, 0.785398)


disp_line (WindowHandle, RowEdge[0], ColumnEdge[0], RowEdge[1], ColumnEdge[1])

distance_pp (RowEdge[0], ColumnEdge[0], RowEdge[1], ColumnEdge[1], Distance1)
```

```csharp
*  Example for the application of the measure package
* including a lot of visualization operators
* 
read_image (Zeiss1, 'zeiss1')
get_image_size (Zeiss1, Width, Height)
dev_close_window ()
dev_open_window (0, 0, Width / 2, Height / 2, 'black', WindowHandle)
set_display_font (WindowHandle, 14, 'mono', 'true', 'false')
dev_display (Zeiss1)
disp_continue_message (WindowHandle, 'black', 'true')
stop ()
* draw_circle (WindowHandle, Row, Column, Radius)
Row := 275
Column := 335
Radius := 107
AngleStart := -rad(55)
AngleExtent := rad(170)
dev_set_draw ('fill')
dev_set_color ('green')
dev_set_line_width (1)
get_points_ellipse (AngleStart + AngleExtent, Row, Column, 0, Radius, Radius, RowPoint, ColPoint)
disp_arc (WindowHandle, Row, Column, AngleExtent, RowPoint, ColPoint)
dev_set_line_width (3)
gen_measure_arc (Row, Column, Radius, AngleStart, AngleExtent, 10, Width, Height, 'nearest_neighbor', MeasureHandle)
disp_continue_message (WindowHandle, 'black', 'true')
stop ()
count_seconds (Seconds1)
n := 10
for i := 1 to n by 1
    measure_pos (Zeiss1, MeasureHandle, 1, 10, 'all', 'all', RowEdge, ColumnEdge, Amplitude, Distance)
endfor
count_seconds (Seconds2)
Time := (Seconds2 - Seconds1) / n
disp_continue_message (WindowHandle, 'black', 'true')
* stop ()
distance_pp (RowEdge[1], ColumnEdge[1], RowEdge[2], ColumnEdge[2], IntermedDist)
* dev_display (Zeiss1)
dev_set_color ('red')
* disp_circle (WindowHandle, RowEdge, ColumnEdge, RowEdge - RowEdge + 1)
disp_line (WindowHandle, RowEdge[1], ColumnEdge[1], RowEdge[2], ColumnEdge[2])
dev_set_color ('yellow')
disp_message (WindowHandle, 'Distance: ' + IntermedDist, 'image', 250, 80, 'yellow', 'false')
* dump_window (WindowHandle, 'tiff_rgb', 'C:\\Temp\\zeiss_result')
close_measure (MeasureHandle)
dev_set_line_width (1)
* disp_continue_message (WindowHandle, 'black', 'true')
stop ()
dev_clear_window ()

```

