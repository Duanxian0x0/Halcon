## halcon之函数的封装（扩展算子area_center）

## 创建新函数

```
* 读取图像
read_image (Image, 'monkey')

* 阈值分割
threshold (Image, Region, 130, 255)
* 填充
fill_up (Region, RegionFillUp)
* 打散
connection (RegionFillUp, ConnectedRegions)
* 筛选
select_shape (ConnectedRegions, SelectedRegions, 'area', 'and', 800, 1000)
* 腐蚀（去除杂质）
erosion_circle (SelectedRegions, RegionErosion, 3.5)
* 膨胀（去除杂质）
dilation_circle (RegionErosion, RegionDilation, 3.5)
* 合并
union1 (RegionDilation, RegionUnion)
```

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/b39e9f6517154dfdad61067f7c724707.png)
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/33d146c65175485ea471cce6e9a9c188.png)
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/a9df7e63b736478fa4ac732500da745f.png)
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/1ef468b1bd214dfaa7ac9e1c04023b97.png)


## 再修改

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/603092cf1a2c41a5a7a3af96b2ce1b0e.png)


## area_center:获取区域的面积和中心点

```
* 获取眼球区域的中心点和面积
area_center (RegionUnion, Area, Row, Column)

```

