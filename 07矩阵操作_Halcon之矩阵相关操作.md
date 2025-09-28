## Halcon之矩阵相关操作

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/e2feacfe94c94f5d88ad50fe10803619.png)


```
* 矩阵的创建（多维数组）
* 2行2列
create_matrix (2, 2,[1,2,3,4], MatrixID)
* 3行2列
create_matrix (3, 2,[1,2,3,4,5,6], MatrixID2)

* 获取矩阵中的值
* 获取单个值
get_value_matrix (MatrixID, 1, 0, Value)
* 获取所有值
get_full_matrix (MatrixID, Values)


* 矩阵的逆
invert_matrix (MatrixID, 'general', 0, MatrixInvID)
* 获取逆值
get_full_matrix (MatrixInvID, Values1)


* 矩阵的范数
norm_matrix (MatrixInvID, '2-norm', Value1)

```

