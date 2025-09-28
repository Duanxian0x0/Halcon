## halcon之机器视觉工业应用通用流程


```
0.参数设置
1.读取图像 
2.定位
3.获取ROI（感兴趣）区域
4.图像预处理 
5.图像算法处理
6.结果输出 
```


![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/527d758c7a95416ea2cf586097ce903a.png)


```
******************************************************************
************************* 工业视觉应用一般流程 *********************
******************************************************************
****************************** 0.参数设置 ******************************
* 获取窗口句柄（句柄可以代表窗口）
dev_get_window (WindowHandle)
* 缺陷的最小面积
minDefectArea :=200
****************************** 1.读取图像 ******************************
* 读取图像
read_image (Image, '1.bmp')

****************************** 2.定位（Blob分析） *********************************
*** 定位内圆
* <2.1> 阈值分割
threshold (Image, Region, 150, 255)
* <2.2> 连通
connection (Region, ConnectedRegions)
* <2.3> 填充
fill_up (ConnectedRegions, RegionFillUp)
* <2.4> 通过特征筛选特征区域
* 选取最大的区域
select_shape_std (RegionFillUp, SelectedRegions, 'max_area', 70)


****************************** 3.获取ROI（感兴趣）区域 ******************
* 拟合最小外接圆
smallest_circle (SelectedRegions, Row, Column, Radius)
* 生成圆区域
gen_circle (ROIRegion, Row, Column, Radius)
* 修整，去除干扰区域
erosion_circle (ROIRegion, RegionErosion1, 15)
* 获取区域对应的图像
reduce_domain (Image, RegionErosion1, ImageReduced)


****************************** 4.图像预处理(缺陷进行凸显) *****************************
* 图像增强：亮的更亮，暗的更暗，增强对比度
mult_image (ImageReduced, ImageReduced, ImageResult, 0.008, 0)

****************************** 5.图像算法处理 ***************************
* <5.1 图像分割>
* 核心：缺陷提取
threshold (ImageResult, Region1, 0, 180)
* 打散
connection (Region1, ConnectedRegions1)
* 根据面积选取特征区域
select_shape (ConnectedRegions1, SelectedRegions1, 'area', 'and', minDefectArea, 99999)

****************************** 6.结果输出 *******************************
count_obj (SelectedRegions1, Number)
if(Number>0)
    * 设置显示文本颜色
    dev_set_color ('red')
    Text := 'NG'
    * 显示缺陷区域
    dev_display (SelectedRegions1)
    * 显示文本
    disp_message (WindowHandle, Text, 'window', 12, 12, '', 'false')
endif




```




```
****************************** 0.参数设置 ******************************
* 获取窗口的句柄
dev_get_window (WindowHandle)
* 缺陷最小面积
minDefectArea :=200


****************************** 1.读取图像 ******************************
read_image (Image1, 'C:/Users/Jumy/Desktop/Halcon-AOI教学资料/Halcon-AOI教学资料/程序与图像/02halcon之机器视觉工业应用通用流程/1.bmp')




****************************** 2.定位（Blob分析） *********************************
* 定位：不在整张图像上检测，就只检测瓶盖这个区域
* 阈值分割
threshold (Image1, Region, 130, 255)
* 填充
fill_up (Region, RegionFillUp)
* 打散
connection (RegionFillUp, ConnectedRegions)
* 筛选面积最大的区域
select_shape_std (ConnectedRegions, SelectedRegions, 'max_area', 70)


****************************** 3.获取ROI（感兴趣）区域图像 ******************
* 拟合最小外接圆
smallest_circle (SelectedRegions, Row, Column, Radius)
* 生成圆区域
gen_circle (ROIRegion, Row, Column, Radius)
* 修整，去除干扰区域
erosion_circle (ROIRegion, RegionErosion1, 15)
* 获取区域对应的图像（核心算子：把对应区域上的图像提取出来）
reduce_domain (Image1, RegionErosion1, ImageReduced)

****************************** 4.图像预处理(缺陷进行凸显) *****************************

* 图像增强：亮的更亮，暗的更暗，增强对比度
mult_image (ImageReduced, ImageReduced, ImageResult, 0.008, 0)


****************************** 5.图像算法处理 ***************************
* <5.1 图像分割>
* 核心：缺陷提取
threshold (ImageResult, Region1, 0, 180)
* 打散
connection (Region1, ConnectedRegions1)
* 根据面积选取特征区域
select_shape (ConnectedRegions1, SelectedRegions1, 'area', 'and', minDefectArea, 99999)

****************************** 6.结果输出 *******************************
* 计算区域的数量
count_obj (SelectedRegions1, Number)
if(Number>0)
    * 设置显示文本颜色
    dev_set_color ('red')
    Text := 'NG'
    * 显示缺陷区域
    dev_display (SelectedRegions1)
    * 显示文本
    disp_message (WindowHandle, Text, 'window', 12, 12, '', 'false')
endif
```

