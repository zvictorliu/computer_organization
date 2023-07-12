---
title: 3.2 主存与CPU连接
type: docs
weight: 2
---

# 主存与CPU连接

这样的基本连接模型：

<img src="https://cdn.jsdelivr.net/gh/zvictorliu/typoraPics@main/img/image-20230712202920910.png" alt="image-20230712202920910" style="zoom:70%;" />

当CPU的DB和AB长度和存储器不一致时，需要进行扩展：

​	字扩展：增大存储单元数量

​	位扩展：增大存储芯片字长

## 单块

一块存储芯片：

<img src="https://cdn.jsdelivr.net/gh/zvictorliu/typoraPics@main/img/image-20230712203018766.png" alt="image-20230712203018766" style="zoom:70%;" />

地址线--存储单元个数，数据线--存储字长，控制线，片选线

而经常是总线数量比接口多，于是有位扩展和字扩展

## 多块

### 位扩展

每块只有一位 ($8K \times 1 bit$)，合起来

<img src="https://cdn.jsdelivr.net/gh/zvictorliu/typoraPics@main/img/image-20230712203351125.png" alt="image-20230712203351125" style="zoom:70%;" />

 ### 字扩展

扩展容量（AB比地址接口多，寻址能力没有充分发挥）

取高位地址为片选信号:

​	线选法：最直接的，有且只有一位使能，高位数量n对应块数量n，地址空间不连续（00 11这样的无效）

​	译码片选法：采用非门，能够利用的就更多了，高位数量n对应块数量$2^n$

### 字位同时扩展

<img src="https://cdn.jsdelivr.net/gh/zvictorliu/typoraPics@main/img/image-20230712204640890.png" alt="image-20230712204640890" style="zoom:67%;" />

横向是字扩展，纵向是位扩展 

## 译码器

CPU的MAR送出地址信号，<u>待稳定后</u>CPU再发出存储器请求信号使能译码器，进一步使能存储器

即译码器有多个使能同时作用才有效：

![image-20230712205454434](https://cdn.jsdelivr.net/gh/zvictorliu/typoraPics@main/img/image-20230712205454434.png)



说存储器容量表示为 xxB时是字数*字长的结果 