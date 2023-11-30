# Provenance-Graph-for-Attack/Threat-Detection

Recording Papers and Notes about ***Threat Detection based on System Provenance Graph***



## 1 Provenance Model

- W3C PROV JSON：https://www.w3.org/Submission/2013/SUBM-prov-json-20130424/
- Open Provenance Model：L. Moreau, B. Clifford, J. Freire, J. Futrelle, Y . Gil, P . Groth, N. Kwasnikowska,S. Miles, P . Missier, and J. Myers, “The open provenance model core specification (v1.1),” Future Generation Computer Systems, vol. 27, no. 6, pp.
  743–756, 2011.
- SPADE JSON format：https://github.com/ashish-gehani/SPADE/wiki/Reporting-provenance-using-JSON

## 2 Provenance Collection

- Camflow - linux：https://camflow.org/#examples
- SPADE - cross flatform：https://github.com/ashish-gehani/SPADE/wiki
- Sysmon - windows

## 3 Provenance Data Reduction

| paper                                                        | paper link                                                   |      |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ---- |
| FAuST：Striking a Bargain between Forensic Auditing's Security and Throughput【ACSAC 2022】 | [paper link](https://dl.acm.org/doi/10.1145/3564625.3567990) |      |
| SoK：History is a Vast Early Warning System：Auditing the Provenance of System Instructions |                                                              |      |
|                                                              |                                                              |      |



## 4 Provenance Graph for Attack/Threat Detection

| paper                                                        | paper link / code link                                       | contributions |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------- |
| Threat Detection and Investigation with System-level Provenance Graph：A Survey【computers & security】【综述】 | [paper link](https://linkinghub.elsevier.com/retrieve/pii/S0167404821001061) |               |
| Provenance-based intrusion detection systems：A survey【ACM Computing Surveys】【综述】 | [paper link](https://dl.acm.org/doi/10.1145/3539605)         |               |
| SLEUTH：Real-time Attack Scenario Reconstruction from COTS Audit Data【USENIX 2017】 | [paper link](https://www.usenix.org/conference/usenixsecurity17/technical-sessions/presentation/hossain) |               |
| Streamspot：Fast Memory-efficient Anomaly Detection in Streaming Heterogeneous Graphs |                                                              |               |
| Holmes: real-time apt detec tion through correlation of suspicious information flows【S&P 2019】 | [paper link](https://ieeexplore.ieee.org/document/8835390/)  |               |
| Pagoda: A hybrid approach to enable efficient real-time provenance based intrusion detection in big data environments【TDSC 2020】 | [paper link](https://doi.org/10.1109/TDSC.2018.2867595)      |               |
| Unicorn: Runtime provenance-based detector for advanced persistent threats【NDSS 2020】 | [paper link](https://www.ndss-symposium.org/ndss-paper/unicorn-runtime-provenance-based-detector-for-advanced-persistent-threats/) |               |
| ATLAS: A Sequence-based Learning Approach for Attack Investigation【USENIX 2021】 | [paper link](https://www.usenix.org/conference/usenixsecurity21/presentation/alsaheel) |               |
| WATSON：Abstracting Behaviors from Audit Logs via Aggregation of Contextual Semantics【NDSS 2021】 | [paper link](https://www.ndss-symposium.org/ndss-paper/watson-abstracting-behaviors-from-audit-logs-via-aggregation-of-contextual-semantics/) |               |
| KRYSTAL：Knowledge graph-based framework for tactical attack discovery in audit data【computer & security 2022】 | [paper link](https://linkinghub.elsevier.com/retrieve/pii/S016740482200222X) |               |
| APT-KGL: An Intelligent APT Detection System Based on Threat Knowledge and Heterogeneous Provenance Graph Learning【TDSC 2022】 |                                                              |               |
| THREATRACE：Detecting and Tracing Host-based Threats in Node Level Through Provenance Graph Learning【TIFS 2022】 | [paper link](https://doi.org/10.1109/TIFS.2022.3208815)      |               |
| Paradise：Real-time, Generalized, and Distributed Provenance-Based Intrusion Detection【TDSC 2022】 | [paper link](https://doi.org/10.1109/TDSC.2022.3160879)      |               |
| Recommendation-guided Cyber Threat Analysis using System Audit Records【S&P 2022】 | [paper link](https://doi.org/10.1109/SP46214.2022.9833669)   |               |
| ANUBIS: A Provenance Graph-Based Framework for Advanced Persistent Threat Detection【SAC 2022】 | [paper link](https://dl.acm.org/doi/10.1145/3477314.3507097) |               |
| Sometimes, You Aren’t What You Do: Mimicry Attacks against Provenance Graph Host Intrusion Detection Systems【NDSS 2023】 | [paper link](https://www.ndss-symposium.org/ndss-paper/sometimes-you-arent-what-you-do-mimicry-attacks-against-provenance-graph-host-intrusion-detection-systems/) |               |
| DISTDET：A Cost-Effective Distributed Cyber Threat Detection System【USENIX 2023】 | [paper link](https://www.usenix.org/conference/usenixsecurity23/presentation/dong-feng) |               |



## 5.相关技术博客：

- 从DARPA TRANSPARENT COMPUTING看终端攻防：http://blog.nsfocus.net/darpa-transparent-computing-0826/
- 按图索“迹” 追踪溯源：http://blog.nsfocus.net/cyber-security-4/
- 攻击推理：基于攻击溯源图的威胁评估技术：http://blog.nsfocus.net/threat-assessment-1209/
- 攻击溯源——基于因果关系的攻击溯源图构建技术：http://blog.nsfocus.net/attack-investigation-0907/
- 溯源图技术在入侵检测与威胁分析中的应用：https://www.secrss.com/articles/42247
- 为什么我们需要溯源图 & 溯源图的基本概念介绍：https://zhuanlan.zhihu.com/p/145941567
- 基于溯源图做入侵检测、关联分析和告警消减中的常见问题和可能的风险：https://zhuanlan.zhihu.com/p/432717580



## 6.网络空间威胁检测面临的研究问题

1. 缓慢、隐蔽的高级持续性威胁的检测难题：由于其缓慢而又隐蔽的特性，传统的防御手段往往很难对其进行有效的监控；
2. 用什么数据、什么结构准确地表示一个具体的威胁行为：不同的表示方式都有其优点和缺点；例如常用的恶意IP、恶意文件的hash、几乎没有鲁棒性可言，但却是现实中最常用的安全工具；其它常见方式包括系统调用序列、API调用树、代码动态执行图等。一般来说，简单的结构意味着更高的效率，但表达能力更差，误报的概率更高；而复杂的结构，表达能力更好的同时检测效率则更低；
3. 如何设计检测算法权衡威胁检测的响应速度与检测精度矛盾；
4. 如何设计存储系统以权衡存储空间与查询效率之间的矛盾；



## 7.入侵/威胁检测数据集

**1.Intrusion Detection Evaluation Dataset-CIC-IDS2017**

数据下载链接：https://www.unb.ca/cic/datasets/ids-2017.html

数据集描述：数据集包含良性事件以及大部分的常见的攻击事件，包含了经过CICFlowMeter处理之后的带标签的流量数据，通过CSV文件的形式进行展示，包括时间戳、源IP、目的IP、源端口、目的端口、协议以及攻击等；

**2.DARPA TC数据集**

DARPA Transparent Computing Program

**3.LANL数据集**

- auth.txt：包含了主机之间的验证事件

  time, source user@domain, destination user@domain, source computer, destination computer, authentication type, logon type, authentication orientation, success/failure

- dns.txt：包含了主机向DNS服务器的请求事件
  time, source computer, computer resolved

- flows.txt：包含了主机之间的发送的网络流
  time, duration, source computer, source port, destination computer, destination port, protocol, packet count, byte count

**4.CICDarknet2020**

包含正常流量数据和暗网流量数据；

Audio-Stream、Browsing、Chat、Email、P2P、Transfer、Video-Stream、VOIP

**5.UNSW-NB15 **

包含了正常系统和网络行为，以及人为模拟的攻击行为，包含Fuzzers,    Analysis,    Backdoors,    DoS,    Exploits,    Generic, Reconnaissance,  Shellcode  and  Worms九类不同的攻击方式。

**6.CSE-CIC-IDS2018**

- 数据集包括七种不同的攻击场景，Brute-force、Heartbleed、Botnet、DoS、DDoS、Web attacks、infiltration of network from inside；
- 攻击者基础设施：50台服务器；
- 受害组织：5个部门、420台主机、30台服务器；
- 数据集包括捕获的每台机器的网络流量和系统日志，使用CICFlowMeter-V3从捕获流量中提取了80个特征；

![image-20220720210436831](C:\Users\YOUNG\AppData\Roaming\Typora\typora-user-images\image-20220720210436831.png)

- 下载主机间的流量记录的CSV文件；
- 原始的主机间的PCAP数据包和系统日志；

> 下载指南：
>
> 1. 安装AWS CLI
>
> 2. cmd 命令获取数据集列表：aws s3 ls --no-sign-request "s3://cse-cic-ids2018" --recursive --human-readable --summarize；
>
> 3. 下载某个特定文件：
>
>    aws s3 cp --no-sign-request "s3://cse-cic-ids2018/Processed Traffic Data for ML Algorithms/Wednesday-14-02-2018_TrafficForML_CICFlowMeter.csv" F:数据集/CICIDS2018/Wednesday-14-02-2018_TrafficForML_CICFlowMeter.csv