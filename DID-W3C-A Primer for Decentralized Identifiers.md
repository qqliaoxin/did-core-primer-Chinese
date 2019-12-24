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
4. DID 记录
5. DID 方法
6. DID 和针对隐私的设计
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

2016年，DID 规范的开发者们接受了 Christopher Allen 的提议。他建议通过使用同一种基本格式来让DID可以在多种区块链上使用，这种基本格式就是所谓 URN 规范：

![图片一 urn:uuid:fe0cde11-59d2-4621-887f-23013499f905](https://w3c-ccg.github.io/did-primer/did-primer-diagrams/urn-format.png "图片一")

对比用于DID时关键的不同，此结构中命名空间组件标识为一个DID方法。DID方法规范部分（第二部分）指定方法特定标识的格式。

![图片二 did:example:123456789abcdefghijk](https://w3c-ccg.github.io/did-primer/did-primer-diagrams/did-format.png "图片二")

DID 方法（将在后面的部分详述）定义了DID如何在一个特定的区块链上工作。所有DID 方法的标准必须定义格式并且生成对于方法唯一的标识。 值得注意，此方法唯一标识字符串**必须**在DID方法的命名空间中唯一。

## 4. DID 记录

你可以把DID 的基础设施看做一个全局的 [键-值数据库](https://en.wikipedia.org/wiki/Key-value_database)。这个数据库可以是所有DID兼容的区块链，分布式账本或去中心化的网络。在这个虚拟的数据库中，键是一个DID，对应的值是一个DID记录。DID记录用于描述公钥信息、认证所用的协议和必需的服务端点来与标识实体之间建立可验证的加密的交互。

一个DID记录是一个有效的 [JSON-LD 对象](https://json-ld.org/spec/latest/json-ld/)。这个对象使用DID规范中的所谓**DID语境**（属性名的RDF词汇表）来标准化。其中有六部分可选：

1. **DID本身**，所以这个DID记录完全只适用于描述自己。
2. **一套加密认证必要参数**，比如公钥，用来进行身份验证或与DID对象进行交互。
3. **一套加密认证所需协议**。用于与DID对象进行一些交互，比如身份认证或者是身份权利授予。
4. **一个服务端点的集合**。这个集合描述在哪里可以如何与DID对象交互。
5. 用于审查的**时间戳**。
6. **一个可选的JSON-LD签名**。用于在需要验证DID记录的完整性的时候。

戳这里**[DID规范](https://w3c-ccg.github.io/did-spec/)**来看看几个DID记录的例子。 （*55555翻译不动了，好长一个文本*）

## 5. DID方法
DID和DID记录可以被任何能正确处理、记录一条**唯一键对唯一值的数据**的现代的区块链，分布式账本或分布式网络兼容。至于区块链是否公开，私有，许可过的甚至无许可的吗，都无所谓。

DID方法规范的作用就是定义在一条特定的链上或者“目标系统”中，一对DID和DID记录是如何被创造、落地和管理的。

DID方法规范是泛用型的DID规范，就像URN命名空间规范是泛用型的[IETF URN 规范](https://tools.ietf.org/html/rfc8141)一样。

DID 方法规范针对一个特定的目标系统基本上定义了至少以下几个操作方法：

1. **Create** 一些DID方法可能直接用一个密码钥匙对生成一条DID。 其他一些则可能利用交易的地址或者一个恰到好处的合约。

2. **Read** 一些DID方法使用可以直接储存DID记录到链上的方式。另外一些可能会让DID解析器根据区块链记录的属性动态的构建他们。还有一些方案可能会选择在链上存储一个指针来指向一个或多个其他去中心化存储网络上的部分，比如 *[IPFS](https://en.wikipedia.org/wiki/InterPlanetary_File_System)* 或 *[STORJ](https://en.wikipedia.org/wiki/STORJ)*。

3. **Update** 更新操作是从安全角度来说一个最最最重要的部分，因为更改一个DID记录意味着可以控制公钥，或者掌控认证一个实体所必需的证明（这样一个攻击者就可以冒充这个实体）。因为DID记录的更新权限验证只能由目标区块链强制执行，DID方法规范必须精确定义如何对任何更新操作执行身份验证和相关授权。

4. **Delete** 区块链上记录的DID实体在定义上是不可变更的。所以他们永远不能做到传统的数据库意义上真正的“被删除”。然而它们可以在密码学意义上被“**废除**”。一个DID方法规范必须定义这样的操作如何被执行。比如，写一个无效DID记录。

戳戳戳[这里](https://w3c-ccg.github.io/did-method-registry/)来查看所有已知DID方法规范。


## 6. DID 和针对隐私的设计

隐私是任何一个身份管理解决方案的重要组成部分。对于一个使用不可更改公钥的区块链的全局身份系统来讲更是如此。不过还好，DID结构可以通过在最底层的基础设施上进行设计来整合隐私问题。因此，如果使用一下最佳实践结果进行部署，它将成为一种强大而全新的保护隐私的技术：

1. **成对的化名DID**。DID可以被用于做知名的的公用身份标识，当然也可以用于按用户关系来发放做为隐私身份标识。 所以与其像手机号或身份证号一样拥有一个唯一的DID，不如搞上几千个没有他的同意就不能关联的成对且唯一的DID。这种情况下，也还能像管理通讯簿一样管理这些玩意儿。

2. **链下私密数据**。 在一个公共区块链上存储任何形式的PII（个人身份信息），即使是加密过或者哈希过，也是非常危险的。原因有二：1）当这条信息与多个部分共享时，这条加密过或者哈希过的数据是一个全局的关联点。2）如果有一天这条加密信息被攻破（比如，被[量子计算](https://en.wikipedia.org/wiki/Quantum_computing)），这条数据就将永远的可以在一个无法更改的公共账本上被接入。。所以最好的方法还是将所有隐私信息下链，并且只在加密、隐私、p2p的连接中交换。

3. **选择性披露**。 分布式PKI（DPKI）使DID可以通过两种方式让个人对个人数据有更大的控制权。 第一种方式，它允许使用加密的数字凭证来共享（见下文）。其次就是这些证书可以使用零知识证明的加密技术来最小化数据，比如，你可以在不透露确切日期的情况下透露您已经超过某个年龄。

