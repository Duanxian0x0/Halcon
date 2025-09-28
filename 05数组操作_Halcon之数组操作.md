## Halcon之数组操作

>  元组数组的索引是从0开始的
> 图像数组的索引是从1开始的

```
a :=1 
a := [1,2,3]

 * 以 []  ,间隔，表示每个数
* 1. 定义数组
Tuple1 := [1,2,3,4,5,6,7,8,9]
 
 
* 2.给Tuple数组元素赋值
Tuple1[1] := 0
 
 Tuple1[2] := 999
 
 
* 3.批量改变数组元素的值

Tuple1[1,3,5] := 'wh'
 
 
* 4.批量给Tuple数组赋值,其值为0到100连续数值
Tuple3 := [0:100]
 
* 5.批量给Tuple数组赋值,其值为3到100连续数值,步长为2
Tuple4 := [3:2:100]
 
 
* 6.批量给Tuple数组赋值,其值为100到-100连续数值,步长为-10
Tuple5 := [100:-10:-100]
 
 
 
* 7.对两个Tuple数组进行合并操作（去重操作）
TupleInt1 := [3,1,2,9,1]
TupleInt2 := [10,2,4,3,2]
tuple_union (TupleInt1, TupleInt2, UnionInt)
 
 
 
* 8.对两个Tuple数组进行交集操作
TupleInt3 := [3,1,2,9,1]
TupleInt4 := [10,2,4,3,2]
tuple_intersection (TupleInt3, TupleInt4, IntersectionInt)
 
 
 
* 9.对Tuple数组元素进行替换
OriginalTuple := [99,101,2,3,4,5]
* [0,1]
tuple_replace (OriginalTuple, [0,1], ['x','y'], Replaced)
 
 
 
* 10.向Tuple数组插入数值
OriginalTuple := [0,1,2,3,4,5]
tuple_insert (OriginalTuple, 3, 'x', InsertSingleValueA)


* 11.数组加法
t1:= [5,7,8,9]
t2 :=t1+1

* 12.获取数组长度
tuple_length (t2, Length)


* 13.获取局部值
t4 := t2[0:2]


* 14.循环赋值
for i :=0 to Length by 1
    t2[i] := 255
endfor


```

