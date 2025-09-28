## Halcon之卡尺找线工具

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/fbf67004048d441789e0ed600dc8238c.png)


```csharp

read_image(Image, '矩形.png')
dev_get_window (WindowHandle)
dev_set_color ('green')

***********************
*一 创建ROI （draw_rake） 可视化
***********************
* 卡尺个数
Elements:=20
* 卡尺高度
DetectHeight:=50
* 卡尺宽度
DetectWidth:=5
* 创建ROI
* 方向：从左往右边画是向上，从右往左画是向下
draw_Line_Tool (Regions, WindowHandle, Elements, DetectHeight, DetectWidth, Row1, Column1, Row2, Column2)


***********************
*二  边缘测量 （rake函数）测量
***********************
* 平滑系数
Sigma:=3
* 边缘阈值
Threshold:=30
* 极性， 'negative'：有明到暗 ，'positive'：由暗到明。'all':所有极性
Transition:='positive'
* 找第几条线:'all':所有，'first':第一条
Select:='all'
* 边缘测量（卡尺出边缘的点）
measureLine (Image, Regions, Elements, DetectHeight, DetectWidth, Sigma, Threshold, Transition, Select, Row1, Column1, Row2, Column2, ResultRow, ResultColumn)


***********************
*三  拟合直线 （pts_to_best_line）  拟合
***********************
* 有效点数（当边缘数量不小于有效点数时进行拟合）
ActiveNum:=5
* 忽略点数（忽略几个点去拟合直线）
IgnoreNum :=3
fit_measure_line (Line, ResultRow, ResultColumn, ActiveNum, IgnoreNum, Row1, Column1, Row2, Column2)


dev_set_color ('red')
dev_set_line_width (3)
dev_display (Line)

```

