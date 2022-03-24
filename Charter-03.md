# 个人数据开发利用的“不可能三角”


个人数据的开发利用的一个很核心的原则是“在隐私、安全以及开放这三个因素构成的“不可能三角”前提下，基于场景寻求最优解决方案”。「数悦坊」是基于此原则而实现的一个协议。

<img src="https://static.wixstatic.com/media/c121d8_bcb8aa6657ac4520b8da971fe138487d~mv2.png/v1/fill/w_980,h_883,al_c,usm_0.66_1.00_0.01,enc_auto/%E4%B8%8D%E5%8F%AF%E8%83%BD%E4%B8%89%E8%A7%92.png" alt="不可能三角.png" style="zoom:10%;" />

## OPS-C模型

$$
\mathrm{O}:开放性；\mathrm{P}：隐私；\mathrm{S}: 安全；   \mathcal{C}：场景
$$

某一个特定的场景$\mathcal{C}_i$可以用其对个人数据的三个属性，即开放、隐私和安全的不同要求进行度量（以下简称：“**OPS-C度量**”）。

```html
OPS度量：是一种定性加定量结合的方式，对一个数据开发利用场景对于个人数据的三个属性的不同要去而进行的度量。

OPS-C模型： OPS度量是一个向量,所有场景的OPS度量构成一个向量空间。这个向量空间存在最优解。
```

我们假设$\mathrm{O},\mathrm{P},\mathrm{S} \in [0,1]$，那么三种场景的度量可以表示为：
$$
\mathcal{C}_i=[0.1, 0.5,0.2],\mathcal{C}_j=[0.8, 0.1,0.4],\mathcal{C}_k=[0.2, 0.2,0.2]
$$
虽然在实际应用中，很难对某个场景的上述度量进行定量的测算。但可以通过一些定性和定量结合的方式来测度。例如，数据安全性设定为5个级别。

**OPS-C模型**是个人数据开发利用新范式的一个基础模型，旨在明确个人数据开发利用过程中需要始终兼顾的原则，并让市场参与各方形成共识。

**OPS-C模型**将基于不同的场景来构建最优的解决方案。例如，同意是需要用到个人消费数据的两个场景，其**OPS-C度量是不一样的**：

```json
例子：
拟开发利用的个人数据类型：个人消费数据。
1. 场景A：消费信贷 
	OPS-C模型:{
		开放： 全部消费数据；
    隐私： 不披露具体购买的商品品牌、型号、规格等；
    安全： 采用隐私计算使用数据；仅对持牌金融机构申请消费信贷场景下开放
	}

场景B：JD促销
	OPS-C模型：{
    开放： 全部消费数据；
    隐私： 披露具体购买的商品品牌、型号、规格，但不披露价格、消费金融等；
    安全： 采用隐私计算使用数据；仅对JD推送促销获得场景开放；
  }

```

## 