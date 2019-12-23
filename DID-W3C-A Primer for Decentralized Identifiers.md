# 初识分布式身份认证
## 一篇献给对自管理身份认证感兴趣的人们的介绍


## 2019年1月19日社区小组报告草稿

### 最新编辑草稿：

[https://w3c-ccg.github.io/did-primer/](https://w3c-ccg.github.io/did-primer/)

### 编辑：

[Andrew Hughes]()  
[Manu Sporny]()  
[Drummond Reed]()  

### 作者：
[Credentials Community Group]()

### 参与者：
[Github w3c-ccg/did-primer]()  
[file a bug]()  
[Commit history]()  
[Pull requests]()  

Copyright © 2019 the Contributors to the A Primer for Decentralized Identifiers Specification, published by the Credentials Community Group under the W3C Community Contributor License Agreement (CLA). A human-readable summary is available.

## **摘要**

**分布式身份标识**（Decentralized Identifier, DID）是一种全球唯一的，高可用性的解决方案，是具有加密型验证能力的新型的身份认证方式* 。DID 代表性的联合一些加密技术以用来加固安全通信通道，比如公共密钥，*服务端点(service endpoints)*。DID 对任何受益于自管理、加密可验证身份标识的应用（比如个人身份标识，组织身份标识或是物联网身份标识的场景）都是有益的。举个例子，目前 W3C Verifiable Credentials 的商业部署中就重度依赖DID来认证个人、组织和物的身份，并且实现多种安全和隐私保护的保障。本文档用于介绍 DID，分布式身份标识的概念。

## **本文档状态**

TO DO

## **目录**

1. 介绍
2. DID 和其他全球唯一标识的区别
3. 一个 DID 的格式
4. DID 文档
5. DID 方法
6. DID 和隐私的设计
7. DID 和可验证凭证
6. 附录 A：DID 社区资源

## 1. 介绍

