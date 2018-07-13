# The Micoservices Based Mainchain/Sidechain (MMS) Architecture: BFDChain Design Principles
# 基于微服务的主链/侧链（MMS）架构：BFDChain设计原则
**Abstract— In this paper, we discuss the design principles and architecture of BFDChain. We propose a new Micoservices based Mainchain/Sidechain (MMS) architecture, which is a four-layer architecture design including mainchian layer, microservices core layer, sidechain layer and Dapps layer. We further discuss the different side chains such as DAOS Management Sidechain, System Service Sidechain, and Ecosystem Sidechain. Finally, the cross-chain communication is also discussed in detail.**

**摘要 - 本文讨论了BFDChain的设计原理和体系结构。我们提出了一种新的基于Micoservices的Mainchain / Sidechain（MMS）架构，这是一种四层架构设计，包括mainchian层，微服务核心层，侧链层和Dapps层。我们进一步讨论了不同的侧链，如DAOS Management Sidechain，System Service Sidechain和Ecosystem Sidechain。最后，还详细讨论了交叉链通信。**

## I.INTRODUCTION
## I.介绍
As a decentralized monetary fund service platform, BFDChain is dedicated on providing fast, robust, secure and easy-to-use fund management service. However, current blockchain technology has issues yet to be solved. Miscellaneous data, the performance of single chain structure, system upgrading[1], all of these issues are blockers to approach BFDChain’s ultimate goal by applying existing blockchain infrastructure.  

作为一个分散的货币基金服务平台，BFDChain致力于提供快速，稳健，安全且易于使用的基金管理服务。然而，目前的区块链技术还有待解决的问题。杂项数据，单链结构的性能，系统升级[1]，所有这些问题都是阻碍通过应用现有区块链基础设施来实现BFDChain的最终目标。

This paper introduces a refined sidechain design which helps BFDChain solve common issues named above without losing the advantages of blockchain architecture.  

本文介绍了一种精炼的侧链设计，它有助于BFDChain解决上述常见问题而不会失去区块链架构的优势。  

The rest of this paper is organized as follows: In Section II, we present the analysis why do we need to design a mainchain and sidechain. We then provide detailed technical discussion of the architect of BFDChain in terms of different components in Section III and some basic scenarios and workflow in IV. Section IV includes concluding remarks of our design.  

本文的其余部分安排如下：在第二部分，我们提出分析为什么我们需要设计一个主链和侧链。然后，我们根据第III部分中的不同组件以及IV中的一些基本方案和工作流程，提供BFDChain架构师的详细技术讨论。第四节包括我们设计的结束语。

##II.WHY DOES BFDCHAIN USE MAINCHAIN AND SIDECHAIN DESIGN?
## II. 为什么BFDCHAIN使用MAINCHAIN和SIDECHAIN设计?  

BFDChain is designed to provide various of services including monetary fund management, issuing data currency, trading transaction certification, legal consulting [1]. These services have completely different service interfaces, data format, contract format. Also, each of the services will have its own metadata, management targets, transaction principle. If all of these data are mounted on the a single chain, like the traditional Ethereum[2] does, the potential issues are multi-folded:  

BFDChain旨在提供各种服务，包括货币资金管理，发行数据货币，交易事务认证，法律咨询[1]。这些服务具有完全不同的服务接口，数据格式，合同格式。此外，每个服务都有自己的元数据，管理目标，交易原则。如果所有这些数据都安装在单个链上，就像传统的以太坊[2]那样，潜在的问题是多方面的：  

**a. Chain Length explosion**     
Because PoW in BFDChain will not be as hard as Ethereum[2] and BitCoin[3], the number of transactions in each block is likely to be small. With all transactions recorded on the mainchain, BFDChain will run out of total asset in a short time.  

**a. 链长爆炸**
因为BFDChain中的PoW不会像以太坊[2]和BitCoin [3]那么难，所以每个块中的事务数量可能很小。由于所有交易记录在主链上，BFDChain将在短时间内耗尽总资产。  

**b. Impossible access control.**
Different services might have different users and group permissions. This will also be applicable to the granularity of the data access and user authentication.

**b. 不可能的访问控制**
不同的服务可能具有不同的用户和组权限。这也适用于数据访问和用户身份验证的粒度。  

**c. Hard chain upgrade and new service registration.**

BFDChain will not be completed in the first version. Instead, new features and services will keep coming up in new releases. Without sidechain, new features and services cannot be rolled out incrementally. Any unforeseen issue will impact all services at the same time.

With sidechain, data isolation is the nature. This will not only mitigate the pressure on the mainchain, but also make each service more focus on its own business logic instead of sacrificing computation power on data segregation.  

**c. 硬链升级和新服务注册**. 
BFDChain will not be completed in the first version. Instead, new features and services will keep coming up in new releases. Without sidechain, new features and services cannot be rolled out incrementally. Any unforeseen issue will impact all services at the same time.  

BFDChain将不会在第一个版本中完成。相反，新功能和服务将不断出现在新版本中。没有侧链，新功能和服务无法逐步推出。任何不可预见的问题都会同时影响所有服务。

使用侧链，数据隔离就是本质。这不仅可以缓解主链上的压力，还可以使每项服务更加专注于自己的业务逻辑，而不是牺牲数据隔离的计算能力。

With sidechain, data isolation is the nature. This will not only mitigate the pressure on the mainchain, but also make each service more focus on its own business logic instead of sacrificing computation power on data segregation.  

## III. BFDChian's Architecture
##III. BFDChian架构
The BFDChain’s architecture is illustrated in Fig.1 below.

BFDChain的架构如下图1所示:

![BFDChain的架构](https://raw.githubusercontent.com/krisdwang/befund/master/images/Fig-3.png)

From Fig.1, we can see the BFDChain has four-layer architecture:  
* Layer 1: Mainchain layer 
* Layer 2: Microservices core layer 
* Layer 3: Sidechains layer 
* Layer 4: Dapps layer
Where the mainchain layer is the backbone layer for BFDChain. Dapps layer is built on top of sidechain layer, and both layers are in parallel to the microservices core layer and are both able to communicate with microservices core layer directly via REST APIs.  

从图1中可以看出BFDChain具有四层架构：
* 层1：主链层
* 层2：微服务核心层
* 层3：侧链层
* 层4：Dapp层
主链层是BFDChain的主干层。 Dapps层构建在侧链层之上，两个层都与微服务核心层并行，并且都能够通过REST API直接与微服务核心层通信。

###3.1 Mainchian Layer 主链层
To well decoupled the functionality of services, all services provided by BFDChain will be on sidechains. As a result, the major usages of the mainchain is to record all BFDT transactions plus providing basic infrastructure of BFDChain, such as Authentication and Consensus mechanism. We will illustrate them as below:  

为了更好地解耦服务功能，BFDChain提供的所有服务都将在侧链上。因此，主链的主要用途是记录所有BFDT事务以及提供BFDChain的基本基础结构，例如身份验证和共识机制。我们将在下面说明它们：

_**Authentication Mechanism Module 认证机制模块**_
In the mainchain, we will use general ECC for cryptophytic purpose. For the organization that has the regulation requirements, we will use Unlikable Secret Handshake (USH) [11] to achieve the sidechain storage, anonymous and unlinkable coin spending for BFDChain.  

在主链中，我们将使用通用ECC进行加密。对于具有监管要求的组织，我们将使用Unlikable Secret Handshake（USH）[11]来实现BFDChain的侧链存储，匿名和不可链接的币支出。

_**Consensus Mechanism Module 共识机制模块**_
BFDChain uses an innovate consensus mechanism named VM-DPoSW to process the consensus. Some high-level principles are listed below:

a. The Virtual machines (VMs) are created by the smart contract for mining and delegation purpose.

b. VMs are put into different categories based on different computing power, and put into the candidate pool.

c. VMs from each category are selected as the delegates and put into the delegates cluster pool, and ensure the sum of sharehold represented by the VMs are >50% of the total sharehold.

d. Divide the time sprearum into N slots. In each time slot, allocate same number of time windows for VMs in each category to perform the mining of the new block.

e. Essentially, VM-DPoSW gives each delegate an equal opportunity to participate the mining process regardless the computing power of the delegate. Furthermore, the delegate with higher computing power will process more transitions request (thus more blocks) and generate more rewards to the shareholder with higher share stake, even it was given equal process time comparing with other delegates with lower computing power.

BFDChain使用名为VM-DPoSW的创新共识机制来处理共识。下面列出了一些高级原则：

a. 虚拟机（VM）由智能合同创建，用于挖掘和委派目的。

b. VM根据不同的计算能力分为不同的类别，并放入候选池中。 

c.选择每个类别的虚拟机作为代理并将其放入代理群集池中，并确保虚拟机所代表的股份总和超过总股本的50％。 

d.将时间sprearum划分为N个槽。在每个时隙中，为每个类别中的VM分配相同数量的时间窗口以执行新块的挖掘。

e. 即从本质上讲，无论代表的计算能力如何，VM-DPoSW都为每个代表提供了参与挖掘过程的平等机会。此外，具有更高计算能力的代表将处理更多的转换请求（因此更多的块）并以更高的份额向股东产生更多的奖励，即使与具有较低计算能力的其他代表相比，它被给予相等的处理时间。

_**Token Module Token模块**_
Similar to the concept of “Gas” in the Ethereum, BFDChain use “BFDT” to fuel the transaction and provide rewards as incentive for miners. Token module is used to manage and record the BFDT transactions.  

与以太坊中的“Gas”概念类似，BFDChain使用"BFDT"为交易提供燃料，并为矿工提供奖励。令牌模块用于管理和记录BFDT事务。

###3.2 Microservices Core Layer 微服务核心层
Mircroservices, as point out in [9][10], is “an architecture style, in which large complex software applications are composed of one or more services.” The main characteristics of Microservices are 1) highly Modulated. Service A does not need to know the implementation detail of another service B before them can communicate; 2) services are independently deployed with loosely coupled. 3) The communication among services are usually through REST API.  

正如[9] [10]中指出的那样，Mircroservices是“一种架构风格，其中大型复杂软件应用程序由一个或多个服务组成。”微服务的主要特征是1）高度调制。服务A在他们可以通信之前不需要知道另一个服务B的实现细节; 2）服务是独立部署的，松散耦合。 3）服务之间的通信通常是通过REST API进行的。

Here in BFDChain, as sidechains share a lot of common characteristics and require a set of common services, we design the microservcies core layer to provide shared services such as service registry, metadata service, smart contract templates and execution, and access control service to sidechains and their DApps.  

在BFDChain中，由于侧链共享许多共同特征并需要一组公共服务，因此我们设计了microservcies核心层，以提供服务注册表，元数据服务，智能合约模板和执行以及侧链访问控制服务等共享服务和他们的DApps。

The following services are included in the BFDChain Microservcies Core: 

以下服务包含在BFDChain Microservcies Core中：

**a. Service Registry 服务注册**
This Service is used to manage the metadata of management services. Service Registry Service will be acted as the Manager of the Manager (MoM) for BFDChain platform and will be responsible for services (including mainchain and sidechains) upgrade, rollout, triage and rollback. Also, any new services provided by BFDChain will be registered here with its metadata before the actual use. Services metadata will typically include service uuid, service symbol, service smart contract address, commission fee type (percentage or fix rate or dynamic lambda function), DApp DNS address and so on.  

此服务用于管理管理服务的元数据。 服务注册服务将担任BFDChain的平台经理（MoM），负责服务（包括主链和侧链）升级，部署，分类和回滚。此外，BFDChain提供的任何新服务都将在实际使用前在其中注册其元数据。服务元数据通常包括服务uuid​​，服务符号，服务智能合约地址，佣金费用类型（百分比或修复率或动态lambda函数），DApp DNS地址等。

**b. DS Metadata Service DS元数据服务**
This Service will be used to manage the metadata of each DS, such as fund raise, issuance, liquidation, authentication, real time exchange rate to BFDT and services using history.  

该服务将用于管理每个DS的元数据，例如资金筹集，发行，清算，身份验证，BFDT的实时汇率和使用历史记录的服务。

**c. Smart Contract Template and Execution Service 智能合约模板和执行服务**
This service is two folded. First, smart contract template will store various templates of smart contract used for fund transaction or metadata modification.

这项服务是两个部分，首先，智能合约模板将存储用于基金交易或元数据修改的各种智能合约模板。

Secondly, the Smart Contract Execution is used as decentralized data store for Smart Contract Execution DApp, such as DSL definition, execution container prerequisites and execution container package location.  

其次，智能合约执行用作智能合约执行DApp的分散数据存储，例如DSL定义，执行容器先决条件和执行容器包的位置。

**d. Access Control Service 访问控制服务**
Used as decentralized data store for Access Control DApp.
用作访问控制DApp的分散数据存储

We need to point out that the services modules above can also be treated as special sidechains so that each module can be further extended to add sub-sidechain functionalities.
我们需要指出的是，上面的服务模块也可以被视为特殊的侧链，以便可以进一步扩展每个模块以添加子侧链功能。

By implementing the microservices core layer, we achieved the following benefits:
通过实施微服务核心层，我们实现了以下好处：

* Technology Heterogeneity 技术异质性
* Resilience 弹性
* Scaling 扩展性
* Ease of Deplyment 易于部署
* Organizational Alignment 组织协调
* Composability 组合性
* Optimizing for Replaceablity 优化可替换性

For example, when a sidechain A wants to perform an upgrade, the microservices layer helps A to achieve the upgrade goal by just upgrading the metadata information registered in the microservices, instead of whole sidechain A upgrade, hence greatly increases the efficiency.  

例如，当侧链A想要执行升级时，微服务层通过仅升级微服务中注册的元数据信息而不是整个侧链A升级来帮助A实现升级目标，因此大大提高了效率。

###3.3 Sidechain Layer 侧链层
The sidechain layer is built on top of mainchain layer. It takes all of innovating features in the mainchain such as authentication and consensus mechanism, and makes use of microservice core to provide support for service DApps as well as customized token managements.  

侧链层构建在主链层的顶部。它充分利用了主链中的所有创新功能，如身份验证和共识机制，并利用微服务核心为服务DApps和自定义令牌管理提供支持。

We define four different types of sidechains, namely, the built-in system service sidechain, the DAOS sidechain, ecosystem sidechain and consortium sidechain. We will address the detail of each kind of sidechain in the next section.  

我们定义了四种不同类型的侧链，即内置系统服务侧链，DAOS侧链，生态系统侧链和财团侧链。我们将在下一节中介绍每种侧链的细节。

###3.4 DApps Layer DApps 层
The DApps layer is built on top of sidechain layer. It provides various application services to investors such as media and legal services. Each DApps is decoupled, single-featured and has a corresponding sidechain as its data support.  

DApps层构建在侧链层之上。它为媒体和法律服务等投资者提供各种应用服务。每个DApps都是分离的，单一特征的，并且具有相应的侧链作为其数据支持。  

## IV. SIDECHAIN DESIGN 
## IV. 侧链设计

The overview of side chain design and its relationship to the mainchain and microservices core is illustrated in Fig.2.

侧链设计概述及其与主链和微服务核心的关系如图2所示。

![sidechain](https://raw.githubusercontent.com/krisdwang/befund/master/images/Fig-4.png)

Fig. 2 The sidechain design and its relationship to the mainchain and microservices

### 4.1 System Service Sidechain





