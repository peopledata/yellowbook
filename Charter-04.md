# datomspod—「数悦坊」

datomspod，「数悦坊」是实现数据资产安全、可信和隐私保护使用和交收的一种工具。

个人数据开发利用的一个很核心的原则是“在隐私、安全以及开放这三个因素构成的“不可能三角”前提下，基于场景寻求最优解决方案”。「数悦坊」是基于此原则而实现的一个方案。

<img src="https://static.wixstatic.com/media/c121d8_bcb8aa6657ac4520b8da971fe138487d~mv2.png/v1/fill/w_980,h_883,al_c,usm_0.66_1.00_0.01,enc_auto/%E4%B8%8D%E5%8F%AF%E8%83%BD%E4%B8%89%E8%A7%92.png" alt="不可能三角.png" style="zoom:25%;" />

## datomspod「数悦坊」的核心原则

- - **数据“不动”**：数据不会传输、复制给数据使用者；数据所有者依然保持对数据的主权。

- - **算法“动”**：数据使用者或第三方提供的算法（算法市场），经过数据所有者审核、审查和同意后处理目标数据。数据使用者将算法提交到datomspod中，处理数据。数据处理结果按照约定返回给数据使用者。

- - **OPS-C模型**：基于在“开发、隐私和安全”不可能三角下，基于场景构建的定性、定量模型作为基准，建立可评价体系。

- - **持续审计**：在datomspod中，参考OPS-C模型，对算法处理数据的全过程存证，确保按照相关的数据使用协议处理数据。规避“隐私洞察”以及未经授权、超授权的行为。
  - **零+架构**：采用“零拷贝、零知识、零信任”的技术堆栈，实现对个人数据的开发利用从“不作恶”（Don’t be Evil）到“不能作恶”（Can’t be Evil）。 

- 

## 1. 概述

可以将datomspod看作是“安全的数据资产交收场所”：数据使用方、数据提供方、见证方和（或）其他中介（如算法服务商）达成协议。协议约定在某个时间段内，数据提供方按照协议的要求把特定的数据资产交付到指定的一个datomspod（「数悦坊」）中；与此同时，数据使用方也按协议的要求把经过审核的算法交付到此。datomspod一个对各方都安全、可信的环境。

在「数悦坊」中，处理数据的算法有多种选择，例如隐私计算。

基于数据交收的典型场景，datomspod主要有以下几种类型：

- **ad hoc型**：数据交易各方协商仅从「数悦坊」的开源镜像开始构建「数悦坊」，不依赖任何第三方。

- **自主型**：数据交易各方基于「数悦坊」开源和IaaS基础设施，自主创建「数悦坊」。

- **委托型**：数据交易各方委托第三方为自己创建「数悦坊」，并使用。

  

## 2. datomspod的结构

datomspod的一般结构包括如下几个部分：

- 任务容器
  - 采用k8s编排和部署的多个容器。
  - 每个容器履行一种特定功能或服务。
  - 必须具备的四个基础容器：
    - datomspace pod：接收、输出datoms的pod;
    - Computing pod：对数据进行处理的pod；
    - Oracle pod：与metachian进行交互的Oracle； 
    - Server pod: 提供任务调度、合约执行以及管理、记账等服务的pod；
- 每个pod的具体功能（v1.1.0）- 见datomspod的说明文档
- 算法/算力平台选择
  - 计算环境：数据使用需要“安全、隐私、可信”的计算环境。datomspod主要是基于python的AI、隐私计算开源框架来构建计算环境，采用pytorch、pypi、pysyft、TFF等开源的AI、隐私计算软件。
  - 算法：提交的算法事先需要经过数据所有者或审核代理人的审核，并在metachain上存证；并在每次运行前验证代码一致性（checksum sha256）。
  - 算力根据数据使用方的预测，进行算力规划，并计入数据使用合约中记账。
- 服务
  - 针对不同的应用场景，定制datomspod的配置；
  - 开发的生态，可以调用peopledata生态经过审核的pods。



## 3. datomspod不同模式的详细说明

从数据使用者、数据所有者的视角看，datomspod是“风险中性”的。根据所使用的datomspod的不同类型，datomspod的架构存在一些差异。

#### 3.1 ad hoc型

数据交易各方协商仅从「数悦坊」的开源镜像开始构建「数悦坊」，不依赖任何第三方。完全由数据交易的双方约定和实施。

交易双方仅使用datomspod提高的开源软件镜像或工具（例如，docker化的工具包）。而其他的所有事项都由交易双方完成，包括但不限于使用谁的算力、在哪里实现以及算法等。

#### 3.2 自主型

数据交易各方基于「数悦坊」开源和IaaS基础设施，自主创建「数悦坊」实例，并基于此实例进行使用和运用。

在此种模式下，数据交易各方需要租用或购买第三方基础设施服务，主要是虚拟机、存储和数据中心等IaaS。

#### 3.3 委托型

数据交易各方委托第三方为自己创建「数悦坊」实例，并使用和运用。



## 4. datomspod的说明

### 4.1 算法资产

针对某类数据处理的算法也是资产。算法通常包括：

- code
- docker image(base image + tag)
- an entry point

算法需要一个计算环境，因此首先定义一个环境。

```json
{
  "algorithm": {
    "container": {
      "entrypoint": "node $ALGO", // the docker entrypoint. $ALGO is a macro that gets replaced inside the compute job, depending where your algorithm code is downloaded.
      "entrypoint": "python3.6 $ALGO",  
      "image": "node",  //The docker image name the algorithm will run with.
      "tag": "latest" // the docker image tag that you are going to use.
    }
  }
}


------ 运行js/node的环境
{
  "algorithm": {
    "container": {
      "entrypoint": "node $ALGO",
      "image": "node",
      "tag": "14"
    }
  }
}

------- 运行python的环境
{
  "algorithm": {
    "container": {
      "entrypoint": "python3.9 $ALGO",
      "image": "python",
      "tag": "3.9.4-alpine3.13"
    }
  }
}
```

### 4.2 数据存储

在k8s中，作为计算任务的一部分，每一个运行在pod中的算法，其存储都需要mounted:

| Path            | Permissions | Usage                                                        |
| :-------------- | :---------- | :----------------------------------------------------------- |
| `/data/inputs`  | read        | Storage for input data sets, accessible only to the algorithm running in the pod. Contents will be the files themselves, inside indexed folders e.g. `/data/inputs/{did}/{service_id}`. |
| `/data/ddos`    | read        | Storage for all DDOs involved in compute job (input data set + algorithm). Contents will json files containing the DDO structure. |
| `/data/outputs` | read/write  | Storage for all of the algorithm’s output files. They are uploaded on some form of cloud storage, and URLs are sent back to the consumer. |
| `/data/logs/`   | read/write  | All algorithm output (such as `print`, `console.log`, etc.) is stored in a file located in this folder. They are stored and sent to the consumer as well. |

### 4.3 算力

依据算法的需求来定制算力。算力提供一般会在数据使用合约中明确。例如，需要GPU的数量等。

考虑到大规模集群的并行计算能力，通常算力需求需要实现声明需要的标准计算节点数量以及对内核的要求。

### 4.4 数据使用合约（计算合约）

每一个datomspod的生命周期都是由数据交易各方签署、确定的**数据使用合约**创建、管理的。

**数据使用合约**首先依据合约的条款和要求，创建一个满足其要求的datomspod实例。然后交付这个实例给**数据使用合约**使用。并在使用完成后，被永久删除。

**数据使用合约**是一个智能代理（agent），一般的，它的工作流程是：

##### 第一步：签署合约

数据交易各方签署数据使用合约，该合约被解析为一个metachain上的智能合约。

```markdown
// 合约的JSON表达的主要内容包括：
1. Input事项
		数据
		算法
		算力资产
2. 商务要求
		记账
		计费
		期限等
3. 附加要求
4. Output事项
		输出结果
		输出方式

// token化
智能合约的执行，需要token化计算的诸元素。

// 依据不同的区块链，合约的实现方式不同

```

##### 第二步：创建datomspod实例

依据数据使用合约，创建datomspod实例。

```yaml
// datomspod实例的yaml描述

apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: datomspod-operator
  name: datomspod-operator
  namespace: datomspod
spec:
  progressDeadlineSeconds: 600
  replicas: 1
  revisionHistoryLimit: 10
  selector:
    matchLabels:
      app: datomspod-operator
  strategy:
    rollingUpdate:
      maxSurge: 25%
      maxUnavailable: 25%
    type: RollingUpdate
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: datomspod-operator
    spec:
      containers:
      - env:
      // 算力的要求
        - name: Cloud_ACCESS_KEY_ID
          value: somekey
        - name: Cloud_SECRET_ACCESS_KEY
          value: somesecret
        - name: Cloud_REGION
          value: beijing 
      // 存储的选择: IPFS或本地存储
        - name: IPFS_OUTPUT
          value: http://youripfsserver:5001
        - name: IPFS_OUTPUT_PREFIX
          value: http://youripfsserver:8080/ipfs/
        - name: IPFS_ADMINLOGS
          value: http://youradminipfsserver:5001
        - name: IPFS_ADMINLOGS_PREFIX
          value: http://youradminipfsserver:8080/ipfs/
        - name: IPFS_EXPIRY_TIME
          value: "3600"
        - name: IPFS_API_KEY
          value: "apikey"
        - name: IPFS_API_CLIENT
          value: "clientid"
        - name: STORAGE_CLASS
          value: standard
       // 日志
        - name: LOG_LEVEL
          value: DEBUG
       // 容器规格
        - name: POD_CONFIGURATION_CONTAINER
          value: atomsbeijing/pod-configuration:v1.0.10
        - name: POD_PUBLISH_CONTAINER
          value: atomsbeijing/pod-publishing:lates
        - name: POD_METACHAIN_CONTAINER
          value: atomsbeijing/pod-metachain:latest // 和metachain的链桥
       # 可选的数据库 postgres db/ 默认情况不采用数据库 
        image: atomsbeijing/operator-engine:latest
        imagePullPolicy: Always
        name: datomspod-operator
        resources: {}
        terminationMessagePath: /dev/termination-log
        terminationMessagePolicy: File
      // 其他参数
```

##### 第三步：datomspod实例工作

数据交易各方按照各种义务，提供相关的输入。满足条件后，datomspod实例启动工作。

完成合约约定的事项后，datomspod实例输出结果。

##### 第四步：删除datomspod实例

输出结构后，从接收方收到凭证后。合约代理删除datomspod。

##### 第五步：合约计算log存证

整个合约计算按约定将log存证在metachian上。

##### 第六步：交易合约的清算/结算

交易各方按合约约定，进行此次交易的清算/结算。

## 5. datomspod V1.1.0

参见[datomspod v1.1.0](https://hub.docker.com/r/atomsbeijing/datomspod)





#### 编写人： [jerry.zhang](jerry.zhang.bill@gmail.com)
#### MIT License
