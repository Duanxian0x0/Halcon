## Halcon语法基础


```
read_image (Image, 'printer_chip/printer_chip_01')

* 区域
threshold (Image, Region, 128, 255)
* 轮廓
gen_contour_region_xld (Region, Contours, 'border')


a := 1

b:=1.1

c:= 'xxx'

if (a>2)
    a :=3
endif


if(a>2)
    
elseif(a >9)
    
endif


for Index := 1 to 5 by 1
    a := Index
endfor
```

```cpp
* 元组类型
* 整数型

* := 是赋值意思
* 2赋值给了a

* 整数类型
a := 2

* 带小数点，就浮点类型
b := 1.2

* 两个单引号之间表示，就是字符串类型
ccc:= 'sdadas22++在'

* 图像类型： 图像 区域 轮廓

* 算子，方法，函数，三个名词是一个意思
* 输入参数，输出参数
* read_image: 算子，方法，函数
* 输出参数：Image
* 输入参数：‘’字符串，需要图像的路径
read_image (Image1, 'C:/Users/Jumy/Desktop/Halcon-AOI教学资料/Halcon-AOI教学资料/程序与图像/01入门程序/1.bmp')
* 这个 'printer_chip/printer_chip_01'是halcon自带的，前面已经有了一个默认路径
read_image (Image, 'printer_chip/printer_chip_01')

* 图像类型

* 区域类型
get_domain (Image, Domain)

* 轮廓类型(边缘轮廓线)
gen_contour_region_xld (Domain, Contours, 'border')

* 快捷键
* tab键，自动补全代码
read_image (Image2, 'printer_chip/printer_chip_01')
* 单步运行：F6

* 重置快捷键：F2

* 运行所有：F5

* F4 :注释，就是不启用代码，进行说明

* F3：取消注释

* IF： 如果

if(a>8)
    a:=999
endif


* for 循环逻辑
* 从1运行到5，每隔1运行一次
g := 1
for Index := 1 to 5 by 1
    g:=g+1
endfor

* 从1运行到5，每隔2运行一次
g := 1
for Index := 1 to 5 by 2
    g:=g+1
endfor
```

