# 第9章-图论算法

TODO：

无边，权值置为0还是Infinity

本章程序数组化

## 思维导图



## 基本概念

| 概念                                     | 描述                                                         |
| ---------------------------------------- | ------------------------------------------------------------ |
| 图 (graph)                               | 由顶点集合V和边集合E组成                                     |
| 边 (edge)                                | 一对点 (v, w), v, w ∈ V                                      |
| 有向图 (directed graph / digraph)        | 点对有序的图，(v, w)与(w, v)为两条不同的边                   |
| 无向图 (undirected graph)                | 点无序的图，(v, w)与(w, v)为同一条边                         |
| 邻接 (adjacent)                          | 顶点w与v邻接当且仅当 (v, w) ∈ E                              |
| 权 (weight) / 值 (cost)                  | 边的权重                                                     |
| 路径 (path)                              | 为一顶点序列 w1, w2, w3,...,wN使得(wi, w(i+1)) ∈ E, 1<=i<=N<br />一个顶点到他自身也可以看成是一条路径，如果路径不包含边，则路径长为0 |
| 路径的长 (path length)                   | 路径上边的数量                                               |
| 简单路径                                 | 路径上的所有顶点都是不同的，但第一个和最后一个可能可以相同   |
| 环 (loop)                                | 一个顶点到它自身的边 (v, v)，本章讨论的图一般都是无环的      |
| 圈 (cycle)                               | 满足w1 = wN的长至少为1的路径                                 |
| 有向无圈图 (directed acyclic graph, DAG) | 无圈的有向图                                                 |
| 连通图 (connected graph)                 | 从一顶点w到另一顶点v有路径相连称w与v连通，任意两顶点之间连通的图称为连通图。对于有向连通图，两点之间的路径上的边必须同向 |
| 强连通 (strongly connected)              | 称有向连通图是强连通的                                       |
| 基础图 (underlying graph)                | 有向图去掉边的方向后的图称为该有向图的基础图                 |
| 弱连通 (weakly connected)                | 有向图不是强连通的，但其基础图是连通的，则称该有向图是弱连通的 |
| 完全图 (complete graph)                  | 每一对顶点间都有边相连的图                                   |
| 邻接矩阵 (adjacent matrix)               | 对每条边(u, v)置矩阵的A[u, v]为true，无(u, v)边则为false，若边有权，则A[u, v]等于该值，以一个很大或很小的数表示该边不存在 |
| 邻接表 (adjacency list)                  | 以邻接矩阵表示图，当图较稀疏时空间代价为\|V\|^2，针对这种情况，对稀疏图可以采用邻接表来表示，对每一个顶点，以一个表存放其邻接顶点，则空间降为\|E\|+\|V\|，相对图的大小而言是线性的 |
| 拓扑排序 (topological sorting)           | 对有向无圈图的顶点的排序，使得若存在从w到v的路径，则v在排序中出现在w之后<br />显然当图含有圈时无法做到拓扑排序 |
| 入度 (indegree)                          | 对有向图顶点v而言，(u, v)边的数量                            |
| 出度 (outdegree)                         | 对有向图顶点v而言，(v, u)边的数量                            |
| 赋权路径长(weighted path length)         |                                                              |
| 无权路径长(unweighted path length)       |                                                              |
| 单源最短路径问题                         |                                                              |
| 无权最短路径                             |                                                              |
| 广度优先搜索                             |                                                              |
|                                          |                                                              |
|                                          |                                                              |



## 总结



## 图的数据结构



```java
```



## 拓扑排序算法

### Kahn算法

拓扑排序是对有向无圈图的一种排序，这种排序如果存在从顶点u到顶点v的路径，那v一定在u之后而不能从v走到u。所以拓扑排序存在的前提是有向图无圈。



#### 算法描述

[Kahn算法](https://en.wikipedia.org/wiki/Topological_sorting)：找出任意一个入度为0的顶点，放入队列中，从队列输出该顶点并将其邻边入度减1，同时检查这些邻边的入度，若算法未结束，则会出现新的入度为0的顶点，将其入队。于是不断有新的入度为0的顶点入队出队，直到队列为空(所有顶点均已入队又出队)，算法结束。若在某个节点找不到入度为0的顶点，则说明该图有圈。



#### 算法过程

1. 设置一个队列q和一个用于保存拓扑排序的列表res。
2. 遍历所有顶点，将入度为0的顶点入队后跳出遍历(有向无圈连通图有且只有一个顶点入度为0)。
3. 用一个while检查当前队列是否为空，不空则队首顶点v出队放入输出列表res中。
4. 将v的邻接顶点的入度减1，并检查减1后是否为0，为0则入队。
5. 当退出while时，输出列表中res出队的顶点顺序即为拓扑排序后的顺序。



#### 时空复杂度

时间复杂度：队列中每个顶点均入队一次，出队一次，O(|V|)。更新并检查邻接顶点的for中，更新和检查的总次数等于边数O(|E|)，故总的时间复杂度为O(|E|+|V|)。

空间复杂度：取决于队列长度，不大于|V|，O(|V|)。



#### 代码

```java
public List<E> topoSort() {
    List<E> res = new ArrayList<>();
    Queue<Vertex<E>> q = new LinkedList<>();
    int counter = 0;

    // 遍历顶点，将入度为0的顶点入队
    for (Vertex<E> v : vertices) {
        if(v.getIndegree() == 0) {
            q.add(v);
            // 如果图是连通的，初始时有且只有一个入度为0的顶点
            break;
        }
    }
    // 若队列不空，队首顶点v出队，输出到res，counter加1(用于设置拓扑排序序号和后续判断是否有圈)，
    // 设置v的拓扑排序序号，接着将v的邻接顶点w的入度减1，同时检w的入度是否变为0，是则入队
    while(!q.isEmpty()) {
        Vertex<E> v = q.remove();
        res.add(v.getName());
        counter++;
        v.setTopoNum(counter);
        for (Vertex<E> w : getAdjList(v)) {
            w.setIndegree(w.getIndegree() - 1);
            if(w.getIndegree() == 0) {
                q.add(w);
            }
        }
    }
    // 若while后counter不等于顶点数，则说明该有向图有圈
    if(counter != vertices.size()) {
        System.err.println("Cycle found!");
    }

    return res;
}
```



## 最短路径算法

**单源最短路径问题：**

给定一个赋权图G和一个顶点s，找出从s到G中每一个其他顶点的最短赋权路径。当图中有负值圈时，该图无最短赋权路径。若G为无权图，可看作边的权均为1的赋权图。

本节讨论并实现如下情形：

单源：

1. 无权图最短路径
2. 无负边赋权图最短路径 (Dijkstra)
3. 无圈赋权图最短路径 (Kahn + Dijkstra)
4. 有负边赋权图最短路径 (Bellman-Ford, SPFA)

所有顶点间：

5. 所有点对最短路径 (Flyod-Warshall)



### 无权最短路径

#### 不使用队列的朴素算法

##### 算法描述

从源点s开始，以广度优先搜索(或者说层序遍历)的方式确定s到其他所有顶点的最短路径。对于s，距离(顶点到源点的距离，下同)为0，其邻接顶点的距离+1。置这些邻接顶点的距离为已确定，并将其前驱顶点置为s。接着对距离为1的顶点v考察它们的距离未确定的邻接顶点w，使w的距离+1，并将w的前驱顶点置为v。以for循环重复上述过程，循环开始时 currentDist = 0，循环次数为顶点个数减1，每次执行后 currentDist+1，表示距离不断递增，且距离最大不超过|V|-1 (当图为链状取到最大，为|V|-1)。



##### 算法过程

1. 设置一个用于输出出队顶点顺序的结果列表res(不是必要的)，给定一个源点s，置其距离(到源点s的距离)为0。
2. for循环按距离递增(广度优先搜索或者说层序遍历的方式)。令currentDist=0，以顶点个数减1为遍历次数，每次currentDist加1，表示距离以1为单位不断递增，最大不超过|V|-1。
3. for循环遍历所有顶点，找到距离未确定且为currentDist的顶点v(下一层顶点)，然后置v的距离为已确定，将v输出到结果列表中。
4. for循环遍历v的未确定距离的邻接顶点w，置w的距离为currentDist+1，置w的前驱为v。
5. 最外层for循环结束后所有顶点距离都会确定，算法结束。



##### 时空复杂度

时间复杂度：两层for均为O(|V|)，故为O(|V|^2)。

空间复杂度：不考虑用于返回的顶点列表时(对于寻找最短路径并不是必要的)时为O(1)。



##### 代码

```java
public List<Vertex<E>> shortestPathUnweightedBasic(Vertex<E> s){
    List<Vertex<E>> res = new ArrayList<>();
    s.setDistance(0);

    for (int currentDist = 0; currentDist < vertices.size(); currentDist++) {
        for (Vertex<E> v : vertices) {
            if(!v.isKnown() && v.getDistance() == currentDist) {
                v.setKnown(true);
                res.add(v);
                for (Vertex<E> w : getAdjList(v)) {
                    // 已经取得距离的w一定是之前取得的，其距离已确定，所以要判断w是否为距离未更新的顶点。
                    if(w.getDistance() == Integer.MAX_VALUE) {
                        w.setDistance(currentDist + 1);
                        w.setPre(v);
                    }
                }
            }
        }

    }
    return res;
}
```



#### 使用队列的改进算法

##### 算法描述

上述算法的一个明显的缺点是，尽管程序运行到后期时大部分顶点的距离均已确定，但距离每+1后仍要遍历所有顶点来寻找距离未确定的下一层顶点。可以使用队列改进这一算法。开始时令源点s距离为0，置其距离为已知并入队，然后以一个while循环使顶点出队，对于每次出队的顶点v，考察其邻接顶点w，若w距离未知，使其距离为v的距离+1，然后置w的距离为已知，置w前驱为v，w入队。while循环结束时算法完成，通过顶点前驱信息可以得到该顶点到源点间的最短路径。



##### 算法过程

1. 设置一个用于输出出队顶点顺序的结果列表res(不是必要的)，设置一个队列q，给定一个起始顶点(源点)s，置s的距离(到源点s的距离)为0，置s的距离为已知，将s加入到队列中。
2. while循环判断q是否为空，不空则队首顶点v出队，输出到结果列表res中。
3. for循环遍历v的邻接顶点w，判断w距离是否已知，若未知则w的距离等于v的距离+1，置w的距离为已知，置w的前驱为v，w入队。
4. 最外层while循环结束后所有顶点距离都会确定，算法结束。



##### 时空复杂度

时间复杂度：与拓扑排序时间复杂度分析类似，每个顶点均入队一次出队一次，O(|V|)。遍历邻接顶点的for，在程序整体运行过程中每个顶点都会遍历其邻接顶点，总遍历的次数为边的总数，O(|E|)，总的时间复杂度为O(|E|+|V|)。

空间复杂度：不考虑用于返回的列表。取决于队列长度，O(|V|)。



##### 代码

```java
public List<Vertex<E>> shortestPathUnweightedQueue(Vertex<E> s){
    List<Vertex<E>> res = new ArrayList<>();
    Queue<Vertex<E>> q = new LinkedList<>();
    s.setDistance(0); // 将起始顶点的距离设为0
    s.setKnown(true);
    q.add(s); // 起始顶点入队

    while(!q.isEmpty()) {
        Vertex<E> v = q.remove();
        res.add(v);
        // 每次顶点v出队，考察其邻接顶点的距离
        for (Vertex<E> w : getAdjList(v)) {
            // 用known字段来表示w的距离是否已更新，此处也可以用w.getDistance() == Integer.MAX_VALUE，
            // 但要限制最大路径不能超过Integer.MAXVALUE，若使用该判断条件，
            // 则该方法内的s.setKnown(true);w.setKnown(true);语句可以去掉
            if(!w.isKnown()) { 
                w.setDistance(v.getDistance() + 1); // 将其距离更新为v.getDistance() + 1
                w.setKnown(true);
                w.setPre(v); // 将v设置为w的其前驱顶点
                q.add(w); // w入队
            }
        }
    }

    return res;
}
```



### 单源赋权最短路径

#### Dijkstra算法

##### 朴素版

###### 算法描述

[Dijkstra算法(狄杰斯特拉算法)](https://en.wikipedia.org/wiki/Dijkstra%27s_algorithm)：一种基于广度优先搜索和贪婪思想(或者说层序遍历思想)的求解无负边图单源最短路径的算法。算法将所有顶点区分为（到源点s的最短）距离未确定的顶点和距离已确定的顶点。初始时所有顶点到源点s的距离为Infinity，然后置s的距离未0。以一个循环查询当前是否有距离未确定的顶点，若有则将其中距离最小者v选为当前顶点，使其距离已知。然后以BFS的方式松弛v的邻接顶点w并更新w的前驱。当不再有未确定距离的顶点时算法结束，此时每一个顶点的距离均为到s的最短距离，通过递归寻找节点的前驱可以得到s到该顶点的最短路径。

松弛操作是最短路径算法的关键，在确定当前顶点v(最新成为已确定距离的顶点)后立即操作，目的是更新v的邻接顶点w的距离和其前驱。如下图，s经过若干个顶点到a和b，a和b邻接c。假设此时dw = Inifinity，da = 5，db = 10。

a先于b成为当前顶点，由于da + d(a, w) = 15 < Infinity，故a松弛dw，并将w的前驱置为a，w.pre = a。

b成为当前顶点时，由于db + d(b, w) = 11 < 15，故b松弛dw，并将w的前驱置为b，w.pre = b。

<div align=center>
  <img src="https://tva1.sinaimg.cn/large/008i3skNgy1gyd7dgs29zj307m0di0sw.jpg" alt="image" width="100"/>
</div>

一个直观的观察是，来自w入边的松弛，使得dw不断更新至所有可能的路径距离的最小值。本算法后续讲解以及同样用到松弛操作的Bellman-Ford和Floyd算法中，我们还会看到对松弛操作的说明。

后续小节将给出Dijkstra算法的正确性证明。



###### 算法过程

朴素版本过程如下：

1. 初始化。选定一个源点s，初始化所有顶点到源点s的距离为Infinity，表示该点到s的距离尚未确定，置s到其自身距离为0。

2. 以while循环寻找当前距离未确定顶点中距离最小者。在当前具有有效距离(即不为无穷大)的距离未确定的顶点中找到距离最小者v，置v的距离为已确定。

3. 松弛操作。更新v的所有邻接顶点w的距离dw。有如下两种情况，因为在松弛前所有顶点的距离默认为Infinity，所以两种情形的形式一样，都是判断dv + d(v, w) < dw。（若初始将顶点距离设置为负数则需要分开处理）

    3.1  如果是初次访问，w到s的距离dw有效化为dw = dv + d(v, w)。更新w的前驱，w.pre = v。

    3.2. 当w不是初次访问，当满足dv + d(v, w) < dw时，更新dw为dv + d(v, w)。更新w的前驱，w.pre = v。

4. 当所有顶点距离都确定时while循环结束，每个顶点到源点的最短距离被求出。

补充说明：

1) 第2步是该算法贪婪的体现，即每次while都确定一个顶点的最短路径，while结束时求得所有顶点的最短路径。
2) 第3步是BFS思想的体现。每次while确定一个顶点后，调整其邻接顶点的距离，即一次处理一层。
3) 针对第2步在当前距离未确定顶点中寻找距离最小值，可以采用优先队列(小顶堆)进行改进，具体实现见后续。
4) Dijkstra算法对有负值边的图无效，原因见后续。

<div align=center> 
  <img src="https://tva1.sinaimg.cn/large/008i3skNgy1gvj8uhzs9nj60es08st9102.jpg" alt="image-20211018111446692" width="300" />
</div>

###### 正确性证明

利用数学归纳法(结合反证法)证明，以下是证明全过程[^1]。

a) 首先回顾数学归纳法的证明过程。

1. 起始验证。对于命题P(n)，当n = 1时命题P成立。
2. 假设命题成立。假设命题P(n)在n = m (m > 1, m ∈ N)时成立。
3. 递推证明。根据2的假设，若能证明n = m + 1时命题P成立，则命题得证。

例如，有命题P：1+2+3...+n = n*(n+1)/2，按照数学归纳法证明如下：

1. 起始验证。当n等于1时，1 = 1*(1+1)/2，命题成立。

2. 假设命题成立。假设命题等于m时成立，1+2+3+...+m = m*(m+1)/2。

3. 递推证明。根据2的假设，如果能证明n = m+1时命题正确，则命题P正确。

   证明：在2所示式子左右两边加上m+1，得到 1+2+3+...+m+(m+1) = m*(m+1)/2 + (m+1)

   等号右边可以写成(m+1)*(m+2)/2，显然该形式就是将n = m+1代入原命题P的形式，证毕。

b) 利用数学归纳法证明如下命题。

命题P：Dijkstra算法第n次进入while时，会将第n个顶点加入距离已确定顶点集合A中，此时对于顶点∀v ∈ A(共n个)，总有d(v) = δ(v)。

※ d(v)表示由Dijkstra算法得到的最短距离估计，对于源点s，在程序开始时赋予d(s) = 0，对于其他顶点，由松弛操作得到。δ(v)表示实际的顶点v到源点的最短距离。

1. 起始验证。当n等于1时，A集合中只有源点s自身，d(s) = 0 (程序开始时赋值得到)，且知道δ(v) = 0，故n=1时命题正确。

2. 假设命题成立。假设命题P在n等于m时，P(m)成立，即算法经过m次while，得到具有m个顶点的集合A，对于顶点∀v ∈ A(共m个)，总有d(v) = δ(v)。

3. P(m+1)递推证明。根据2的假设，如果能证明第m+1个顶点u被放入集合A时有 d(u) = δ(u)，则命题P正确。

   证明：|A| = m时，在集合B (B = S - A) 中根据算法规则找到距离最短的顶点u，将该顶点将作为第m+1个顶点放入A中，放入后|A| = m + 1，如果能证明 d(u) = δ(u)，使得P(m+1)成立，则对于顶点∀v ∈ A(共m+1个)，有d(v) = δ(v)。

   以反证法证明之。

   3.1 假设m+1时d(u) = δ(u)不成立，即有如下式(1)， 之后的目标是根据已知条件导出某种矛盾情形，推翻该假设。

   δ(u) < d(u)       式 (1) 

   ※ δ(u)是实际的u到源点的最短距离，d(u) = δ(u)不成立时只能是δ(u) < d(u)。

   3.2 根据3.1的假设，存在一条从源点s到u的路径Pu，该路径是s到u的最短路径，即 len(Pu) = δ(u)  < d(u)。路径Pu一定有不在A集内的顶点(至少有u不在A集中)，同时也有在A集中的点(至少有s点在A集中)，可以假设Pu经过x和y，其中x在A中(可以是s)，y在B中(可以是u本身)，y到u的过程中也可以再进入A，如下图。Px为Pu在顶点x结束的子路径，因为路径Px加边(x, y)为路径Pu的一部分，所以有：

   len(Px) + len(x, y) ≤ len(Pu) = δ(u)       式(2)

   这是显然的，因为len(Px) + len(x, y)是len(Pu)的一部分，当y=u时取到等号。

   <div align=center> 
     <img src="https://tva1.sinaimg.cn/large/008i3skNgy1gvlx6lkq0wj60nw0f6mxj02.jpg" width="250" />
   </div>

   3.3 在x被选中进入A集内时，对其邻接顶点y执行过松弛操作，该操作比较d(x) + len(x, y)是否小于d(y)，若小于则以d(x) + len(x, y)更新d(y)的值，所以如果更新了，更新之后有d(y) = d(x) + len(x, y) ，如果没更新，说明d(y) < d(x) + len(x, y)。假设之后y还会被y的其他前驱顶点更新d(y)值(当该前驱顶点进入A集时)，那d(y)只会变得更小，所以一定有：

   d(y) ≤ d(x) + len(x, y)        式(3)

   比较式(2)和式(3)中的len(Px)和d(x)，因为d(x) = δ(x) (由步骤2的P(m)假设给出，顶点x是P(m)假设的m个顶点之一)，而Px只是若干从s到x的路径之一，必有d(x) ≤ len(Px)，当Px就是s到x的最短路径时取到等号。所以根据式(2)和式(3)有：

   d(y) ≤ d(x) + len(x, y) ≤ len(Px) + len(x, y) ≤ len(Pu)，即 

   d(y) ≤ len(Pu) = δ(u)       式(4)
   
   3.4 顶点y与u均在B集中，根据算法规则，u之所以是第m+1个被放入A集中的顶点，是因为第m+1次进入while时，u在B集中相比于B集中的其他顶点(自然也包括y)，到源点s的距离最小，显然有：
   
   d(u) ≤ d(y)        式(5)
   
   结合式(1)，式(4)，式(5)得到：
   
   δ(u) < d(u) ≤ d(y) ≤ len(Pu) = δ(u) ，即 δ(u) < δ(u)，至此，由3.1的假设“d(u) = δ(u)不成立”导出了矛盾，所以d(u) = δ(u)是成立的，反证结束。

至此，对Dijkstra算法正确性得证。



###### 时空复杂度

时间复杂度：O(|E|+|V|^2)，由于|E| < |V|^2，所以也可以写为 O(|V|^2)。

1. 寻找拥有最小距离的顶点的时间为O(|V|^2)。 先遍历一遍顶点确认存在距离未确定的顶点O(|V|)，再遍历一遍顶点寻找未确定距离的顶点中距离最小者O(|V|)。

2. 所有顶点的距离被松弛的次数上限为O(|E|)。由算法可知顶点距离松弛只发生在找到当前距离未确定且距离最小的顶点之后，一次更新的次数是该顶点的邻接顶点数(即出边数，出度)。每一次循环松弛某一个顶点的所有出边。所有顶点的出边数即总边数|E|。故总松弛次数，也即所有顶点的距离被更新的次数上限为O(|E|)。

空间复杂度：算法过程中只用到常数个变量，不考虑顶点/边/图的数据结构本身消耗的空间时为O(1)。



###### 代码

```java
// 为了更清楚地分析时间复杂度，将检查未确定距离顶点的方法checkUnknown和
// 获取未确定距离顶点中最小者的方法getUnknownMin写在外部。

public List<Vertex<E>> shortestPathDijkstraBasic(Vertex<E> s){
    List<Vertex<E>> res = new ArrayList<>(); 
    s.setDistance(0);

    // 遍历所有顶点确认是否有距离未确定的顶点，O(|V|)
    while(checkUnknown(vertices)) { 
        // 从所有顶点中取出距离unknown的顶点中的距离最小者v，O(|V|)
        Vertex<E> v = getUnknownMin(vertices);
        v.setKnown(true); // 置v的距离已确定
        res.add(v); // 按顺序输出已确定距离的顶点
        // 松弛v的邻接顶点w，本方法总共O(|E|)
        for (Vertex<E> w : getAdjList(v)) {
            // 松弛对象是未确定距离的邻接顶点w，本程序赋权图以邻接矩阵方式存储顶点的邻接顶点信息，
            // 故以边权是否为0判断是否为邻边
            if(!w.isKnown() && getEdgeValue(adjListsWeighted, v, w) != 0) { 
                int dw = v.getDistance() + getEdgeValue(adjListsWeighted, v, w);
                if(dw < w.getDistance()) { // 松弛条件
                    w.setDistance(dw); // 更新dw
                    w.setPre(v); // 置w前驱为v
                }
            }
        }
    }

    return res;
}

/**
 * 检查顶点列表中是否存在unknown顶点，O(|V|)
 */
private boolean checkUnknown(List<Vertex<E>> vertices) {
    for (Vertex<E> v : vertices) {
        if(!v.isKnown()) {
            return true;
        }
    }
    return false;
}

/**
 * 返回顶点列表中unknown且到源点距离最小的顶点，O(|V|)
 */
private Vertex<E> getUnknownMin(List<Vertex<E>> list){
    Vertex<E> min = new Vertex<>();
    // 先将初始min的距离设置为无穷大
    min.setDistance(Integer.MAX_VALUE); 
    for (int i = 0; i < vertices.size(); i++) {
        Vertex<E> cur = vertices.get(i);
        // 在unknown顶点中找到具有有效距离的最小者，顶点距离初识为无距离(Integer.MAX_VALUE)
        if(!cur.isKnown() && cur.getDistance() < min.getDistance()) {
            min = cur;
        }
    }
    return min;
}
```



##### 优先队列(堆)版

###### 算法描述

针对朴素版第2步在当前距离未确定顶点中寻找距离最小者，可以采用优先队列(小顶堆)进行改进，其他过程与朴素版一致。需要注意的是一个顶点的距离在被确定前可能经过多次松弛，算法在每次顶点距离松弛时将其入堆，于是同一时间，堆中可能有多个相同的顶点(松弛过几次就有几个)。这其中最靠顶的将会先出堆，出堆即表明该顶点距离已确定，所以顶点出堆时要判断是否是第一次出堆，若不是则跳过。



###### 算法过程

1. 初始化。设置一个用于输出出堆顶点顺序的结果列表res(不是必要的)。选定一个源点s，初始化所有顶点到源点s的距离为无穷大，表示该点到s的距离尚未确定，置s到其自身距离为0。准备一个小顶堆pq，将s入堆。

2. 一次有效出堆完成一个顶点最短路径的确定。以while循环对pq判空，若不空，堆顶顶点v出堆。若v不是第一次出堆则跳过，若是则v即此时未确定距离的顶点中距离最小者，置v的距离为已确定。

3. 松弛操作。更新上述顶点v的所有邻接顶点w到源点s的距离dw。有如下两种情况，因为在松弛前所有顶点的距离默认为无穷大，所以两种情形的形式一样，都是判断dv + d(v, w) < dw。dw被松弛则将w入堆，以支持第2步(在当前具有有效距离的顶点中找到距离最小者)。

   3.1  如果是初次访问，w到s的距离dw有效化为dw = dv + d(v, w)。

   3.2. 当w不是初次访问，当满足dv + d(v, w) < dw时，更新dw为dv + d(v, w)。

4. 当堆中无元素即所有顶点距离都确定时while循环结束，每个顶点v到源点的最短距离被求出。



###### 时空复杂度

时间复杂度：O(|E|log|E|+|E|log|E|)，化简为O(|E|log|E|)，可进一步化简为O(|E|log|V|) ，化简依据后述。

1. 在当前距离未确定顶点中寻找距离最小者 O(|E|log|E|)。while中pq判空次数与堆大小有关，为O(|E|)，下述。获取距离最小者耗时也与堆大小有关，为O(log|E|)，故为O(|E|log|E|)。

   ※ 优先队列判空操作isEmpty()本身是常数时间操作，但总共要执行O(|E|)次。

2. 所有顶点的距离被更新(松弛)的次数上限为O(|E|)。由算法可知顶点距离的更新(或者说边的松弛)只发生在找到当前距离未确定且距离最小的顶点之后，一次更新的次数是该顶点的邻接顶点数(即出边数，出度)。每一次循环确定一个这样的顶点，也就是每一次循环松弛某一个顶点的所有出边。所有顶点的出边即总边数为|E|。故总松弛次数，也即所有顶点的距离被更新的次数，也即顶点入堆总次数为O(|E|)。dw更新时w入堆，插入操作的时间复杂度为O(log|E|)，入堆次数与更新次数相同，于是所有顶点的距离更新与顶点入堆的时间复杂度为O(|E| + |E|log|E|)，由于|E| <= |V|*(|V| - 1) ，故log|E| <= 2log|V|，故可化简为O(|E|log|V|)。

   ※ 根据上述顶点入堆次数取决于总边数的分析，堆的大小上限不是|V|而是|E|(或者说是O(|E|))，所以dw更新时w入堆的插入操作的时间复杂度为O(log|E|)，只是借助边数与顶点数的关系，可化简为O(log|V|)。

空间复杂度：不考虑用于返回的顶点列表时为O(1)。

※ 连通图顶点数与边数的关系：

无向连通图：|V| - 1 <= |E| <= |V|*(|V| - 1) / 2 

链状时|E|取到最小|V|-1，完全连通即两两相连时取到最大|V|*(|V| - 1) / 2 

有向连通图：|V| - 1 <= |E| <= |V|*(|V| - 1) 

链状时|E|取到最小|V|-1，完全连通即两两相连且正反向成对时取到最大|V|*(|V| - 1) 



###### 代码

```java
public List<Vertex<E>> shortestPathDijkstraPQ(Vertex<E> s){
    List<Vertex<E>> res = new ArrayList<>(); 
    s.setDistance(0);
    // 泛型PriorityQueue的写法，重写compare方法，比较元素getDistance()方法的结果，小顶堆
    PriorityQueue<Vertex<E>> pq = new PriorityQueue<>(new Comparator<Vertex<E>>() {
        @Override
        public int compare(Vertex<E> v1, Vertex<E> v2) {
            return v1.getDistance() - v2.getDistance();
        }
    });
    pq.add(s);

    while(!pq.isEmpty()) { // 判空 O(|E|)
        // 从unknowns中取出距离最小的顶点， O(log|E|)
        Vertex<E> v = pq.remove();
        // 当顶点v被更新过多次(多次入堆)，则存在v多次出堆的情况，
        // 跳过不是第一次出堆的顶点(v.isKnown = true)
        if(v.isKnown()) { 
            continue;
        }
        v.setKnown(true); // 置v的距离为已确定
        res.add(v); // 按顺序输出已确定距离的顶点
        // 松弛v的邻接顶点w，O(|E|log|E|)
        for (Vertex<E> w : getAdjList(v)) {
            if(!w.isKnown() && getEdgeValue(adjListsWeighted, v, w) != 0) { // 松弛对象是距离未确定的w
                // 将dw初次赋有效值或dw有更新时的w加入到pq中，以保证算法过程第2步总是
                // 能够在所有“具有有效距离值但距离未确定”的顶点中寻找距离最小者。
                // 注意dw有更新的情况下堆中可能有多个w(dw不同)，因此插入次数上限与更新次数上限相同，为O(|E|)。
                int dw = v.getDistance() + getEdgeValue(adjListsWeighted, v, w);
                if(dw < w.getDistance()) { // 松弛条件
                    w.setDistance(dw); // 更新dw
                    w.setPre(v); // 置w的前驱为v
                    pq.add(w); // 插入堆中，注意一个顶点的dw可能更新多次，O(log|E|)
                }
            }
        }
    }

    return res;
}
```



##### 斐波那契堆版

###### 算法描述

###### 算法过程

###### 时空复杂度

###### 代码



##### 负边图

以下图为例说明Dijkstra算法为何无法处理负边图。

顶点A是源点，从A开始执行算法，dB和dC被更新为1，2，在距离未确定的顶点B和C中dB更小，B的距离设为已确定，dB = 1。然而实际上从A到C再到B会得到一条更短的的路径(-1)。

<div align=center>
  <img src="https://tva1.sinaimg.cn/large/008i3skNgy1gvys7ulosej308w08mjrb.jpg" alt="image-20211031204826493" width="140" />
</div>

##### 无圈图 

###### 算法描述

连通的有向无圈图有且仅有一个入度为0的顶点(超过1个则不连通，0个则有圈)，以该顶点为源点s，从s开始以拓扑排序方式实现Dijkstra算法。与拓扑排序一样，每次入度为0的顶点v出队，由于已无入边，故该顶点距离dv不会再被更新，此时dv即已确定。标准的Dijkstra算法的关键操作为“在当前具有有效距离且距离未确定的顶点中寻找距离最小者，并置其距离为已确定”，目的是每一次循环确定一个顶点的最短路径(贪婪)，所以也可将本算法看作是免去了引号所描述的操作的Dijkstra算法，即无需寻找，顶点出队时其距离即被确定。

**该算法可应用于有负边的图。** 对任意顶点而言，若到源点有更短路径，会一直更新至其入度为0。以下图为例，从A开始执行算法，A出队，B.dist和C.dist被更新为1和2，然后B，C入度减为1，0，C无入边，其距离不会再被更新，即最终C.dist = 1，C入队。接着C出队，B.dist被更新为-1，B的入度减为0，此时B无入边，其距离不会再被更新，即其距离被最终确定为B.dist = -1。可以看到B的距离在其入度减至0的过程中总有机会更新至最短。

<div align=center> 
  <img src="https://tva1.sinaimg.cn/large/008i3skNgy1gvys7ulosej308w08mjrb.jpg" alt="image-20211031204826493" width="140" />
</div>

###### 算法过程

1. 设置一个用于保存拓扑排序的列表res(不是必要的)。设置一个队列q，置s的距离为0，s入队。
2. while中对q判空，若不空则队首顶点v出队，置其距离为已知，将v放入输出列表res中。
3. 遍历v的邻接顶点w，更新dw，置w的前驱为v。
4. 将v的邻接顶点w的入度减1，并检查减1后是否为0，为0则入队。
5. 当while结束时，输出列表中res出队的顶点顺序为拓扑排序，此时所有顶点前驱确定，源点s到其他顶点的最短路径确定。



###### 时空复杂度

时间复杂度：队列中每个顶点均入队一次，出队一次O(|V|)，在更新并检查邻接顶点的for中，更新和检查的总次数等于边数O(|E|)，故总的时间复杂度为O(|E|+|V|)。

空间复杂度：取决于队列长度，不大于|V|，O(|V|)。



###### 代码

```java
public List<Vertex<E>> shortestPathDijkstraDAG(Vertex<E> s){
    List<Vertex<E>> res = new ArrayList<>();
    Queue<Vertex<E>> q = new LinkedList<>();
    s.setDistance(0);
    q.add(s);

    while(!q.isEmpty()) {
        Vertex<E> v = q.remove(); 
        v.setKnown(true);
        res.add(v);
        for (Vertex<E> w : getAdjList(v)) {
            if(getEdgeValue(adjListsWeighted, v, w) != 0) {
                int dw = v.getDistance() + getEdgeValue(adjListsWeighted, v, w); // 顶点v的距离值+边(v,w)的权值
                if(dw < w.getDistance()) { // 若“v的距离+vw边权”小于w的距离，更新w的距离dw
                    w.setDistance(dw);
                    w.setPre(v);
                }
                w.setIndegree(w.getIndegree() - 1); // w的入度减1
                if(w.getIndegree() == 0) { // 考察w此时入度是否为0，为0则入队
                    q.add(w);
                }
            }
        }
    }

    return res;
}
```



#### Bellman-Ford算法

##### 算法描述

[Bellman-Ford Algorithm(贝尔曼-福特算法)](https://en.wikipedia.org/wiki/Bellman%E2%80%93Ford_algorithm): 与Dijkstra算法的相同点是对边(或者说对顶点)不断执行松弛操作，以逐渐得到所有顶点到源点的最短距离。Dijkstra每次循环完成一个顶点最短路径的确定，而BF算法则对图的所有边(|E|条边)，简单地进行|V|-1次全量松弛操作，即能保证(无负圈图)所有顶点距离取得最短。这是一种思路简单但复杂度较高的做法，每次全量松弛要操作|E|条边，共|V|-1次，复杂度为O((|V|-1)|E|)，即O(|V||E|)。算法正确性证明如下。



##### 正确性证明

1. 已知若一个顶点v的所有入边完成了所有可能的松弛(一条入边可被多次松弛)，则在最后一次松弛后，必有dv=δv。 (dv表示由算法得到的v的距离，δv表示实际的v的最短距离)。
2. 易知，第i次全量松弛，第i层顶点的出边必被松弛。例如第1次全量松弛，s(第1层顶点)的出边必被松弛。第2次全量松弛，由于有了第1次全量松弛的结果，s的邻接顶点(第2层顶点)的出边必被松弛。
3. 考察顶点v的所有入边，p(s, v)路径长(边数)最长不会超过|V|-1，当s到v为链状时取到最长|V|-1 。当v的入边是第1层入边时，将在第1次全量松弛时被松弛，若是第2层入边，则会在第2次全量松弛时被松弛(这条入边的发出顶点在第1次全量松弛时已被松弛)，以此类推，第i层入边会在第i次全量松弛时被松弛。所有顶点的入边组成了该图的所有边，任意一边一定是某一层次的入边(可以同时属于多个层次)。某一层顶点距离的更新，一定来自其入边的松弛。
4. 由此，v的入边能否被全部松弛只取决于最深的入边能否被松弛(当一条入边属于多个层次时，取最深层次)，也即取决于s到v的最长路径max{p(s, v), v ∈ V}的长度。如前述，max{p(s, v), v ∈ V}  <= |V| - 1，故至多经过|V| - 1次全量松弛，图的所有入边必定都松弛过且完成了所有可能的松弛 (某条入边属于多个层次时，可能经过多次松弛) 。如果把图看成以s为根的树，也可以说为求得所有顶点的最短路径，所需的全量松弛的次数取决于树的高度。对某一顶点，可以说该顶点至多经过其深度次全量松弛后取得最短路径。
5. 上述过程对任意顶点均成立，故BF算法正确性得证。

以下考察BF算法对下图的求解过程，重点关注顶点v。

<div align=center> 
  <img src="https://tva1.sinaimg.cn/large/008i3skNly1gyelpsd4sfj30f60cqdga.jpg" alt="image-20211215141921008" width="200"/>
</div>

p(s, v)有如下四种可能：

p1: s > v ，|p1| = 15，(s, v)是v的第1层入边

p2: s > a > v，|p2| = 14，(a, v)是v的第2层入边

p3: s > b > a > v，|p3| = 12，(a, v)是v的第3层入边

p4: s > b > c > v，|p4| = 6，(c, v)是v的第3层入边

可以看到(a, v)边同时属于v的第2和第3层入边。这四条路径最长者长3 (指边数，p3和p4)，根据上述分析，只需要执行3次全量松弛即可完成对v最短路径的确定。又因为源点到所有顶点的所有路径中最长的长度也是3，所以执行3次全量松弛可以确定所有顶点的最短路径。假设全量松弛时顶点的处理顺序为s, a, c, b, v，以下表展示算法对该图的运行过程(第3次之后的全量松弛不会再松弛任何边，省略)。

| 松弛过程      | ds        | da            | dc            | db            | dv                              | 备注                                                         |
| ------------- | --------- | ------------- | ------------- | ------------- | ------------------------------- | ------------------------------------------------------------ |
| 初始          | **0 (*)** | ∞             | ∞             | ∞             | ∞                               | 初始时令ds为0，(*)表示已取得该顶点最短路径。                 |
| 第1次全量松弛 | 0         | **∞ > 9**     | ∞             | **∞ > 3 (*)** | **∞ > 15**                      | (s, a), (s,b), (s, v)属于第1层入边，被松弛。<br />且b只有一条入边，即经过这趟松弛操作，实际上已取得db = δb。 |
| 第2次全量松弛 | 0         | **9 > 7 (*)** | **∞ > 5 (*)** | 3             | **15 > 14**                     | 第1层入边不会再被松弛。第2层入边(a, v), (b, a), (b, c)被松弛。<br />经过这趟松弛操作，a, c的全部入边松弛完毕，取得da = δa，dc = δc。 |
| 第3次全量松弛 | 0         | 7             | 5             | 3             | **14 > 12**<br />**12 > 6 (*)** | 第1，2层入边不会再被松弛，第3层入边(a, v), (c, v)被松弛。<br />v的全部入边松弛完毕，取得dv = δv。 |



##### 算法过程

1. 设置一个用于保存按顺序被松弛的顶点列表res(不是必要的)。置源点s的距离为0。
2. |V|-1次for循环，在该循环内执行对所有边的松弛操作。
3. 进入for后按顶点顺序依次对所有顶点v考察其所有邻接顶点w，是否有v.dist + d(v, w) < w.dist，若有，则松弛之，即令w.dist更新为v.dist + d(v, w)。
4. 在对所有边进行松弛的循环操作中，若某一次没有任何边被松弛，则退出第2步的循环，表明所有可能的松弛已完成(负圈图除外)。
5. 检查图是否有负圈。做法是再对所有边执行松弛操作，若有边可被松弛，则有负圈，结束程序，否则循环一遍后程序正常结束，所有顶点最短路径被求出。



补充说明：

* 该算法可应用于有负边的图

由于该算法在|V|-1次对所有边的松弛操作中会穷尽所有边被松弛的可能，类似以拓扑排序方式针对DAG图的Dijkstra算法(通过入度为0保证穷尽所有松弛的可能)，所以也适用于有负边的图。

* 最坏情形举例

 当图为链状且每次松弛均从底端开始时达到最坏情形，需要对所有边执行|V|-1次松弛后才能求得所有顶点的最短路径。如下图：

假定每次循环均以v, c, b, a, s的顺序松弛，程序开始时置源点s的距离为0。

1. 第1次循环只有a的距离被松弛为1
2. 第2次循环b的距离被松弛为3
3. 第3次循环c的距离被松弛为6
4. 第4次循环v的距离被松弛为10

每次循环都对所有边尝试松弛，但每次只有一个顶点的距离被更新，经过|V|-1次即4次循环后才完成。

<div align=center> 
  <img src="https://tva1.sinaimg.cn/large/008i3skNgy1gxekg5u1idj30ts04i0sz.jpg" alt="image-20211215154902192" width="350" />
</div>

* 提前结束优化

当某一次全量松弛过程中没有边被松弛，说明所有可能的松弛已被穷尽，可提前结束程序。



##### 时空复杂度

时间复杂度：由上述分析可知为O(|V||E|)。

空间复杂度：不考虑用于返回的顶点列表时(对于寻找最短路径并不是必要的)时为O(1)。



##### 代码

```java
public List<Vertex<E>> shortestPathBF(Vertex<E> s){
    List<Vertex<E>> res = new ArrayList<>();
    s.setDistance(0);

    for (int i = 0; i < vertices.size(); i++) {
        // 若某一次全量松弛没有边被松弛，则提前结束
        boolean finished = true;
        // 由于本程序未实现Edge类，用如下两个for实现对顶点v，松弛其所有边
        for (Vertex<E> v : vertices) {
            for(Vertex<E> w : getAdjList(v)) {
                if(getEdgeValue(adjListsWeighted, v, w) != 0) {
                    // 当v的距离尚未被有效化时，dw会大于Integer.MAX_VALUE，所以要此处以long类型处理
                    long dw = (long)v.getDistance() + (long)getEdgeValue(adjListsWeighted, v, w); 
                    if(dw < w.getDistance()) {
                        w.setDistance((int)dw);
                        w.setPre(v);
                        res.add(w); // 这里res表示对于被松弛的顶点，按被松弛的顺序放入，一个顶点可以被多次松弛
                        finished = false; // 若本次全量松弛过程中有边被松弛，标记为false表示未完成
                    }
                }
            }
        }
        if(finished) { // 若未true，表示某一次全量松弛没有边被松弛，提前结束
            break;
        }
    }
    // 负圈检测，再次对所有边执行松弛操作，若有边被松弛，说明存在负圈
    for (Vertex<E> v : vertices) {
        for(Vertex<E> w : getAdjList(v)) {
            if(getEdgeValue(adjListsWeighted, v, w) != 0) {
                int dw = v.getDistance() + getEdgeValue(adjListsWeighted, v, w);
                if(dw < w.getDistance()) {
                    System.err.println("存在负圈！");
                    return res;
                }
            }
        }
    }

    return res;
}
```



#### SPFA

##### 算法描述

[SPFA算法(最短路径快速算法)](https://en.wikipedia.org/wiki/Shortest_Path_Faster_Algorithm)：SPFA算法是对BF算法的一种改进。BF算法通过对所有边执行|V|-1次松弛操作，保证了所有顶点的距离都能更新到最短。这一做法的本质是确保所有顶点的所有入边的所有可能的松弛均已完成，SPFA以更高的效率实现这一点。源点外(源点的距离直接给出，为0)，一个顶点w的距离能够被更新，隐含着这样一个前提：w的前驱v的距离被更新过。因为dv + d(v, w) < dw时才会更新dw，而d(v, w)是不变的，初始时dv和dw都是无穷大，所以只有dv更新(变小)，dw才会更新(变小)，从这种链式反应风格的做法中可以看出SPFA算法本质上也是是DP。从源点出发指向其邻接顶点，对一个连通的有向图，总能遍历所有顶点，每次考察已松弛的顶点v是否能松弛其邻接顶点w，w成为已松弛的顶点后再考察是否能松弛w的邻接顶点u，一直循环到“当前已松弛顶点均无法再松弛任何顶点”为止。所以SPFA设置了一个队列q，程序开始时置源点s的距离为0，s入队。然后对q判空，不空时队首顶点v出队，松弛其边(即更新v的邻接顶点w的距离)，根据上述分析，如果dw有更新，那它的邻接顶点将有机会被更新，所以将w入队，等待出队时尝试松弛w的边。需要注意的是，w入队前需要检查当前队列中是否已有w，若有则无需入队。该检查使得时间复杂度为O(|V||E|)，否则这一复杂度将无法得到保证，在复杂度小节中会详细说明。重复上述过程，最后当q为空时(假设图无负圈)代表所有被更新过距离的顶点，都无法再触发其邻接顶点距离的更新。也可以说对任意一个顶点v来说，s到v的所有路径带来的所有可能的对dv的更新已被穷尽。程序结束时得到所有顶点到源点的最短路径。但若图存在负圈，负圈上的顶点将无限循环入队，算法无法结束。可以记录顶点入队的次数，从后续正确性证明中将会看到，一个顶点入队次数大于|V|-1时，该图存在负圈，此时即可结束程序。



##### 正确性证明

SPFA算法与BF算法的核心内容都在于穷尽所有路径带来的所有可能的松弛。BF算法通过|V|-1次全量松弛达到 (详见BF算法正确性证明)，第i次全量松弛中，有效松弛仅作用于第i+1层顶点，其他层深顶点已不能够被松弛却还是会被尝试，这就产生了冗余操作。SPFA算法利用前述一个顶点的距离能够被松弛的隐含前提，通过队列来减少松弛顶点距离的次数。第i层顶点出队时发生的松弛，效果上相当于BF算法中外层循环第i次对所有边的全量松弛。在连通且无负圈的情况下，按层推进一定能够执行最大层深第|V|层，因此该算法是正确的。

仍以下图为例考察SPFA算法的求解过程，进一步看清其正确性及SPFA与BF的关系。

<div align=center> 
  <img src="https://tva1.sinaimg.cn/large/008i3skNgy1gxehuw9wg2j30f60cqdga.jpg" alt="image-20211215141921008" width="200" />
</div>

第1层顶点只有s，所以第2步s出队相当于BF算法中第1次全量松弛的效果。a，b，v是第2层顶点，所以第3，4，5步相当于BF算法中第2次全量松弛的效果。此时v，a，c是第3层顶点 (根据所在路径层深的不同，一个顶点可以属于不同层)，所以接下来的第6，7相当于BF算法中的第3次全量松弛的效果。最后一步使得队列为空，结束。

| 松弛过程 | ds        | da            | dc            | db            | dv             | 队列q        | 备注                                                         |
| -------- | --------- | ------------- | ------------- | ------------- | -------------- | ------------ | ------------------------------------------------------------ |
| 1. 初始  | **0 (*)** | ∞             | ∞             | ∞             | ∞              | s;           | 初始时令ds为0，(*)表示已取得该顶点最短路径。「;」是层的分隔符。 |
| 2. s出   | 0         | **∞ > 9**     | ∞             | **∞ > 3 (*)** | **∞ > 15**     | **a, b, v**; | s出队时松弛(s, a), (s,b), (s, v)。<br />且b只有一条路径，即经过这趟松弛操作，实际上已取得db = δb。 |
| 3. a出   | 0         | 9             | ∞             | 3             | **15 > 14**    | b, v;        | a出队时松弛(a, v)，这是v的距离第2次被松弛。                  |
| 4. b出   | 0         | **9 > 7 (*)** | **∞ > 5 (*)** | 3             | 14             | v; **a, c**  | b出队时松弛(b, a), (b, c)，a有两条路径，c只有一条，于是da = δa，dc = δc。 |
| 5. v出   | 0         | 7             | 5             | 3             | 14             | a, c;        | 无可松弛边                                                   |
| 6. a出   | 0         | 7             | 5             | 3             | **14 > 12**    | c; **v**     | a出队时松弛(a, v)，这是v的距离第3次被松弛。                  |
| 7. c出   | 0         | 7             | 5             | 3             | **12 > 6 (*)** | v;           | c出队时松弛(c, v)，这是v的距离第4次被松弛。<br />此时p(s, v)所有可能的路径带来的v的入边的松弛均已完成，于是dv = δv。 |
| 8. v出   | 0         | 7             | 5             | 3             | 14             | 空           | 无可松弛边，队列空，程序结束。                               |



##### 算法过程

1. 初始时所有顶点距离为Infinity。设置一个队列q，置源点s的距离为0，s入队。
2. 设置一个用于支持负圈检测的记录所有顶点入队次数的哈希表。
3. 以while循环对q判空，若q不空，队首顶点v出队。
4. 遍历v的邻接顶点w，若满足松弛条件，则更新w的距离，置w的前驱为v，若当前队列无w则将w入队。然后检查w的入队次数是否大于|V| - 1，若大于则表示存在负圈，结束程序。接着将w入队次数加1。

SPFA能够应用于有负权的非负圈图，原因与BF算法一样，因为算法已经处理了所有顶点的所有可能的距离更新。



负圈判定

由于出现负圈时while会无限循环下去，因此需要提前记录保存每个顶点入队的次数(例如用哈希表记录)，顶点v的距离更新后判断当前更新次数是否超过了|V|-1次，若超过则说明存在负圈，若不超过则将更新次数加1。以层为单位追踪顶点入队出队的过程，不难理解无负圈情况下，一个顶点的距离至多被松弛(顶点入队) |V|-1次，若超过则说明存在经过该顶点的负圈。

当图只有一个节点时，要小心处理负圈检测。如下伪代码，次数检测写在次数加一之前可以避免单节点图被判为负圈的问题。

```text
if(!q.contains(w)) { // 检查w是否已经在q中
    q.add(w) // 将w加入队列  
    if(w.inCount > |V| - 1) { // 若大于|V|-1则检出负圈
        System.err.println("存在负圈！")
    }
    w.inCount++
}
```

当然也可以调换次数检测和加一的顺序，并把 > |V| - 1改成 > |V|，如下。同样不会有单顶点图被检测出负圈的问题。前一种 > |V| - 1更能直接表现算法实际。

```text
if(!q.contains(w)) { // 检查w是否已经在q中
    q.add(w) // 将w加入队列  
    w.inCount++
    if(w.inCount > |V|) { // 若大于|V|则检出负圈
        System.err.println("存在负圈！")
    }
}
```



##### 时空复杂度

时间复杂度：O(|V||E|)。

```text
SPFA(Bellman-Ford-Moore)算法伪代码:
1    Queue q
2    q.add(s) // s的距离初始为0, 其他顶点的距离初始为Infinity

3    while(!q.isEmpty())
4        v = q.remove()
5        for (w : v.adjs)  // w是v的邻接顶点
6            if(dv + d(v, w) < dw)
7                dw = dv + d(v, w)
8                if(!q.contains(w)) //检查w是否在当前队列中，不在则入队
9                    q.add(w)
```

按层分析很容易得到SPFA的复杂度上限。顶点一层一层地入队出队，一张图最多有|V|层(以s为第1层)，所以**按层计**，顶点最多入队|V|次。第1层顶点个数为1，其余每层顶点数不会超过|V|（第8行所保证）。虽然一个顶点可能会通过不同的路径重复属于某一层，例如s > a > c，s > b > c，但有了第8行的检查，使得一个顶点只能在一层顶点里出现一次，否则算法复杂度将超过O(|V||E|)。当第一层有|V| - 1 个，其余所有层都有|V|个顶点时达到复杂度上限。

除第1层外每一层顶点入队引起|E|次松弛尝试，除第一层外的其他|V| - 1层的总时间复杂度为O((|V| - 1)*|E|)。因为第一层(顶点s)引起的松弛尝试不会超过|E|，所以总的时间复杂度为O(|V||E|)。

补充说明：

关于第8行的检查，应该是一种思考后的优化，因为每次顶点松弛后紧接着让它入队是个比较自然的想法。所以应该反过来要说明一下为什么加了这个检查优化能够不影响结果的正确性。从s经过长度相同的不同路径都可以到达处于第k层的v，每条路径带来的松弛都能执行到，只是除了第一次之外不把它放入队列而已。放入队列的目的是使其邻接顶点w有松弛的机会。对v来说该有的松弛都经历了，对w来说来自v的松弛机会也确保了，所以可以这么做。

用层次的语言来说明SPFA的话，每出队第i层顶点，使得在最短路径上第i+1层的顶点得到松弛。发现了吗，SPFA第i层顶点出队的效果等同于BF第i轮对边的全量松弛的效果，且比BF的操作更有效率的地方在于，SPFA仅仅松弛它够得着得邻边。BF暴力地对所有边松弛，但有效的只有第i+1层，其他更深或更浅的顶点无法松弛（不相连，够不到），但一律以if(dv + d(v, w) < dw)询问了一次。所以说SPFA是BF的一种改进。



空间复杂度：用于支持负圈检测的哈希表的空间，O(|V|)。

对时间复杂度的分析可以看出，稀疏图中顶点的p(s, v)路径平均条数很少，SPFA实际运行速度会很快。稠密图下会达到O(|V||E|)。



##### Bellman-Ford-Moore和SPFA

SPFA实际上应当称作Bellman-Ford-Moore算法。根据Wiki词条[Bellman-Ford algorithm](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Bellman%E2%80%93Ford_algorithm)的介绍，「对图中所有的边，简单地松弛|V| - 1轮。」算法在相近的几年里被三个人分别独立发明。只是不知道什么原因算法名称后来定型成了Bellman-Ford（真正首创的Shimbel或成最大输家）。

```text
1955年 Alfonso Shimbel
1956年 Lester Ford Jr.
1958年 Richard Bellman
```

又过了一年，1959年的时候Edward F. Moore提出了BF算法的一个[改进](https://link.zhihu.com/?target=https%3A//mathscinet.ams.org/mathscinet-getitem%3Fmr%3D0114710)，即前文的伪代码。照理这个改进版本应该叫做Bellman-Ford-Moore，但为啥都习惯称作SPFA呢？

时间来到1994年，西南交通大学的段凡丁在该年4月的《西南交通大学学报》里发表了题为《[关于最短路径的SPFA快速算法](https://link.zhihu.com/?target=https%3A//xueshu.baidu.com/usercenter/paper/show%3Fpaperid%3D39798c8bf2d1b5236cdaae3152d490ed%26site%3Dxueshu_se)》的论文，重新发明了Moore的改进，并且给了个比较通俗的名字Shortest Path Fast Algorithm。段老师显然没看过Moore当初的论文，否则不会给出一个错误的复杂度估计（给出的复杂度是O(k|E|)）。现在用Google搜SPFA，也能在各种英文论坛中查到大家对这个算法的讨论，用的名字就是SPFA。



##### 代码

```java
public List<Vertex<E>> shortestPathSPFA(Vertex<E> s){
    List<Vertex<E>> res = new ArrayList<>();
    s.setDistance(0);
    Queue<Vertex<E>> q = new LinkedList<>();
    q.add(s);
    // 设置一个HashMap记录每个顶点入队次数，以支持负圈检测
    Map<Vertex<E>, Integer> inCounter = new HashMap<>();
    for (Vertex<E> v : vertices) {
        inCounter.put(v, 0);
    }

    while(!q.isEmpty()) {
        Vertex<E> v = q.remove(); 
        res.add(v);
        for (Vertex<E> w : getAdjList(v)) {
            if(getEdgeValue(adjListsWeighted, v, w) != 0) { // 只对v的邻边操作
                int dw = v.getDistance() + getEdgeValue(adjListsWeighted, v, w);
                if(dw < w.getDistance()) { // 若源点经过顶点v到顶点w的距离小于源点到w的距离，更新源点w的距离
                    w.setDistance(dw);
                    w.setPre(v);
                    if(!q.contains(w)) { // 检查w是否已经在q中
                        q.add(w); // 当w有松弛操作时将w加入队列  
                        // 在w入队后判断当前入队次数，若大于|V|-1则检出负圈，输出提示信息并结束程序
                        int wInCount = inCounter.get(w);
                        if(wInCount > vertices.size() - 1) {
                            System.err.println("存在负圈！");
                            return res;
                        }
                        inCounter.put(w, wInCount + 1);
                    }
                }
            }
        }
    }

    return res;
}
```



### 所有顶点对最短路径

#### Floyd-Warshall算法

##### 算法描述

[Floyd-Warshall算法(弗洛伊德算法)](https://en.wikipedia.org/wiki/Floyd%E2%80%93Warshall_algorithm): 求解图中任意两点的最短路径的算法。图可以是有向图或无向图，可以有负权边，但不能有负圈(负圈上任意两点间无最短路径)。算法以3个循环考察任意顶点i到任意顶点j是否有经过任意顶点k的可松弛路径，即对每一个顶点k(外层循环)，考察是否有d(i, k) + d(k, j) < d(i, j)，若有则更新d(i, j) = d(i, k) + d(k, j)。该算法的本质是动态规划，可以这样理解其工作过程。已知确定的任意两点i, j间有确定的最短路径p(i, j) （只要无负圈），p(i, j)由多次松弛操作得到。先假设算法是正确的 (详细证明见后述)，那么在p(i, j)的最后一次松弛后(通过顶点y松弛)，得到p(i, j) = p(i, y) + p(y, j)。同理，p(i, y)和p(y, j)是p(i, j)的两个部分，它们由之前的松弛操作得到。例如松弛顶点x后得到p(i, y)，可知p(i, y) = p(i, x) + p(x, y)，松弛顶点z后得到p(y, j)，可知p(y, j) = p(y, z) + p(z, j)。路径p(i, j)的构建过程可以一直溯源拆分到某三个相邻顶点的连接。例如在p(i, j)上有三个相邻顶点为i > a > b，那么在外层循环处理a，中层循环处理i，内层循环处理b时，松弛操作将i > a > b“连接起来“（因为最短路径上的任意子路径都是最短的，必松弛）。顺着算法执行过程不难看出，算法通过外循环的k来连接边，然后不断连接短路径产生长路径，最终增长完整的最短路径。

以状态转移方程的形式描述如下，其中d(k, i, j)表示最终得到的最短路径长，k是导致最后一次松弛的顶点。注意k-1在不同部分中的含义，在d(k-1, i, j)中表示在考察经过k是否能松弛p(i, j)前的最后一个导致p(i, j)松弛的顶点。在d(k-1, i, k)中表示得到p(i, k)的最后一次松弛所操作的外层循环顶点，在d(k-1, k, j)中表示得到p(k, j)的最后一次松弛外层循环操作的顶点。

d(k, i, j)  = min{d(k-1, i, j), d(k, i, j) = d(k-1, i, k)  + d(k-1, k, j)}

最短路径不经过顶点k: d(k, i, j) = d(k-1, i, j)

最短路径经过顶点k：d(k, i, j) = d(k-1, i, k)  + d(k-1, k, j)

```
// floyd核心内容伪代码
for(k : V)
  for(i : V)
    for(j : V)
      if(d(i, k) + d(k, j) < d(i, j))
        d(i, j) = d(i, k) + d(k, j)
```



##### 算法过程

1. 顶点间最短路径(距离)矩阵初始化并保存。顶点到自身距离为0，相邻顶点间距离为边权，不相邻顶点间距离设为无穷大(Integer.MAX_VALUE)。若要求输出路径本身，则需要另一个最短路径信息矩阵。
2. 以3个循环(循环操作对象是所有顶点)考察i到j是否可经由k松弛。k, i, j分别对应外，中，内层循环。
3. 松弛操作。若d(i, k) + d(k, j) < d(i, j)，则d(i, j) = d(i, k) + d(k, j)，若有路径信息矩阵，也要更新。

**递归输出路径：**上述过程只能得到一个任意顶点到任意顶点的最短路径长度值矩阵。想要输出路径，只需要再增加一个保存路径信息的矩阵。d[i] [j]的值是k，即i经过k到j，在每次松弛时更新k。例如有最短路径i > a > b > c > j。程序结束后得到的路径信息矩阵如下。

| 顶点 | i    | a    | b    | c    | j    |
| ---- | ---- | ---- | ---- | ---- | ---- |
| i    | null | null | a    | null | b    |
| a    | null | null | null | null | null |
| b    | null | null | null | null | c    |
| c    | null | null | null | null | null |
| j    | null | null | null | null | null |

利用该矩阵，通过递归即可找到i > a > b > c > j。递归过程大致如下，顶点输出顺序即为路径顺序。

```
i > j, 找到b
  i > b，找到a
    i > a为null，输出i
    a > b为null，输出a
  b > j，找到c
    b > c为null，输出b
    c > j为null，输出c
 最后输出j
```



##### 正确性证明

命题P：给定一张有k个顶点的图，任意顶点i到任意顶点j之间存在最短路径p(i, j)，若这条路径的点集是{i, v<sub>1</sub>, v<sub>2</sub>, ..., v<sub>n</sub>, j}，其中下标中1~n (n ≤ k - 2)表示在Floyd-Warshall算法外层循环中被处理的先后顺序（除i在起点，j在终点外其他顶点的连接顺序是任意的）。命题宣称在外层循环处理顶点vn后，i到j的最短路径长度d(i, j)被确定，且算法结束时得到任意两点间的最短路径。

补充：由于i, j是任意的，本命题等同于在外循环处理u<sub>1</sub>时，当中内两层循环结束后得到所有以顶点集{i, u<sub>1</sub>, j}构成的最短路径。接着外循环处理u<sub>2</sub>，当中内两层循环结束时得到所有以顶点集{i, u<sub>1</sub>, u<sub>2</sub>, j}构成的最短路径。再次强调，i, j是任意顶点，u<sub>1</sub>, u<sub>2</sub>, u<sub>3</sub>...为外层循环处理顶点的顺序，其在所构成的最短路径上可以是除了起止点外的任意位置（注意与v的下标的意义区分开）。

本命题的递推关系不太典型，有必要在证明开始前说明如下p(i, j)与递推过程的联系。

命题P宣称在外层循环处理v<sub>n</sub>时，d(i, j)被确定。其内涵是通过中内层for循环验证是否有d(i, v<sub>n</sub>) + d(v<sub>n</sub>, j) < d(i, j)，满足则更新d(i, j)。这就要求d(i, v<sub>n</sub>)和d(v<sub>n</sub>, j)在此时是已知的。外层循环在处理v<sub>n</sub>前处理的是v<sub>n-1</sub>，v<sub>n-1</sub>要么在p(i, v<sub>n</sub>)上，要么在p(v<sub>n</sub>, j)上。处理v<sub>n-1</sub>时得到的是d(i, v<sub>n</sub>)  (在p(i, v<sub>n</sub>)上时) 或d(v<sub>n</sub>, j) (在p(v<sub>n</sub>, j)上时)。用同样的方法继续考察v<sub>n-2</sub>, v<sub>n-3</sub>, ...，最终我们会溯源到路径p(i, j)上的某条边(两个直接连起来的顶点)，该边的长度是初始时被赋予的。

于是我们就可以给出下面的不太典型的归纳法证明：

当n = 0时，表示外层循环尚未开始执行，得到点集为{i, j}的路径p(i, j)，意思是求得所有仅有一条边(i, j)的最短路径，实际上就是所有的边。这在程序一开始的初始化边权时即得到，故n = 0满足命题。

当n = 1时，表示外层循环处理了第一个顶点u<sub>1</sub>，得到点集为{i, u<sub>1</sub>, j}的路径p(i, j)，意思是求得所有仅有两条边(i, u<sub>1</sub>), (u<sub>1</sub>, j)的最短路径。外层循环在处理u<sub>1</sub>时，通过中内两个循环，能够将不相邻的i, j间初始为Infinity的距离更新为d(i, u<sub>1</sub>) + d(u<sub>1</sub>, j)，故n = 1满足命题。

当n = 2时，表示外层循环处理了u<sub>1</sub>, u<sub>2</sub>，得到点集为{i, u<sub>1</sub>, u<sub>2</sub>, j}和{i, u2, j}的路径p(i, j)，后者与n = 1时的情况类似，说明略。前者是求得所有有三条边(i > u<sub>1</sub> > u<sub>2</sub> > j 或 i > u<sub>2</sub> > u<sub>1</sub> > j )的最短路径。外层循环在处理u<sub>2</sub>时，通过中内两个循环，能够利用d(i, u<sub>2</sub>) + d(u<sub>2</sub>, j)来更新d(i, j)，这是因为d(i, u<sub>2</sub>)和d(u<sub>2</sub>, j)在n=0或n=1时就已求出，故n = 2满足命题。

此后，n = 3, 4 5...，当n = k时，总能通过u<sub>k</sub>这个顶点将当前所有经过u<sub>k</sub>的所有长度的路径连起来。

因此对于顶点集为{i, v<sub>1</sub>, v<sub>2</sub>, ..., v<sub>n</sub>, j}的路径，当外循环的处理完v<sub>n</sub>时，得到该最短路径。

当算法结束时，得到以任意顶点集合构成的路径（只要存在），也即得到任意两点间的最短路径。证毕。



补充说明：已知任意两点i, j之间的最短路径为p，那么p上的任意两点的最短路径确定在p上。反证法简单可证。假设p上有b, c两点，这两点之间的最短路径经过一不在p上的顶点，那i, j的最短路径也就不是p，而是p(i, b) + p(b, c) + p(c, j)。



下面通过一个例子观察Floyd算法的动态规划过程，对其正确性可以有更直观的感受。由于算法总是在整张图上进行处理，展示整张图将使过程变得杂乱，因此我们只选取一条i到j的最短路径，展现该路径的构建过程。由于这条路径是随意选取的，其他所有在图上的任意两点的路径的构成都是类似的。

设图G中i, j间最短路径p为 i > a > b > c > d > e > f > g > h > j。最外层循环对该路径上顶点的处理顺序可以是任意顺序，例如b, h, i, g, a, e, j, f, c, d(分别标上序号1, 2, 3, 4, 5, 6, 7, 8, 9, 10，表示外层循环处理的先后顺序)。现在我们以溯源的方式从处理最后一个顶点d得到d(i, j)开始观察，且只观察程序对上述十个顶点的处理（对其他顶点的处理形成其他路径）。该路径的取得只与三个循环对该路径上的顶点的操作有关，循环对其他顶点的操作不影响结果，因为其他顶点不在该最短路径上，由它们导致的松弛不影响路径p，或者说p会从若干条i到j的路径中胜出。

外层循环处理d之后由d(i, d) + d(d, j)的结果得到i到j的最短路径长d(i, j)，因此d(i, d)和d(d, j)此时必是已知的。继续溯源，看看d(i, d)和d(d, j)是如何得到的。到d的路径上c是最后被处理的，处理c时计算d(i, d) = d(i, c) + d(c, d)，其中d(c, d)是边长，程序开始时已知，d(i, c)需要继续溯源，d(i, c) = d(i, a) + d(a, c)，其中d(i, a)是边长，d(a, c) = d(a, b) + d(b, c)，d(a, b)和d(b, c)是边长。其余过程如图，标红处表示相邻的顶点的边长，在程序开始时得到。

可以看出，外层循环处理i到j的最短路径的所有顶点的过程中，先处理的顶点得到子路径总能够为后处理的顶点构建更长的子路径，直到处理(最短路径上的)最后一个顶点时，将两个子路径连接起来形成最终的最短路径。以路径增长的角度看，正是动态规划过程的体现。

不同典型动态规划的是，某一次的松弛形成的路径不一定直接作用于下一次松弛，而是在之后某一次松弛中发生作用。例如处理b(1)和处理h(2)是外层循环的两次相邻的操作，它们分别产生了两条不相连的子路径a>b>c和g>h>j。之后外层循环处理a(5)时a>b>c增长为i>a>b>c，处理c(9)时增长为i>a>b>c>d。g>h>j在外层循环处理g(4)时增长为f>g>h>j，处理f(8)时增长为d>e>f>g>h>j (d>e>f在外层循环处理e(6)时得到)。最终在处理d(10)时得到i > a > b > c > d > e > f > g > h > j。

<div align=center> 
  <img src="https://tva1.sinaimg.cn/large/008i3skNly1gxlre6dkzlj312k0pyace.jpg" alt="image-20211221210813747" width="700" />
</div>

<div align=center> 
  <img src="https://tva1.sinaimg.cn/large/008i3skNgy1gyd3nnjv03g30fq07anpe.gif" alt="floyd_demo_202112231607_compressed3" width="500" />
</div>

总结Floyd算法的过程，外层循环执行完第k次，给出由k+1条边组成的路径，下一次会利用长度为1, 2, ... , k+1的路径连接出长度为k+2的路径。这就好像将任意两点连成单边线（只要这两点之间存在路径），然后再将两条单边线作为零件连成长度为2的线，因为具备所有单边线，所以无论长度为2的线是由哪些单边线组成的，都可以找到并连起来。然后再利用长度为1，2的线作为零件连成长度为3的线，因为无论一条长度为3的线是如何构成的，构成它的单边线和2边线都已具备。以此类推直到连出所有可能长度的线。



##### 时空复杂度

时间复杂度：三个循环，O(|V|^3)。

空间复杂度：用于保存任意两点间最短路径长和路径信息的矩阵空间，O(|V|^2)。



##### 代码

```java
public List<Object> allShortestPathFloyd(){
    // 创建最短路径矩阵(以哈希表套哈希表形式)，并初始化。
    List<Object> res = new ArrayList<>();
    Map<Vertex<E>, Map<Vertex<E>, Integer>> interVerticesDist = copyMap(adjListsWeighted);
    Map<Vertex<E>, Map<Vertex<E>, Vertex<E>>> pathRecord = new HashMap<>(); // 路径信息矩阵
    for (Vertex<E> v : vertices) {
        Map<Vertex<E>, Vertex<E>> neighbors = new HashMap<>(); // 路径信息矩阵初始化步骤
        for (Vertex<E> w : vertices) {
            neighbors.put(w, null); // 路径信息矩阵初始化步骤
            if(v == w) { // 顶点到自身的边权初始化为0
                setEdgeValue(interVerticesDist, v, w, 0);
            }
            // 不存在的边权初始化为Infinity (在构造器中初始化为0，这里修改为Integer.MAX_VALUE表示无穷大，便于后续松弛操作)
            else if(getEdgeValue(interVerticesDist, v, w) == 0){
                setEdgeValue(interVerticesDist, v, w, Integer.MAX_VALUE);
            }
        }
        pathRecord.put(v, neighbors); // 路径信息矩阵初始化步骤
    }
    // 核心部分：3个for实现floyd算法
    for (Vertex<E> k : vertices) {
        for (Vertex<E> i : vertices) {
            for (Vertex<E> j : vertices) {
                // 若D(i,k) + D(k, j) < D(i, j)，则更新D(i, j)
                int ik = interVerticesDist.get(i).get(k);
                int kj = interVerticesDist.get(k).get(j);
                int ij = interVerticesDist.get(i).get(j);
                // 由于不存在的边权设为了Integer.MAX_VALUE，需要转型为long
                if((long)ik + (long)kj < (long)ij){
                    Map<Vertex<E>, Integer> neighbors = interVerticesDist.get(i);
                    neighbors.put(j, ik + kj);
                    interVerticesDist.put(i, neighbors); // 更新最短路径长度矩阵
                    pathRecord.get(i).put(j, k); // 更新路径信息矩阵
                }
            }
        }
    }
    res.add(interVerticesDist);
    res.add(pathRecord);
    return res;
}

private Map<Vertex<E>, Map<Vertex<E>, Integer>> copyMap(Map<Vertex<E>, Map<Vertex<E>, Integer>> origin){
    Map<Vertex<E>, Map<Vertex<E>, Integer>> copiedMap = new HashMap<>();

    for (Map.Entry<Vertex<E>, Map<Vertex<E>, Integer>> entry : origin.entrySet()) {
        Vertex<E> key = entry.getKey();
        Map<Vertex<E>, Integer> value = new HashMap<>();
        for (Map.Entry<Vertex<E>, Integer> neighbor : entry.getValue().entrySet()) {
            value.put(neighbor.getKey(), neighbor.getValue());

        }
        copiedMap.put(key, value);
    }

    return copiedMap;
}

/**
 * 通过递归方式从Floyd算法得到的路径信息矩阵中得到两点间的最路径并返回
 */
public List<Vertex<E>> getPath(Map<Vertex<E>, Map<Vertex<E>, Vertex<E>>> pathRecord, 
        Vertex<E> i, Vertex<E> j) {
    List<Vertex<E>> res = new ArrayList<>();
    getPre(pathRecord, i, j, res);
    res.add(j);

    return res;
}

private void getPre(Map<Vertex<E>, Map<Vertex<E>, Vertex<E>>> pathRecord, 
        Vertex<E> i, Vertex<E> j, List<Vertex<E>> res) {
    Vertex<E> k = pathRecord.get(i).get(j);
    // 当i, j之间无k，将i加入res中
    if(k == null) { 
        res.add(i); 
        return;
    }
    getPre(pathRecord, i, k, res);
    getPre(pathRecord, k, j, res);

    return;
}
```



### 词梯问题



## 网络流算法

### 最大流最小割定理

[最大流最小割定理(Max-flow min-cut theorem)](https://en.wikipedia.org/wiki/Max-flow_min-cut_theorem)：对于一个网络图，从源点到目标点的最大流量等于最小割的所有边的容量之和。

本节内容来源于[B站南大老师蒋炎岩的授课视频](https://www.bilibili.com/video/BV1Q7411R7ie?spm_id_from=333.999.0.0)。先从路径存在问题引入割和割的大小的概念，再由割的大小和s-t不相交路径数量的关系，证明Ford-Fulkerson方法的正确性。在证明过程中就能看到s-t最大流即s-t最小割的大小。



#### 从路径存在问题到割

在一张有向图中，s到t的路径是否存在，我们通常会想到用DFS/BFS算法判定。算法从s出发找到t则说明s-t路径存在，否则不存在。为了引入割的概念，尝试以更直观也更严格的方式考察该问题。

以排列组合的方式简单地罗列顶点以构成所有可能的s-t路径。例如一张包含顶点s, a, b ,c d, t的图，可以罗列出如下路径。

s > t

s > a > t

s > b > t

...

s > a > b > t

s > a > c > t

...

若上述列表中存在这样一条路径，该路径中的每对相邻顶点构成的边均存在，则该路径即s-t路径。反之，若列表的每一条路径都能找到不存在的边，则证明不存在s-t的路径。

罗列路径列表的方式在数学上虽是严格的，但过于复杂，需要更方便的证明。再次考虑DFS/BFS，算法从s出发，会找到所有能到达的顶点，将这些顶点作为集合S，剩下s无法到达的顶点作为集合T。s-t路径不存在，等价于S中任意顶点都没有到T任意顶点顶点的连边。由此引入如下割的概念。

**割的定义**：割简单地说就是对G的顶点集的一种划分。有向图G(V, E)的 s-t 割C = (S, T)指V被划分为顶点集S和T，使得s ∈ S，t ∈ T。

**割的大小**：对于上述割，其大小指从S到T的边的数量，即{(u, v) ∈ E | u ∈ S, v ∈ T}。

根据割及其大小的定义，只要存在s-t路径，无论如何划分V得到s-t割，总能在找到一条边，是穿过S到T的，即该割的大小一定大于0。例如下面的s-t路径，s, a, d, e, h ∈ S， b, c, f, g, i, t ∈ T，红箭头表示s-t割的边。只要满足s ∈ S (s在绿色阴影里)，t ∈ T (t不在绿色阴影里)，在有路径的情况下，红箭头一定存在，而与绿色阴影的形状(s-t割)无关。反过来所有s-t割的大小都大于0，则s-t路径存在。

因此，要想证明s-t路径不存在，只需要找到一个大小为0的s-t割即可。 

<div align=center> 
  <img src="https://tva1.sinaimg.cn/large/008i3skNgy1gyb3myal6dj30e603cjrb.jpg" alt="graph cut example" width="400" />
</div>

#### 不相交路径数量与割的大小

s-t的不相交路径指s-t路径间没有公共边。本节将建立图中s-t不相交路径数量与s-t割大小的联系。

假设图有k条s-t的不相交路径，想要知道k的大小，可以逐步假设k的大小(k = 0, k = 1, k = 2,...)，再验证是否确实有那么多条。例如假设k=0时，可视作s-t路径存在性的问题，可以通过上一小节的结论，若存在大小为0的s-t割，则k=0。当k > 0时，考察是否还能用s-t割的大小表达不相交路径数量。下图k=1，以图中的阴影表示s-t割，可以看出该割的大小为1。不相交路径数量显然受到割边(b, c)的限制，更进一步，如果所有s-t割的大小最大为1，那么所有s-t路径都会穿过同一条割边，而如果有大小大于1的割，例如大小为2，则可以分别通过两条不同的割边从s到t。因此，s-t割的大小l决定了s-t不相交路径数量k的上限，即 k ≤ l。

<div align=center> 
  <img src="https://tva1.sinaimg.cn/large/008i3skNgy1gyb4kwuxbjj306b0373yc.jpg" alt="size one cut" width="250" />
</div>

上述以割的大小给出了不相交路径数量k的上界，而直接找到m条不相交路径能够给出k的下界，即 m ≤ k ≤ l。当s-t割的大小为m时，就可以确定不相交路径数量为m。根据这个思路，给出如下求k的算法。

用DFS/BFS在G中寻找一条s-t路径p，找不到则结束，k=0；若能找到，则从G中删去p，k++。

但实际考察下图后会发现，当第一次选择的路径为s > d > a > t后，将无法再找到下一条路径，而该图k=2，因此该算法不总能得到正确结果。

<div align=center> 
  <img src="https://tva1.sinaimg.cn/large/008i3skNgy1gyb5g414o5j305i03hglg.jpg" alt="graph" width="200" />
</div>

Ford & Fulkerson在1956年的[论文](http://www.cs.yale.edu/homes/lans/readings/routing/ford-max_flow-1956.pdf)中提出了对上述算法的改进方法，并证明了改进后算法的正确性。改进的内容十分简洁，即在每一次找到一条路径并删除该路径后，在t-s的相反方向上添加该路径。如下，找到s > d > a > t的路径后将其删除，但添加t > a > d > s的路径。于是在下图中还能找到s > a > d > t，继续删除并添加反向路径，最终无法再找到s-t路径，k=2被正确求出。

<div align=center> 
  <img src="https://tva1.sinaimg.cn/large/008i3skNgy1gyb5zoni2aj305i03n3yd.jpg" alt="reverse path" width="200" />
</div>

在Ford & Fulkerson提出的这个方法中，s-t的路径称为增广路径(Augmenting Path)。可以将反向路径称作反向增广路径，而添加这一反向路径的操作是整个算法最为精彩的部分，下面来证明为何添加反向路径能够使算法有效。



#### Ford-Fulkerson方法正确性证明

本小节不以严谨的数学推导，而以图形化的直观方式说明为何FF方法是正确的。

##### 添加反向边的有效性

如下左图，p1, p2, p3是先找到的3条s-t不相交路径，p4是第4条。p4由两种边组成，如下中图，一种是不与p1, p2, p3交叠的边，即此前尚未出现在不相交路径上的原边，另一种是与p2和p3交叠的部分，这些边是找到p2，p3后添加的反向边，它们使得p4可以沿着这样的反向边到达t。假设之后找不到新的路径，我们就可以说该图的不相交路径数量是4。这是通过添加反向边来间接得到的，现在的问题是如何知道原图中确实有4条不相交路径。为了更清楚地看到这个事实，去掉p4走过的反向边，如下右图，p1不变。而p'2，p'3，p'4显然就是原图中的s-t不相交路径(注意下右图是图初始时的一部分)，只不过一开始没找到p'2, p'3, p'4，而是找到了p2, p3,，然后通过反向边找到了p4。尽管算法找到的路径与下右图显示的原图上的路径不同，但数量是正确的。如果还有p5，仍然可以通过相同的方法证明原图中确实有5条不相交路径。在示意图中那些通过反向边得到的路径看起来就像是切开了起初得到的路径，又通过自己的“原边”部分将这些路径的断点和汇点连接起来。

<div align=center> 
  <img src="https://tva1.sinaimg.cn/large/008i3skNgy1gyc4phqd7wj315r0djmyk.jpg" alt="FF correctness" width="850" />
</div>

##### 算法结束时的结果正确性

上述内容说明了添加反向边能够找到新的不相交路径，并且能够验证算法找到的路径与原图实际的路径一一对应(数量相等)，但我们还必须说明为什么算法终止时得到的路径数量是正确的。

以上中图为例，找到p4后将边反向得到下图。假设此时算法无法再在残留图中找到新的路径，算法终止，得到的不相交路径数量是m = 4。要证明k = 4是正确的，通过m ≤ k ≤ l关系，只要证明原图中存在大小为l = 4的s-t割即可。我们观察上右图，通过一条线简单地切断p1, p'2, p',3, p'4，线左边的点归入S集，其他顶点(包括未在图中体现的点)归入T集，似乎能轻易得到一个大小为4的s-t割，但要注意的是该图只表现了部分顶点，所以只能说这样的割的大小至少是4。

回到下图，不难发现，残留图中由两种边构成，一种是不相交路径的反向边（注意，不完全是通过算法找到的路径，而是前述说明中与p2, p3, p4对应的p'2, p'3, p'4那样的路径），一种是其他没有在不相交路径上的原边。由于s-t已不连通，我们不必指出具体的割就可以知道此时在残留图中存在一个s-t割大小为0。

这说明残留图的4条t-s路径只能穿过一次T-S，因为如果穿过一次以上，则必有穿过S-T的边(t → s → t → s，会导致出现一次s → t)，这与s-t不连通矛盾。所以t-s确定只有4条路径，而残留图与原图的区别仅为不相交路径的方向相反，由于残留图中只有4条t-s路径，也就是说原图中只有4条s-t不相交路径。再次强调，残留图中的t-s路径与原图中的s-t路径一一对应，当算法结束时，如果能确定残留图中确实只有4条t-s路径，就等同于原图中确实只有4条s-t路径，因为如果残留图中还能找到t-s路径，那这条路径一定来自于原图的s-t路径。到这里我们可以宣称，原图存在一个大小为4的s-t割。

<div align=center> 
  <img src="https://tva1.sinaimg.cn/large/008i3skNgy1gyc7fvlvavj306b0643yg.jpg" alt="residual graph after p4" width="240" />
</div>

上述内容整理如下。

证明：算法结束时得的不相交路径数是正确

1. 算法结束时知原图有4条不相交路径，需要证明只有4条
2. 原图s-t不相交路径数量与残留图t-s不相交路径数量严格相等，转为证明残留图中只有4条不相交路径
3. 若t-s路径只穿过T-S一次就能证明t-s路径有且只有4条
4. 反证法，以残留图上有大小为0的s-t割证明3

证毕。



因为找不到更多的s-t路径，所以此时的s-t流（不相交路径数）是最大的。

因为m ≤ k ≤ l的关系，所以找到的大小为4的割在原图所有s-t割中最小。

所以实际上FF正确性的证明也是最大流最小割定理的证明。



#### 推广至有权图

只需将边权为n的边看作n单位条边，转换成无权图即可适用前述证明。例如s > a > t路径，(s, a)边权为2，(a, t)边权为3，将边拆成单位边后，原容量为2的一条路径被分成两条单位容量的路径。



### Ford-Fulkerson方法

#### 算法描述

[Ford-Fulkerson方法(福特-富尔克森方法)](https://en.wikipedia.org/wiki/Ford%E2%80%93Fulkerson_algorithm):1956年由L. R. Ford Jr和D. R. Fulkerson发表的[论文](http://www.cs.yale.edu/homes/lans/readings/routing/ford-max_flow-1956.pdf)中描述的基于贪婪思想的求有向图最大流的算法。因为该算法未指定求增广路径的方法，所以通常也将其称作Ford-Fulkerson方法。若增广路径以BFS方式求出，则为Edmonds-Karp算法。此外也可以以DFS方式求最短路径，后续还会看到结合BFS和DFS方式求增广路的Dinic's算法。FF算法先设置要求解的目标，即源点s到目标点t的最大流，设为f，初始值为0。算法寻找s到t的路径p1，p1上最小权边的边权即s能沿p发往t的流量c(p1)。由于不适当的增广路选择会使得最大流量f在求出前s到t就没有了增广路，因此每次求出一条增广路后，要在相反方向路径上添加c(p1)，可以理解为t到s在必要时能够返回原先s发送到t的流量。该做法的正确性证明见"最大流最小割"一节的说明。无法再找到s到t的增广路时所得到的f即为s到t的最大流。



#### 算法过程

给定一张图G(V, E)，源点为s，汇点为t。求G中从s到t的最大流f。

1. 设置一张残留网络(残余图)Gf，Gf初始为G。

2. 考察Gf中是否有从s到t的路径p，使得p上的每一条边(u,v) ∈ p，都有c(u, v) > 0。p称作增广路径(Augmenting Path)。若存在p，则：
   2.1 将p中最小边的权c加入f。
   2.2 在Gf中p的每条边减去该最小边的权c。
   2.3 在Gf中p的反向路径上的每条边上加上该最小边权c。
   
3. 在Gf中反复寻找增广路径p并执行2中的操作直到Gf中找不到p，算法停止。此时得到的f即为s到t的最大流。

   

#### 算法正确性证明

参考“最大流最小割定理“一节中的”Ford-Fulkerson方法正确性证明”小节。



#### 两种坏情形

**小边权增广路径情形**

如下图，寻找从A到B的增广路径时，若选取A>B>C>D后，再选取A>C>B>D (C>B由反向边操作得到)，如此反复选取。由于B，C之间的小权边限制了流的大小，需经历2000次选取才能结束算法，而选取A>B>D，A>C>D则只需要两次。针对如何避免选取含有小边权的增广路，有以下两种做法。

做法1: 总是选取流最大的增广路径。显然能够避免小权边引起的多次增广路选取。利用Dijkstra算法，将顶点距离的更新策略从选取较小值改为选取较大值，即可实现该做法。

做法2: 总是选取最短的增广路径。较短的增广路降低了路径上出现小权边的概率。

<div align=center> 
  <img src="https://tva1.sinaimg.cn/large/008i3skNgy1gvev606qcyj60ey0a0mxg02.jpg" width="250" />
</div>

以BFS方式应用无权最短路径方法实现做法2即为Edmonds-Karp算法。



**算法无法终止的情形**

算法能够结束的一个隐含假设是每次找到的增广路至少使s到t的发送流增加1个单位(例如整数1)，只要最大流是固定的，经过有限次操作后总能发送到最大流。如果边权存在无理数，则算法可能无法结束。下面说明Wiki上的例子。

如下图左的流量网络，e1 = (b, a), e2 = (d, c), e3 = (b, c)， |e1| = |e3| = 1，|e2| = r = (√5 - 1) / 2。其他边的容量为M (M >= 2)，且r^2 = 1 - r， r^3 = r - r^2, r^4 = r^2 - r^3...。有路径 p0 = s > b > c > t，p1 = s > d > c > b > a > t， p2 = s > b > c > d > t，p3 = s > a > b > c > t。若按路径p0, p1, p2, p1, p3, p1, p2, p1, p3, p1, p2, p1, p3...的顺序增广，则算法无法结束。

观察下表经过上述顺序一次循环后的结果，步骤1和步骤5时e1, e2, e3剩余容量为r^n, r^(n+1), 0的形式。发送流为1+2(r^1+r^2)。经过无限次完整的循环，e1, e2, e3剩余容量都会是r^n, r^(n+1), 0的形式，而发送流为1+2∑r^i  (r是从1到正无穷的整数)。根据等比数列求和公式有：

1+2∑r^i   = 1 + 2*r/(1-r)

将r = 1 - r^2 = (1+r)*(1-r)代入上式分子中的r，得到3+2r，即发送流会向2+√5趋近，但从原图可以看出最大流为2M + 1 > 5 > 2+√5。

| 步骤 | 增广路 | 发送流 | e1剩余  | e2剩余 | e3剩余 |
| ---- | ------ | ------ | ------- | ------ | ------ |
| 0    |        |        | r^0 = 1 | r^1    | 1      |
| 1    | p0     | 1      | r^0     | r^1    | 0      |
| 2    | p1     | r      | r^2     | 0      | r^1    |
| 3    | p2     | r      | r^2     | r^1    | 0      |
| 4    | p1     | r^2    | 0       | r^3    | r^2    |
| 5    | p3     | r^2    | r^2     | r^3    | 0      |

<div align=center> 
  <img src="https://tva1.sinaimg.cn/large/008i3skNgy1gxw1wu8pnjj31co0n8q8f.jpg" alt="image-a" width="800" />
</div>

#### 时空复杂度

时间复杂度：O(E*f) 。

设最大流为f，算法过程为穷尽增广路径的过程，最短路径可以保证在O(E)时间内找到(例如以DFS方式寻找，O(|V|+|E|))，而每找到一条增广路径，至少增加1个单位的流量(不考虑边权为无理数的情况，该情况可能会导致算法无法终止，见后续说明)，故总的时间复杂度为O(E*f)。此时间复杂度与最大流的大小正相关，当f较大时会导致较高的时间复杂度。若以BFS算法寻找增广路，即Edmonds-Karp算法，时间复杂度为O(|V||E|^2)；若以BFS和DFS相结合的Dinic's算法寻找增广路，则时间复杂度进一步降为O(|E||V|^2)。后续详细说明这两种改进算法。

空间复杂度：依赖于求最短路径的具体方法，若以BFS求最短路径，即Edmonds-Karp算法，则取决于队列的长度，O(|V|)。若以DFS求最短路径，则取决于递归深度，O(|V|)。

<br />

### Edmonds-Karp算法

#### 算法描述

[Edmonds-Karp算法(埃德蒙兹-卡普算法)](https://en.wikipedia.org/wiki/Edmonds%E2%80%93Karp_algorithm): 以BFS方式 (无权最短路径) 寻找增广路径实现Ford-Fulkerson方法即为Edmonds-Karp算法。每找到一条增广路并确定该路径上的最小边权(该路径最大可发送流)后，将此边权计入最大流中，并在此路径上对s>t方向的边减去该权值，t>s方向上加上该权值。当BFS无法再找到增广路时算法结束，得到s到t的最大流。



#### 算法过程

给定一张图G(V, E)，源点为s，汇点为t。求G中从s到t的最大流f。

1. 设置一张残留网络(残余图)Gf，Gf初始为G。
2. 以BFS方式考察Gf中是否有从s到t的路径p，使得p上的每一条边(u,v) ∈ p，都有c(u, v) > 0。p称作增广路径(Augmenting Path)。若存在p，则：
   2.1 将p中最小边的权c加入f。
   2.2 在Gf中p的每条边减去该最小边的权c。
   2.3 在Gf中p的反向路径上的每条边上加上该最小边权c。
3. 在Gf中反复寻找增广路径p并执行2中的操作直到Gf中找不到p，算法停止。此时得到的f即为s到t的最大流。



#### 时空复杂度

时间复杂度：O(|V||E|^2)

参考这两处的[证明1](http://pisces.ck.tp.edu.tw/~peng/index.php?action=showfile&file=f6cdf7ef750d7dc79c7d599b942acbaaee86a2e3e)、[证明2](https://brilliant.org/wiki/edmonds-karp-algorithm/)，说明如下。

a) EK算法中一次BFS寻找增广路的时间复杂度为O(|E|)。

每次BFS增广，在增广路上的未饱和边会多出一条反向边，故增广操作导致的边数增长不会使总边数超过原来的2倍，即算法的任意时刻总边数< 2|E|。一次BFS的时间复杂为O(2|E|)，即O(|E|)。详细可参考利用队列的无权最短路径复杂度分析。

b) BFS寻找增广路的次数的时间复杂度为O(|V||E|)。

b-1)首先证明EK算法通过BFS寻找的增广路长度非递减。

也就是要证明算法过程中的s到t的增广操作，即正向边删除和反向边增加的操作，不会导致源点s到任意一点v的最短路径距离减少。残留图G<sub>f</sub>经过一次BFS增广变为G<sub>f'</sub>后，对任意顶点v，源点到v的最短路径距离不会变小，即有d'(s, v) ≥ d(s, v)。以下利用反证法证明。

b-1-1) 假设某一次s-t的增广后，使得某些顶点到源点s的最短路径距离相比增广前变小了，且这其中距离源点s距离最近者为v。则根据该假设有

d'(s, v) < d(s, v)       ①　

b-1-2) 令u为G<sub>f'</sub>中v的前一个顶点，则有

d'(s, u) + 1= d'(s, v)         ②

如b-1-1)所述，v是我们有意选择的在G<sub>f'</sub>中最短距离相比在G<sub>f</sub>中变小的且距离s最近的顶点，u比v更靠近s，但在G<sub>f'</sub>中相比在G<sub>f</sub>中距离s的最短路径未变小，即有

d(s, u) ≤ d'(s, u)        ③

b-1-3) 假设(u, v) ∈ E<sub>f</sub> ，已知s到v的最短路径距离为d(s, v)，则有

d(s, u) + 1 = d(s, v)        ④

结合①、②、③有 d(s, u) ≤ d'(s, u) → d(s, u) + 1 ≤ d'(s, u)+ 1 = d'(s, v)  < d(s, v)，即

d(s, u) + 1 <  d(s, v)        ⑤

④是由b-3)的假设得到的，与由①、②、③得到的 ⑤矛盾，故在b-1)假设成立的前提下，b-3)的假设(u, v) ∈ E<sub>f</sub>不成立，即(u, v) ∉ E<sub>f</sub>。

b-1-4) 由上述知 (u, v) ∉ E<sub>f</sub>，但如b-2)所述，(u, v) ∈ E<sub>f'</sub>，故在G<sub>f</sub>上的增广经过了(v, u)，且此边饱和，导致在G<sub>f'</sub>中产生了反向边(u, v)。于是可知在G<sub>f</sub>中有

d(s, v) + 1 = d(s, u)        ⑥

与⑤矛盾，于是最初b-1)的假设不成立，即不存在这样的顶点v，也即增广操作使得源点到任意一点v的长度非递减，故EK算法寻找的增广路(s到t)长度非递减。

每次增广，在增广路上的未饱和边会多出一条反向边，故增广操作导致的边数增长不会使总边数超过原来的2倍，即|E'| < 2|E|。又知道增广路长度是非递减的，增广路上界是总边数 ( < 2|E| )，故任意一次BFS寻找增广路的时间复杂度总是O(|E|)。

b-2) 利用b-1)结论证明BFS寻找增广路的次数小于|E||V-1|次。

b-2-1) 定义增广路中的饱和边为关键边。某次增广中的(u, v)为关键边，增广后(u, v) ∉ E<sub>f'</sub>，(v, u) ∈ E<sub>f'</sub>。之后(u, v)要再次出现的前提是(v, u)为关键边。

在G<sub>f</sub>中 (u, v)第一次成为关键边，有

d(s, v) = d(s, u) + 1        ⑦

b-2-2) 若(u, v)第二次成为关键边，可知在此前的G<sub>f'</sub> 中(v, u)在成为关键边，在的G<sub>f'</sub> 的这次增广中有

d'(s, u) = d'(s, v) + 1        ⑧

 b-2-3) 由b-1)的结论，s到任意顶点的最短路径非递减，有d'(s, v)  ≥ d(s, v) ，结合⑦和⑧，有 d'(s, u) = d'(s, v) + 1 ≥ d(s, v) + 1 = d(s, u)+ 2，即

d'(s, u)   ≥ d(s, u) + 2        ⑨

也就是说，(u, v)第二次成为关键边时s到u的最短距离至少比前一次成为关键边时大2。而s到任意顶点的距离最多不超过|V| - 1，故(u, v)可以成为关键边的次数最多为(|V| - 1) / 2。每次增广至少有一条边成为关键边，根据b-1)中的说明，EK算法过程中边数< 2|E|，故考虑所有边的总的增广次数不会超过2|E|*(|V| - 1)/2，即增广次数复杂度为O(|V||E|)。

综上，EK算法复杂度为O(|V||E|^2)。

证明过程中BFS增广使得增广路非递减的结论是关键，网上有的文章声称BFS寻找增广路的操作使得增广路递增，这是不对的。根据b-1)的证明，只能得到**非递减**的结果，但这一结论在b-2)中足以证明任意边第二次成为关键边时其最短路径长至少增加2，由此得到BFS次数的上界。



空间复杂度：取决于每次BFS寻找增广路时使用的队列大小，为O(|V|)。



#### 代码

```java
public int maxFlowEK(Vertex<E> src, Vertex<E> des) {
    int incFlow = 0;
    int maxFlow = 0;
    // 不断寻找增广路径直到找不到 (== -1时)
    while((incFlow = augPathBFS(src, des)) != AUGMENTING_PATH_NOT_FOUND) {
        Vertex<E> current = des;
        // 每找到一条增广路径，从des顶点开始，向前对路径上每一条边(src > des方向)
        // 执行-inc操作，对其反向边执行+inc操作，直到到达src顶点为止
        while(current != src) {
            Vertex<E> pre = current.getPre();
            // 增广路方向上边权-1
            setEdgeValue(adjListsWeighted, pre, current, getEdgeValue(adjListsWeighted, pre, current) - incFlow); 
            // 逆增广路方向边权+1
            setEdgeValue(adjListsWeighted, current, pre, getEdgeValue(adjListsWeighted, current, pre) + incFlow);
            current = pre;
        }
        clearVertices(); // 重置所有顶点信息准备下一条增广路的寻找(主要是顶点访问标志)
        maxFlow += incFlow; // 将每次增广路带来的流量增加到maxFlow上
    }

    return maxFlow;
}

/**
 * bfs方式寻找增广路径，返回增广路径中最小权边的权值
 */
private int augPathBFS(Vertex<E> src, Vertex<E> des){
    int minEdge = Integer.MAX_VALUE;
    Queue<Vertex<E>> q = new LinkedList<>();
    q.add(src); // 起始顶点入队
    boolean isExist = false;

    while(!q.isEmpty()) {
        Vertex<E> v = q.remove();
        if(v == des) {
            isExist = true;
            break;
        }
        // 每次顶点v出队，考察其邻接顶点的距离
        for (Vertex<E> w : getAdjList(v)) {
            // 寻找未访问过且存在邻接顶点 (以是否有前驱节点作为是否访问过的标志，效果相当于!w.isKnown())
            if(w.getPre() == null && getEdgeValue(adjListsWeighted, v, w) > 0) {
                w.setPre(v);
                q.add(w); // w入队
                minEdge = Math.min(minEdge, getEdgeValue(adjListsWeighted, v, w)); // 更新增广路最小边权值
            }
        }
    }
    if(!isExist) {
        return AUGMENTING_PATH_NOT_FOUND;
    }

    return minEdge;
}
```



### Dinic's(Dinitz's)算法

#### 算法描述

[Dinic's算法(迪尼茨算法)](https://en.wikipedia.org/wiki/Dinic%27s_algorithm)：EK算法以BFS方式实现FF方法中的增广操作，在Dinic算法中，采用BFS和DFS结合的方式增广。首先引进高度标号(层次标号)和分层图的概念。以BFS算法从s到t，标记s为第1层，s的邻接顶点为第2层，以此类推t为最后一层。执行一次BFS，赋予每个顶点高度标号信息，此时的图称作分层图。在此分层图内以DFS方式反复寻找增广路并执行增广操作(找到饱和边，发送和发回流)，累计发送流。完成当前分层图的所有增广操作称作一个阶段。一个阶段结束后，重置高度标号信息，再次执行BFS得到新的分层图并重复DFS的增广操作，直到无法分层时说明s到t已无增广路，算法结束，此时得到的发送流总和即为s到t的最大流。

高度标号的作用：在以DFS增广时，每次从顶点v到其邻接顶点w的路径增长，都要考察w的的高度是否比v的高度大1，以此来保证路径总是能按步增长到最后一层的t(在有增广路的前提下)。



#### 算法过程

给定一张图G(V, E)，源点为s，汇点为t。求G中从s到t的最大流f。

1. 设置一张残留网络(残余图)Gf，Gf初始为G。
2. 调用BFS方法，赋予Gf中所有顶点以高度标号，建立当前分层图。注意在赋予高度时会先判断是否已赋过值，若有则跳过，因此顶点高度不会因为它处于多个层次而被较高的高度值覆盖，即一个顶点的高度是是它离源点最近的高度。此方法返回布尔值，有分层图(即有增广路)时返回true，否则返回false。
3. 求当前分层图发送流总和的方法(为方便叙述，称作主调函数f)，调用求单条增广路发送流的DFS方法搜索s到t的路径p，使得p上的每一条边(u,v) ∈ p，都有c(u, v) > 0。p称作增广路径(Augmenting Path)。DFS为一递归调用方法，返回值为本次增广路发送流。在方法中比较当前边与上一条边的边权，取较小者，并将此取值作为递归DFS方法的入参数(具体看代码)。若存在p，也即递归调用到基本情形(遇到t)，以当前边权减去c，当前边的反向边权加上c，然后返回c。
4. 在返回c的过程中完成增广路上的边权调整，返回到主调函数，以while(f.res > 0)判断，满足则将f.res加入当前分层图发送流总和的累积结果中。当前分层图完成所有增广后再递归调用DFS，由于已无增广路，故不会递进到基本情形(遇到t)，返回0且层层返回0，于是while(f.res > 0)不成立，返回当前分层图下的发送流总和。
5. 累计分层图发送流，清空所有顶点的高度标号信息，再次调用BFS方法建立当前分层图，重复2、3、 4，直到建立分层图的BFS方法返回false，表明当前图无法分层，即s到t无增广路，算法结束。此时得到的累积发送流为s到t的最大流。



#### 优化

上述朴素Dinic算法中，在当前分层图下，每次主程序(对单条增广路方法singleAugPathDfs来说的主程序是分层图增广路方法levelGraphAugPathDfsDrive)主调用DFS(singleAugPathDfs)找到一条增广路后，会一直return到本次主调用结束，层层返回本次增广路的发送流，并在每次返回时修改当前边的正反向边权。接着主程序基于`(singleAugFlow = singleAugPathDfs(des, src, singleAugFlow)) > 0`的判断再次调用singleAugPathDfs，又一次从src顶点开始搜索下一条到des的增广路。以下图为例，增广过程如下：

<div align=center> 
  <img src="https://tva1.sinaimg.cn/large/008i3skNgy1gvfw5jat8ij608a0aiaa502.jpg" width="150" />
</div>

第一次主调：s>a>c>t, 找到增广路后return，得到minEdge，修改当前边正向和反向边权后return，再次修改当前边的正向和反向边权后return(层层返回并如此操作)，t>c>a>s然后退出主调用，本次主调结束，得到本次增广路发送流为2。整体的顶点访问顺序为s>a>c>t>c>a>s。

第二次主调：s>a>c，发现(c, t)边权为0，不满足邻边边权要大于0的条件，找a的下一个满足层号关系的邻接顶点d，然后d>t，同上，找到增广路后在return过程中修改正反向边权，t>d>a>s后退出主调用，本次主调结束，得到本次增广路发送流为1。整体的顶点访问顺序为s>a>c>d>t>d>a>s。

第三次主调：从s开始，发现(s, a)边权为0，不满足邻边边权要大于0的条件，找s的下一个满足层号关系的邻接顶点b，然后b>d>t，同上，找到增广路后在return过程中修改正反向边权，t>d>b>s后退出主调用，本次调用结束，得到本次增广路发送流为2。整体的顶点访问顺序为s>b>d>t>d>b>s。

第四次主调：从s开始，for循环遍历所有邻接顶点均不满足邻边边权要大于0的条件，返回0 (`return 0;`)，于是主调方法的while也结束，得到最终结果maxFlow = 5.

注意：使用对DFS“主调”这个表述是为了区别于DFS自身的递归调用。

第一次主调详细递归过程如下，黑框表示levelGraphAugPathDfsDrive方法，蓝框表示singleAugPathDfs方法。如图所示，levelGraphAugPathDfsDrive对singleAugPathDfs的一次主调，在singleAugPathDfs的多次递归调用后，找到第一条增广路s>a>c>t，并得到该增广路的可发送流(大小为2)，将其加入multipleAugFlow中。

<div align=center> 
  <img src="https://tva1.sinaimg.cn/large/008i3skNgy1gxvvfct28bj30cs0cq0tt.jpg" width="510"/>
</div>

##### 优化: 当前弧优化

朴素Dinic算法中，假设源点src有多个邻接顶点A，B，C，D....。每次DFS都会从源点第一个邻接顶点A开始尝试增广，假设经过A有两条增广路。那么两次主调用DFS后，经过A的两条增广路都被找到，接着第3次主调用DFS，从A开始尝试增广，显然已无法经过A增广，于是尝试B，假设经过B有一条增广路，于是第3次主调用DFS的结果是找到经过B的一条增广路。接着第4次主调用DFS，此时仍然会从A开始尝试，到这里我们就可以发现，尽管经过A的增广路早已被穷尽，但每次对DFS的新的主调用，还是会从源点的第一个邻接顶点A开始尝试增广，这显然是无谓的开销。每次主调DFS时，不是从源点的第一个邻接顶点开始，而是从上一次主调所处理的那个源点的邻接顶点开始，就可以避免上述无谓的开销。如前述，经过3次对DFS的主调用，已经完成了经过A，B共3条增广路的寻找，在第4次主调用DFS时，不是从A开始，而是从B开始(注意不是C，因为要等到判断B.isBlocked = true才跳过)尝试，已无经过B的增广路(B.isBlocked = true)，于是寻找下一个邻接顶点C，执行经过C的增广路的寻找。下一次对DFS的主调用，从C开始(注意不是D)。这就是“当前弧优化”。



##### 优化: 分层图多路增广优化

通过上述对程序运行过程的分析，可以看到每次对DFS的主调，得到增广路后不断回退到主调结束，然后再利用下一次主调寻找下一条增广路。每一次主调寻找当前分层图下的一条增广路，如果在找到增广路后不是一路回退，而是在回退到上一个节点后继续探索其下一个满足条件的邻接顶点，那么可以期待一次主调就能完成当前分层图下所有增广路的寻找，得到当前分层图下s到t的最大流。做法如下：

1. 为每一个顶点维护一个可用流数据(flow)，源点s的可用流设置为Integer.MAX_VALUE。该属性表示顶点的可发送流。
2. 在分层图中以DFS搜索增广路的过程中，for循环考察当前顶点v的邻接顶点w，对每一个w，更新w.flow值，则w.flow = min(v.flow, edgeValue(v, w))。
3. 找到增广路时通过汇点的可用流得到该增广路的最小边权(饱和边权)singleAugFlow。注意，朴素版本在DFS增广过程维护一个增广路上的最小边权，增广完成后以其为该次增广的发送流，在本优化中，增广完成时，本次增广的发送流是当前顶点即汇点的可用流`des.flow`。在2中已经知道，每一个顶点的可用流都是在以它为当前顶点时赋予或更新的。
4. 得到当前增广路发送流currentFlow后回退，将其累积到分层图发送流incFlow上，并将当前顶点v的可用流中减去singleAugFlow，即v.flow -= currentFlow。之后可以加入一个即时判断优化，if(v.flow == 0)，若v的可用流为0，则不必再考察通往v的其他邻接顶点的增广路，直接跳出for循环。
4. for循环结束后，表明经过当前顶点v(假设其前驱顶点为u)的所有增广路已穷尽，于是将(u, v)边权减去incFlow，将(v, u)边权加上incFlow。
4. 当前分层图无增广路时，即递归调用的DFS返回到最外层并结束for循环时，返回的incFlow即为当前分层图下总发送流。
4. 累计分层图发送流，清空所有顶点的高度标号信息，再次调用BFS方法建立当前分层图，重复2、3、 4、5、6，直到建立分层图的BFS方法返回false，表明当前图无法分层，即s到t无增广路，算法结束。此时得到的累积发送流为s到t的最大流。

以下图为例分析展示应用了分层图多路增广优化的Dinic算法对初次建立的分层图的详细运行过程。

<div align=center> 
  <img src="https://tva1.sinaimg.cn/large/008i3skNgy1gy59ygvtc5j3035069jr8.jpg" width="100" />
</div>

每一个蓝框表示一次multipleAugPathDfs调用。可以看到最外层的对multipleAugPathDfs的一次主调用即可得到当前分层图下的最大流。此后无法再建立分层图，程序结束，得到正确的最大流为5。

<div align=center> 
  <img src="https://tva1.sinaimg.cn/large/008i3skNgy1gy59u61skuj31z40o5797.jpg" alt="dinic opt" width="1000" />
</div>

注意：当前弧优化是针对朴素Dinic算法来说的，如果已经实现了分层图多路增广优化，在当前分层图下，对DFS只需执行一次主调用，也就不存在当前弧优化应对的场景了。



#### 时空复杂度

时间复杂度：O(|E||V|^2)

如下证明参考了[COMPSCI 638: Graph Algorithms, Lecture3](https://courses.cs.duke.edu/fall19/compsci638/fall19_notes/lecture3.pdf)。

a) BFS建立分层图，可参考无权最短路径复杂度分析，为O(|E|)。

b) 在分层图中，由于高度标号加一的判断条件限制，DFS只能沿着高度递增的方向从s到t推进，因此单次DFS推进次数最多不超过|V|-1次，时间复杂度为O(|V|)。

c) 在分层图中，每次DFS增广的结果使得一条高度递增方向上的饱和边(u, v)消失，此边的反向边(v, u)增加。如前述，增广路只能沿着高度递增的方向，而分层图不变的情况下反向边是沿着高度递减方向增加的。(u, v)消失后再次出现的条件是(v, u)为此后某次增广路上的饱和边。再次强调，在同一个分层图内，(v, u)方向是高度递减的，不可能出现在增广路上，因此一条高度递增边(u, v)被删除后不会再次出现。一次增广至少令一条高度递增边饱和并删除，高度递增边数量不大于|E|，于是一个阶段内DFS的次数最多为|E|，即O(|E|)。结合b)可知，一个阶段的总复杂度为单次DFS的复杂度与DFS次数的乘积，即O(|V||E|)。

d) 分层图建立次数的复杂度, O(|V|)，详细证明如下。

如前述，在层高递增条件的限制下，s-t最短路径长度即t的层高，因此最短路径最大不超过|V| - 1。如果能证明每次分层图使得s-t最短路径长「严格」递增，则分层图建立上限为|V|，复杂度为O(|V|)。

令相邻的两次分层图为G<sub>f</sub>和G<sub>f'</sub>，以d(u, v)和d'(u, v)形式分别表示G<sub>f</sub>和G<sub>f'</sub>中的u到v的最短距离。

d-1) 由EK算法复杂度证明已知，删正向边加反向边的操作，使得源点s到任意一点u的距离是非递减的，即有d'(s, u) ≥ d(s, u)。实际对证明过程稍加改造很容易得到一个对称的结论，即删正向边加反向边的操作，使得任意一点u到汇点t的距离是非递减的，即有d'(u, t) ≥ d(u, t)。

d'(s, u) ≥ d(s, u)        ①

d'(u, t) ≥ d(u, t)        ②

对于s-t来说有d'(s, t) ≥ d(s, t)。接下来的证明目标是拿掉该不等式种的等号，证明G<sub>f'</sub>相比G<sub>f</sub>，有严格的d'(s, t) > d(s, t)。

d-2) 假设d'(s, t) ≥ d(s, t)可以取到等号。G<sub>f'</sub>中有一条s到t的最短路径f'，则一定存在边(x, y) ∈ E<sub>f'</sub>，是G<sub>f</sub> 某条增广路饱和边的反向边。因为如果E<sub>f'</sub>都是G<sub>f</sub>中存在的边，由于d'(s, t) = d(s, t)，这样的f'路径在G<sub>f</sub>中就一定已经被找到了。因此在G<sub>f</sub> 中有(y, x) ∈ E<sub>f</sub> ，有

d(s, x) > d(s, y)        ③ (实际上是d(s, x) = d(s, y) + 1)

<div align=center> 
  <img src="https://tva1.sinaimg.cn/large/008i3skNgy1gy9v7zlivej304q065mx1.jpg" alt="dinic proof graph" width="150" />
</div>

d-3) 由①和②易知

d'(s, x) ≥ d(s, x)        ④

d'(y, t) ≥ d(y, t)        ⑤

d-4) f'路径长可写成d'(s, t) = d'(s, x) + 1 + d'(y, t)，应用④和⑤得到

d'(s, t) = d'(s, x) + 1 + d'(y, t) ≥ d(s, x) + 1 + d(y, t) ，又由③得到

 d'(s, t) = d'(s, x) + 1 + d'(y, t) ≥ d(s, x) + 1 + d(y, t) > d(s, y) + d(y, t) + 1，即

d'(s, t) > d(s, t) + 1        ⑥

由此，我们证明了Dinic算法中下一分层图的最短路径长度与前一分层图相比严格递增。

实际上从⑥中还可以看出，每次递增的长度至少增加2。



综上，总时间复杂度为每个阶段建立分层图的复杂度O(|E|)与在该分层图内执行的总DFS复杂度O(|V||E|)之和乘以阶段数O(|V|)，为O((|E|+|V||E|)*|V|)，即O(|E||V|^2)。

空间复杂度：



#### 代码

##### 朴素版本

```java
public int maxFlowDinicBasic(Vertex<E> src, Vertex<E> des) {
    int maxFlow = 0;
    // 不断构建分层图，levelGraphBFS(src, dec)返回是否有增广路的布尔值
    while(levelGraphBFS(src, des)) {
        // 在一个确定有增广路的分层图中，求多条增广路可发送流的和，加入到最大流中
        maxFlow += levelGraphAugPathDfsDrive(src, des);
        // 当前分层图处理完毕后重置顶点，主要是层号信息
        clearVertices();
    }

    return maxFlow;
}
/**
 * bfs方式构建分层图，确定每个顶点的层号属性值(int level，取值0,1,2,3...)
 * 返回判断是否存在增广路径的布尔值
 */
private boolean levelGraphBFS(Vertex<E> src, Vertex<E> des){
    Queue<Vertex<E>> q = new LinkedList<>();
    src.setLevel(0);
    q.add(src); // 起始顶点入队
    boolean isExist = false; // 包含des的分层图是否存在，即当前图是否存在增广路

    while(!q.isEmpty()) {
        int levelSize = q.size(); // 获取当前队列大小，也即本层顶点个数
        for (int i = 0; i < levelSize; i++) { // 分配层号的关键
            Vertex<E> v = q.remove(); // 由于有levelSize的边界控制，本轮for只会将本层顶点出队
            // 每次顶点v出队
            for (Vertex<E> w : getAdjList(v)) {
                // 寻找未访问过且存在的邻接顶点，通过level标志判断是否访问过，所以不需要!isKnown条件
                if(w.getLevel() == -1 && getEdgeValue(adjListsWeighted, v, w) > 0) {
                    w.setLevel(v.getLevel() + 1);
                    q.add(w); // w入队
                }
            }
        }
    }
    if(des.getLevel() != -1) {
        return isExist = true;
    }

    return isExist;
}
/**
 * 求当前分层图下所有增广路径发送流的和
 */
private int levelGraphAugPathDfsDrive(Vertex<E> src, Vertex<E> des) {
    int singleAugFlow = Integer.MAX_VALUE;
    int multipleAugFlow = 0;
    // 将singleAugPathDfs方法换成singleAugPathDfsCurrentEdgeOpt应用当前弧优化
    // while((singleAugFlow = singleAugPathDfsCurrentEdgeOpt(des, src, singleAugFlow)) > 0) {
    while((singleAugFlow = singleAugPathDfs(des, src, singleAugFlow)) > 0) {
        multipleAugFlow += singleAugFlow;
    }
    return multipleAugFlow;
}
/**
 * 朴素Dinic算法未应用当前弧优化的版本
 */
private int singleAugPathDfs(Vertex<E> des, Vertex<E> current, int singleAugFlow){
    if(current == des) {
        return singleAugFlow;
    }
    for(Vertex<E> w : getAdjList(current)) {
        // 层号关系是关键 w.getLevel() == current.getLevel() + 1
        // 保证了每次dfs总是当前图src到des的最短路径
        if(w.getLevel() == current.getLevel() + 1 && getEdgeValue(adjListsWeighted, current, w) > 0) {
            int minEdge = singleAugPathDfs(des, w, Math.min(singleAugFlow, getEdgeValue(adjListsWeighted, current, w)));
            if(minEdge > 0) {
                setEdgeValue(adjListsWeighted, current, w, getEdgeValue(adjListsWeighted, current, w) - minEdge);
                setEdgeValue(adjListsWeighted, w, current, getEdgeValue(adjListsWeighted, w, current) + minEdge);
                return minEdge; 
            }
        }
    }

    // 在当前分层图下完成所有增广后，s已无到t的增广路，
    return 0;
}
```



##### 当前弧优化版本

```java
private int singleAugPathDfsCurrentEdgeOpt(Vertex<E> des, Vertex<E> current, int singleAugFlow){
    if(current == des) {
        return singleAugFlow;
    }
    boolean blockFlag = true;
    for(Vertex<E> w : getAdjList(current)) {
        // 层号关系是关键 w.getLevel() == current.getLevel() + 1
        // 保证了每次dfs总是当前图src到des的最短路径
        // 添加 !w.isBlocked() 条件实现当前弧优化
        if(!w.isBlocked() && w.getLevel() == current.getLevel() + 1 && getEdgeValue(adjListsWeighted, current, w) > 0) {
            blockFlag = false;
            int minEdge = singleAugPathDfsCurrentEdgeOpt(des, w, Math.min(singleAugFlow, getEdgeValue(adjListsWeighted, current, w)));
            if(minEdge > 0) {
                setEdgeValue(adjListsWeighted, current, w, getEdgeValue(adjListsWeighted, current, w) - minEdge);
                setEdgeValue(adjListsWeighted, w, current, getEdgeValue(adjListsWeighted, w, current) + minEdge);
                return minEdge; 
            }
        }
    }
    // 若current经过整个for都无法找到满足条件的邻接顶点，即blockFlag保持不变，则将current置为阻塞
    if(blockFlag) {
        current.setBlock(true);
    }

    return 0;
}
```



##### 分层图多路增广优化版本

```java
public int maxFlowDinic(Vertex<E> src, Vertex<E> des) {
    int maxFlow = 0;
    // 不断构建分层图，在一个确定有增广路的分层图中，求多条增广路可发送流的和，加入到最大流中
    while(levelGraphBFS(src, des)) {
        // 每次在当前分层图下多路增广前设置src的可用流
        src.setRemainFlow(Integer.MAX_VALUE); 
        maxFlow += multipleAugPathDfs(src, des);
        clearVertices(); // 每次结束当前分层图下的多路增广后重置顶点信息(层号信息)
    }

    return maxFlow;
}

/**
 * 以DFS方式查找当前分层图上所有增广路
 * 返回这些增广路可发送流之和
 */
private int multipleAugPathDfs(Vertex<E> current, Vertex<E> des) {
    int incFlow = 0;
    // 如果当前顶点是汇点，说明找到一条增广路，得到本条增广路的发送流量incFlow，
    // 调整des.pre和des正反向边权后返回incFlow
    if(current == des) {
        Vertex<E> v = current.getPre();
        incFlow = des.getRemainFlow();
        setEdgeValue(adjListsWeighted, v, current, getEdgeValue(adjListsWeighted, v, current) - incFlow);
        setEdgeValue(adjListsWeighted, current, v, getEdgeValue(adjListsWeighted, current, v) + incFlow);
        return incFlow;
    }
    // 考察当前顶点的邻接顶点，对满足条件的顶点执行dfs
    for (Vertex<E> w : getAdjList(current)) {
        if(w.getLevel() == current.getLevel() + 1 && getEdgeValue(adjListsWeighted, current, w) > 0) {
            w.setPre(current); // 将current记录为其邻接顶点w的前驱
            // 设置w的可用流为current的可用流和(current, w)边权中的较小者
            w.setRemainFlow(Math.min(current.getRemainFlow(), getEdgeValue(adjListsWeighted, current, w)));
            // dfs递归寻找增广路
            incFlow += multipleAugPathDfs(w, des);
            // 找到增广路回退后，对回退到的当前顶点，从它的可用流减中减去求当次增广路发送流incFlow
            current.setRemainFlow(current.getRemainFlow() - incFlow);;
            // 如下是一个高效优化，当前顶点的流减到0后，直接退出当前for循环，
            // 即由于当前顶点current已无剩余流可发送，也即不再存在经过current的其他增广路，所以不必再尝试它的其他邻接顶点
            if(current.getRemainFlow() == 0) {
                break;
            }
        }
    }
    // 上述for循环结束，说明当前顶点current的邻接顶点均已考察完毕，即已
    // 穷尽经过current节点的增广路，此时的incFlow就是这些增广路的可发送流量的和，
    // 用incFlow对(curPre, current)和(current, curPre)边权进行调整。
    Vertex<E> curPre = current.getPre();
    if(curPre != null) { // 注意当current为src时，其前驱为null
        setEdgeValue(adjListsWeighted, curPre, current, getEdgeValue(adjListsWeighted, curPre, current) - incFlow);
        setEdgeValue(adjListsWeighted, current, curPre, getEdgeValue(adjListsWeighted, current, curPre) + incFlow);
    }

    return incFlow;
}
```



### 最小费用流问题



### 二分图



## 最小生成树算法



### Prim算法

Prim算法与Dijkstra算法非常类似，在形式上仅有一行松弛操作不同，后续以表格对比形式给出该算法的描述。



#### 朴素版

##### 算法描述

| Dijkstra                                                     | Prim                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 贪婪算法                                                     | 贪婪算法                                                     |
| 算法将顶点区分为已确定到源点s最短路径的距离的顶点和未确定的顶点 | 算法将顶点区分为已经加入最小生成树的顶点和尚未加入的顶点     |
| 将所有顶点到源点s的距离初始化为Infinity<br />源点s的距离初始化为0 | 将所有顶点到父顶点的距离初始化为Infinity<br />生成树起始点s的距离初始化为0 |
| while判断是否还有未确定顶点，若有则以BFS的方式访问当前顶点v的所有邻接顶点w，以如下方式松弛dw，并更新w的前驱顶点为v<br />**dw = min(dw, dv + d(v, w))** | while判断是否还有未确定顶点，若有则以BFS的方式访问当前顶点v的所有邻接顶点w，以如下方式松弛，并更新w的前驱顶点为v<br />**dw = min(dw, d(v, w))** |
| 在具有有效距离(d != infinity)的未确定顶点中找到距离最小者作为当前顶点 | 在具有有效距离(d != infinity)的未确定顶点中找到距离最小者作为当前顶点 |
| 当前不存在未确定顶点时退出while，算法结束<br />取得所有顶点最短路径距离 | 当前不存在未确定顶点时退出while，算法结束<br />取得所有顶点在最小生成树上的距离 |



##### 算法过程

朴素版本过程如下：

1. 初始化。选定一个起始点s，初始化所有顶点到源点s的距离为Infinity，表示该点到父顶点的距离尚未确定（尚未确定选择哪条父顶点接入生成树），置s的距离为0。

2. 以while循环寻找当前距离未确定顶点中距离最小者。在当前具有有效距离(即不为无穷大)的距离未确定的顶点中找到距离最小者v，置v的距离为已确定。

3. 松弛操作。更新v的所有邻接顶点w的距离dw。有如下两种情况，因为在松弛前所有顶点的距离默认为Infinity，所以两种情形的形式一样，都是判断d(v, w) < dw。（若初始将顶点距离设置为负数则需要分开处理）

   3.1  若初次访问，dw有效化为dw = d(v, w)。更新w的前驱，w.pre = v。

   3.2. 若不是初次访问，当满足d(v, w) < dw时，更新dw，dw = dv + d(v, w)。更新w的前驱，w.pre = v。

4. 当所有顶点距离都确定时while循环结束，每个顶点的父顶点和到父顶点的距离被确定。

补充说明：

1) 第2步是该算法贪婪的体现，即每次while都确定一个顶点的最短路径，while结束时求得所有顶点的最短路径。
2) 第3步是BFS思想的体现。每次while确定一个顶点后，调整其邻接顶点的距离，即一次处理一层。
3) 针对第2步在当前距离未确定顶点中寻找距离最小值，可以采用优先队列(小顶堆)进行改进，具体实现见后续。
4) 与Dijkstra算法不同的是，Prim算法只比较所有可能的父边(到父顶点的边)的边权的大小，不受Dijkstra中路径上边权累计的影响，所以负值边和圈都不影响算法的正确性，Prim能够处理负边图和有圈图。



##### 时空复杂度

与Dijkstra算法相同。

时间复杂度：O(|E|+|V|^2)，由于|E| < |V|^2，所以也可以写为 O(|V|^2)。

1. 寻找拥有最小距离的顶点的时间为O(|V|^2)。 先遍历一遍顶点确认存在距离未确定的顶点O(|V|)，再遍历一遍顶点寻找未确定距离的顶点中距离最小者O(|V|)。

2. 所有顶点的距离被松弛的次数上限为O(|E|)。由算法可知顶点距离松弛只发生在找到当前距离未确定且距离最小的顶点之后，一次更新的次数是该顶点的邻接顶点数(即出边数，出度)。每一次循环松弛某一个顶点的所有出边。所有顶点的出边数即总边数|E|。故总松弛次数，也即所有顶点的距离被更新的次数上限为O(|E|)。

空间复杂度：算法过程中只用到常数个变量，不考虑顶点/边/图的数据结构本身消耗的空间时为O(1)。



##### 代码

```java
public List<Vertex<E>> primBasic(Vertex<E> s){
    // 按顶点确定的顺序输出，res最终包含所有顶点，且每个顶点的距离满足最小生成树
    List<Vertex<E>> res = new ArrayList<>(); 
    s.setDistance(0);

    // 遍历所有顶点确认是否有未访问过的顶点， O(|V|)
    while(checkUnknown(vertices)) { 
        // 从unknowns中取出距离最小的顶点， O(|V|)
        Vertex<E> v = getUnknownMin(vertices);
        v.setKnown(true); // 设置为距离已确定
        res.add(v); // 按顺序输出已确定距离的顶点
        // 松弛v的邻接顶点w，O(|E|)
        for (Vertex<E> w : getAdjList(v)) {
            // 松弛对象是未确定距离的w
            // 因为是无向图，以v的邻接顶点以邻接表实现，不需要边是否存在的判断条件 getEdgeValue(v, w) != 0
            if(!w.isKnown()) { 
                int dw = getEdgeValue(adjListsWeighted, v, w);
                if(dw < w.getDistance()) { // 松弛条件
                    w.setDistance(dw);
                    w.setPre(v); 
                }
            }
        }
    }

    return res;
}
```



#### 优先队列(堆)版

##### 算法描述

针对朴素版算法第2步在当前距离未确定顶点中寻找距离最小者，可以采用优先队列(小顶堆)进行改进，其他过程与朴素版一致。需要注意的是一个顶点的距离在被确定前可能经过多次松弛，算法在每次顶点距离松弛时将其入堆，于是同一时间，堆中可能有多个相同的顶点(松弛过几次就有几个)。这其中最靠顶的将会先出堆，出堆即表明该顶点距离已确定(或者说在此时确定)，所以顶点出堆时要判断是否是第一次出堆，若不是则跳过。



##### 算法过程

1. 初始化。选定一个起始点s，初始化所有顶点到s的距离为Infinity，表示该点到s的距离尚未确定，置s的距离为0。准备一个小顶堆pq，将s入堆。

2. 一次有效出堆完成一个顶点最短路径的确定。以while循环对pq判空，若不空，堆顶顶点v出堆。若v不是第一次出堆则跳过，若是则v为此时未确定距离的顶点中距离最小者，置v的距离为已确定。

3. 松弛操作。更新上述顶点v的所有邻接顶点w的距离dw。有如下两种情况，因为在松弛前所有顶点的距离默认为无穷大，所以两种情形的形式一样，都是判断是否有d(v, w) < dw。dw被松弛则将w入堆，以支持第2步(在当前具有有效距离的顶点中找到距离最小者)。

   3.1  如果是初次访问，满足d(v, w) < dw，w到s的距离dw有效化为dw = d(v, w)。

   3.2. 当w不是初次访问，当满足d(v, w) < dw时，更新dw，dw = d(v, w)。

4. 当堆中无元素即所有顶点距离都确定时while循环结束，每个顶点的父顶点和到父顶点的距离被确定。



##### 时空复杂度

时间复杂度：O(|E|log|E|+(|E| + |E|log|E|)，化简为O(|E|log|V|) ，化简依据后述。

1. 在当前距离未确定顶点中寻找距离最小者 O(|E|log|E|)。while中pq判空次数与堆大小有关，为O(|E|)，下述。获取距离最小者耗时也与堆大小有关，为O(log|E|)，故为O(|E|log|E|)。

   ※ 优先队列判空操作isEmpty()本身是常数时间操作，但总共要执行O(|E|)次。

2. 所有顶点的距离被更新(松弛)的次数上限为O(|E|)。由算法可知顶点距离的更新(或者说边的松弛)只发生在找到当前距离未确定且距离最小的顶点之后，一次更新的次数是该顶点的邻接顶点数(即出边数，出度)。每一次循环确定一个这样的顶点，也就是每一次循环松弛某一个顶点的所有出边。所有顶点的出边即总边数为|E|。故总松弛次数，也即所有顶点的距离被更新的次数，也即顶点入堆总次数为O(|E|)。dw更新时w入堆，插入操作的时间复杂度为O(log|E|)，入堆次数与更新次数相同，于是所有顶点的距离更新与顶点入堆的时间复杂度为O(|E| + |E|log|E|)，由于|E| <= |V|*(|V| - 1) ，故log|E| <= 2log|V|，故可化简为O(|E|log|V|)。

   ※ 根据上述顶点入堆次数取决于总边数的分析，堆的大小上限不是|V|而是|E|(或者说是O(|E|))，所以dw更新时w入堆的插入操作的时间复杂度为O(log|E|)，只是借助边数与顶点数的关系，可化简为O(log|V|)。

空间复杂度：不考虑用于返回的顶点列表时为O(1)。

※ 连通图顶点数与边数的关系：

无向连通图：|V| - 1 <= |E| <= |V|*(|V| - 1) / 2 

链状时|E|取到最小|V|-1，完全连通即两两相连时取到最大|V|*(|V| - 1) / 2 

有向连通图：|V| - 1 <= |E| <= |V|*(|V| - 1) 

链状时|E|取到最小|V|-1 (只差常数1)，完全连通即两两相连且正反向成对时取到最大|V|*(|V| - 1) 



##### 代码

```java
public List<Vertex<E>> primPQ1(Vertex<E> s){
    List<Vertex<E>> res = new ArrayList<>(); 
    s.setDistance(0);
    // 泛型PriorityQueue的写法，重写compare方法，比较元素getDistance()方法的结果，小顶堆
    PriorityQueue<Vertex<E>> pq = new PriorityQueue<>(new Comparator<Vertex<E>>() {
        @Override
        public int compare(Vertex<E> v1, Vertex<E> v2) {
            return v1.getDistance() - v2.getDistance();
        }
    });
    pq.add(s);

    while(!pq.isEmpty()) { // 判空 O(|E|)
        // 从unknowns中取出距离最小的顶点， O(log|E|)
        Vertex<E> v = pq.remove();
        // 当顶点v被更新过多次(多次入堆)，则存在v多次出堆的情况，
        // 跳过不是第一次出堆的顶点(v.isKnown = true)
        if(v.isKnown()) { 
            continue;
        }
        v.setKnown(true); // 置v的距离为已确定
        res.add(v); // 按顺序输出已确定距离的顶点
        // 松弛v的邻接顶点w，O(|E|log|E|)
        for (Vertex<E> w : getAdjList(v)) {
            if(!w.isKnown() && getEdgeValue(adjListsWeighted, v, w) != 0) { // 松弛对象是距离未确定的w
                // 将dw初次赋有效值或dw有更新时的w加入到pq中，以保证算法过程第2步总是
                // 能够在所有“具有有效距离值但距离未确定”的顶点中寻找距离最小者。
                // 注意dw有更新的情况下堆中可能有多个w(dw不同)，因此插入次数上限与更新次数上限相同，为O(|E|)。
                int dw = getEdgeValue(adjListsWeighted, v, w);
                if(dw < w.getDistance()) { // 松弛条件
                    w.setDistance(dw); // 更新dw
                    w.setPre(v); // 置w的前驱为v
                    pq.add(w); // 插入堆中，注意一个顶点的dw可能更新多次，O(log|E|)
                }
            }
        }
    }

    return res;
}

public int getMSTCost() {
    int cost = 0;
    for (Vertex<E> v : vertices) {
        cost += v.getDistance();
    }
    return cost;
}
```



### Kruskal算法



#### 算法描述



#### 算法过程



#### 时空复杂度



#### 代码



## 深度优先搜索



### 无向图



#### 判断无向图的连通性



#### 寻找无向图中的割点



#### 解决欧拉回路问题



### 有向图



#### 判断有向图是否无圈



#### 判断有向图是否强连通



## 广度优先搜索



## 应用

[^1]: https://www.zhihu.com/question/57206374 为什么Dijkstra算法每轮递推能够保证找到一个顶点的最短路径？