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

- WATSON

## 4 Provenance Graph for Attack/Threat Detection

1.Streamspot——Fast Memory-efficient Anomaly Detection in Streaming Heterogeneous Graphs

![image-20220926092832885](C:\Users\YOUNG\AppData\Roaming\Typora\typora-user-images\image-20220926092832885.png)

2.SLEUTH：Real-time Attack Scenario Reconstruction from COTS Audit Data【基于标签传播】





## 技术博客：

- 从DARPA TRANSPARENT COMPUTING看终端攻防：http://blog.nsfocus.net/darpa-transparent-computing-0826/
- 按图索“迹” 追踪溯源：http://blog.nsfocus.net/cyber-security-4/
- 攻击推理：基于攻击溯源图的威胁评估技术：http://blog.nsfocus.net/threat-assessment-1209/
- 攻击溯源——基于因果关系的攻击溯源图构建技术：http://blog.nsfocus.net/attack-investigation-0907/
- 溯源图技术在入侵检测与威胁分析中的应用：https://www.secrss.com/articles/42247
- 为什么我们需要溯源图 & 溯源图的基本概念介绍：https://zhuanlan.zhihu.com/p/145941567
- 基于溯源图做入侵检测、关联分析和告警消减中的常见问题和可能的风险：https://zhuanlan.zhihu.com/p/432717580



## 网络空间威胁检测面临的研究问题

1. 缓慢、隐蔽的高级持续性威胁的检测难题：由于其缓慢而又隐蔽的特性，传统的防御手段往往很难对其进行有效的监控；
2. 用什么数据、什么结构准确地表示一个具体的威胁行为：不同的表示方式都有其优点和缺点；例如常用的恶意IP、恶意文件的hash、几乎没有鲁棒性可言，但却是现实中最常用的安全工具；其它常见方式包括系统调用序列、API调用树、代码动态执行图等。一般来说，简单的结构意味着更高的效率，但表达能力更差，误报的概率更高；而复杂的结构，表达能力更好的同时检测效率则更低；
3. 如何设计检测算法权衡威胁检测的响应速度与检测精度矛盾；
4. 如何设计存储系统以权衡存储空间与查询效率之间的矛盾；