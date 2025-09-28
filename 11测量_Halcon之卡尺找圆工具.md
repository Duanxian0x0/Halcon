## Halcon之卡尺找圆工具


![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/e49927dfbdb34947900708787711aeac.png)



```csharp
read_image (Image, '2.bmp')
dev_display (Image)
dev_set_color ('red')
dev_get_window (WindowHandle)


* 卡尺个数
Elements:=10
* 检测高度
DetectHeight:=600
* 检测宽度
DetectWidth:=30

**************
* 一 ROI
**************
draw_Circle_Tool (Image, Regions, WindowHandle, Elements, DetectHeight, DetectWidth, \
            ROIRows, ROICols, Direct)

**************
* 二 边缘查找
**************
* 极性
Transition:='negtive'
* 选择第几个
Select:='first'
* 平滑系数
Sigma:=2
* 阈值
Threshold:=20

measureCircle (Image, Regions, Elements, DetectHeight, DetectWidth, Sigma, Threshold, \
       Transition, Select, ROIRows, ROICols, Direct, ResultRow, ResultColumn, ArcType)

**************
* 三 拟合圆
**************
* 最少拟合点数
ActiveNum:=5
fit_measure_circle (Circle, ResultRow, ResultColumn, ActiveNum, ArcType, RowCenter, \
                    ColCenter, Radius, StartPhi, EndPhi, PointOrder)

dev_set_line_width (3)
dev_set_color ('blue')
dev_display (Circle)
```

