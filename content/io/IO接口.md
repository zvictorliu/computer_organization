---
title: 7.1 IO接口
type: docs
weight: 1
---

# IO接口

<img src="https://cdn.jsdelivr.net/gh/zvictorliu/typoraPics@main/img/image-20230720132332767.png" alt="image-20230720132332767" style="zoom:67%;" />

## 功能

- 数据缓冲

- 错误或状态监测

- 控制和定时设备

- 数据格式转换：串并转换

## 结构

![image-20230720132904050](https://cdn.jsdelivr.net/gh/zvictorliu/typoraPics@main/img/image-20230720132904050.png)

设备侧：可以一个IO接口连多个设备

<img src="https://cdn.jsdelivr.net/gh/zvictorliu/typoraPics@main/img/image-20230720133038984.png" alt="image-20230720133038984" style="zoom: 50%;" />

主机侧：

- 主机CPU发送命令字到IO控制寄存器（格式需要设备驱动程序）

- 主机也可以从状态寄存器读出状态字以获取设备状态信息（轮询or通过控制线发出中断）

- 主机通过数据缓冲寄存器进行发送或读取

由于IO控制逻辑从控制寄存器读取出命令字之和就不需要了，只用到一次，命令和状态在时间上错开，因此一般状态和控制是同一个寄存器只是数据传输方向不同

各种寄存器被称为`IO端口`，通过地址线指明对哪个寄存器进行交换（多个设备，每个设备一组寄存器，对不同组的寄存器操作就是对不同设备进行操作）

控制线：CPU发出读写信号，设备发出中断请求

中断类型信号还是经过数据线传递的

## IO端口的编址

统一编址：在主存的地址空间，根据地址码的范围就区分了是访存还是访问外设，访问外设的指令和访存是一样的

独立编址：就需要特定的IO指令 `IN/OUT N`

<img src="https://cdn.jsdelivr.net/gh/zvictorliu/typoraPics@main/img/image-20230720134353983.png" alt="image-20230720134353983" style="zoom:67%;" />