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

**分布式身份标识**（Decentralized Identifier, DID）是一种全局唯一的，高可用性的解决方案，是具有加密型验证能力的新型的身份认证方式* 。DID 代表性的联合一些加密技术以用来加固安全通信通道，比如公共密钥，*服务端点(service endpoints)*。DID 对任何受益于自管理、加密可验证身份标识的应用（比如个人身份标识，组织身份标识或是物联网身份标识的场景）都是有益的。举个例子，目前 W3C Verifiable Credentials 的商业部署中就重度依赖DID来认证个人、组织和物的身份，并且实现多种安全和隐私保护的保障。本文档用于介绍 DID，分布式身份标识的概念。

## **本文档状态**

TO DO

## **目录**

1. 介绍
2. DID 和其他全局唯一标识的区别
3. 一个 DID 的格式
4. DID 文档
5. DID 方法
6. DID 和隐私的设计
7. DID 和可验证凭证
6. 附录 A：DID 社区资源

## 1. 介绍

在表面上来看，**DID** 是一种全新的全局唯一标识。往更深层次来说，DID 是互联网分布式数字身份和PKI一个全新的层面中的核心部件。这种 *[分布式公钥基础设施](https://github.com/WebOfTrustInfo/rebooting-the-web-of-trust/blob/master/final-documents/dpki.pdf)*（DPKI）对全球网络安全和网络隐私的影响，可能与用于加密Web流量的 *[SSL/TLS 协议](https://en.wikipedia.org/wiki/Transport_Layer_Security)* 的PKI（现在是世界上最大的PKI）一样大。

本篇初级读物的目的是让DID架构的新手了解一下相关背景，不仅是DID的规范，还有被目前正在开发的 DID 相关规范族所代表的分布式身份的整体架构。它包括：

- DID 和其规范的起源背景
- DID 与其他全局唯一身份标识的区别
- DID 语法为何能与分布式网络们兼容
- DID 如何*解释为*包含公钥和服务端点的DID文档
- DID 方法在其基础设施实施中的关键角色
- DID 使用中对于隐私的考虑
- DID 基础设施是如何为可验证凭证奠定基础的
                    
## 2. DID 和其他全局唯一标识的区别

对于不需要中心注册机构的全局唯一身份标识的需求已经不再新鲜。*[UUID](https://en.wikipedia.org/wiki/Universally_unique_identifier)*(通用唯一标识 —— Universally Unique Identifiers, 也被叫做 GUIDs, 全局唯一标识 —— Globally Unique Identifiers) 就是1980年以此为目的被开发出来的，首先由 Open Software Foundation 标准化，接着又被 *[IETF RFC 4122](https://tools.ietf.org/html/rfc4122)* 标准化。

持久身份标识（persistent identifiers, 对实体授权一次就能永久不更改的身份标识）的需求也不是第一次了。这种类型的身份标识已经被 *[IETF RFC 2141](https://www.ietf.org/rfc/rfc2141.txt)* 第一次标准化为 *[URNs](https://en.wikipedia.org/wiki/Uniform_Resource_Name)* (Uniform Resource Names, 统一资源名称)。最近又被 *[RFC 8141](https://tools.ietf.org/html/rfc8141)* 多标准化了一些东西。

然而，一般来说 UUID 不是可用的全局解决方案，可做解决方案的URN也需要一个中心注册机构。不仅如此，UUID和URN都没有本质上具有解决第三个特性，即**通过加密的方式验证标识的所有权**的能力。

对于*自身份认同*来说，它应被定义为终生便携的不需要任何中心机构的数字身份。我们需要一种新的身份标识类型，这种类型需要满足四种需求：持久性，全局解决方案，加密认证，去中心化。


## 3. 一个DID身份的格式
