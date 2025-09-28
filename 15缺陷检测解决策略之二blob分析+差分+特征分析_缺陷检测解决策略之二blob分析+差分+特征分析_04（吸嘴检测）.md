## 缺陷检测解决策略之二blob分析+差分+特征分析_04（吸嘴检测）

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/4f2699c86b3649a79723fc260a82dd8d.png)

```csharp
* 差分：带缺陷的区域-不带缺陷的区域 = 缺陷
* 进行特征，获取缺陷区域的面积、宽高等进行是否是NG品的判定

* 读取图像

read_image (Image, 'hull')

* blob分析， 区域提取
threshold (Image, Region, 0, 90)

fill_up (Region, RegionFillUp)

* 先膨胀后腐蚀
closing_rectangle1 (RegionFillUp, RegionClosing, 30, 30)
* 去杂质(不带缺陷区域)
opening_rectangle1 (RegionClosing, RegionOpening, 10, 10)

* 获取带缺陷的区域

shape_trans (RegionOpening, RegionTrans, 'convex')

* 差分
difference (RegionTrans, RegionOpening, RegionDifference)


opening_circle (RegionDifference, RegionOpening1,8)

area_center (RegionOpening1, Area, Row, Column)
if(Area<3000)
    dev_get_window (WindowHandle)
    disp_message (WindowHandle, 'NG', 'window', Row, Column, 'red', 'true')
    
else
        dev_get_window (WindowHandle)
    disp_message (WindowHandle, 'OK', 'window', 12, 12, 'green', 'true')
endif
```

```csharp
******************************************第一步  初始化**************************************************
*读取一张图像

read_image (Hull, 'hull')

*获取图像大小
get_image_size (Hull, Width, Height)

*关闭已经打开的窗口
dev_close_window ()

*打开一个新的窗口
dev_open_window (0, 0, Width, Height, 'black', WindowID)

*显示图像
dev_display (Hull)
 
******************************************第二步  图像处理**************************************************
*阈值操作,分割2.阈值分割出吸嘴

threshold (Hull, Dark, 0, 80)

* 补集运算,获取背景区域
difference (Hull, Dark, Light)

*对背景区域进行连通处理
connection (Light, ConnectedRegions)

*过滤出背景区域
select_shape (ConnectedRegions, NoHullCand, 'area', 'and', 50000, 9999999)

*对过滤的背景区域进行闭运算,填充背景间隙和平滑背景边界
closing_circle (NoHullCand, NoHull, 13.5)

*补集运算,获取吸嘴区域（重要）
difference (Hull, NoHull, Region)

*对吸嘴区域开运算
opening_circle (Region, RegionOpening, 2.5)

*对吸嘴区域进行连通处理
connection (RegionOpening, ConnectedRegions)

*过滤出吸嘴区域
select_shape (ConnectedRegions, RegionHull, 'area', 'and', 5000, 9999999)

*将吸嘴区域转换为凸包区域（重要）

shape_trans (RegionHull, ConvexHull, 'convex')

*补集运算,获取吸嘴的缺口（重要）

difference (ConvexHull, RegionHull, Region)

*对吸嘴缺口区域进行连通处理
connection (Region, ConnectedRegions)

*过滤出吸嘴缺口
select_shape (ConnectedRegions, LargeHoles, 'area', 'and', 2000, 99999)
select_shape (LargeHoles, Holes, 'convexity', 'and', 0, 0.85)

*显示图像
dev_display (Hull)

*设置输出对象的线宽度
dev_set_line_width (5)

*设置区域的填充方式
dev_set_draw ('margin')

*设置输出对象显示颜色为红色
dev_set_color ('red')

*显示吸嘴缺口
dev_display (Holes)


```

