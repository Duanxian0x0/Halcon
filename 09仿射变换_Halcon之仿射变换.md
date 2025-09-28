## Halcon之仿射变换

### **仿射变换（Affine Transformation）**

- **平移**：仅改变位置，不改变形状和大小。
- **缩放**：均匀或非均匀地改变大小。
- **旋转**：绕某点旋转。
- **剪切**：使形状发生倾斜。

```csharp
* 获取窗口
dev_get_window (WindowHandle)

* 绘制矩阵
draw_rectangle2 (WindowHandle, Row, Column, Phi, Length1, Length2)

gen_rectangle2 (Rectangle, Row, Column, Phi, Length1, Length2)


* 仿射变换：依靠仿射矩阵，对对象进行平移、旋转、缩放等操作

* 生成单位矩阵
hom_mat2d_identity (HomMat2DIdentity)





****(以某一点为旋转中心，进行旋转)*****
* 旋转（旋转物体的中心点为旋转中心）
* 获取旋转中心
area_center (Rectangle, Area, Row1, Column1)
* 旋转仿射矩阵的生成
hom_mat2d_rotate (HomMat2DIdentity, rad(90), Row1 ,Column1, HomMat2DRotate)
* 仿射变换
affine_trans_region (Rectangle, RegionAffineTrans, HomMat2DRotate, 'nearest_neighbor')

****(以某一点为缩放中心，进行水平和垂直缩放)*****
* 缩放 1.5 表示垂直方向的系数，3 表示x水平方向的缩放系数，缩放原点）
hom_mat2d_scale (HomMat2DIdentity, 1.5, 3,Row1 , Column1, HomMat2DScale)
affine_trans_region (Rectangle, RegionAffineTrans1, HomMat2DScale, 'nearest_neighbor')

****（水平或者垂直移动）*****
* 平移(100 表示垂直方向上的移动，200表示水平方向的移动)
hom_mat2d_translate (HomMat2DIdentity, 100, 200, HomMat2DTranslate)
affine_trans_region (Rectangle, RegionAffineTrans2, HomMat2DTranslate, 'nearest_neighbor')
```

