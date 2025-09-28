## 模板、区域、变量、图像的保存和读取

```cpp

******************
* 保存
******************
read_image (Image, 'printer_chip/printer_chip_01')

gen_rectangle1 (Rectangle, 30, 20, 100, 200)

reduce_domain (Image, Rectangle, ImageReduced)
* 保存几何模板
create_shape_model (ImageReduced, 'auto', -0.39, 0.79, 'auto', 'auto', 'use_polarity', 'auto', 'auto', LM_ModelID)
write_shape_model (LM_ModelID, 'D:/LM.shm')
* 保存tuple
TM_Row := 1111
write_tuple (TM_Row, 'D:/TM_Row.tup')
* 保存区域
write_region (Rectangle, 'D:/modelROI.hobj')
* 保存图像
write_image (ImageReduced, 'bmp', 0, 'D:/modelROIImage.bmp')


******************
* 读取
******************
* 读取几何模板
read_shape_model ('D:/LM.shm', L_ModelID)
* 读取tuple
read_tuple ('D:/TM_Row.tup', TM_Row)
* 读取区域
read_region (RM_ROI,'D:/modelROI.hobj')
* 读取图像
read_image (Modelroiimage, 'D:/modelROIImage.bmp')


```

