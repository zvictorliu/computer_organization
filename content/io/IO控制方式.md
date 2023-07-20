---
title: 7.2 IO控制方式
weight: 2
---

# IO控制方式

## 程序查询方式

就传输一大段数据来说：

- 初始化，设置传输参数
- 发出命令字以启动
- 读取状态信息检查，没准备好就继续取-查
- 准备好，传送一次数据
- 修改传输参数（计数器）
- 轮询是否传送完成

{{< details "例题：检查输入和进行输出" >}}
![image-20230720140953320](https://cdn.jsdelivr.net/gh/zvictorliu/typoraPics@main/img/image-20230720140953320.png)

（1）鼠标：30次查询一共多少时间占1s的比例

（2）硬盘：1s内传输的 $2 \times 2^{20}B$的数据，每32位就查询一次，一共需要查询多少次，一共多少时间？

{{< /details >}}



轮询：

- 独占查询
- 定时查询

## 程序中断方式

- 中断检查：

  每个指令的末尾都会对中断请求进行检查

  中断请求标记寄存器：<img src="https://cdn.jsdelivr.net/gh/zvictorliu/typoraPics@main/img/image-20230720142531214.png" alt="image-20230720142531214" style="zoom:80%;" />

- 中断判优：多个中断的响应顺序

  硬件排队器

  <img src="https://cdn.jsdelivr.net/gh/zvictorliu/typoraPics@main/img/image-20230720142717757.png" alt="image-20230720142717757" style="zoom:67%;" />

  软件：按优先级查询先处理

- 中断处理

  中断隐指令：

  保存原来的PC值（这些是原子操作，需要先关中断）；

  获取入口：硬件产生向量地址（中断类型号），根据中断向量找到入口地址

  <img src="https://cdn.jsdelivr.net/gh/zvictorliu/typoraPics@main/img/image-20230720143459373.png" alt="image-20230720143459373" style="zoom:67%;" />

  这样的设计便于修改

- 中断服务程序：

  保护现场和恢复现场是在服务程序里面的

  前后两段原子操作，中间才是真正的服务

  屏蔽字：限制特定的中断不响应，以确保优先级

PSW中的标志位标识现在是开中断还是关中断



{{< expand "补充" >}}
<img src="https://cdn.jsdelivr.net/gh/zvictorliu/typoraPics@main/img/image-20230720145426347.png" alt="image-20230720145426347" style="zoom:80%;" />
在开中断的情况下使能，设备发出中断到IRR，经过优先权电路选择最先处理的哪个到ISR，将中断类型号通过数据总线告诉CPU，然后使能INTR告诉有中断了

{{< /expand >}}

## DMA方式

### 组成

DMA控制器：

指明从外设中要读取的地址，以及要存在内存中的地址

从磁盘一个字一个字的读取，所以有一个数据缓冲寄存器，控制器收到一个就放入主存（外设是主动发送），然后修改地址继续

DMA控制器写入主存也占用系统总线，并不是直接相连（单总线结构）

DMA请求：写满一个字后，设备发出DMA请求，DMA控制器收到后去向CPU请求总线使用，得到后就开始向内存写数据

中断请求：当计数器溢出意味着全部传送完，发出中断

<img src="https://cdn.jsdelivr.net/gh/zvictorliu/typoraPics@main/img/image-20230720152326530.png" alt="image-20230720152326530" style="zoom:67%;" />

### 传送方式

如果是三总线结构：

<img src="https://cdn.jsdelivr.net/gh/zvictorliu/typoraPics@main/img/image-20230720121149876.png" alt="image-20230720121149876" style="zoom:67%;" />

就不需要占用系统总线了，但是他可能和CPU对主存的访问冲突

- 停止CPU访问主存

  <img src="https://cdn.jsdelivr.net/gh/zvictorliu/typoraPics@main/img/image-20230720152712430.png" alt="image-20230720152712430" style="zoom:67%;" />

- DMA和CPU交替访存

  <img src="https://cdn.jsdelivr.net/gh/zvictorliu/typoraPics@main/img/image-20230720152740282.png" alt="image-20230720152740282" style="zoom:67%;" />

  但是这种由于DMA并不总是经常使用主存，有点浪费CPU周期

- 周期挪用

  CPU没有访存时无冲突使用

  CPU正在访存时还在存取周期，那就DMA等待

  CPU和DMA同时发出请求，优先满足DMA

### 传输过程

- 预处理：CPU完成准备工作，包括测试、设置DMA控制器的寄存器初值等
- 数据传送
- 后处理：DMA发出中断，CPU响应，进行校验等等
