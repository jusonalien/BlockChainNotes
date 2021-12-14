
在POW对共识机制针对那些解决特定密码学问题的参与者给予奖励，以此授予验证交易和创建区块的权限，而在POS的场景下，validators都会有机会被轮流到来做这些事情，而每个validators到权重依据所质押的token数量，POS的显著的优势就是安全性、减少中心化的危险、减少能耗。

POS的共识机制粗略的来说，在链上持续追踪哪些被质押过token的节点，我们管他们叫做`validators`，区块产生和共识都发生在这些`validators`,在这种场景下，衍生出多种一致性的算法，多种用来奖励参与到共识机制的`validators`的方案。

对于validators之间的共识算法主要分两类
- 基于链POS共识
> 再某个时间段内按照特定要求选择一个validator，并授予它创建区块（这个区块必须是指向前一个区块，遵循最长链原则）
- 基于BFT（拜占庭容错）的POS共识

## POS相对于POW的好处

- 不用担心电力损耗的问题
- 不用担心发行新币去激励参与者，
- 通过经济惩罚使得51%的攻击，使其攻击成本远超POW的攻击场景

## 参考

- [Proof of Stake FAQs](https://eth.wiki/concepts/proof-of-stake-faqs)