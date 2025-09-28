## Halcon之提取猴子的眼睛

```
* 读取图像（read_imagej就是算子，也叫函数，也叫方法）
read_image (Image, 'monkey')


* C:/Users/Public/Documents/MVTec/HALCON-20.11-Steady/examples/images

* 一般读取图像需要全路径，但是对于halcon自带的图像，自需要图像名称，或者部分路径即可
read_image (Image1, 'C:/Users/Jumy/Desktop/Halcon-AOI教学资料/Halcon-AOI教学资料/程序与图像/01入门程序/1.bmp')


```
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/618bc60758a440519ae096a7add600b5.png)

## 案例
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

---


## 什么是Blob分析（AOI行业专业名词）
## 阈值分割、填充、打散、腐蚀、膨胀、筛选这个流程通称为Blob分析
