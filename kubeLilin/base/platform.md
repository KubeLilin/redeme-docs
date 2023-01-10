# 平台设计
## 平台设计图
![](https://mnur-prod-public.oss-cn-beijing.aliyuncs.com/0/tech/physical_architecture.png)
## 平台设计目标
KubLilin设计目标是希望通过一套可视化的UI来实现用户无需编写繁琐的yaml文件和配置，只需简单的操作即可完成应用在K8S上的部署，来降低K8S的使用门槛，提升开发者部署效率。
## 平台设计思路
KubLilin是一套抽象于k8s之上的管理平台，其设计的思路是希望隔离用户和k8s，用户无需关心底层的基础设施，把注意力集中到应用的生命周期管理上，用户所有的操作都是围绕应用来进行的，用户在KubeLilin上定义好应用的边界和描述后，KubeLilin会通过k8s的API来完成对k8s的操作，按照用户的要求完成对应用的部署。
因为是使用API直接操作k8s，所以KubeLilin天生支持多云平台，用户无需关心自己的目标环境是在阿里云还是腾讯云，或者其他云厂商甚至是自建的k8s集群，在KubeLlin导入K8s的配置文件后即可实现无差别的部署操作。
![](https://mnur-prod-public.oss-cn-beijing.aliyuncs.com/0/tech/functional_architecture.png)