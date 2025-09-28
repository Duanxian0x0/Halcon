## 缺陷检测解决策略之二blob分析+差分+特征分析_01（毛刺检测）

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/1515e9f868a54e9a87af14428d51ca55.png)

```csharp
* blob分析： 以阈值分割为基础，提取区域
* 差分：把带有缺陷的区域，和不带缺陷的区域，相减，得到缺陷区域
* 特征分析：面积、宽度、高度等待特征信息判定是不是缺陷

* 读取图像

read_image (Image, 'fin1')


* blob分析（提取带缺陷的区域）

binary_threshold (Image, Region, 'max_separability', 'dark', UsedThreshold)

* 获取不带缺陷的区域
* 腐蚀 后膨胀
opening_rectangle1 (Region, RegionOpening, 60,60)
difference (Region, RegionOpening, RegionDifference)

opening_rectangle1 (RegionDifference, RegionOpening1, 10, 10)

* 特征分析
area_center (RegionOpening1, Area, Row, Column)

if(Area>1000)
    dev_get_window (WindowHandle)
    disp_message (WindowHandle,'NG', 'window', 12, 12, 'red', 'true')
endif
```

```csharp
* 窗口设置
dev_update_window ('off')
* 读取图像
read_image (Fins, 'fin' + [1:3])
* 获取图像大小
get_image_size (Fins, Width, Height)
* 设置窗口显示
dev_close_window ()
dev_open_window (0, 0, Width[0], Height[0], 'black', WindowID)
set_display_font (WindowID, 14, 'mono', 'true', 'false')
* 遍历
for I := 1 to 3 by 1
    * 选取图像
    select_obj (Fins, Fin, I)
    * 显示图像
    dev_display (Fin)
    * 二值化分割
     binary_threshold (Fin, Background, 'max_separability', 'light', UsedThreshold)
    binary_threshold (Fin, Foreground, 'max_separability', 'dark', UsedThreshold1)
    * 设置窗口显示
    dev_set_color ('blue')
    dev_set_draw ('margin')
    dev_set_line_width (4)
    dev_display (Background)
    disp_continue_message (WindowID, 'black', 'true')
    stop ()
    * 闭运算
    closing_circle (Background, ClosedBackground, 250)
    * 开运算
     opening_rectangle1 (Foreground, RegionOpening, 50, 100)
    * 设置窗口显示
    dev_set_color ('green')
    dev_display (ClosedBackground)
    disp_continue_message (WindowID, 'black', 'true')
    stop ()
    * 区域相减
    difference (ClosedBackground, Background, RegionDifference)
    
    difference (Foreground, RegionOpening, RegionDifference1)
    * 开运算
    opening_rectangle1 (RegionDifference, FinRegion, 5, 5)
    
     opening_rectangle1 (RegionDifference1, FinRegion2, 5, 5)
    * 显示结果
    dev_display (Fin)
    dev_set_color ('red')
    dev_display (FinRegion)
    * 获取区域面积和中心
    area_center (FinRegion, FinArea, Row, Column)
    if (I < 3)
        disp_continue_message (WindowID, 'black', 'true')
        stop ()
    endif
endfor


```

