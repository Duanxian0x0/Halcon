## 检测解决策略之一blob分析+特征分析-02（瓶嘴破裂检测）

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/6c9d34a8917243f6bb2d52d06dad30be.png)





> 明星算子：
> polar_trans_region_inv
> 极坐标系转换



```csharp
* AOI缺陷检测策略之Blob分析+特征分析


* 极坐标系转换： 将圆环图像转矩形图像，有利于均值滤波等适合矩阵图像的操作
* polar_trans_image_ext
for Index := 1 to 16 by 1
    * 瓶嘴 01 02 03
    * Index 1  --> 01
    read_image (Image, 'bottles/bottle_mouth_'+Index$'.02')
    * blob分析（定位）
    
    threshold (Image, Region, 0, 80)
    
    * 开操作 先腐蚀后膨胀
    opening_circle (Region, RegionOpening, 5)
    * 填充
    fill_up (RegionOpening, RegionFillUp)
    
    * 转最小外界圆
    smallest_circle (RegionFillUp, Row, Column, Radius)
    gen_circle (Circle, Row, Column, Radius)
    
    * 腐蚀 30个像素
    erosion_circle (Circle, RegionErosion, 60)
    
    difference (Circle, RegionErosion, RegionDifference)
    
    reduce_domain (Image, RegionDifference, ImageReduced)
    
    
    *** 圆环图像转矩形图像
    ringSize :=60
    
    * 原图（环图）
    * 输出矩形图
    * 圆心
    *开始角度
    *角度：360
    * 半径的开始
    * 半径的结束
    * 输出矩形的宽
    * 输出矩形图的高
    * 插值算法
    polar_trans_image_ext (ImageReduced, PolarTransImage, Row, Column, 0, rad(360), Radius-ringSize, Radius, 700, 100, 'nearest_neighbor')
    
    
    * 灰度均衡化 0-10  ==> 0-255 操作余地变大了
    scale_image_max (PolarTransImage, ImageScaleMax)
    
    * blob分析
    mean_image (ImageScaleMax, ImageMean, 100, 3)
    dyn_threshold (PolarTransImage, ImageMean, RegionDynThresh, 80, 'light')
    
    * 开操作（去杂质）
    opening_circle (RegionDynThresh, RegionOpening1, 4)
    * 打散
    connection (RegionOpening1, ConnectedRegions)
    * 计数
    count_obj (ConnectedRegions, Number)
    if(Number>0)
        dev_get_window (WindowHandle)
        disp_message (WindowHandle, 'NG', 'window', 12, 12, 'red', 'true')
        stop()
    endif
    
endfor
```

```csharp
* 平滑卷积X长度
SmoothX := 501
* 阈值偏置
ThresholdOffset := 25
* 最小缺陷大小
MinDefectSize := 50
* 极坐标分辨率
PolarResolution := 640
* 环大小
RingSize := 70
* 获取空区域对象
get_system ('store_empty_region', StoreEmptyRegion)
set_system ('store_empty_region', 'false')
* 读取图像
read_image (Image, 'bottles/bottle_mouth_01')
* 更新设备
dev_update_off ()
* 关闭窗口
dev_close_window ()
dev_close_window ()
* 打开固定大小窗口
dev_open_window_fit_image (Image, 0, 0, 640, 512, WindowHandle1)
* 设置显示字体的大小
set_display_font (WindowHandle1, 16, 'mono', 'true', 'false')
* 显示图像
dev_display (Image)
* 设置显示模式
dev_set_draw ('margin')
* 设置显示线宽
dev_set_line_width (3)
* 打开固定大小窗口
dev_open_window_fit_size (0, 648, RingSize, PolarResolution, 150, 512, WindowHandle)
dev_set_draw ('margin')
dev_set_line_width (3)
dev_set_color ('red')
* 遍历
for Index := 1 to 16 by 1
    * 读取图像
    read_image (Image, 'bottles/bottle_mouth_' + Index$'.02')
    * 阈值分割
    * 1.获取输入图像的灰度直方图
    * 2.使用标准差为Sigma的一维高斯滤波器对直方图进行滤波
    * 3.计算滤波后的直方图的极小值
    * 4.以计算得到的极小值所对应的灰度值为分割阈值对图像进行分割
    * 直方图阈值分割
    auto_threshold (Image, Regions, 2)
    * 选择分割区域
    select_obj (Regions, DarkRegion, 1)
    * 开运算
    opening_circle (DarkRegion, RegionOpening, 3.5)
    * 闭运算
    closing_circle (RegionOpening, RegionClosing, 25.5)
    * 填充
    fill_up (RegionClosing, RegionFillUp)
    * 获取边界
    boundary (RegionFillUp, RegionBorder, 'outer')
    * 膨胀
    dilation_circle (RegionBorder, RegionDilation, 3.5)
    * 获取区域图像
    reduce_domain (Image, RegionDilation, ImageReduced)
    * 获取边缘
    edges_sub_pix (ImageReduced, Edges, 'canny', 0.5, 20, 40)
    * 分割轮廓
    segment_contours_xld (Edges, ContoursSplit, 'lines_circles', 5, 4, 2)
    * 合并轮廓
    union_cocircular_contours_xld (ContoursSplit, UnionContours, 0.9, 0.5, 0.5, 200, 50, 50, 'true', 1)
    * 轮廓个数
    length_xld (UnionContours, Length)
    * 选择对象
    select_obj (UnionContours, LongestContour, sort_index(Length)[|Length| - 1] + 1)
    * 拟合圆轮廓
    fit_circle_contour_xld (LongestContour, 'ahuber', -1, 0, 0, 3, 2, Row, Column, Radius, StartPhi, EndPhi, PointOrder)
    * 
    * Part 2: Transform the ring-shaped bottle neck region to a rectangle
    * 生成圆
    gen_circle (Circle, Row, Column, Radius)
    * 膨胀
    dilation_circle (Circle, RegionDilation, 5)
    * 腐蚀
    erosion_circle (Circle, RegionErosion, RingSize - 5)
    *区域相减
    difference (RegionDilation, RegionErosion, RegionDifference)
    * 获取区域图像
    reduce_domain (Image, RegionDifference, ImageReduced)
    * 极坐标转换
    polar_trans_image_ext (ImageReduced, ImagePolar, Row, Column, 0, rad(360), Radius - RingSize, Radius, PolarResolution, RingSize, 'nearest_neighbor')
    * 
    * Part 3: Find defects with a dynamic threshold
    * 灰度值拉伸到0-255
    scale_image_max (ImagePolar, ImageScaleMax)
    * 均值滤波
    mean_image (ImageScaleMax, ImageMean, SmoothX, 3)
    * 动态阈值分割
    dyn_threshold (ImageScaleMax, ImageMean, Regions1, 55, 'not_equal')
    * 连通
    connection (Regions1, Connection)
    * 根据特征选取区域
    select_shape (Connection, SelectedRegions, 'height', 'and', 9, 99999)
    * 闭运算
    closing_rectangle1 (SelectedRegions, RegionClosing1, 10, 20)
    * 区域合并
    union1 (RegionClosing1, RegionUnion)
    * re-transform defect regions for visualization
    polar_trans_region_inv (RegionUnion, XYTransRegion, Row, Column, 0, rad(360), Radius - RingSize, Radius, PolarResolution, RingSize, 1280, 1024, 'nearest_neighbor')
    * 
    * Part 4: Display results
    * 显示结果
    dev_set_window (WindowHandle1)
    dev_display (Image)
    dev_set_color ('blue')
    dev_display (RegionDifference)
    dev_set_color ('red')
    dev_display (XYTransRegion)
    * display polar transformed inspected region with results
    * The image and resulting region are rotated by 90 degrees
    * only for visualization purposes! (I.e. to fit better on the screen)
    * The rotation is NOT necessary for the detection algorithm.
    dev_set_window (WindowHandle)
    * 旋转图像（重要）
    rotate_image (ImagePolar, ImageRotate, 90, 'constant')
    dev_display (ImageRotate)
    * 区域计数
    count_obj (RegionUnion, Number)
    if (Number > 0)
        mirror_region (RegionUnion, RegionMirror, 'diagonal', PolarResolution)
        mirror_region (RegionMirror, RegionMirror, 'row', PolarResolution)
        dev_display (RegionMirror)
        disp_message (WindowHandle1, 'Not OK', 'window', 12, 12, 'red', 'false')
    else
        disp_message (WindowHandle1, 'OK', 'window', 12, 12, 'forest green', 'false')
    endif
    if (Index < 16)
        disp_continue_message (WindowHandle1, 'black', 'true')
        stop ()
    endif
endfor
* Reset system parameters
set_system ('store_empty_region', StoreEmptyRegion)




```

