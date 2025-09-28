## Halcon之几何模板制作

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/986529776f7a4d27a828120d3878c234.png)

```csharp
* 获取窗口句柄
dev_get_window (WindowHandle)


*** 一 制作模板

* 读取图像
read_image (Image, '1.bmp')
* 绘制模板区域
draw_rectangle1(WindowHandle, Row1, Column1, Row2, Column2)
* 生成矩形区域
gen_rectangle1(Rectangle, Row1, Column1, Row2, Column2)
* 获取区域图像
reduce_domain(Image, Rectangle, ImageReduced)
* 创建模板
create_scaled_shape_model (ImageReduced, 'auto', rad(-180), rad(360), 'auto', 0.4, 1.6, 'auto', 'auto', 'use_polarity', 'auto', 'auto', ModelID)
* 模板匹配
find_scaled_shape_model(Image, ModelID,  rad(-180), rad(360), 0.4, 1.6, 0.5, 1, 0.5, 'least_squares', 0, 0.9, Row, Column, Angle, Scale, Score)
* 匹配成功
if(|Row|>0)
    * 显示匹配结果
    dev_display_shape_matching_results(ModelID, 'red', Row, Column, Angle, Scale, Scale, 0)
  
endif




*** 模板匹配

read_image (Image2, '2.bmp')
* 进行模板匹配
* 图像
* 模板
* 开始角度
* 角度范围
* 最小缩放系数
* 最大缩放系数
* 最小分数
* 匹配个数
* 重叠度
* 亚像素
* 金字塔层数
* 贪婪度
***
* 输出
* 匹配后模板的Y坐标
* 匹配后模板的X坐标
* 匹配后模板的角度
* 匹配后模板的缩放系数
* 匹配后模板的分数
find_scaled_shape_model(Image2, ModelID,  rad(-180), rad(360), 0.4, 1.6, 0.5, 1, 0.5, 'least_squares', 0, 0.9, Row, Column, Angle, Scale, Score)
* 显示模板
get_shape_model_contours(ModelContours, ModelID, 1)
* 匹配成功
if(|Row|>0)
    
    * 方式1：对模板进行仿射变换
    
   * 生成仿射矩阵
   hom_mat2d_identity(HomMat2DIdentity1)
   * 缩放
   hom_mat2d_scale(HomMat2DIdentity1, Scale, Scale, 0, 0, HomMat2DScale)
   * 角度
   hom_mat2d_rotate(HomMat2DScale, Angle, 0, 0, HomMat2DRotate)
   * 平移
   hom_mat2d_translate(HomMat2DRotate, Row,Column,  HomMat2DTranslate)
   * 对模板进行仿射变换
   affine_trans_contour_xld(ModelContours, ContoursAffinTrans, HomMat2DTranslate)
endif


 * 方式2：对模板进行仿射变换

* 进行平移旋转仿射生成（核心）
vector_angle_to_rigid(0, 0, 0, Row, Column, Angle, HomMat2D)
* 缩放矩阵生成
hom_mat2d_scale(HomMat2D, Scale, Scale, Row, Column, HomMat2DScale)
* 对模板进行仿射变换
affine_trans_contour_xld(ModelContours, ContoursAffinTrans2, HomMat2DScale)




```



```csharp
* 获取窗口的句柄
dev_get_window (WindowHandle)

* 读取图像
read_image (Image1, 'C:/Users/Jumy/Desktop/Halcon-AOI教学资料/Halcon-AOI教学资料/程序与图像/06几何定位/1.几何模板制作/1.bmp')

* 绘制一个区域，把瓶盖的图像抠出来，做成几何模板

draw_rectangle1 (WindowHandle, Row1, Column1, Row2, Column2)
gen_rectangle1 (Rectangle, Row1, Column1, Row2, Column2)

* 获取区域图像
reduce_domain (Image1, Rectangle, ImageReduced)

* 创建几何模板
create_scaled_shape_model (ImageReduced, 'auto', -0.39, 0.79, 'auto', 0.9, 1.1, 'auto', 'auto', 'use_polarity', 'auto', 'auto', ModelID)


* 做完模板之后，模板的中心点在（0，0）
get_shape_model_contours (ModelContours, ModelID, 1)




find_scaled_shape_model (Image1, ModelID, -0.39, 0.78, 0.9, 1.1, 0.5, 1, 0.5, 'least_squares', 0, 0.9, Row, Column, Angle, Scale, Score)


dev_display_shape_matching_results (ModelID, 'red', Row, Column, Angle, 1, 1, 0)



* 模板匹配
read_image (Image2, 'C:/Users/Jumy/Desktop/Halcon-AOI教学资料/Halcon-AOI教学资料/程序与图像/06几何定位/1.几何模板制作/2.bmp')



* 新盖子的位置，角度、缩放系数（相对于模板是变大了还是变小，和模板的盖子一样大就是1）、分数（相似度，越像就是越接近1）

*** 输入参数
* 检测图像
* 创建模板
* 匹配开始角度
* 角度范围
* 最小缩放系数
* 最大缩放系数
* 最小分数（新图像盖子和模板盖子的相似度）
* 匹配个数
* 重叠度
* 压像素
* 金字塔层数
* 贪婪度
find_scaled_shape_model (Image2, ModelID, rad(-180), rad(360), 0.9, 1.1, 0.5, 1, 0.5, 'least_squares', 0, 0.9, Row3, Column3, Angle1, Scale1, Score1)


* 仿射变换的使用



* 仿射变换，平移 旋转 缩放

* 生成单位矩阵
hom_mat2d_identity (HomMat2DIdentity)


* 缩放
hom_mat2d_scale (HomMat2DIdentity, Scale1, Scale1, 0, 0, HomMat2DScale)

* 旋转
hom_mat2d_rotate (HomMat2DScale, Angle1, 0, 0, HomMat2DRotate)

* 平移的效果
hom_mat2d_translate (HomMat2DRotate, Row3, Column3, HomMat2DTranslate)




affine_trans_contour_xld (ModelContours, ContoursAffineTrans, HomMat2DTranslate)


```

