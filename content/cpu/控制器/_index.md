---
bookCollapseSection: true
title: 5.3 控制器
type: docs
weight: 3
---

# 控制器

CU每一个节拍就发出一个微命令，完成一个微操作

同一个微操作在不同周期都可能出现

简化设计：定长机器周期，即每种周期的长度相等，若实际永不了那么多，则先等待再在末尾的几个节拍执行

现在需要知道如何根据各自信息在当前节拍需要发出什么样的微命令：

![image-20230718134842418](https://cdn.jsdelivr.net/gh/zvictorliu/typoraPics@main/img/image-20230718134842418.png)

仍是各种逻辑电路，当特定的输入后特定的C就会有效，这就控制了微操作的执行

