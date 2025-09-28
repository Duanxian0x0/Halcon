## 检测解决策略之一blob分析+特征分析-03（塑料网孔检测）

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/8113b9cfc7e9488da4f70b80a2771699.png)

```csharp


read_image (Image, 'plastic_mesh/plastic_mesh_06')

read_image (Image, 'plastic_mesh/plastic_mesh_01')

* Blob分析（动态阈值分割）+特征分析（面积）


mean_image (Image, ImageMean, 49, 49)

dyn_threshold (Image, ImageMean, RegionDynThresh, 5, 'dark')

* 打散
connection (RegionDynThresh, ConnectedRegions)


* 筛选

select_shape (ConnectedRegions, SelectedRegions, 'area', 'and', 400, 99999)

count_obj (SelectedRegions, Number)

if(Number>0)
    dev_get_window (WindowHandle)
    disp_message (WindowHandle, 'NG', 'window', 12, 12, 'red', 'true')
else
    dev_get_window (WindowHandle)
    disp_message (WindowHandle, 'OK', 'window', 12, 12, 'green', 'true')
endif
```

```csharp
* 窗口设置
dev_update_window ('off')
* 读取图像
read_image (Image, 'plastic_mesh/plastic_mesh_01')
* 关闭窗口
dev_close_window ()
* 获取图像大小
get_image_size (Image, Width, Height)
* 设置窗口显示
dev_open_window_fit_image (Image, 0, 0, Width, Height, WindowHandle)
set_display_font (WindowHandle, 18, 'mono', 'true', 'false')
dev_set_draw ('margin')
dev_set_line_width (3)
* 遍历
for J := 1 to 14 by 1
    * 读取图像
    read_image (Image, 'plastic_mesh/plastic_mesh_' + J$'02')
    * 中值滤波
    mean_image (Image, ImageMean, 49, 49)
    * 动态阈值分割
    dyn_threshold (Image, ImageMean, RegionDynThresh, 5, 'dark')
    * 连通
    connection (RegionDynThresh, ConnectedRegions)
    * 通过面积选取特征区域
    select_shape (ConnectedRegions, ErrorRegions, 'area', 'and', 500, 99999)
    * 计数
    count_obj (ErrorRegions, NumErrors)
    * 显示结果
    dev_display (Image)
    dev_set_color ('red')
    dev_display (ErrorRegions)
    * If the number of errors exceeds zero, the message 'Mesh not
    * OK' is displayed. Otherwise the web is undamaged
    * and 'Mesh OK' is displayed.
    if (NumErrors > 0)
        disp_message (WindowHandle, 'Mesh not OK', 'window', 24, 12, 'black', 'true')
    else
        disp_message (WindowHandle, 'Mesh OK', 'window', 24, 12, 'black', 'true')
    endif
    * If the sequence number of the image to be inspected is
    * lower than 14, the request to press 'Run' to continue appears.
    * If the last image is read, pressing 'Run' will clear the SVM.
    if (J < 14)
        disp_continue_message (WindowHandle, 'black', 'true')
        stop ()
    endif
endfor


```

