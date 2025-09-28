## Halcon之多线程

```cpp
******************************************************************
************************* halcon实现多线程*************************
******************************************************************
* 开启多个子线程
par_start <ThreadID1> : read_image (Image1, 'printer_chip/printer_chip_01')
par_start <ThreadID2> : read_image (Image2, 'printer_chip/printer_chip_01')
par_start <ThreadID1> : read_image (Image3, 'printer_chip/printer_chip_01')
par_start <ThreadID2> : read_image (Image4, 'printer_chip/printer_chip_01')
par_start <ThreadID1> : read_image (Image5, 'printer_chip/printer_chip_01')
par_start <ThreadID2> : read_image (Image6, 'printer_chip/printer_chip_01')
par_start <ThreadID1> : read_image (Image7, 'printer_chip/printer_chip_01')
par_start <ThreadID2> : read_image (Image8, 'printer_chip/printer_chip_01')
par_start <ThreadID1> : read_image (Image9, 'printer_chip/printer_chip_01')
par_start <ThreadID2> : read_image (Image10, 'printer_chip/printer_chip_01')
* 等到多个子线程都结束（从这里开始执行子线程）
par_join ([ThreadID1, ThreadID2])


```

