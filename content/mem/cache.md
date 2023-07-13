---
title: 3.4 cache
weight: 4
type: docs
---

# 高速缓冲存储器

将更常用的拿出来放到速度更快的地方（局部性原理）

空间局部性：下一次需要到就在这次旁边

时间局部性：下一次很可能用到同样的信息

CPU优先去查找cache，有命中和不命中的可能，设命中概率为$H$
$$
t = Ht_c + (1-H)(t_c + t_m)
$$
而如果同时访问cache和主存，cache一旦找到就停下查找主存：
$$
t = Ht_c + (1-H)t_m
$$
主存分块（也叫页），以块为单位进行交换

​	块号+块内地址

每次在主存当中找到，就把所在块调入cache

## cache和主存的映射

主存块以何种规则放到cache块的哪个位置

<img src="https://cdn.jsdelivr.net/gh/zvictorliu/typoraPics@main/img/image-20230713161101261.png" alt="image-20230713161101261" style="zoom:67%;" />

在cache中应当记录其来自何处：主存块号

还应记录是否有效：有效位（块号是从0开始的，不能通过正数来区分）

### 随意放：全相联映射

可以放到任何位置

访存：由主存地址换算出块号，在cache中检索

### 直接映射：固定位置

一般是块号余cache总块数

如果cache总块数也是$2^n$那么可以直接通过主存块号的低$n$位来得到cache位置块号

<img src="https://cdn.jsdelivr.net/gh/zvictorliu/typoraPics@main/img/image-20230713162138621.png" alt="image-20230713162138621" style="zoom:50%;" />

也因为这样，标记就不需要存末尾几位

所以访存是将地址的块号的末尾几位去检查对应的cache块，就不需要检索了，主存地址理解为：标记位-cache块号-块内地址

### 组相联：放到特定分组的任意位置

余组号

主存地址理解为：标记位-cache组号-块内地址

n路组相联：每n个cache为一组

## 替换算法

全相联：全满后才会考虑替换

直接映射：如果所属位置被占了那就直接替换它

组相联：组满才考虑组内替换

- RAND：随机算法

- FIFO：

- **LRU**：最近最少使用

  计算器记录主存块多久没有被访问了

  细节：*其余不变的意义*

- LFU：最不经常使用

  反过来，将访问次数最少的换出，记录的是被访问过多少次

  出现相等时，FIFO或者行号递增的顺序

  需要很大的计数器；浓度影响

## 写策略：一致性

当修改时，修改了cache中的副本，那么需要同步到主存

### 写命中

- 写回法：只在改块被替换时（且是被修改过的）才写回主存，容易数据不一致

- 全写法：修改cache同时也写主存

  这样会影响写的速度，于是设计之间有一个写缓冲，在专门的控制电路上逐一写回 

​		<img src="https://cdn.jsdelivr.net/gh/zvictorliu/typoraPics@main/img/image-20230713165408584.png" alt="image-20230713165408584" style="zoom:57%;" />

​	一旦满了就会阻塞

### 写不命中

- 写分配法：先调入cache写，然后再用写回法
- 非写分配：直接往主存写，也可以搭配写缓冲的全写法，**并不调入cache**（只有读不命中才调入）

对于多级cache，在cache之间数据一致性要求高一些采用全写，而cache-主存则是写回

<img src="https://cdn.jsdelivr.net/gh/zvictorliu/typoraPics@main/img/image-20230713165833690.png" alt="image-20230713165833690" style="zoom:67%;" />