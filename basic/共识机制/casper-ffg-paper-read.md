## 论文地址

- [Casper the Friendly Finality Gadget](https://arxiv.org/pdf/1710.09437.pdf)
- [Casper FFG算法及流程解析](https://www.21ic.com/article/788310.html)

Casper-FFG为ETH2的共识协议

FFG是参考了BFT的共识机制，然后引入了一些新的特性开发出来的，相比BFT有如下特点：

- 问责制：Accountability
> validator是可以被检查到作恶行为，并且可以对作恶validator进行惩罚，POS机制的安全性很大程度上依赖这一特点

- 动态变换validator
> 成为validator的节点是会不断变化的

- 保护机制
- 模块化的overlay


一个投票的信息包含

- $s$：任何一个合理的检查点的hash，可以当作是source
- $t$：目标检查点的hash
- $h(s)$：检查点s在认证树当中的高度
- $h(t)$：检查点t在认证数当中的高度
- $S$：$<s,t,h(s),h(t)>$ validator的验证签名格式


validator在做提议认证的时候，

$$<v,s_1,t_1,h(s_1),h(t_1)>$$
$$<v,s_2,t_2,h(s_2),h(t_2)>$$

必须要遵循如下两个规则（这两个规则也后续做安全性证明的关键条件）：

- 规则1：$h(t_1)=h(t_2)$:不能在相同的高度上选出两个投票
- 规则2：$h(s_1)<h(s_2)<h(t_2)<h(t_1)$ 


## 接下来是对FFG的安全性和活跃性的证明

安全性(accountable safety)

>怎么理解？：两个有冲突的检查点最终只有一个会被选举认证，除非有超过$1/3$的validator一起来干这件坏事！

活跃性(Plausible Liveness)
>怎么理解？：无论之前发生了什么事情，只要还有超过$2/3$的validator还在遵循这个协议，就能继续进行选举过程

基于只有小于$1/3$作恶节点的假设场景，可以推出如下四条FFG的性质

- 性质1、如果$s_1 \rightarrow t_1$与$s_2 \rightarrow t_2$是两条独立的链，那么$h(t_1) \neq h(t_2)$，基于（规则1）
- 性质2、如果$s_1 \rightarrow t_1$与$s_2 \rightarrow t_2$是两条独立的链，那么 $h(s_1)<h(s_2)<h(t_2)<h(t_1)$是不存在的，基于（规则2）

对于特定高度n

- 性质3、最多存在一条链$s \rightarrow t$ 能满足$h(t)=n$
- 性质4、最多只有一个检查点满足高度n

基于上述的两个规则和四个性质，开始开始证明FFG安全性，

> 要证明安全性，等价于证明如果两个checkpoint，$a_m$和$b_m$有冲突的，最终只有一个会被敲定

如何理解冲突的checkpoint？可以理解成，在同一个高度上存在两个checkpoint等着被敲定

证明过程：假设有两个checkpoint $a_m$（$a_{m+1}$为$a_m$的子节点，满足$h(a_m)+1=h(a_{m+1})$）和$b_n$（$b_{n+1}$为$b_n$的子节点，满足$h(b_n)+1=h(b_{n+1})$）,现在$a_m$和$b_n$遇到冲突了，不失一般性，令$h(a_m) < h(b_n)$（如果$h(a_m)=h(b_n)$,就说明有$1/3$个validator违背了规则1）

我们让
$$r \rightarrow b_1 \rightarrow b_2 \rightarrow ... \rightarrow b_n$$
作为一个检查链，因此也会存在一个检查链

$$r \rightarrow b_1 \rightarrow b_2 \rightarrow ...,b_i \rightarrow b_{i+1},...,\rightarrow b_n \rightarrow b_{n+1}$$

由于性质4的原因，$h(b_i)$是不等于$h(a_m)$或$h(a_{m+1})$的，

让$j$为满足$h(b_j)>h(a_{m+1})$的最小整数，就有$b_{j-1}<h(a_{m+1})$（如果$h(b_{j-1})=h(a_m)$，就违反了规则一）。基于上述，我们可以推断有一个检查链是包围住
$a_m$和$a_{m+1}$,从而导致与规则二相矛盾了

心得：第一次看这个证明的时候可能会有疑问，不是已经明明有了规则1，每个validator都不能在相同高度上选择两个票了吗？为啥还要去证明不可能会有两个同高度的票不可能被最重敲定呢？感觉就像一开始规定了1+1=2，然后还要跑去证明1+1不等于2是错的一样。其实读者要认识到FFG要解决的是在拜占庭环境下的一致性问题，也就是即使你规定了每个validator要做什么，但是这也只是诚实的validator，你无法保证恶意的validator会做一些与规则相反的事情

证明思路总结：用反证法去证明，协议一开始给定两个规则，由这两个规则衍生出四个特性，然后假定存在有冲突checkpoint被敲定了，基于此，将各不符合这个框架内的分支场景都排除掉，最重剩下一个场景，但是也会造成与规则矛盾

活性证明：新的checkpoint的敲定行为都能够基于存量的检查数的最高层去执行，假设$a$是最高点，$b$是目标检查点，$a$的子孙$a^{\prime}$，满足$h(a^{\prime})=h(b)+1$ 也能在不违背规则1、规则2的条件下去完成敲定，然后$a^{\prime}$能通过增加一个从$a^{\prime}$到其子孙checkpoint的检查树，有点数学归纳法那味了