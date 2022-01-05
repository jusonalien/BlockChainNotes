研究ETH2.0的一些参考资料

最好的学习资料还是要去看官方的文档以及相关的paper，而不是那些看起来似懂非懂的二手资料


## 区块链可扩展性的前置知识

- [Notes on Scalable Blockchain Protocols (version 0.3.2)](https://github.com/vbuterin/scalability_paper/blob/master/scalability.pdf)
## ETH2.0共识协议

ETH正在从POW转成POS，但是不可能一下子就能直接转换，肯定是相互共存的模式作为切换方案

相关的一些历史背景
一开始有Capser CBC 和Casper FFG两条主导线路，

- [Casper the Friendly Finality Gadget](https://arxiv.org/pdf/1710.09437.pdf)

[[casper-ffg-paper-read]]

## 激励机制

- [Incentives in Ethereum’s Hybrid Casper Protocol](https://arxiv.org/pdf/1903.04205.pdf)

## ETH2.0的相关实现

信标链的相关客户段prysm
- https://github.com/prysmaticlabs/prysm

官方协议文档：

- https://github.com/ethereum/consensus-specs