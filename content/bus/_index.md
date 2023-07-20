---
title: 第六章 总线
type: docs
weight: 6
---

# 总线

每个总线内部可能是多根信号线，所以是可以并行传输多少位的数据的

同一时刻只能有一个部件发送（分时），而总线上多个设备都可以接收（共享）

<img src="https://cdn.jsdelivr.net/gh/zvictorliu/typoraPics@main/img/image-20230720115247869.png" alt="image-20230720115247869" style="zoom: 50%;" />

## 分类

{{< columns >}} <!-- begin columns block -->

串行总线：（如USB）

<img src="https://cdn.jsdelivr.net/gh/zvictorliu/typoraPics@main/img/image-20230720115807560.png" alt="image-20230720115807560" style="zoom:67%;" />

<---> <!-- magic separator, between columns -->

并行总线：干扰会更明显

<img src="https://cdn.jsdelivr.net/gh/zvictorliu/typoraPics@main/img/image-20230720115755952.png" alt="image-20230720115755952" style="zoom:67%;" />

{{< /columns >}}

注意串行并不一定比并行慢，带宽可能更高（频率影响）

- 片内总线：CPU芯片内部的寄存器与寄存器、ALU之间的连接线
- 系统总线：
  - 数组总线：位数与机器字长和存储字长相关
  - 地址总线：和主存地址空间、设备数量（统一编址）有关，单向（CPU -> MEM）
  - 控制总线：单根是单向的
- 通信总线：连接不同计算机

