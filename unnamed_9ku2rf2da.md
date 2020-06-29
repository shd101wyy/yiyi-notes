---
tags:
  - algorithm
created: 2020-05-19T09:52:09.508Z
modified: 2020-05-19T09:53:06.882Z
---

# Dijkstra algorithm

[Source](https://www.cnblogs.com/biyeymyhjob/archive/2012/07/31/2615833.html "Permalink to 最短路径—Dijkstra算法和Floyd算法 - as_ - 博客园")

**注意：以下代码 只是描述思路，没有测试过！！**

**Dijkstra 算法**

1.定义概览

Dijkstra(迪杰斯特拉)算法是典型的单源最短路径算法，用于计算一个节点到其他所有节点的最短路径。主要特点是以起始点为中心向外层层扩展，直到扩展到终点为止。Dijkstra 算法是很有代表性的最短路径算法，在很多专业课程中都作为基本内容有详细的介绍，如数据结构，图论，运筹学等等。注意该算法要求图中不存在负权边。

问题描述：在无向图 G=(V,E) 中，假设每条边 E[i] 的长度为 w[i]，找到由顶点 V0 到其余各点的最短路径。（单源最短路径）

2.算法描述

1)算法思想：设 G=(V,E)是一个带权有向图，把图中顶点集合 V 分成两组，第一组为已求出最短路径的顶点集合（用 S 表示，初始时 S 中只有一个源点，以后每求得一条最短路径 , 就将加入到集合 S 中，直到全部顶点都加入到 S 中，算法就结束了），第二组为其余未确定最短路径的顶点集合（用 U 表示），按最短路径长度的递增次序依次把第二组的顶点加入 S 中。在加入的过程中，总保持从源点 v 到 S 中各顶点的最短路径长度不大于从源点 v 到 U 中任何顶点的最短路径长度。此外，每个顶点对应一个距离，S 中的顶点的距离就是从 v 到此顶点的最短路径长度，U 中的顶点的距离，是从 v 到此顶点只包括 S 中的顶点为中间顶点的当前最短路径长度。

2)算法步骤：

a.初始时，S 只包含源点，即 S ＝{v}，v 的距离为 0。U 包含除 v 外的其他顶点，即:U={其余顶点}，若 v 与 U 中顶点 u 有边，则\<u,v\>正常有权值，若 u 不是 v 的出边邻接点，则\<u,v\>权值为 ∞。

b.从 U 中选取一个距离 v 最小的顶点 k，把 k，加入 S 中（该选定的距离就是 v 到 k 的最短路径长度）。

c.以 k 为新考虑的中间点，修改 U 中各顶点的距离；若从源点 v 到顶点 u 的距离（经过顶点 k）比原来距离（不经过顶点 k）短，则修改顶点 u 的距离值，修改后的距离值的顶点 k 的距离加上边上的权。

d.重复步骤 b 和 c 直到所有顶点都包含在 S 中。

执行动画过程如下图

![](https://pic002.cnblogs.com/images/2012/426620/2012073019540660.gif)

3.算法代码实现：

    const int  MAXINT = 32767;
    const int MAXNUM = 10;
    int dist[MAXNUM];
    int prev[MAXNUM];

    int A[MAXUNM][MAXNUM];

    void Dijkstra(int v0)
    {
      　　bool S[MAXNUM];                                  // 判断是否已存入该点到S集合中
          int n=MAXNUM;
      　　for(int i=1; i<=n; ++i)
     　　 {
          　　dist[i] = A[v0][i];
          　　S[i] = false;                                // 初始都未用过该点
          　　if(dist[i] == MAXINT)
                　　prev[i] = -1;
     　　     else
                　　prev[i] = v0;
       　　}
       　 dist[v0] = 0;
       　 S[v0] = true; 　　
     　　 for(int i=2; i<=n; i++)
     　　 {
           　　int mindist = MAXINT;
           　　int u = v0; 　　                            // 找出当前未使用的点j的dist[j]最小值
          　　 for(int j=1; j<=n; ++j)
          　　    if((!S[j]) && dist[j]<mindist)
          　　    {
             　　       u = j;                             // u保存当前邻接点中距离最小的点的号码
             　 　      mindist = dist[j];
           　　   }
           　　S[u] = true;
           　　for(int j=1; j<=n; j++)
           　　    if((!S[j]) && A[u][j]<MAXINT)
           　　    {
               　    　if(dist[u] + A[u][j] < dist[j])     //在通过新加入的u点路径找到离v0点更短的路径
               　    　{
                       　　dist[j] = dist[u] + A[u][j];    //更新dist
                       　　prev[j] = u;                    //记录前驱顶点
                　　    }
            　    　}
       　　}
    }

4.算法实例

先给出一个无向图

![](https://pic002.cnblogs.com/images/2012/426620/2012073019593375.jpg)

用 Dijkstra 算法找出以 A 为起点的单源最短路径步骤如下

![](https://pic002.cnblogs.com/images/2012/426620/2012073020014941.jpg)
