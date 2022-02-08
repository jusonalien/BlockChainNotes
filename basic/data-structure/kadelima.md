## 关键部分

-   **Node ID** 在 P2P 网络中， 节点是通过唯一 ID 来进行标识的，在原始的 Kad 算法中，使用 160-bit 哈希空间来作为 Node ID。  
    
-   **Node Distance** 每个节点保存着自己附近（nearest）节点的信息，但是在 Kademlia 中，这个距离不是指物理距离，而是指一种逻辑距离，通过运算得知。  
    
-   **XOR** 异或运算，XOR 是一种位运算，用于计算两个节点之间距离的远近。把两个节点的 Node ID 进行 XOR 运算，XOR 的结果越小，表示距离越近。  
    
-   **K-Bucket** 用一个 Bucket 来保存与当前节点距离在某个范围内的所有节点列表，比如 bucket0, bucket1, bucket2 ... bucketN 分别记录[1, 2), [2, 4), [4, 8), ... [2^i, 2^(i+1)) 范围内的节点列表  
    
-   **Bucket 分裂** 如果初始 bucket 数量不够，则需要分裂（具体跟实现有关）  
    
-   **Routing Table** 记录所有 buckets，每个 bucket 限制最多保存 k 个节点，如下图：

## 参考

- https://zhuanlan.zhihu.com/p/40286711