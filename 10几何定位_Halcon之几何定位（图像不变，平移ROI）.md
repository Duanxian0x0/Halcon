## Halcon之几何定位（图像不变，平移ROI）

```csharp
* 一 准备标准图像，进行模板的制作

* 1.获取窗口

dev_get_window (WindowHandle)
* 设置窗口显示模式为Margin，把区域用轮廓进行显示
dev_set_draw ('margin')

* 2.读取标准图像
read_image (Image1, 'C:/Users/Jumy/Desktop/Halcon-AOI教学资料/Halcon-AOI教学资料/程序与图像/06几何定位/1.几何模板制作/1.bmp')

* 绘制模板区域
draw_rectangle1 (WindowHandle, Row1, Column1, Row2, Column2)

gen_rectangle1 (Rectangle, Row1, Column1, Row2, Column2)

reduce_domain (Image1, Rectangle, ImageReduced)

* 创建模板
create_shape_model (ImageReduced, 'auto', -0.39, 0.79, 'auto', 'auto', 'use_polarity', 'auto', 'auto', ModelID)

get_shape_model_contours (ModelContours, ModelID, 1)


* 定位：无论盖子怎么移动，我们都可以获取到盖子中间的检测区域

* 确定ROI检测区域

draw_circle (WindowHandle, Row, Column, Radius)
gen_circle (Circle_ROI, Row, Column, Radius)
read_image (Image2, 'C:/Users/Jumy/Desktop/Halcon-AOI教学资料/Halcon-AOI教学资料/程序与图像/06几何定位/2.几何定位/图片/2.bmp')



find_shape_model (Image2, ModelID, -0.39, 0.79, 0.5, 1, 0.5, 'least_squares', 0, 0.9, Row3, Column3, Angle, Score)

* 生成仿射矩阵
* 仿射变换的对象，刚刚画出来的圆ROI区域

vector_angle_to_rigid (Row, Column, 0, Row3, Column3, Angle, HomMat2D)

affine_trans_region (Circle_ROI, RegionAffineTrans, HomMat2D, 'nearest_neighbor')
```

```csharp
*******************************
* 一 图像不变，平移ROI
* 几何模板匹配定位
*******************************
* 获取窗口
dev_get_window (WindowHandle)
* 读取图像
read_image (Image, './图片/1.bmp')
* 绘制ROI区域（加测区域）
draw_circle (WindowHandle, Row, Column, Radius)

gen_circle (Circle_ROI, Row, Column, Radius)

* 绘制模板区域
draw_rectangle1 (WindowHandle, Row11, Column11, Row2, Column2)
gen_rectangle1 (Rectangle, Row11, Column11, Row2, Column2)
* 获取区域图像
reduce_domain(Image, Rectangle, ImageReduced)
* 创建模板
create_shape_model(ImageReduced, 'auto', 0, 0, 'auto', 'auto', 'use_polarity', 'auto', 'auto', ModelID)
* 获取模板（显示模板
get_shape_model_contours(ModelContours, ModelID, 1)

* 获取所哟图像
list_files ('./图片', ['files','follow_links'], ImageFiles)
tuple_regexp_select (ImageFiles, ['\\.(tif|tiff|gif|bmp|jpg|jpeg|jp2|png|pcx|pgm|ppm|pbm|xwd|ima)$','ignore_case'], ImageFiles)
for Index := 0 to |ImageFiles| - 1 by 1
    * 读取图像
    read_image (Image, ImageFiles[Index])
    * 模板匹配
    find_shape_model(Image, ModelID, 0, 0, 0.5, 1, 0.5, 'least_squares', 0, 0.9, Row3, Column3, Angle, Score)
    * 匹配成功
    if(|Score|>0)
        dev_display_shape_matching_results(ModelID, 'red', Row3, Column3, Angle, 1, 1, 0)
       *ROI跟随

        area_center (Circle_ROI, Area, Row4, Column4)

       * 模板图像到当前图像的仿射变换矩阵（重点
        vector_angle_to_rigid(Row4, Column4, 0, Row3, Column3, 0, HomMat2D)
        * 对ROI区域进行仿射变换
        affine_trans_region(Circle_ROI, RegionAffineTrans, HomMat2D, 'false')
        
        
    endif
    
    stop()
    
endfor


*******************************
* 二 图像不变，平移ROI
* Blob定位
*******************************
* 读取图像
read_image (Image, './图片/1.bmp')

rgb1_to_gray (Image, GrayImage)
* 阈值分
threshold(Image, Region, 200, 255)
* 连通
connection(Region, ConnectedRegions)
*填充
fill_up(ConnectedRegions, RegionFillUp)
* 根据圆度和外接圆半径进行特征区域选择
select_shape (RegionFillUp, SelectedRegions, ['circularity','outer_radius'], 'and', [0.7,150], [1,250])
* 获取区域的中心点
area_center(SelectedRegions, Area1, Row0, Column0)
gen_cross_contour_xld(Cross, Row0, Column0, 30, Angle)


list_files ('./图片', ['files','follow_links'], ImageFiles)
tuple_regexp_select (ImageFiles, ['\\.(tif|tiff|gif|bmp|jpg|jpeg|jp2|png|pcx|pgm|ppm|pbm|xwd|ima)$','ignore_case'], ImageFiles)
for Index := 0 to |ImageFiles| - 1 by 1
    read_image (Image, ImageFiles[Index])
   threshold(Image, Region, 220, 255)
   connection(Region, ConnectedRegions)
    fill_up(ConnectedRegions, RegionFillUp)
   dev_display(Image)
   select_shape (RegionFillUp, SelectedRegions, ['circularity','outer_radius'], 'and', [0.7,150], [1,250])
    area_center(SelectedRegions, Area1, Row1, Column1)
    gen_cross_contour_xld(Cross, Row1, Column1, 30, Angle)
    if(|Row1|>0)
      
     
        vector_angle_to_rigid(Row0, Column0, 0, Row1, Column1, 0, HomMat2D)
        affine_trans_region(Rectangle, RegionAffineTrans, HomMat2D, 'false')
        
        
    endif
    
    stop()
    
endfor

```

