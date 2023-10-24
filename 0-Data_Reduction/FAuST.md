# FAuST：Striking a Bargain between Forensic Auditing's Security and Throughput

> ACSAC-2022
>
> keywords: Auditing, Log Reduction

本文提出了FAuST系统，用于在系统终端进行流式的审计日志规模压缩。在注册完日志源后，FAuST在内存中增量地构建系统事件的溯源图，在溯源图的构建过程中，通过注册事件回调函数来调用日志压缩方法对局部子图进行压缩，而全图的规模压缩在整个处理过程中周期性地进行。

## 1 introduction

系统日志可能会快速增长到难以处理的规模，因此许多企业或机构仅仅存储过往几个月或者几天的系统日志数据，这些保留时间太过短暂，网络攻击发起者的入侵活动通常会有一个“dwell times”，对于存储成本的考虑，导致基于现有审计日志难以检测和响应这些网络威胁。

【因此，有效减少审计日志规模，保证日志数据对于攻击检测的可用性的同时有效控制存储开销】

现在已有相关文献提出相关的日志数据缩减方法，核心思想就是选择性地删除对攻击调查没有价值的事件。[ Algorithms that selectively remove log events with little forensic value ]

**Obstacles**

1. 大部分文献中介绍的日志缩减方法并未开源；
2. 这些方法只适用于特定的日志格式，并且服务于特定的模型，缺乏适用性；
3. 这些方法是否可以应用到各个系统终端上，而不是仅在专门用于处理日志的中心服务器上；

**FAuST**

- Locally-applicable：通过事件驱动的方式在溯源图的构建过程中适时调用
- Globally-applicable：整个压缩过程中周期性地进行调用

- 集成了八种日志压缩算法
- 集成使用压缩算法存在天花板效应
- 算法之间相互结合能够有效互补



## 2 Log Reduction Techniques

**2.1 LogGC**【Garbage Collection】

识别溯源图中对于整个系统没有持续性影响的系统实体，将这些系统实体从溯源图中清除。例如临时文件的创建和删除。

**2.2 NodeMerge**【Template-based】

训练阶段：基于现有审计日志学习每个进程的对于只读文件的频繁读取的模板

压缩阶段：在新的审计日志中应用学习到的模板进行节点的合并。

训练阶段需要整个溯源图数据，压缩阶段可以应用于局部子图。

**2.3 CPR**【Causality Preserving Reduction】

移除审计日志中的冗余信息，保留溯源图中完整的信息流和因果关联。

系统事件有相同的依赖和影响范围，因此可以进行聚合。

![img](https://img-blog.csdnimg.cn/9d20bfff21d443748de966da5defb62f.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATlVBQVlZTU0=,size_20,color_FFFFFF,t_70,g_se,x_16)

**2.4 PCAR**【Process-centric Causality Approximation Reduction】

一些进程在短时间内会产生大量语义上类似的事件

**2.5 F-DPR and S-DPR**【Dependency Preserving Reduction】

Idea：在规模压缩后的溯源图中进行forward tracing和backward tracing，需要能够找出完整溯源图中所包含的所有系统实体。【压缩后的溯源图和压缩前的溯源图，具有相同的系统实体，并且这些系统实体之间仍然保有相同且必要的因果关系】

【或许在攻击路径调查时能提高效率？攻击检测时不一定发挥作用】

**2.6 Winnower**

**2.7 LogApprox**



## 3 System Design

![image-20230209104007176](C:\Users\YOUNG\AppData\Roaming\Typora\typora-user-images\image-20230209104007176.png)

- log parser: input raw audit logs and interprets different log formats and incrementally creates provenance graph ans log event buffer
- local filters: directly invoked on the graph  using event callback handlers
- batch filters: invoked periodically in an epoch fashion
- reduction engine: output the reduction results after each epoch

**Log Parsing**

provenance graph

log event buffer：缓存了每个系统事件的必要信息

**Log filtering**

![image-20230209105030756](C:\Users\YOUNG\AppData\Roaming\Typora\typora-user-images\image-20230209105030756.png)



## 4 Implementation

![image-20230209105318948](C:\Users\YOUNG\AppData\Roaming\Typora\typora-user-images\image-20230209105318948.png)



## 5 Case Study

![image-20230209105553197](C:\Users\YOUNG\AppData\Roaming\Typora\typora-user-images\image-20230209105553197.png)

## 6 Related Work

**6.1 Provenance Analysis**

**6.2 Log Reduction**



## 7 Conclusion

FAuST是第一个去中心化的审计日志规模压缩框架，结合了多种日记缩减技术在系统终端进行本地的审计日志规模压缩。





