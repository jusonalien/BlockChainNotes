

 [Casper the Friendly Finality Gadget](https://arxiv.org/pdf/1710.09437.pdf) 这篇论文只是比较粗略的介绍和简单证明了Casper-FFG的模型，很多细节都没有涉及到，但是用来大致了解Capser-FFG已经很够了，参考里面的 [Combining GHOST and Casper](https://arxiv.org/pdf/2003.03052.pdf) 这篇论文是主要介绍了FFG的实现以及更严谨的数学推导
 
 
 ## 2 Setup and Goals
 
 ### 2.5 Safety and Liveness
 
 ### 2.6  Time, Epochs, and Synchrony
 
 使用`slots`和`epoch` 来衡量时间单位，slot 是一个固定的秒数常量，比如12秒为一个slot，epoch 为固定个数的slot，比如64个slot为一个epoch，这样做可以将时间做好分片管理，他们之间的边界可以当作检查点(`checkpoints`),起始区块的slot号就是0，也是epoch为0的第一个区块
 
 我们不应该去对验证节点收到消息的时间做任何假设，
 
 ## 3  Main Ingredients(主要组成)

### 3.1 Casper FFG

casper使用树状结构来设计出一种更宽泛的区块链一致性协议，
- 每个区块都有对应的高度（height），由当前区块与初始区块（高度为0）的距离定义，也就是说区块B的高度就是$chain(B)-1$
- 定义检查点区块，高度是常量$H$的整数倍（H=100），那么检查点区块下的可以当作子树来看
- 证人(Attestations): 是包含检查点A和B作为边的签名消息，可以看作是从A到B的一次投票，选择区块A，B的逻辑不包括在Casper内，由区块链底部实现，每个`证人`都包含权重，由验证器所质押的数目决定。

$h(B)=h(A)+1$是最理想的一种情况，但事实上并不强制要求如此，如果H=100，那么诚实的验证器有可能会丢失200号的区块，这种场景下底层的区块链可能会想让他发送一个从100号区块到300号区块到`证人`

Casper-FFG引入了 justification 和finalization两个概念，类似于PBFT理论里面的prepare 和commit基于阶段的概念

### 3.2  LMD GHOST Fork-Choice rule(分叉规则)

LMD-GHost : Latest Message Driven Greediest Heaviest Observed SubTree
类似于一种贪心的规则
![[Pasted image 20220111213332.png]]

## 4 Main Protocol: Gasper
### 4.1  Epoch Boundary Blocks and Pairs

### 4.2  Committees（委员会）

委员会的意义在于将验证器之间的责任划分开来

## 参考
- [Combining GHOST and Casper](https://arxiv.org/pdf/2003.03052.pdf)
