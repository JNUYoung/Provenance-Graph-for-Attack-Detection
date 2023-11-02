# ProGrapher：An Anomaly Detection System based on Provenance Graph Embedding

> paper link：https://www.usenix.org/conference/usenixsecurity23/presentation/yang-fan

## abstract

1）解决问题：提出ProGrapher系统，解决依赖爆炸（dependency explosion）问题的同时实现高效（high efficiency）检测；

2）技术特点：日志转换为时序快照、图级别的嵌入（whole-graph embedding）、基于序列的表示学习；



## 1 introduction

本文认为难以检测APT攻击存在两大原因：

1. 传统的网络防护系统依赖于攻击特征或指标，这些特征容易被攻击者通过相关手段进行隐匿；
2. 现有的检测方法没有充分利用计算机或网络系统内的不同实体之间的因果关联关系；

因此，为了解决这两个主要问题，最近大部分工作提出基于溯源图的入侵检测，从而充分利用海量日志数据中包含的丰富的上下文信息。

基于对已有工作的调研，现有的PIDS系统都无法在复杂的现实生产环境中进行有效部署和使用：

- 已有的工作中，部分系统依赖于攻击签名、启发式算法或者已知的攻击路径进行检测，然而这些很容易被攻击者进行绕过；
- 某些系统基于所有审计日志，构建单个溯源图，如此大规模的图数据带来严重的资源开销，同时也会导致大量的误报；
- 某些系统按照时间顺序构建流式的溯源图快照，但是其检测粒度过大，聚焦于判断某个快照是否为异常，仍需要安全分析人员对该快照下的所有系统实体和交互进行分析；



**contributions：**

- 基于graph2vec的对溯源图快照的图级别的embedding策略、基于TextRCNN的序列学习方法对溯源图快照序列进行分析，实现正常系统行为的有效且高效的表示学习；
- 基于rooted subgraph的攻击识别方法，从检测出的异常的图快照中识别攻击相关实体，显著减少分析工作量；



## 2 background

### 2.1 data provenance for attack investigation

### 2.2 learning-based attack detection on provenance graph

![image-20231031160740996](C:\Users\YOUNG\AppData\Roaming\Typora\typora-user-images\image-20231031160740996.png)

聚焦于不同的检测粒度：

| 检测粒度                                | advancetages                                             | disadvantages                                                | 代表工作            |
| --------------------------------------- | -------------------------------------------------------- | ------------------------------------------------------------ | ------------------- |
| edge / node                             | 检测粒度最细                                             | 图规模较大时检测效果不好，无法提供相关上下文信息供后续攻击调查 | shadewatcher、SIGL  |
| path                                    | 能够最大程度还原攻击路径                                 | 路径的选择基于启发式规则，规则的选择具有实践上的难度         | ProvDetector、Atlas |
| graph / sub-graph / graph-decomposition | 能最大程度上利用好溯源图所提供的上下文信息和时序演化信息 | 检测粒度最大，需要分析人员介入进行检测维度下钻               | Unicorn、ProvGem    |

### 2.3 graph embedding





## 3 overview

goals：

1）无监督范式，无需攻击先验知识（<u>通用方法：仅利用系统的正常行为数据训练检测模型</u>）；

2）能够按时间段对整个溯源图进行拆分，<u>挖掘时序快照之间的动态演化特征</u>进行检测；

3）准确识别包含异常行为的图快照，<u>并且找出具体的攻击相关的系统实体</u>，缩小攻击调查范围；

![image-20231011152819247](C:\Users\YOUNG\AppData\Roaming\Typora\typora-user-images\image-20231011152819247.png)

1）数据收集层（Data collection）：审计日志的收集和解析；

2）时序溯源图快照构建（snapshot builder）：将审计日志按照时序进行溯源图的转换和构建；

3）编码学习层（encoder）：将各个时序下的溯源图进行embedding、基于无监督范式学习正常的系统行为；

4）异常检测层（anomaly detector）：检测可能存在异常系统事件的snapshot；

5）取证报告层（key indicators）：检测异常snapshot中的异常节点，并按照异常程度进行排序和输出报告以供进一步分析；



【**总览**】基于审计日志按照时序进行划分和构建溯源图 -> 对各个子图进行embedding，学习正常状态下的图特征 -> 检测出可能包含异常行为的子图 -> 基于检测出的子图再进行更细粒度的节点级的检测



**challenges of provenance graph embedding**

- 现有的图嵌入方法<u>无法直接应用</u>到溯源图的表示学习当中；
- 溯源图中良性节点的数目远远大于恶意节点的数目，因此将图嵌入方法应用于溯源图表示学习中，需要进行相应的修改适配；



## 4 components

### 4.1 pre-processing and snapshot builder

**将整个溯源图按照时间顺序划分为若干个子图**

1）key problem：如何进行流式子图的构建？

将日志按照时序进行解析，维持一个cache graph，当图中的节点数目达到上限n时，将此时的图作为一个snapshot；

然后基于一个预设定的遗忘率（forgetting rate）f_r，移除掉最先加入图中的n × f_r个节点，然后继续解析日志并将节点和边加入图中，直到到达图中节点数目再次达到n；

| name        | explanation                                                  |
| ----------- | ------------------------------------------------------------ |
| cache graph | 构造snapshot的中间工具，cache graph中的节点数目每达到n时，保存作为一个snapshot |
| n           | 每个snapshot的节点上限数目                                   |
| f_r         | 每次达到上限后，需要移除的旧节点的比例                       |

### 4.2 encoder

**将第一步得到的若干个子图进行表示学习**

1. 目标：将第一步生成的图快照进行embedding，也即将每个图表示为一个向量；

2. 核心模型：graph2vec，将快照中每个节点的RSG（rooted subgraph 根子图）作为vocabulary，应用NLP领域中的doc2vec技术获得图的嵌入表示；

3. 关键步骤：

   - 生成RSG
   - 图快照的embedding的更新

4. graph kernel方法：

   将机器学习中的核方法拓展到了非欧结构的图数据上，本质上用于计算图与图之间的相似度。

   将两个图用一定的方法（图分解、最小路径、随机游走等）分解为更小结构，再定义核函数来计算两个图的相似度（例如两个图中相似的子结构的数目）

   相较于直接embedding，graph kernel方法不会损失太多图的结构化信息，直接面向图结构的数据，保留了核函数高效计算的优点，同时包含了结构化的信息；

5. 目标函数：![image-20231031170502460](C:\Users\YOUNG\AppData\Roaming\Typora\typora-user-images\image-20231031170502460.png)

### 4.3 anomaly detector

**基于序列的预测方法，将预测得到的子图embedding和graph2vec计算得到的embedding的差值作为衡量标准**

prographer主要关注连续的图快照之间的变化，将其作为一个重要的检测指标。

因此使用了TextRCNN模型对这种序列结构进行建模。

首先，每一个图快照s，都会计算得到一个latent representation y：

![image-20231031172113551](C:\Users\YOUNG\AppData\Roaming\Typora\typora-user-images\image-20231031172113551.png)

其中x_i，是left(Si), E(Si), right(Si)的拼接：

![image-20231031172220280](C:\Users\YOUNG\AppData\Roaming\Typora\typora-user-images\image-20231031172220280.png)

相应的计算公式如下：

![image-20231031172238532](C:\Users\YOUNG\AppData\Roaming\Typora\typora-user-images\image-20231031172238532.png)

因此，对于图序列中的每一个图，最终都能得到一个y，于是整个序列的embedding作为输入，经过一个全连接层，得到该序列的整体的最终向量表示：

![image-20231031172355850](C:\Users\YOUNG\AppData\Roaming\Typora\typora-user-images\image-20231031172355850.png)

故在训练过程中，给定一个图序列（1 - k），可以基于得到的TextRCNN模型计算其下一个图k+1的embedding，将该预测得到的embedding与其本身的经过graph2vec模型得到的embedding的差值的l2范数作为损失函数进行训练。

测试阶段，若某个序列中下张子图的实际embedding和预测embedding的差值大于一个阈值，则将该子图判断为异常子图。

### 4.4 key indicator generator





## 5 implementation

![image-20231102141310256](C:\Users\YOUNG\AppData\Roaming\Typora\typora-user-images\image-20231102141310256.png)

- L：图快照序列的最大长度
- n：每张图快照的最大节点数目
- fr：图快照节点更新时的历史节点遗忘率
- d：生成RSG时的深度



## 6 evaluation

### 6.6 robustness

攻击者在进行攻击活动时，例如apt攻击，通常会同时进行大量的正常网络行为和系统操作，通过大量的攻击无关行为来掩盖其真正的攻击动作，也被称为mimicry attack；

本文对于测试集中的每个攻击相关的系统实体节点，以一定概率随机在插入一些正常的系统事件（边）来模拟攻击者的攻击逃逸行为，从而通过实验验证prographer系统的检测鲁棒性；



## 7 discussion & limitation

**changes of normal behaviors**

大多数PIDS本质上基于异常检测的思想，因此若某个时刻出现新的正常的系统行为，基于异常的检测系统往往会将此类行为判定为异常，从而导致误报，可以被视为机器学习领域中的概念漂移（concept drift）问题。





## 999.comments

1. 溯源图的表示学习：本工作将溯源图按照时序进行流式建模，改造graph2vec算法对图快照进行表示学习；
2. 异常检测方法：本工作考虑溯源图快照之间的差异作为一个检测维度，根据前k个快照的embedding推断出第k+1个快照的embedding，并基于其本身的encoder之后的表示作为参照指标；
3. 攻击调查方法：由于encoder时采用基于RSG的graph2vec方法，所以可以对每个RSG的共现概率进行排序，避免人工调查；
4. ***problems to solve***：
   - PIDS系统的鲁棒性问题：
     - 如何更全面的模拟攻击者的对抗行为，从而提升系统鲁棒性？
     - 系统鲁棒性的衡量指标的建设，形成PIDS祥光工作的标准化的量化指标？
   - 基于异常思想的PIDS系统，由于系统正常行为添加或演化所导致的误报问题：
     - 通常的解决方法是对模型进行重训练，但是一方面重训练的周期无法界定，一方面也会增大开销，需要找到PIDS对于该类问题的有效的解决方法？



