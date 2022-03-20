---
title: 算法导论知识点
tags: [Algorithm]
categories:
  - [专业基础, 算法]
date: 2021-01-03 22:34:47
cover: bg.png
top_img: false
toc_number: false
---

## CH22 基本图算法
概述：本章介绍图的表示和图的搜索。图有邻接链表和邻接矩阵两种表示方法。有BFS和DFS两种搜索方法。使用DFS完成的实际应用有拓扑排序和计算强联通分量。

### 22.1 图的表示
- 邻接链表
    + 空间需求：Θ(V+E)
    + 遍历一遍的时间复杂度：Θ(V+E)   --先访问数组，再访问链表
    + 由包含|V|条链表的数组Adj构成，伪代码中将Adj看作图G的一个属性(G.adj)
    + 鲁棒性很高，可以对其进行简单的修改来支撑许多其他的图变种，例如加权图，只需在节点添加存放权重的w位置即可
- 邻接矩阵
    + 空间需求：Θ(V^2)
    + 无向图的矩阵转置等于自身：G'=G
    + 对于不存在的边记录NIL，使用0或∞更为便捷

### 22.2 广度优先搜索BFS
- 颜色
    + 白色：未知
    + 灰色：已知，已知与位置的边界
    + 黑色：已知，且所有与之相邻的节点均被发现
- 属性
    + π：前驱
    + d：距离
    + color：颜色
- 时间复杂度：O(V+E)
    + 对队列：O(V)
    + 扫描邻接链表：O(E)  --难道不应该是O(V+E)吗？..虽然不影响总结论

#### 1. 最短路径的证明（BFS的正确性证明）
以下引理、推论、定理逻辑结构：
- Lemma1 => Lemma2
- Lemma3 => Corollary4
- Lemma2 + Corollary4 => Theorem5-2
- Theorem5-2 => Theorem5-1 + Theorem5-3

##### <table><tr><td bgcolor=pink>Lemma1</td></tr></table>
- 描述：（“三角形”）对于任意边`(u,v)`，有`δ(s,v) <= δ(s,u) + 1`
- 证明：分类+三角形三边之和

##### <table><tr><td bgcolor=pink>Lemma2</td></tr></table>
- 描述：（上界）`v.d >= δ(s,v)`
- 证明：归纳法（对入队次数归纳）+引理1

##### <table><tr><td bgcolor=pink>Lemma3</td></tr></table>
- 描述：（队列中d的大小）
    + `vr.d <= v1.d + 1`
    + `vi.d <= v(i+1).d`
- 证明：归纳法（对入队次数归纳，考虑出队时情况）

##### <table><tr><td bgcolor=steelblue>Corollary4</td></tr></table>
- 描述：（d值随入队时间单增）若`vi`先于`vj`入队，有`vi.d <= vj.d`
- 证明：由引理3显然可得

##### <table><tr><td bgcolor=yellow>Theorem5 BFS正确性</td></tr></table>
- 描述：
    1. 能够发现从`s`可达的所有结点`v`
    2. 算法终止时，`v.d = δ(s,v)`
    3. `s->v = s->v.π + (v.π,v)`
- 证明
    + 1、3由2易得
    + 证2：反证法+引理2+**设最短路径+设前驱**+(对颜色)分类讨论+推论4

#### 2. 广度优先树
- 前驱子图
- 广度优先树：
    1. 前驱子图`Gπ`包含`s`到`v`的唯一一条简单路径
    2. `s`到`v`路径为源图G中`s`到`v`的一条最短路

##### <table><tr><td bgcolor=pink>Lemma6</td></tr></table>
- 描述：由BFS构造的`π`属性使前驱子图`Gπ`成为一棵广度优先树
- 证明：树的性质+定理5

### 22.3 深度优先搜索DFS
- 基本性质：生成的前驱子图是由多棵树构成的森林
    + `u=v.π`，当且仅当DFS-VISIT(G,v)在是在对u的邻接链表搜索时被调用
    + v是u的后代，当且仅当v在u的灰色时间段内被发现
- 边的分类（对边u,v)
    + 树边：v为白色
    + 前向边：v为黑色
    + 后向边：v为灰色（是灰色一定是自己的祖先）
    + 横向边：v为黑色
- DFS的其他性质
    + 括号化定理
    + 后代区间的嵌套
    + 白色路径定理
    + 无向图搜索中，不会出现前向边和横向边，仅存在树边和后向边

#### 各大定理
以下定理、推论的逻辑结构：
- Theorem1 => Corollary2
- Theorem1 + Corollary2 => Theorem3
- Theorem4

##### <table><tr><td bgcolor=yellow>Theorem1 括号化定理</td></tr></table>
- 描述：对有向或无向图DFS过程中，对于任意2点，仅以下之一成立：
    + [u.d, u.f]和[v.d, v.f]，2个区间完全分离
    + [u.d, u.f]包含[v.d, v.f]
    + [v.d, v.f]包含[u.d, u.f]
- 证明：(对区间)分类讨论+BFS基本性质

##### <table><tr><td bgcolor=steelblue>Corollary2 后代区间嵌套</td></tr></table>
- 描述：有向或无向图的**深度优先森林**中，v是u的后代，当且仅当`u.d<v.d<v.f<u.f`
- 证明：由定理1的分类枚举显然可得

##### <table><tr><td bgcolor=yellow>Theorem3 白色路径定理</td></tr></table>
- 描述：有向或无向图的**深度优先森林**中，v是u的后代，当且仅当存在一条u到v的路径结点全为白色
- 证明：
    1. =>: 推论2
    2. <=: 反证法+定理1+推论2

##### <table><tr><td bgcolor=yellow>Theorem4 无向图的边分类</td></tr></table>
- 描述：在对无向图的DFS中，仅存在树边和后向边，不存在后向边和横向边
- 证明：(对特定时刻v的颜色)分类讨论


### 22.4 DFS应用1 --拓扑排序
- 定义：若图G含边(u,v)，则拓扑排序中，u在v的前面
- 算法描述：dfs遍历DAG，按结束时间的大小逆序(后结束的在前)即为拓扑序

#### <table><tr><td bgcolor=pink>Lemma1 DAG图性质</td></tr></table>
- 描述：一个有向图G无环，当且仅当DFS不产生后向边
- 证明：反证法+白色路径定理

#### <table><tr><td bgcolor=yellow>Theorem2 DFS拓扑排序算法的正确性</td></tr></table>
- 描述：该算法能得到正确的拓扑序列 => 即证恒有 v.f < u.f(根据定义转换问题描述)
- 证明：(对边的类型、或点v的颜色)分类讨论+引理1


### 22.5 DFS应用2 --强连通分量
- 定义：图中任意2点u.v相互可达
- 算法描述：
    1. dfs遍历G，将结束时间u.f按由大到小排序
    2. 求转置图GT
    3. 按1得到的f顺序，使用dfs遍历图GT，得到深度优先森林
    4. 输出森林中每棵树的点集，即为SCC
- 算法时间复杂度
    + 求转置图：O(V+E)
    + dfs：O(V+E)
    + 输出：O(V)










## CH23 最小生成树
树的性质：（当且仅当）
1. 无环且连通
2. 共|V|个点，|V|-1条边
3. 任意2点间有唯一的一条路径
4. 添加一条边产生环
5. 删除一条边增加一个连通分支

> 补充性质：**在树中添加一条边产生环，在这个环上删除一条边能重构成一棵树**

### 23.1 最小生成树的形成
- 基本算法
```python
A = []
while A无法形成一棵生成树：
    找到A的一条安全边(u,v)加入集合A
```
- 边集合A的说明
    + 定义：遵守循环不变式：**每次循环开始前，边集A是某棵最小生成树的子集**
- 名词解释
    + 安全边：加入集合A使得A仍是某棵最小生成树的子集
    + 切割：点集的一个划分(S,V-S)
    + 横跨：边的一个端点属于S，另一个端点属于V-S
    + 切割尊重A：A中不存在横跨切割的边
    + 轻量级边：横跨的边中，权值最小的边

#### <table><tr><td bgcolor=yellow>Theorem1 辨认安全边的规则</td></tr></table>
- 描述：图G(V,E)是连通无向图，A包含于E且包括在G的某棵最小生成树中。满足以下条件的边(u,v)对集合A是安全的：
    + 设切割(S,V-S)尊重集合A
    + 设边(u,v)横跨切割(S,V-S)
    + 且边(u,v)是一条轻量级边
- 证明：树的性质(补充性质)+反证法+(对边的删除/添加)

#### <table><tr><td bgcolor=steelblue>Corollary2</td></tr></table>
- 描述：图G(V,E)是连通无向图，A包含于E且包括在G的某棵最小生成树中。满足以下条件的边(u,v)对集合A是安全的：
    + 设C=(Vc,Ec)是森林Ga(V,A)中的一棵树（一个连通分支）
    + 边(u,v)是连接C和{Ga-C}中任意一个分支的一条轻量级边
- 证明：由定理1显然

### 23.2 Kruskal和Prim算法
- 区别：对维护的集合A的选择不同
    + A in Kruskal：是森林
    + A in Prim：是一棵树


## CH24 单源最短路径
- 环路：从一点出发，最终能回到该点。最短路中一定没有环路，且G的任意无环路径最多|V|个点，|V|-1条边
- 负环：环路，且该路径总权重为负值。负环存在，与其相连的节点，最短路都为-∞
- 证明最短路算法正确：算法终止有`v.d=δ(s,v)`，且Gπ为一棵最短路径树

#### <table><tr><td bgcolor=pink>Lemma1 最短路径的子路径也是最短路径</td></tr></table>
- 描述：`p=<v0,...,vk>`为最短路，则子路径`pij=<vi,...,vj>`也是最短路
- 证明：反证法

### 松弛操作
本章算法、引理、定理均是基于以下两个操作
```python
## 初始化
INITIALIZE-SINGLE-SOURCE(G,s)
    for each vertex v∈G.V:
        v.d = ∞
        v.π = NIL
    s.d = 0

## 松弛
RELAX(u,v,w)
    if v.d > u.d + w:
        v.d = u.d + w
        v.π = u
```

### 最短路径和松弛操作的性质
#### <table><tr><td bgcolor=pink>Lemma1 三角不等式</td></tr></table>
- 描述：任意边(u,v)∈E，都有`δ(s,v) <= δ(s,u) + w(u,v)`

#### <table><tr><td bgcolor=pink>Lemma2 上界性质</td></tr></table>
- 描述：所有点v∈V，都有`v.d >= δ(s,v)`。一旦v.d的取值达到δ(s,v)，则值不再变化

#### <table><tr><td bgcolor=pink>Lemma3 非路径性质</td></tr></table>
- 描述：若结点s与结点v之间不存在路径，则`v.d = δ(s,v) = ∞` （其逆否命题常使用）

#### <table><tr><td bgcolor=pink>Lemma4 收敛性质</td></tr></table>
- 描述：若结点u,v∈V，图G中某条最短路为`s~>u->v`，并且在对边(u,v)松弛前的任意时间，有`u.d = δ(s,u)`，则在边(u,v)松弛后的所有时间，有`v.d = δ(s,v)`

#### <table><tr><td bgcolor=pink>Lemma5 路径松弛性质</td></tr></table>
- 描述：若图中某条最短路`p=<v0,...,vk>`，是从**源节点s=v0**到vk的最短路。若松弛顺序有`(v0,v1),(v1,v2)...(vk-1,vk)`，则一定有`v.d = δ(s,v)`。且，该性质的成立与其他松弛操作无关，即使其他松弛操作是对以上边所进行的松弛穿插执行的。

#### <table><tr><td bgcolor=pink>Lemma6 前驱子图性质</td></tr></table>
- 描述：对于所有点v∈V，有`v.d = δ(s,v)`，则前驱子图是一棵以s为根节点的最短路径树


### 24.1 Bellman-Ford算法
```python
BELLMAN-FORD(G,s)
    # 对每条边松弛|V|-1次
    for i=1 to |G.V|-1:
        for each edge (u,v)∈E:
            RELAX(u,v,w(u,v))
    for each edge (u,v)∈E:
        if v.d > u.d + w(u,v)
            return False  # 有负环
    return True
```

#### <table><tr><td bgcolor=pink>Lemma1 没有负环存在算法正确</td></tr></table>
- 描述：若图G中不含从源节点s可达的负环，则在|V|-1次循环之后，对所有s可达的v，都有`v.d = δ(s,v)`
- 证明：最短路无环最多|V|-1条边 + Lemma5(路径松弛性质)

#### <table><tr><td bgcolor=steelblue>Corollary2</td></tr></table>
- 描述：若图G中不含从源节点s可达的负环，则对于所有的v∈V，存在一条s到v路径**当且仅当**Bellman-Ford算法结束时有`v.d < ∞`
- 证明：Lemma1(无负环的算法正确性) + lemma3(非路径性质的逆否命题)

#### <table><tr><td bgcolor=yellow>Theorem3 Bellman-Ford正确性</td></tr></table>
- 描述：算法结束，若不包含负环，返回True且Gπ为一棵最短路径树；若包含负环，返回False
- 证明：
    1. 假设不含负环。证明在|V|-1次循环后得到`v.d = δ(s,v)`，再证明能返回True（Lemma1 + Lemma1(三角不等式)）
    2. 假设含负环。证明会返回False（负环定义 + 反证法(假设会返回True)）


### 24.2 DAG的单源最短路问题
```python
DAG-SHORTEST-PATH(G,s)
    拓扑排序G中的节点
    INITIALIZE-SINGLE-SOURCE(G,s)
    for each vertex u(按拓扑序取):
        for each vertex v∈G.Adj[u]:
            RELAX(u,v,w(u,v))
```
#### <table><tr><td bgcolor=yellow>Theorem1 DAG单源最短路算法正确性</td></tr></table>
- 描述：DAG，在算法终止时，对于所有v∈V，有`v.d = δ(s,v)`，且Gπ是一棵最短路径树
- 证明：
    1. `v.d=δ(s,v)`：拓扑序定义 + 无环 + Lemma5(路径松弛性质)
    2. Gπ是最短路径树：有Lemma6(前驱子图性质)+证明1 显然可得

### 24.3 Dijkstra算法
主要思想：
- S集合中的节点的最短路均已找到
- 有循环不变式：队列Q = V - S
```python
DIJKATRA(G,s)
    INITIALIZE-SINGLE-SOURCE(G,s)
    S = ∅
    Q = G.V
    while Q != ∅:
        u = EXTRACT-MIN(Q)  # 取出d值的最小值
        S = S ∪ {u}  # u加入集合S
        for each vertex v∈G.Adj[u]:
            RELAX(u,v,w(u,v))
```
#### <table><tr><td bgcolor=yellow>Theorem1 Dijkstra算法正确性</td></tr></table>
- 描述：在算法终止时，对所有节点v∈V，有`v.d = δ(s,v)`
- 证明：
    + 循环不变式：对于每个结点u∈V，在u被加入集合S时，有`u.d = δ(s,u)`，根据Lemma2(上界性质)能得到后续时间内不变
    + 在**保持**步的证明：(基本证明思想似BFS+Prim安全边定理证明)
        * 反证法 + Lemma3(非路径性质的逆否命题) + 假设某条最短路和某前驱 + Lemma4(收敛性质) + Lemma2(上界性质)
    
> 证明某循环不变式，需要证明的几个阶段：1.初始化 2.保持 3.终止

### 24.4 差分约束(difference constraint)
将某种特定类型的线性规划转变为求最短路问题：
- 特定的线性规划：每个约束条件都是 `xj-xi <= b`
- 转换方法：将m×n的线性规划矩阵A，视为n个点、m条边构成的图的**关联矩阵**的转置
    + 关联矩阵：`bij= -1(边j从i发出);  1(边j进入i);  0(其他)`
- 使用最短路的算法
    + 添加一额外节点v0，到n个结点的边权重为0,
    + 若图G不含负环，则`x = (δ(v0,v1), δ(v0,v2),..., δ(v0,vn))`是一个可行解
    + 若图G包含负环，则没有可行解
    + 使用Bellman-Ford算法即可求解



## CH25 所有节点对的最短路径问题
### 25.1 最短路径和矩阵乘法
- 动态规划：定义k为最短路i->j上，j的直接前驱k
- 复杂度
    + 未优化: O(n^4)
    + 快速幂优化: O(n^3lgn)
- 算法
    + 未优化
    ```python
    for m in range(1,n):  # 最多不超过n-1条边
        # 以下为 EXTEND-SHORTEST-PATH(L,W)的内容
        for i in range(1,V):
            for j in range(1,V):
                for k in range(1,V):
                    lij = min(lij, lik+wkj)  # 松弛
    ```
    + 快速幂优化
    ```python
    while m < n-1:
        let L(2m)
        L(2m) = EXTEND-SHORTEST-PATH(L(m),L(m))  # 函数见上
        m = 2m
    ```

#### 关于快速幂优化的原理
- 运算替换
    + `min -> +`
    + `+ -> ·`
    + **重点**：`L' = L * W`, 推导出如下规律：
        * L1 = W
        * L2 = W^2
        * ...


> 证明了原算法的结果L'=L*W的实质，再使用矩阵快速幂算法优化即可 

### 25.2 Floyd-Warshall算法
- 动态规划：定义k为 p从i到j之间的中间结点均取自集合{1,2,...,k}(k < n)
- 复杂度：O(V^3)
- 算法
```python
for k in range(1,V):
    for i in range(1,V):
        for j in range(1,V):
            dij = min(dij, dik+dkj)
```

#### 构建一条最短路径
- 方法1：利用Floyd的结果在进行一次Floyd构造
- 方法2：Floyd运算的同时构造
    
    <img src="fig2.png" width="500" height="100">

#### 有向图的传递闭包
目的：判断图G中，对于所有结点对是否包含一条从i到j的路径
- 方法1：直接Floyd算最短路，`dij = ∞`即不包含
- 方法2：使用逻辑运算替换操作
    + 替换：
        * 或(∨): min
        * 与(∧): +
        * 松弛操作: `tij = tij ∨ (tik ∧ tkj)`
    + 定义tij
        * = 0, i不等于j且ij∉E
        * = 1, i等于j或ij∈E
    + 复杂度分析
        * 也是O(n^3)，但是改为逻辑运算，空间、时间均减少

### 25.3 用于稀疏图的Johnson算法
- 适用场景：稀疏图
    + 稀疏图定义：|E| << |V|^2
- 算法步骤
    1. 添加源节点s与每个点相连，生成新图G'，赋新权值均为0
    2. 在G'上以s为源节点运行Bellman-Ford：计算最短路δ(s,v) = h(v)，并判断负环
    3. 若负环不存在，公式`w'(u,v) = w(u,v) + h(u) - h(v)` 计算新权重
    4. 分别以G中每个结点为源结点，在G上运行dijkstra
    5. 最终，公式`d(u,v) = δ(u,v) - h(u) + h(v)` 计算真实的最短路（恢复减去的权重）
- 复杂度：
    + 斐波那契堆：O(V^2lgV+VE)
    + 二叉最小堆：O(VElgV)

 
## CH26 最大流
物料从生产的源节点经过一个系统，流向消耗它的汇点t。每条通道有限定容量，除源节点、汇点，物料仅流过，不积累或聚集

### 26.1 流网络
- 流网络定义
    + 每条边有一个非负容量值c(u,v)
    + 若(u,v)∈E，则(v,u)∉E

> 1. 若(u,v)∉E，则定义c(u,v)=0，则相应的f(u,v)=0
> 2. 图中不允许自循环
> 3. 源结点s，汇点t
- 流定义：f(u,v)
    + 容量限制： `0 <= f(u,v) <= c(u,v)`
    + 流量守恒： 除源结点s和汇点t之外，每个结点流量**总流入 = 总流出** 
        * 对于每个非s/t的节点u，流入=流出：`sum(f(u,v))=sum(f(v,u)), 其中u∈V-{s,t},v∈V`
        * 对于网络中所有节点，流入流出相等，根据**反对称性质**和为0：`sum(f(u,v))=0, 其中u∈V-{s,t},v∈V`
    + 流的值 `|f| = sum(f(s,v)) = f(s,V)`

> 与课本定义不同，但本质相同，因为在流网络中不存在反平行边，因此省略后半部分
- 部分性质
    + `f(X,X) = 0`
    + 反对称：`f(X,Y) = -f(Y,X)`
    + `f(X∪Y,Z) = f(X,Z)+f(Y,Z)  (X∩Y=∅)`

#### 存在反平行边
解决方案：在反平行边中间添加另一结点作中间点，使其“删除”，以满足流网络定义

#### 具有多个源节点和多个汇点的网络
解决方案：
- 添加一个超级源节点s与多个源节点连接
- 并添加一个超级汇点t与多个汇点连接
- 并设置这些添加的新边的容量：`c(s,si) = c(ti,t) = ∞`

### 26.2 Ford-Fulkerson方法
基本算法：
```python
FORD-FULKERSON(G,s,t): 
    for each edge (u,v)∈G.E:
        f(u,v) = 0  # 1. 初始化所有边流量为0
    while 残存网络Gf中有增广路p: # 2. 算Gf，并在找一条增广路p
        cf = min{cf(u,v): (u,v) in p}  # 3. 算增广路的残存容量
        for each (u,v)∈p:  # 4. 更新增广路p中每条边的流量f
            if (u,v)∈E:
                f(u,v) = f(u,v) + cf  # 原边存在，流量增加
            else:
                f(v,u) = f(v,u) - cf  # 原边不存在，其对应的反向边(原边)流量减少 -> 抵消操作
# 最终的最大流f，利用|f| = f(s,V)计算，即源节点s发出-流入(G仅含发出)
```

#### 残存网络Gf
- 定义:当残存容量`cf(u,v)>0`时，边`(u,v)∈Gf`
    + `= c(u,v) - f(u,v),   (u,v)∈G.E`
    + `= f(v,u),    (v,u)∈G.E`  //增加反向边
    + `0,    otherwise`
- 性质：
    + 除包含反平行边，其余性质与流网络相同
    + `|Ef| <= 2|E|`
- 残存网络中的流
    + 指出一条路线图：如何在原来的流网络中增加流
    + 假设f是G中的流，f'是Gf中的流，定义`f↑f'`为流f'对f的递增 `(f↑f')(u,v)`:
        * `= f(u,v) + f'(u,v) - f'(v,u),  if(u,v)∈G.E`
        * ` = 0,  otherwise`

##### <table><tr><td bgcolor=pink>Lemma1 残存网络 流的递增</td></tr></table>
- 描述：`f↑f'`是G(原图)的一个流，其值`|f↑f'| = |f| + |f'|`
- 证明：定义 + 流网络部分性质 + (缩点/扩点)
    + 证明流：容量限制 + 流量守恒

#### 增广路径p
- 定义：在图Gf中，一条从s到t的**简单**路径
- 残存容量：在增广路径p上能为每条边增加的流量最大值（即最小边容量值）
    + `cf(p) = min{ cf(u,v): (u,v)∈p }`

##### <table><tr><td bgcolor=pink>Lemma2 残存网络 中的流fp</td></tr></table>
- 描述
    + 残存网络中的流fp，其值`|fp|=cf(p)>0`
        * `fp(u,v) = cf(p),  (u,v)∈p`
        * `= 0, otherwise`
- 证明略

##### <table><tr><td bgcolor=steelblue>Corollary3</td></tr></table>
- 描述：将f增加fp的量，则`f↑fp`是G(原图)的一个流，其值`|f↑fp| = |f| + |fp| > |f|`
- 证明：Lemma1 + Lemma2

#### 流网络的切割cut
- 流网络切割定义
    + G(V,E)的一个切割(S,T)，将顶点集V划分为S/T(S∪T=V, S∩T=∅)
    + 与最小生成树时切割类似，不同点：
        * 切割的是有向图，而不是无向图
        * 要求有 `s∈S, t∈T`
- 横跨切割(S,T)的净流量和容量
    + 净流量f(S,T)定义：
        * `f(S,T) = sum( sum( f(u,v) )) - sum( sum( f(v,u) )),  u∈S,v∈T`
    + 切割的容量c(S,T)定义：
        * `c(S,T) = sum( sum( c(u,v) )),  u∈S,v∈T`

> 注意：
> - 切割中净流量，考虑的是 S到T的流量 减去 T到S的流量
> - 切割中的容量，仅考虑 S到T的边的容量，不考虑反方向

##### <table><tr><td bgcolor=pink>Lemma4 切割 中的流净流量</td></tr></table>
- 描述：f是网络G的一个流，(S,T)该网络的任意切割。横跨切割(S,T)的净流量f(S,T) = |f|
- 证明：使用流网络部分性质 + 流值的定义(f(s,V))

##### <table><tr><td bgcolor=steelblue>Corollary5</td></tr></table>
- 描述：任意切割(S,T)，穿过的流<=切割的容量，即`|f| <= c(S,T)`
    + PPT: <img src="fig1.png" width="400" height="40">
- 证明：Lemma4 + 容量限制

##### <table><tr><td bgcolor=yellow>Theorem5 最大流最小割</td></tr></table>
- 描述：以下描述等价（**最大流=某个最小割容量**）
    + |f|是G的一个最大流
    + 残存网络Gf中不包括任何增广路径
    + |f| = c(S,T)，其中(S,T)是网络G中的某个切割


### 26.3 最大二分匹配
二分图G(V,E)：最大匹配是最大基数的匹配

**在构建的网络G'中求得的最大流|f| = 最大匹配基数**

#### 网络图G'的构建
- 结点集V'：`V' = V ∪ {s, t}`. 即添加原图外的2个新节点s和t
- 边集E': `E' = {(u,v): u∈L,v∈R} ∪ {(s,u): u∈L} ∪ {(v,t): v∈R}`，且所有边`c(u,v) = 1`. 
    + 点集划分为L和R，E中从L流向R都是G'的边









