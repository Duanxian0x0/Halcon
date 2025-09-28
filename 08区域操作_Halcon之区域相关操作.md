## Halcon之区域相关操作

```csharp
* 获取窗口

dev_get_window (WindowHandle)


*** 一 绘制区域
* 绘制矩阵1
draw_rectangle1 (WindowHandle, Row1, Column1, Row2, Column2)
gen_rectangle1 (Rectangle, Row1, Column1, Row2, Column2)

* 绘制矩形2

draw_rectangle2 (WindowHandle, Row, Column, Phi, Length1, Length2)
gen_rectangle2 (Rectangle1, Row, Column, Phi, Length1, Length2)

* 绘制圆
draw_circle (WindowHandle, Row3, Column3, Radius)
gen_circle (Circle, Row3, Column3, Radius)

* 绘制椭圆
draw_ellipse (WindowHandle, Row4, Column4, Phi1, Radius1, Radius2)
gen_ellipse (Ellipse, Row4, Column4, Phi1, Radius1, Radius2)

* 画直线
draw_line (WindowHandle, Row11, Column11, Row21, Column21)
gen_region_line (RegionLines, Row11, Column11, Row21, Column21)


* 绘制任意区域
draw_region (Region, WindowHandle)


*** 二 区域的形态学操作
* 腐蚀
erosion_circle (Region, RegionErosion, 13.5)
erosion_rectangle1 (RegionErosion, RegionErosion1, 11, 11)

* 膨胀
dilation_circle (RegionErosion1, RegionDilation, 3.5)
dilation_rectangle1 (RegionDilation, RegionDilation1, 11, 11)


*** 三 区域之间的交集，并集，差集，补集等
* 区域之间的交集
dev_clear_window ()
* 绘制任意区域
draw_region (Region1, WindowHandle)
* 绘制任意区域
draw_region (Region2, WindowHandle)

intersection (Region1, Region2, RegionIntersection)


* 区域之间的差集

difference (Region1, Region2, RegionDifference)

* 区域的并集
union2 (Region1, Region2, RegionUnion)

* 补集
complement (Region1, RegionComplement)


*** 四 区域形状的变换
shape_trans (Region1, RegionTrans, 'convex')
shape_trans (Region1, RegionTrans, 'rectangle1')
shape_trans (Region1, RegionTrans0, 'rectangle2')
shape_trans (Region1, RegionTrans1, 'inner_circle')
shape_trans (Region1, RegionTrans2, 'outer_circle')



*** 五 移动区域
move_region (Region1, RegionMoved, 100, 100)


*** 六 放射变换，移动和旋转区域
* 生成变换变换矩阵
vector_angle_to_rigid(0, 0, 0, 0, -100, 0, HomMat2D)
* 对区域进行移动
affine_trans_region(Region1, RegionAffineTrans, HomMat2D, 'false')

* 生成变换变换矩阵
vector_angle_to_rigid(0, 0, 0, 100, 0, 0, HomMat2D)
* 对区域进行移动
affine_trans_region(Region1, RegionAffineTrans, HomMat2D, 'false')


* 角度变换
* 获取旋转中心
area_center (Region1, Area, Row5, Column5)
* 生成对应旋转变换矩阵
vector_angle_to_rigid (Row5, Column5, 0, Row5, Column5, rad(-90), HomMat2D1)
* 使用变换矩阵进行旋转
affine_trans_region (Region1, RegionAffineTrans1, HomMat2D1, 'nearest_neighbor')


```

