---
title: 第五章 中央处理器
type: docs
weight: 5
---

# CPU

运算器+控制器

## 功能

CPU的功能包括：

- 指令控制（顺序性）

- 操作控制（操作信号控制部件进行动作，即执行指令）

- 时间控制

- 数据加工（运算）

- 中断处理

## 结构

### 运算器

`ALU`进行算术逻辑运算，它需要一些寄存器

专用数据通路方式连接：

<img src="https://cdn.jsdelivr.net/gh/zvictorliu/typoraPics@main/img/image-20230718105838026.png" alt="image-20230718105838026" style="zoom:67%;" />

同时应能选择哪个寄存器：多路选择器MUX、三态门

单总线方式：为避免冲突，设置了暂存寄存器

<img src="https://cdn.jsdelivr.net/gh/zvictorliu/typoraPics@main/img/image-20230718105914856.png" alt="image-20230718105914856" style="zoom: 50%;" />

{{< details title="完整的是这样" open=true >}}
<img src="https://cdn.jsdelivr.net/gh/zvictorliu/typoraPics@main/img/image-20230718110219570.png" alt="image-20230718110219570" style="zoom:80%;" />
{{< /details >}}

### 控制器

程序计数器PC：指明下一条指令的地址

指令寄存器IR：保存当前正在执行的指令

指令译码器ID：对操作码译码，向控制器提供特定操作的信号（不同指令的输出端不一样）

微操作信号发生器：判断并生成对应的微操作序列

时序信号控制下，还有标志和程序状态字寄存器PSW来控制下一步

![image-20230718111053530](https://cdn.jsdelivr.net/gh/zvictorliu/typoraPics@main/img/image-20230718111053530.png)

控制单元CU就是 ID、微操作信号发生器那部分

用户可见的寄存器（汇编语言可以改变）：通用寄存器、PSW（条件转移、比较）、PC（JMP等，不是直接改）、ACC

用户不可见：MAR、MDR、IR、暂存寄存器
