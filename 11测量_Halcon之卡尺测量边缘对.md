## Halcon之卡尺测量边缘对

## measure_pairs

```csharp
read_image (Image, 'fuse')

* 获取图像的大小
get_image_size (Image, Width, Height)

* 创建测量句柄
dev_get_window (WindowHandle)
draw_rectangle2 (WindowHandle, Row, Column, Phi, Length1, Length2)
gen_rectangle2 (Rectangle, Row, Column, Phi, Length1, Length2)

* 创建测量卡尺
gen_measure_rectangle2 (Row, Column, Phi, Length1, Length2, Width, Height, 'nearest_neighbor', MeasureHandle)


* 测量边缘对
* 平滑系数
* 过度阈值（核心常用）
* 极性：由黑到白，还是由白都黑
* 选取第一个点(满足要求的第一个点、最后一个点，所有点；最强点)
* 输出：第一队边缘对 Y X 振幅
* 第二队的 Y X 振幅
* 间距：20 17
* 两个边缘对的距离：63
measure_pairs (Image, MeasureHandle, 1, 30, 'all', 'all', RowEdgeFirst, ColumnEdgeFirst, AmplitudeFirst, RowEdgeSecond, ColumnEdgeSecond, AmplitudeSecond, IntraDistance, InterDistance)
dev_clear_window ()
gen_cross_contour_xld (Cross, RowEdgeFirst, ColumnEdgeFirst, 16, Phi)

gen_cross_contour_xld (Cross1, RowEdgeSecond, ColumnEdgeSecond, 16, Phi)

measure_pos (Image, MeasureHandle, 1, 30, 'all', 'all', RowEdge, ColumnEdge, Amplitude, Distance)
```

```csharp
* 保险丝测量
* 
dev_update_window ('off')
dev_close_window ()
* ****
* 获取图像
* ****
read_image (Fuse, 'fuse')
get_image_size (Fuse, Width, Height)
dev_open_window_fit_image (Fuse, 0, 0, Width, Height, WindowID)
set_display_font (WindowID, 12, 'mono', 'true', 'false')
dev_set_draw ('margin')
dev_set_line_width (3)
dev_display (Fuse)
set_display_font (WindowID, 12, 'mono', 'true', 'false')
disp_continue_message (WindowID, 'black', 'true')
stop ()
* ****
* 创建测量句柄
* ****
* -> specify ROI
Row := 297
Column := 545
Length1 := 80
Length2 := 10
Angle := rad(90)
gen_rectangle2 (ROI, Row, Column, Angle, Length1, Length2)
* 创建测量句柄
gen_measure_rectangle2 (Row, Column, Angle, Length1, Length2, Width, Height, 'bilinear', MeasureHandle)
dev_display (ROI)
disp_continue_message (WindowID, 'black', 'true')
stop ()
* ****
* 测量边缘对
* ****
measure_pairs (Fuse, MeasureHandle, 1, 1, 'negative', 'all', RowEdgeFirst, ColumnEdgeFirst, AmplitudeFirst, RowEdgeSecond, ColumnEdgeSecond, AmplitudeSecond, IntraDistance, InterDistance)
disp_continue_message (WindowID, 'black', 'true')



gen_cross_contour_xld (Cross, RowEdgeFirst, ColumnEdgeFirst, 6, 0.785398)
gen_cross_contour_xld (Cross, RowEdgeSecond, ColumnEdgeSecond, 6, 0.785398)

```

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/6909dc479bb44a61bc4698c90c786663.png)
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/541c97ca1e56458b83cf728331513a20.png)

```csharp
* fuse.hdev: measuring the width of a fuse wire
* 
dev_update_window ('off')
dev_close_window ()
* ****
* step: acquire image
* ****
read_image (Fuse, 'fuse')
get_image_size (Fuse, Width, Height)
dev_open_window_fit_image (Fuse, 0, 0, Width, Height, WindowID)
set_display_font (WindowID, 12, 'mono', 'true', 'false')
dev_set_draw ('margin')
dev_set_line_width (3)
dev_display (Fuse)
set_display_font (WindowID, 12, 'mono', 'true', 'false')
disp_continue_message (WindowID, 'black', 'true')
stop ()
* ****
* step: create measure object
* ****
* -> specify ROI
Row := 297
Column := 545
Length1 := 80
Length2 := 10
Angle := rad(90)
gen_rectangle2 (ROI, Row, Column, Angle, Length1, Length2)
* -> create measure object（创建测量矩形）
gen_measure_rectangle2 (Row, Column, Angle, Length1, Length2, Width, Height, 'bilinear', MeasureHandle)
dev_display (ROI)
disp_continue_message (WindowID, 'black', 'true')
stop ()
* ****
* step: measure
* ****
measure_pairs (Fuse, MeasureHandle, 1, 1, 'negative', 'all', RowEdgeFirst, ColumnEdgeFirst, AmplitudeFirst, RowEdgeSecond, ColumnEdgeSecond, AmplitudeSecond, IntraDistance, InterDistance)
disp_continue_message (WindowID, 'black', 'true')
stop ()
* ****
* step: visualize results
* ****
for i := 0 to |RowEdgeFirst| - 1 by 1
    gen_contour_polygon_xld (EdgeFirst, [-sin(Angle + rad(90)) * Length2 + RowEdgeFirst[i],-sin(Angle - rad(90)) * Length2 + RowEdgeFirst[i]], [cos(Angle + rad(90)) * Length2 + ColumnEdgeFirst[i],cos(Angle - rad(90)) * Length2 + ColumnEdgeFirst[i]])
    gen_contour_polygon_xld (EdgeSecond, [-sin(Angle + rad(90)) * Length2 + RowEdgeSecond[i],-sin(Angle - rad(90)) * Length2 + RowEdgeSecond[i]], [cos(Angle + rad(90)) * Length2 + ColumnEdgeSecond[i],cos(Angle - rad(90)) * Length2 + ColumnEdgeSecond[i]])
    dev_set_color ('cyan')
    dev_display (EdgeFirst)
    dev_set_color ('magenta')
    dev_display (EdgeSecond)
    dev_set_color ('blue')
    if (i == 0)
        set_tposition (WindowID, RowEdgeFirst[i] + 5, ColumnEdgeFirst[i] + 20)
    else
        set_tposition (WindowID, RowEdgeFirst[i] - 40, ColumnEdgeFirst[i] + 20)
    endif
    write_string (WindowID, 'width: ' + IntraDistance[i] + ' pix')
endfor
disp_continue_message (WindowID, 'black', 'true')
stop ()
* ****
* step: destroy measure object
* ****
close_measure (MeasureHandle)
dev_update_window ('on')
dev_clear_window ()
```

