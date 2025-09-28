## 检测解决策略之一blob分析+特征分析-04（药丸检测）
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/ebf96f24947244e68586d7acdcb24595.png)

```csharp
* 窗口设置
dev_close_window ()
dev_update_off ()
* 一 模板制作
* 读取图像
read_image (ImageOrig, 'blister/blister_reference')
* 窗口显示设置
dev_open_window_fit_image (ImageOrig, 0, 0, -1, -1, WindowHandle)
set_display_font (WindowHandle, 14, 'mono', 'true', 'false')
dev_set_draw ('margin')
dev_set_line_width (3)
* 获取通道1 图像
access_channel (ImageOrig, Image1, 1)
* 阈值分割
threshold (Image1, Region, 90, 255)
* 进行convex变换
shape_trans (Region, Blister, 'convex')
* 获取角度
orientation_region (Blister, Phi)
* 获取区域的面积和中心
area_center (Blister, Area1, Row, Column)
* 生成仿射变换矩阵
vector_angle_to_rigid (Row, Column, Phi, Row, Column, 0, HomMat2D)
* 进行仿射变换
affine_trans_image (ImageOrig, Image2, HomMat2D, 'constant', 'false')
* 生成空对象
gen_empty_obj (Chambers)
for I := 0 to 4 by 1
    Row := 88 + I * 70
    for J := 0 to 2 by 1
        Column := 163 + J * 150
        * 生成矩形
        gen_rectangle2 (Rectangle, Row, Column, 0, 64, 30)
        * 连结矩形
        concat_obj (Chambers, Rectangle, Chambers)
    endfor
endfor
* 对区域进行仿射变换
affine_trans_region (Blister, Blister, HomMat2D, 'nearest_neighbor') 
* 区域相减
difference (Blister, Chambers, Pattern)
* 区域合并
union1 (Chambers, ChambersUnion)
* 获取角度变换
orientation_region (Blister, PhiRef)
* 加180
PhiRef := rad(180) + PhiRef
* 获取区域的中心和面积
area_center (Blister, Area2, RowRef, ColumnRef)
* 遍历
Count := 6
for Index := 1 to Count by 1
    * 读取图像
    * 1.读取图像
    read_image (Image, 'blister/blister_' + Index$'02')
    *  阈值分割（Blob定位）
    threshold (Image, Region, 90, 255)
    * 连通
    connection (Region, ConnectedRegions)
    * 根据面积选取区域
    select_shape (ConnectedRegions, SelectedRegions, 'area', 'and', 5000, 9999999)
    * 进行convex变换
    shape_trans (SelectedRegions, RegionTrans, 'convex')
    * 获取角度
    orientation_region (RegionTrans, Phi)
    * 获取面积和中心
    area_center (RegionTrans, Area3, Row, Column)
    * 生成仿射矩阵
    vector_angle_to_rigid (Row, Column, Phi, RowRef, ColumnRef, PhiRef, HomMat2D)
    * 图像进行仿射变换
    affine_trans_image (Image, ImageAffineTrans, HomMat2D, 'constant', 'false')
    * 获取区域图像（重要）3.获取ROI(感兴趣)区域
    reduce_domain (ImageAffineTrans, ChambersUnion, ImageReduced)
    * 图像分解成三个通道* 4.图像预处理
    decompose3 (ImageReduced, ImageR, ImageG, ImageB)
    * 对B通道进行阈值分割5.图像算法处理
    * mean
    var_threshold (ImageB, Region, 7, 7, 0.2, 2, 'dark')
    * 连通
    connection (Region, ConnectedRegions0)
    * 闭运算
    closing_rectangle1 (ConnectedRegions0, ConnectedRegions, 3, 3)
    * 填充
    fill_up (ConnectedRegions, RegionFillUp)
    * 根据面积选取区域
    select_shape (RegionFillUp, SelectedRegions, 'area', 'and', 1000, 99999)
    * 开运算
    opening_circle (SelectedRegions, RegionOpening, 4.5)
    * 连通
    connection (RegionOpening, ConnectedRegions)
    * 根据面积选取区域
    select_shape (ConnectedRegions, SelectedRegions, 'area', 'and', 1000, 99999)
    * 进行convex变换
    shape_trans (SelectedRegions, Pills, 'convex')
    * 计数
    count_obj (Chambers, Number)
    * 生成空对象
    gen_empty_obj (WrongPill)
    * 生成空对象
    gen_empty_obj (MissingPill)
    * 遍历6.结果输出
    for I := 1 to Number by 1
        * 选取对象
        select_obj (Chambers, Chamber, I)
        * 交集
        intersection (Chamber, Pills, Pill)
        * 获取区域中心和面积
        area_center (Pill, Area, Row1, Column1)
        * 面积大于0
        if (Area > 0)
            * 湖区灰度值的最值和范围
            min_max_gray (Pill, ImageB, 0, Min, Max, Range)
            if (Area < 3800 or Min < 60)
                * 连结对象
                concat_obj (WrongPill, Pill, WrongPill)
            endif
        else
            * 连结对象
            concat_obj (MissingPill, Chamber, MissingPill)
        endif
    endfor
    * 显示结果
    dev_clear_window ()
    dev_display (ImageAffineTrans)
    dev_set_color ('forest green')
    count_obj (Pills, NumberP)
    count_obj (WrongPill, NumberWP)
    count_obj (MissingPill, NumberMP)
    dev_display (Pills)
    if (NumberMP > 0 or NumberWP > 0)
        disp_message (WindowHandle, 'Not OK', 'window', 12, 12 + 600, 'red', 'true')
    else
        disp_message (WindowHandle, 'OK', 'window', 12, 12 + 600, 'forest green', 'true')
    endif
    * 
    Message := '# Correct pills: ' + (NumberP - NumberWP)
    Message[1] := '# Wrong pills  :  ' + NumberWP
    Message[2] := '# Missing pills:  ' + NumberMP
    * 
    Colors := gen_tuple_const(3,'black')
    if (NumberWP > 0)
        Colors[1] := 'red'
    endif
    if (NumberMP > 0)
        Colors[2] := 'red'
    endif
    disp_message (WindowHandle, Message, 'window', 12, 12, Colors, 'true')
    dev_set_color ('red')
    dev_display (WrongPill)
    dev_display (MissingPill)
    if (Index < Count)
        disp_continue_message (WindowHandle, 'black', 'true')
    endif
    stop ()
endfor


```

