## Halcon之边缘提取拟合直线
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/8f58637bcd76450fbee631292c504785.png)

```csharp
* 1 读取图像
read_image (Image, 'C:/Users/Jumy/Desktop/Halcon-AOI教学资料/Halcon-AOI教学资料/程序与图像/08边缘测量和拟合/矩形.png')

* 2. 预处理

* 3. 边缘提取
edges_sub_pix (Image, Edges, 'canny', 1, 20, 40)

* 4.分割
segment_contours_xld (Edges, ContoursSplit, 'lines_circles', 5, 4, 2)

* 联合
* 联合邻近的
* union_adjacent_contours_xld (ContoursSplit, UnionContours, 10, 1, 'attr_keep')
* 共圆的
* union_cocircular_contours_xld (UnionContours, UnionContours1, 0.5, 0.1, 0.2, 30, 10, 10, 'true', 1)
* 共线的
* union_collinear_contours_xld (UnionContours1, UnionContours2, 10, 1, 2, 0.1, 'attr_keep')

* 选取
sort_contours_xld (ContoursSplit, SortedContours, 'upper_left', 'true', 'row')
select_obj (SortedContours, ObjectSelected, 1)

* 筛选
select_shape_xld (ContoursSplit, SelectedXLD, 'width', 'and', 417, 99999)

* 拟合直线
fit_line_contour_xld (SelectedXLD, 'tukey', -1, 0, 5, 2, RowBegin, ColBegin, RowEnd, ColEnd, Nr, Nc, Dist)

dev_get_window (WindowHandle)
disp_line (WindowHandle, RowBegin, ColBegin, RowEnd, ColEnd)
```

```csharp
**************************
* 一 采集图像1.采集图像
**************************
* 读取图像
read_image (Image, '矩形.png')

**************************
* 二 预处理
**************************
mult_image (Image, Image, ImageResult, 0.005, 0)
**************************
* 三 亚边缘提取
**************************
edges_sub_pix (ImageResult, Edges, 'canny', 1, 20, 40)

**************************
* 四 分割、联合
**************************
segment_contours_xld (Edges, ContoursSplit, 'lines_circles', 5, 4, 2)
* 联合常用三函数：联合共线的，联合邻近的，联合圆的
* 联合邻近的
* union_adjacent_contours_xld (ContoursSplit, UnionContours1, 10, 1, 'attr_keep')
* 联合共圆的
* union_cocircular_contours_xld (ContoursSplit, UnionContours, 0.5, 0.1, 0.2, 30, 10, 10, 'true', 1)
* 联合共线的
union_collinear_contours_xld (ContoursSplit, UnionContours, 10, 1, 2, 0.1, 'attr_keep')
**************************
* 五 选择并拟合
**************************
* 排序
sort_contours_xld (UnionContours, SortedContours, 'upper_left', 'true', 'row')
* 选取
select_obj (SortedContours, ObjectSelected, 1)
* 拟合
fit_line_contour_xld (ObjectSelected, 'tukey', -1, 0, 5, 2, RowBegin, ColBegin, RowEnd, ColEnd, Nr, Nc, Dist)

**************************
* 六 显示结果
*************************
*根据拟合结果，产生直线xld
gen_contour_polygon_xld (Line, [RowBegin,RowEnd], [ColBegin,ColEnd])
dev_display (ImageResult)
dev_set_colored (6)
dev_display (Line)

```

