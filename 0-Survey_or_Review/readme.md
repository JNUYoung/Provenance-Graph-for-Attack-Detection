# Introduction

> 请说明目前同类技术应用的现状及缺陷（结合现有技术文献或者附图说明）

随着信息革命的快速发展，网络空间已然成为现代社会中信息传播的新渠道、人类生产生活的新空间、社会经济发展的新引擎、国家主权的新疆域，但其同时也带来了新的安全风险和挑战。网络空间中的大量的攻击事件以及不断发展的攻击手段，使得正常的政治、经济、文化、社会活动等面临严峻的风险与挑战。在各种攻击活动中，高级持续性威胁（Advanced Persistent Threat）成为了最具代表性的攻击方式之一。APT攻击通常由特定组织或机构出于商业或政治动机，针对特定目标开展隐匿而持久的非法入侵活动，其所具有的攻击技术先进性、攻击过程长期性、攻击危害严重性等特点，使得研究对于该类网络威胁的检测方法具有重要的必然性和现实意义。

传统的基于网络和主机终端侧的网络流量的入侵检测系统，难以有效适应APT攻击等具有长期潜伏性和针对性的复杂攻击模式，对于未知攻击的检测也存在较大难度。近年来，基于溯源图（Provenance Graph）的APT攻击检测成为解决该类问题的一个新的解决思路。

系统级溯源图（System-level Provenance Graph）是一个带有时间信息的有向图，如下图所示为一个包含了正常系统行为和攻击行为的溯源图实例，其记录了系统运行过程中系统实体（进程、文件、套接字等）之间的交互关系，通过图的形式将系统行为进行因果关联[1]。因此，溯源图能够较好还原系统中的各种行为，同时其图结构的表示包含了丰富的语义信息，系统实体之间以及事件之间的关联性也能够得到较为完整的保留，利用溯源图进行威胁发现、攻击检测和攻击溯源具有重要应用价值。

![img](http://blog.nsfocus.net/wp-content/uploads/2020/12/%E5%9B%BE1-1-1024x428.png)

近年来已有的基于溯源图的APT检测方法大致可以分为如下几类：

一、基于异常检测思路的攻击检测

基于异常检测方法的核心思想在于对正常的系统行为进行建模，对于检测过程中严重偏离学习到的正常模式的行为即可判定其为攻击行为。

![img](http://blog.nsfocus.net/wp-content/uploads/2020/12/%E5%9B%BE2-1.png)

如上图所示为基于溯源图的利用异常检测思想的通用检测框架，首先需要采集记录主机运行状态的审计日志数据并构建溯源图，利用深度学习模型对正常系统行为进行建模，学习正常系统行为模式和特征，进而以正常系统行为模型为参考，进行后续的攻击行为的检测。相关研究也提出了若干检测模型，Streamspot[2]模型采用流式异构图的建模方式对系统溯源图进行建模，采用基于质心的聚类和异常检测方法进行主机级的APT检测；PIDAS系统[3]基于溯源图信息所提供的历史数据，通过计算一系列系统事件组成的路径的异常程度并与设定的异常阈值进行比较从而判断是否存在入侵行为；Pagoda[4]系统进一步分析了图级别的异常程度，P-Gaussian[5]系统则将检测任务建模为时间序列顺序或不同时间序列之间长度的变化的检测；Unicorn系统[6]针对APT特点设计了运行时的基于溯源图的攻击检测方法，其提出的概要图方法能够在长时间运行的系统中分析溯源图并识别异常。

同时，基于异常检测的方法也存在相应局限性：

1.必须保证参考模型是基于正常行为所构建的，不能包含有未知的隐蔽的攻击所混淆的数据；

2.检测模型的通用性和可扩展性较差，应用到其它终端中通常需要重新收集数据进行训练；

3.模型对于异常的判定往往并不完全准确且检测性能随时间不断降低，存在较高的误报率；

4.模型的检测结果缺乏可解释性，需要安全专家对于结果进行进一步分析；

二、基于规则匹配思路的攻击检测

![image-20220913161622442](C:\Users\YOUNG\AppData\Roaming\Typora\typora-user-images\image-20220913161622442.png)

​                                                                                               HOLMES系统所制定的部分规则

基于规则的检测指的即是根据已有的攻击相关的先验知识制定规则策略。SLEUTH[7]系统通过设定告警触发规则，利用基于标签传播的策略实现因果关系追踪。Morse系统[8]通过标签在溯源图中的传播实现攻击步骤的关联，并且定义了进程注入、恶意文件执行等规则来实现攻击检测。HOLMES系统[9]基于可疑信息流之间的关联性，设计了层次化的TTP规则模板，实现了日志、告警等低级信息到高级杀伤链步骤的映射，实现了有效的语义相关的威胁检测。POIROT系统[10]通过IOC和专家知识构建了威胁查询子图，利用子图匹配的方式在溯源图中进行匹配，查找满足威胁子图的相关子图并产生告警。

同时，基于规则匹配的方法也存在相应局限性：

1.规则的制定高度依赖专家知识，需要大量的领域先验知识；

2.规则的匹配和子图的匹配具有很大的计算开销，难以满足检测的时效性的要求；

3.基于规则的检测忽略了大数据时代下数据本身的价值，缺乏对于溯源图中数据的语义信息的深度挖掘和使用；

4.基于规则的检测具有较好的可解释性，但是缺乏泛化能力，对于新型攻击或未知攻击的检测难度较大；

此外，系统的正常运行过程中会产生大量的数据，因此对于溯源数据的有效收集、管理和存储也是一项重要的关键技术问题。



基于上述内容，本发明的目的在于提供一种基于溯源图的APT攻击检测和威胁发现系统，其能够适应多种操作系统环境，进行有效的数据收集、数据降噪和溯源图构建，融合溯源图中节点的结构特征和语义特征，进而对溯源图进行有效的数据挖掘，保证计算的时间开销和空间开销相平衡的同时，有着较高的检测精度和尽可能低的误报率。



**Reference**

[1] Li Z, Chen Q A, Yang R, et al. Threat detection and investigation with system-level provenance graphs: a survey[J]. Computers & Security, 2021, 106: 102282.

[2] Manzoor E, Milajerdi S M, Akoglu L. Fast memory-efficient anomaly detection in streaming heterogeneous graphs[C]//Proceedings of the 22nd ACM SIGKDD International Conference on Knowledge Discovery and Data Mining. 2016: 1035-1044.

[3] Xie Y, Feng D, Tan Z, et al. Unifying intrusion detection and forensic analysis via provenance awareness[J]. Future Generation Computer Systems, 2016, 61: 26-36.

[4] Xie Y, Feng D, Hu Y, et al. Pagoda: A hybrid approach to enable efficient real-time provenance based intrusion detection in big data environments[J]. IEEE Transactions on Dependable and Secure Computing, 2018, 17(6): 1283-1296.

[5] Xie Y, Wu Y, Feng D, et al. P-Gaussian: provenance-based gaussian distribution for detecting intrusion behavior variants using high efficient and real time memory databases[J]. IEEE Transactions on Dependable and Secure Computing, 2019, 18(6): 2658-2674.

[6] Han X, Pasquier T, Bates A, et al. Unicorn: Runtime provenance-based detector for advanced persistent threats[J]. arXiv preprint arXiv:2001.01525, 2020.

[7] Hossain M N, Milajerdi S M, Wang J, et al. {SLEUTH}: Real-time attack scenario reconstruction from {COTS} audit data[C]//26th USENIX Security Symposium (USENIX Security 17). 2017: 487-504.

[8] Hossain M N, Sheikhi S, Sekar R. Combating dependence explosion in forensic analysis using alternative tag propagation semantics[C]//2020 IEEE Symposium on Security and Privacy (SP). IEEE, 2020: 1139-1155.

[9] Milajerdi S M, Gjomemo R, Eshete B, et al. Holmes: real-time apt detection through correlation of suspicious information flows[C]//2019 IEEE Symposium on Security and Privacy (SP). IEEE, 2019: 1137-1152.

[10] Milajerdi S M, Eshete B, Gjomemo R, et al. Poirot: Aligning attack behavior with kernel audit records for cyber threat hunting[C]//Proceedings of the 2019 ACM SIGSAC Conference on Computer and Communications Security. 2019: 1795-1812.

