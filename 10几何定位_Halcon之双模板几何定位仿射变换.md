## Halcon之双模板几何定位仿射变换
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/f32e1afff1584d0d86524665ec530225.png)

```csharp
* 1.获取窗口

dev_get_window (WindowHandle)

* 2. 读取标准图像
read_image (Image1, 'C:/Users/Jumy/Desktop/Halcon-AOI教学资料/Halcon-AOI教学资料/程序与图像/06几何定位/2.几何定位/图片/1.bmp')
* 3. 绘制做模板的区域
draw_rectangle1 (WindowHandle, Row1, Column1, Row2, Column2)
* 4.生成区域
gen_rectangle1 (Rectangle, Row1, Column1, Row2, Column2)
* 5.获取对应区域的图像
reduce_domain (Image1, Rectangle, ImageReduced)
* 区域图像做一个模板
create_shape_model (ImageReduced, 'auto', -0.39, 0.79, 'auto', 'auto', 'use_polarity', 'auto', 'auto', ModelID)
get_shape_model_contours (ModelContours, ModelID, 1)

* ROI区域不变，变换图像

** 确定检测区域

draw_circle (WindowHandle, Row, Column, Radius)
gen_circle (Circle, Row, Column, Radius)

* 新图像，盖子在新的位置
read_image (Image2, 'C:/Users/Jumy/Desktop/Halcon-AOI教学资料/Halcon-AOI教学资料/程序与图像/06几何定位/2.几何定位/图片/2.bmp')

* 图像不变，变ROI（ROI跟随），变换仿射变换矩阵的生成上

find_shape_model (Image2, ModelID, -0.39, 0.79, 0.5, 1, 0.5, 'least_squares', 0, 0.9, Row3, Column3, Angle, Score)
* （模板匹配点到原ROI点）
vector_angle_to_rigid (Row3, Column3, 0, Row, Column, 0, HomMat2D)

affine_trans_image (Image2, ImageAffineTrans, HomMat2D, 'constant', 'false')


```

```csharp
* 获取窗口

dev_get_window (WindowHandle)

read_image (Image1, 'C:/Users/Jumy/Desktop/Halcon-AOI教学资料/Halcon-AOI教学资料/程序与图像/06几何定位/2.几何定位/图片/1.bmp')

* 绘制模板区域1
draw_rectangle1 (WindowHandle, Row1, Column1, Row2, Column2)
gen_rectangle1 (Rectangle, Row1, Column1, Row2, Column2)
reduce_domain (Image1, Rectangle, ImageReduced)
create_shape_model (ImageReduced, 'auto', -0.39, 0.79, 'auto', 'auto', 'use_polarity', 'auto', 'auto', ModelID1)

get_shape_model_contours (ModelContours, ModelID1, 1)
* 绘制模板区域2
draw_rectangle1 (WindowHandle, Row11, Column11, Row21, Column21)
gen_rectangle1 (Rectangle1, Row11, Column11, Row21, Column21)
reduce_domain (Image1, Rectangle1, ImageReduced1)
create_shape_model (ImageReduced1, 'auto', -0.39, 0.79, 'auto', 'auto', 'use_polarity', 'auto', 'auto', ModelID2)

get_shape_model_contours (ModelContours1, ModelID2, 1)
* 双模板匹配的特别：匹配算子。仿射变换矩阵声生成算子

read_image (Image2, 'C:/Users/Jumy/Desktop/Halcon-AOI教学资料/Halcon-AOI教学资料/程序与图像/06几何定位/2.几何定位/图片/2.bmp')

* 注意匹配数量设置为2
find_shape_models(Image2, [ModelID1,ModelID2], rad(-180),rad(360), 0.5, 2, 0.5,'least_squares', 0, 0.9, Row, Column, Angle, Score, Model)

* 仿射变换（定位）图像不变，变ROI区域,
area_center (Rectangle, Area, Row3, Column3)
area_center (Rectangle1, Area, Row4, Column4)

* 绘制ROI区域
draw_circle (WindowHandle, Row5, Column5, Radius)
gen_circle (Circle, Row5, Column5, Radius)
* 生成仿射矩阵(roi点到模板匹配的点)
vector_to_rigid ([Row3,Row4], [Column3,Column4], Row, Column, HomMat2D)

* 仿射变换
affine_trans_region (Circle, RegionAffineTrans, HomMat2D, 'nearest_neighbor')
```

```csharp
*****************************
* 创建多模板和ROI区域
*****************************
* 读取图像
read_image (Image, './图片/1.bmp')

* 生成矩形
gen_rectangle1(Rectangle1, 201, 156, 369,672)
* 获取区域图像
reduce_domain(Image, Rectangle1, ImageReduced)
* 根据图像创建几何模板
create_shape_model(ImageReduced, 'auto', rad(-180),rad(360), 'auto', 'auto',\
'use_polarity', 'auto', 'auto', ModelID1)
* 生成矩形区域
gen_rectangle1(Rectangle2, 684, 681, 833,1130)
* 获取区域图像
reduce_domain(Image, Rectangle2, ImageReduced)
* 根据区域图像创建几何模板
create_shape_model(ImageReduced, 'auto', rad(-180),rad(360), 'auto', 'auto',\
'use_polarity', 'auto', 'auto', ModelID2)

* 获取模板1（显示模板）
get_shape_model_contours(ModelContours1, ModelID1, 1)
* 获取模板2
get_shape_model_contours(ModelContours2, ModelID2, 1)


*ROI区域
gen_rectangle2(Rect, 236, 421, 0.248, 115, 69)
* draw_circle(3600, Row1, Column1, Radius)
gen_circle(Circle, 621, 867, 115)
* 1.获取特征点坐标、角度（点对）
* 进行多模板匹配
find_shape_models(Image, [ModelID1,ModelID2], rad(-180),rad(360), 0.5, 2, 0.5,\
'least_squares', 0, 0.9, Row, Column, Angle, Score, Model)

*****************************
*一 图像不变，ROI进行仿射变换（不含缩放）
*****************************
* 如果有两个匹配结果
if(|Row|=2)
   * 显示匹配结果
   dev_display_shape_matching_results( [ModelID1,ModelID2], 'red', Row, Column, Angle, 1, 1,Model)
   area_center(Rectangle1, Area, Row01, Column01)
   area_center(Rectangle2, Area, Row02, Column02)
   * 根据两个以上特征点获取仿射变换矩阵
   * 模板图转当前图仿射矩阵
   * 2.生成变换矩阵
   vector_to_rigid ([Row01,Row02], [Column01,Column02], Row,Column, HomMat2D)
     * ROI区域进行仿射变换
     * 3.对图像、区域、轮廓进行仿射变换
    affine_trans_region(Rect, RegionAffineTrans1, HomMat2D, 'false')
    affine_trans_region(Circle, RegionAffineTrans2, HomMat2D, 'false')
 endif
 
 
* 选取所有图像
list_files ('./图片', ['files','follow_links'], ImageFiles)
tuple_regexp_select (ImageFiles, ['\\.(tif|tiff|gif|bmp|jpg|jpeg|jp2|png|pcx|pgm|ppm|pbm|xwd|ima)$','ignore_case'], ImageFiles)
* 遍历所有图像
for Index := 0 to |ImageFiles| - 1 by 1
    * 读取图像
    read_image (Image, ImageFiles[Index])
    * 1.获取特征点坐标、角度（点对）
    * 进行模板匹配
    find_shape_models(Image, [ModelID1,ModelID2], rad(-180),rad(360), 0.5, 2, 0.5,'least_squares', 0, 0.9, Row, Column, Angle, Score, Model)
    * 匹配成功
    if(|Row|=2)
        * 显示匹配结果
       dev_display_shape_matching_results( [ModelID1,ModelID2], 'red', Row, Column, Angle, 1, 1,Model)
       * 获取模板图的中心点
       area_center(Rectangle1, Area, Row01, Column01)
       area_center(Rectangle2, Area, Row02, Column02)
       * 固定和模板图匹配上
       if(Model[0]==0)
           * 模板图转当前图仿射矩阵
           * 2.生成变换矩阵
         vector_to_rigid ([Row01,Row02], [Column01,Column02], Row,Column, HomMat2D)
       else
         vector_to_rigid ([Row02,Row01], [Column02,Column01], Row,Column, HomMat2D)
       endif
        * ROI区域进行仿射变换
         * 3.对图像、区域、轮廓进行仿射变换
        affine_trans_region(Rect, RegionAffineTrans1, HomMat2D, 'false')
        affine_trans_region(Circle, RegionAffineTrans2, HomMat2D, 'false')
     endif
*      stop()
    
endfor

****************************************
*二 ROI不变，平移图像（不含缩放）
******************************************
* 选取所有图像
list_files ('./图片', ['files','follow_links'], ImageFiles)
tuple_regexp_select (ImageFiles, ['\\.(tif|tiff|gif|bmp|jpg|jpeg|jp2|png|pcx|pgm|ppm|pbm|xwd|ima)$','ignore_case'], ImageFiles)
* 遍历所有图像
for Index := 0 to |ImageFiles| - 1 by 1
    * 读取图像
    read_image (Image, ImageFiles[Index])
    * 进行模板匹配
    * * 1.获取特征点坐标、角度（点对）
    find_shape_models(Image, [ModelID1,ModelID2], rad(-180),rad(360), 0.5, 2, 0.5,\
    'least_squares', 0, 0.9, Row, Column, Angle, Score, Model)
    * 匹配成功
    if(|Row|=2)
        * 显示匹配结果
       dev_display_shape_matching_results( [ModelID1,ModelID2], 'red', Row, Column, Angle, 1, 1,Model)
       * 获取区域的中心点
       area_center(Rectangle1, Area, Row01, Column01)
       area_center(Rectangle2, Area, Row02, Column02)
       * 固定和模板图匹配上 
       if(Model[0]==0)
            * 2.生成变换矩阵
         vector_to_rigid (Row,Column,[Row01,Row02], [Column01,Column02], HomMat2D)
       else
         vector_to_rigid (Row,Column,[Row02,Row01], [Column02,Column01],  HomMat2D)
       endif
        * 对图像进行仿射变换
        * 3.对图像、区域、轮廓进行仿射变换
        affine_trans_image(Image, ImageAffinTrans, HomMat2D, 'constant', 'false')
        * 显示结果
        dev_display(Rect)
        dev_display(Circle)
     endif
      stop()
    
endfor

clear_shape_model(ModelID1)
clear_shape_model(ModelID2)


*****************************
*三 图像不变，ROI进行仿射变换（含缩放）
*****************************
* 读取图像
read_image (Image, './图片/1.bmp')
* 生成矩形区域
gen_rectangle1(Rectangle1, 201, 156, 369,672)
* 获取区域图像
reduce_domain(Image, Rectangle1, ImageReduced)
* 根据图像创建几何模板
create_shape_model(ImageReduced, 'auto', rad(-180),rad(360), 'auto', 'auto',\
'use_polarity', 'auto', 'auto', ModelID1)
* 生成矩形区域
gen_rectangle1(Rectangle2, 684, 681, 833,1130)
* 获取区域图像
reduce_domain(Image, Rectangle2, ImageReduced)
* 根据图像创建几何模板
create_shape_model(ImageReduced, 'auto', rad(-180),rad(360), 'auto', 'auto',\
'use_polarity', 'auto', 'auto', ModelID2)

* 获取模板
get_shape_model_contours(ModelContours1, ModelID1, 1)
get_shape_model_contours(ModelContours2, ModelID2, 1)

*确定ROI区域
gen_rectangle2(Rect, 236, 421, 0.248, 115, 69)
* draw_circle(3600, Row1, Column1, Radius)
gen_circle(Circle, 621, 867, 115)

* 模板匹配
find_shape_models(Image, [ModelID1,ModelID2], rad(-180),rad(360), 0.5, 2, 0.5,\
'least_squares', 0, 0.9, Row, Column, Angle, Score, Model)
* 匹配成功
if(|Row|=2)
    * 显示匹配结果
   dev_display_shape_matching_results( [ModelID1,ModelID2], 'red', Row, Column, Angle, 1, 1,Model)
   * 获取模板区域的中心点
   area_center(Rectangle1, Area, Row01, Column01)
   area_center(Rectangle2, Area, Row02, Column02)
   * 生成模板图像当当前图像的仿射变换矩阵
*     vector_to_rigid ([Row01,Row02], [Column01,Column02], Row,Column, HomMat2D)
    * 2.生成变换矩阵
    vector_to_similarity([Row01,Row02], [Column01,Column02], Row,Column, HomMat2D)  
    * 对roi区域进行仿射变换
    * 3.对图像、区域、轮廓进行仿射变换
    affine_trans_region(Rect, RegionAffineTrans1, HomMat2D, 'false')
    affine_trans_region(Circle, RegionAffineTrans2, HomMat2D, 'false')
 endif
 
 
*****************************
*三 图像不变，ROI进行仿射变换（含缩放）
*****************************
list_files ('./图片', ['files','follow_links'], ImageFiles)
tuple_regexp_select (ImageFiles, ['\\.(tif|tiff|gif|bmp|jpg|jpeg|jp2|png|pcx|pgm|ppm|pbm|xwd|ima)$','ignore_case'], ImageFiles)
for Index := 0 to |ImageFiles| - 1 by 1
    * 读取图像
    read_image (Image, ImageFiles[Index])
    * 模板匹配
    find_shape_models(Image, [ModelID1,ModelID2], rad(-180),rad(360), 0.5, 2, 0.5,\
    'least_squares', 0, 0.9, Row, Column, Angle, Score, Model)
    * 匹配成功
    if(|Row|=2)
        * 获取匹配结果
       dev_display_shape_matching_results( [ModelID1,ModelID2], 'red', Row, Column, Angle, 1, 1,Model)
       * 获取模板区域的中心点
       area_center(Rectangle1, Area, Row01, Column01)
       area_center(Rectangle2, Area, Row02, Column02)
       * 固定和模板图匹配上 
       if(Model[0]=0)
          * 生成模板图像到当前图像的仿射变换矩阵
          * 2.生成变换矩阵
         vector_to_similarity ([Row01,Row02], [Column01,Column02], Row,Column, HomMat2D)
       else
         vector_to_similarity ([Row02,Row01], [Column02,Column01], Row,Column, HomMat2D)
       endif
       * 对roi区域进行仿射变换
        affine_trans_region(Rect, RegionAffineTrans1, HomMat2D, 'false')
        affine_trans_region(Circle, RegionAffineTrans2, HomMat2D, 'false')
     endif
     stop()
    
endfor


*****************************
*四 图像不变，ROI进行仿射变换（含缩放）
*****************************
list_files ('./图片', ['files','follow_links'], ImageFiles)
tuple_regexp_select (ImageFiles, ['\\.(tif|tiff|gif|bmp|jpg|jpeg|jp2|png|pcx|pgm|ppm|pbm|xwd|ima)$','ignore_case'], ImageFiles)
for Index := 0 to |ImageFiles| - 1 by 1
    * 读取图像
    read_image (Image, ImageFiles[Index])
    * 进行模板匹配
    find_shape_models(Image, [ModelID1,ModelID2], rad(-180),rad(360), 0.5, 2, 0.5,\
    'least_squares', 0, 0.9, Row, Column, Angle, Score, Model)
    * 匹配成功
    if(|Row|=2)
         * 获取匹配结果
       dev_display_shape_matching_results( [ModelID1,ModelID2], 'red', Row, Column, Angle, 1, 1,Model)
       * 获取模板区域的中心点
       area_center(Rectangle1, Area, Row01, Column01)
       area_center(Rectangle2, Area, Row02, Column02)
       * 固定和模板图匹配上 
       if(Model[0]=0)
         * 生成当前图像到模板图像的仿射变换矩阵
         * 2.生成变换矩阵
         vector_to_similarity ( Row,Column,[Row01,Row02], [Column01,Column02], HomMat2D)
       else
         vector_to_similarity (Row,Column,[Row02,Row01], [Column02,Column01],  HomMat2D)
       endif
       * 对图像进行仿射变换
       * 3.对图像、区域、轮廓进行仿射变换
         affine_trans_image(Image, ImageAffinTrans, HomMat2D, 'constant', 'false')
         * 显示结果
         dev_display(Rect)
          dev_display(Circle)
     endif
     stop()
    
endfor
* 清除句柄
clear_shape_model(ModelID1)
clear_shape_model(ModelID2)

```

