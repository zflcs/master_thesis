# 论文大纲交流反馈

## 20250228

老师修改意见如下：

1. 标题：基于软硬协同的中断响应和任务调度
   - [x] 已修改
2. 综述：补充异步编程方法综述、补充任务模型综述
   - [x] 已修改，文献综述首先介绍任务模型，最后介绍异步编程方法，最后落脚点在并发上
3. 设计：任务模型需要先讨论；然后才是任务状态维护和软硬件接口定义；
   - [x] 已修改
4. 实现：我认为还需要讨论任务切换过程和软件适配；
   - [x] 增加了软件适配的章节，将任务切换的过程放到软件适配的一小节中
5. 评估：加一个章节进行定性的特征分析；
   - [x] 已修改
6. 加写作时间估计
   - [x] 已添加

修改后的大纲在[这里](../outline.md)

## 20250227

修改了第一轮反馈意见后的大纲在[这里](https://github.com/zflcs/master_thesis/blob/c619883046ad16a93969cb86051c70103f3eaaf5/outline.md)

## 20250226

这里使用这个时间（2025 年 2 月 26 号）是因为在这一天才开始增加了对论文的大纲、以及要修改的内容进行的反馈意见交流的目录（disscussions），之前没有进行。

这里先记录第一版论文大纲：

```markdown
# Outline

核心亮点：

1. 调度机制
   1. 任务队列的同步互斥
   2. 用户态的抢占式调度
2. 中断处理机制（通知机制）
   1. 快速唤醒机制，中断控制器直接处理（仅在必要时发送中断给 CPU）
   2. 设备虚拟化，在 hypervisor 中的应用（可能）

## 引言

### 研究背景

1. 云服务上的微秒级调度，需要专用的调度核、缺少用户态抢占
2. 已有的中断控制器没有将调度与通知机制结合起来
3. 已有的 IPC 机制

### 已有工作

1. RISC-V N 扩展，x86 用户态中断，其他的中断控制器的设计
2. 用户态驱动框架（DPDK）、用户态任务运行时（Tokio）
3. Chisel 硬件描述语言、rocket-chip SoC 生成器

## 设计方案

### 整体架构

### 控制器内部数据结构

1. 资源分配器
2. 任务队列
3. 外部中断槽
4. 软件中断槽
5. 中断处理机制

### 控制器端口分配

1. 全局端口
2. 局部端口

## QEMU 和 rocket-chip 中的实现

1. N 扩展的实现
2. 控制器数据结构的实现
3. 控制器与 CPU 的交互

## 软件适配

1. 软件接口
2. 软硬件协作的任务状态管理
3. 使用方法（跳板页、异步系统调用、IPC 通信等）

## 性能评估

对比实验：在 Linux 上进行

1. 在用户态进行调度，与已有的微秒级调度结果进行对比（对比突出同步互斥开销，用户态抢占式调度，以往的用户态的调度只能是协作式，需要抢占只能通过内核进行），对 tokio 进行修改，对比
2. DPDK 对比增加了用户态的中断处理后的结果，在用户态使用中断控制器与轮询的对比，与已有的用户态的网络协议栈的对比
3. ipc-bench 对比各种类型的 IPC
4. 将 FlexSC 与控制器结合，与原来的 FlexSC 进行对比，展示系统调用的结果
5. 增加时钟设备的虚拟化实验

## 结论
```

老师给的反馈意见如下：

```markdown
1. 解决的问题
   1. 调度，状态维护、事件触发、切换（从综述调研出发）
   2. 中断，中断的响应速度更快，cpu 的开销更小
   3. 不提设备虚拟化

2. 解决的方法
   1. 硬件维护执行流状态（同步互斥的开销，减少 CPU 的参与）
   2. 用硬件实现由中断导致的执行流状态变换（特权级、切换次数）
   3. 统一执行流的抽象（没有线程、协程明确的区分），执行流在哪执行不关注，开题的题目不需要过多的关注，（从完善任务管理、进程管理等系统调用的 API，需要补实验，尝试写出来，控制块，地址空间的解耦，把 JYK 的工作结合起来）

3. 实验
   1. 切换
   2. 维护
   3. 分析切换的延时
   4. 确定在哪个上面来做
   5. 在 AsyncOS 上改 Tokio，再移植到 Linux，
   6. 综合起来的性能（真实应用）

```

之前临时记录的反馈意见的 commit 在[这里](https://github.com/zflcs/master_thesis/blob/965844212ca13ae54cf9183eaa84e56d45f60d7e/outline.md)
