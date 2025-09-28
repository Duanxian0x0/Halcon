## 检测解决策略之一blob分析+特征分析-05（划伤检测）

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/71851c77559744f4a0c3bf234c63efd0.png)

> 明星算子
> skeleton :骨干检测


```csharp
read_image (Image, 'surface_scratch')

* blob分析 +特征分析（骨干+面积）

mean_image (Image, ImageMean, 9, 9)

dyn_threshold (Image, ImageMean, RegionDynThresh, 10, 'dark')

connection (RegionDynThresh, ConnectedRegions)

* 去杂质
select_shape (ConnectedRegions, SelectedRegions, 'area', 'and', 10, 99999)

union1 (SelectedRegions, RegionUnion)

* 利于找到骨干
dilation_circle (RegionUnion, RegionDilation, 3.5)

skeleton (RegionDilation, Skeleton)

connection (Skeleton, ConnectedRegions1)

* 面积(划伤)
select_shape (ConnectedRegions1, SelectedRegions1, 'area', 'and', 50, 99999)

* 点状缺陷
select_shape (SelectedRegions1, SelectedRegions2, 'area', 'and', 15, 50)
```

```csharp
* 读取图像
read_image (Image, 'surface_scratch')
* 均值滤波
mean_image (Image, ImageMean, 7, 7)
* 动态阈值分割
dyn_threshold (Image, ImageMean, DarkPixels, 5, 'dark')
* 连通
connection (DarkPixels, ConnectedRegions)
* 去杂志
select_shape (ConnectedRegions, SelectedRegions, 'area', 'and', 10, 1000)
* 合并
union1 (SelectedRegions, RegionUnion)
* 膨胀（更好的提取骨干）
dilation_circle (RegionUnion, RegionDilation, 3.5)
* 获取骨干（重要技巧）
skeleton (RegionDilation, Skeleton)
* 连通
connection (Skeleton, Errors)
* 通过面积选取划伤
select_shape (Errors, Scratches, 'area', 'and', 50, 10000)
* 通过面积选取点状缺陷
select_shape (Errors, Dots, 'area', 'and', 1, 50)
```

