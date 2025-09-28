## 缺陷检测解决策略之一blob分析+特征分析-01（破碎饼干检测）

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/c3fa513aed8447d8ab617759735b533e.png)


> 明星算子：
> area_holes 
> rectangularity 


```csharp
* 读取图像

* blob分析 + 特征分析 解决策略


* 明星算子

* area_holes ：计算区域内空的面积总和
* rectangularity：计算区域有多像矩形：1完全是矩形，0 完全不是矩形

for index := 1 to 24 by 1
    
    * $'.02' ===> 1 --->01
    file_path :='food/hazelnut_wafer_'+ index$'.02'
    * 采集图像
    read_image (Image, 'food/hazelnut_wafer_'+ index$'.02')
    
    *** blob分析：阈值分割为基础
    binary_threshold (Image, Region, 'max_separability', 'light', UsedThreshold)
    
    * 开操作：先腐蚀后膨胀 目的：杂质的去除
    opening_circle (Region, RegionOpening, 8.5)
    
   *** 特征分析的步骤：获取区域特征
   * 检测饼干是否破裂
   
   * (特征1)计算空洞面积的算子去进行计算
   area_holes (RegionOpening, Area)
   * （特征2）矩形度进行一个判别
   rectangularity (RegionOpening, Rectangularity)
   
   * 分析阶段
   * 获取窗口
   dev_get_window (WindowHandle)
   if(Area>1000 or Rectangularity<0.9)
       * 结果的显示
       * 不良品
       disp_message (WindowHandle, 'NG', 'window', 12, 12, 'red', 'true')
       stop()
   else
       * 良品
       disp_message (WindowHandle, 'OK', 'window', 12, 12, 'green', 'true')

   endif
   
   
    
    
endfor
```


```csharp

* 读取图像
read_image (Image, 'food/hazelnut_wafer_01')
****************************************一 窗口显示设置*************************************************************
* 关闭窗口
dev_close_window ()
* 打开窗口
dev_open_window_fit_image (Image, 0, 0, -1, -1, WindowHandle)
* 关闭更新
dev_update_window ('off')
* 设置显示线宽
dev_set_line_width (3)
* 设置显示模式为边缘模式：margin
* dev_set_draw ('margin')
* 设置显示字体大小
set_display_font (WindowHandle, 20, 'mono', 'true', 'false')
****************************************二 算法处理：策略：Blob分析+特征（孔洞的面积和矩形度）分析*************************************************************


* 遍历缺陷图片
for Index := 1 to 24 by 1
    * 1.采集图像

    read_image (Image, 'food/hazelnut_wafer_' + Index$'.02')
    *2.图像分割： 二值化分割

    binary_threshold (Image, Foreground, 'smooth_histo', 'light', UsedThreshold)
    *3.形态学处理 开运算（腐蚀）

    opening_circle (Foreground, FinalRegion, 8.5)
    
    * 计算孔洞面积
    area_holes (FinalRegion, AreaHoles)
    * 计算矩形度
    rectangularity (FinalRegion, Rectangularity)
    * 显示图像
    dev_display (Image)
    * 用孔洞面积和矩形度特征，来判断NG/OK
    if (AreaHoles > 300 or Rectangularity < 0.92)
        * 设置显示颜色为红色
        dev_set_color ('red')
        Text := 'NG'
    else
        dev_set_color ('forest green')
        Text := 'OK'
    endif
    * 显示缺陷区域
    dev_display (FinalRegion)
    * 显示文本
    disp_message (WindowHandle, Text, 'window', 12, 12, '', 'false')
    if (Index < 24)
        *在屏幕右下角显示“单击“运行”以继续”
        disp_continue_message (WindowHandle, 'black', 'true')
        stop ()
    endif
endfor



```

