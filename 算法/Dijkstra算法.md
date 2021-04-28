# Dijkstra算法
<p align="right">2021年4月，时间结余。</p>

---
### 1.介绍

- 提出者：1959年，荷兰计算机科学家**狄克斯特拉** 。

- 中文名：迪杰斯特拉算法，迪克斯特拉算法，狄克斯特拉算法。

- 描述 ：从一个点到其余各个点的**最短路径**算法，解决的是**有权图**中最短路径问题。即从图中的某个点出发到达另外一个点所经过的边的权重之和最小的一条路径。从起点开始，遍历离起点距离最近且未访问过的点，然后将该点作为起点继续遍历，直到扩展到终点。

- 特点：
	1. 一次遍历能够得到从起点到遍历过的所有点的最短路径。
	2. 无方向扩散，直到遍历到目标点，效率不高。
	3. 无法处理权重有负值的情况。

---

### 2.食用

什么是有权图？

有权图对应的是无权图，这里的**权**指的是权重，权重可以理解为**代价**。现实中，代价可以指两个城市之间的距离，也可以指从一个城市去到另一个城市的花费，等等。比如：从西安到北京的距离是1100km，我们就可以表示为西安到北京的权重为1100。

无权图如下所示：

 <img src="..\pictures\no-weight.png" title="无权图" width="400px" height="200px">

有权图如下所示：

 <img src="..\pictures\weight.png" title="有权图" width="400px" height="200px">

以上面的有权图为例，从 **A** 到 **B** 的距离是 **5km**，从 **A** 到 **C** 的距离是 **1km**，以此类推。

问题来了，法外狂徒 张三 在 **A** 城市犯案，现在要逃往 **D** 城市躲起来，走哪条路线最近呢？

&nbsp;

张三 想到了dijkstra算法。

要求从 **A** 到 **D** 的最短路径，他需要知道2个东西：a)路径， b)总距离。

其中，路径 形如 A->B->D, 我们可以用一个字典dict存储每个点的爸爸。

比如：**parent_dict** = { A:None, B:A, D:B } 表示 **A** 的爸爸是None，即没有爸爸，它就是最开始被生活所迫的第一只鸡，以后才有了鸡蛋 **B** ... ，**B** 的爸爸是 **A** ，即 **B** 是从 **A** 走过来的，同理，**D** 是从 **B** 走过来的，所以想知道怎么去到 **D** , 我们就得先去 **B**， 怎么去 **B** ，我们得先去 **A** ，怎么去 **A** ，我们就在 **A**，于是，就有了路径 A->B->D。

于是，就有了自底向上的思想，从后往前解决问题。现实中，一旦用户有了问题，利益相关者想的总是解决提出问题的人，然后就没有问题了，这种自顶向下的思考方式是不对的。应该先解决自己的问题，再解决中间问题，最后自然而然就解决了用户的问题。

再次，总距离 怎么得到呢？我们还可以用一个字典dict存储从 **A** 点到该点的总距离。

比如：**distance_dict** = { A:0, B:5, C:1 } 表示 从 **A** 到 **A** 的总距离是 0，从 **A** 到 **B** 的总距离是 5，从 **A** 到 **C** 的总距离是 1，以此类推。

&nbsp;

所以，最后我们只需要2个结果，**parent_dict**和**distance_dict**。

于是，张三 开始模拟算法流程：

0）定义一个列表list存储未走到的城市，即**queue_list**；定义一个列表list(或集合set)存储已经走过的城市，防止走第2遍，即**visited_list**。这2个变量只是中间变量，我们可以不用太关心值的变化

1）初始化

| 变量          | 值         | 说明                            |
| :------------ | ---------- | --------------------------------|
| queue_list    | [ (A, 0) ] | 待访问的点是A, 起点A到点A的距离是0 |
| visited_list  | [ ]        | 已经走过的点，目前还没有          |
| **parent_dict** | { A:None } | 起点A没有爸爸                    |
| **distance_dict** | { A:0 }    | 起点A到A的距离是0                |

2）开始遍历。

 <img src="..\pictures\weight.png" title="有权图" width="400px" height="200px">

step1: 从queue_list里取出队首点(A, 0)， 将A加入到visited_list里表示走过了，即visited_list=[ A ]。

step2: 获取和A相连的点，即B,C。并计算从A到B和C的距离，即(B, 5)和(C, 1)。接下来做3个操作：

- 将2个点加入到queue_list，即queue_list=[ (B, 5), (C, 1) ] (**不准确**)

- 将2个点加入到parent_dict，即parent_dict={ A:None, B:A, C:A }

- 将2个点加入到distance_dict，即distance_dict={ A:0, B:5, C:1 }

**说明**:   调整queue_list，按距离从小到大排序，距离小的点放在队首，距离大的点放在队尾。为什么这样调整呢？我们每次遍历都希望从距离较短的点开始扩展，然后再去处理距离长的点，这样才能**保证遍历到的每个点都是从起点到该点的最短路径**。例如：如果从距离较长的点开始遍历，比如此时先遍历B再遍历C的话，因为每个点只会遍历一次，就导致A到B的最短距离就变成了5，这是不对的。那具体怎么实现这种调整呢？不用我们去考虑实现，每个语言都有**堆**这种数据结构，我们直接拿来用就行了。其实，在step2中将(B, 5)和(C, 1)加入queue_list时就已经自动调整好了 ，此时queue_list=[ (C, 1), (B, 5) ] (**准确**)

| 变量          | 值         | 说明                            |
| :------------ | ---------- | --------------------------------|
| queue_list    | [ (C, 1), (B, 5) ] | 用堆结构自动排好序 |
| visited_list  | [ A ]      | 已经走过的点，目前只有A |
| **parent_dict** | { A:None, B:A, C:A } | B,C点的爸爸**暂时**是A |
| **distance_dict** | { A:0, B:5, C:1 } | A到B和C点的距离**暂时**是5和1 |

step3: 继续从queue_list里取出队首点(C, 1)，将C加入到visited_list里表示走过了，即visited_list=[ A, C ]。

step4: 获取和C相连的点，即A,B,D,E。A已经走过了。计算从起点A到B,D,E的距离，怎么计算呢？我们是从A点出发的，目前在C点，要计算从A到B的距离，即 `A到C的距离 + C到B的距离 = distance_dict['C'] + 2 = 3`。即(B, 3)，同理，(D, 5), (E,9)。接下来做3个操作：

- 将3个点加入到queue_list，即queue_list=[ (B, 3), (B, 5), (D, 5), (E,9) ] (**准确**)
- 将3个点**更新**到parent_dict，即parent_dict={ A:None, B:C, C:A, D:C, E:C }
- 将3个点**更新**到distance_dict，即distance_dict={ A:0, B:3, C:1, D:5, E:9 }

**说明**：**更新**的依据是什么呢？依据是已经存在的，比较距离，选择短的更新，不存在的，直接插入。比如：B点已经在parent_dict和distance_dict中了，但是此时的B的距离3，比已有的5小，需要更新为3，而D,E不在其中，直接插入即可。

| 变量              | 值                                | 说明               |
| :---------------- | --------------------------------- | ------------------ |
| queue_list        | [ (B, 3), (B, 5), (D, 5), (E,9) ] | 用堆结构自动排好序 |
| visited_list      | [ A, C ]                          | 已经走过的点，A，C |
| **parent_dict**   | { A:None, B:C, C:A, D:C, E:C }    | **暂时**的结果     |
| **distance_dict** | { A:0, B:3, C:1, D:5, E:9 }       | **暂时**的结果     |

step5: 继续从queue_list里取出队首点(B, 3)， 以此类推，直到遍历完所有点自动结束，或判断到达目的地中断结束。

---

### 3.代码

```
#引入优先队列，自动调整优先级，将距离较短的放在队首，距离较长的放在队尾。
from queue import PriorityQueue

#定义函数，初始化起点到所有点的距离为无穷大，后面只需替换这个值即可。
def init_distence(graph, s):
    distance_dict = {}
    for vert in graph:
        distance_dict[vert] = float('inf') if vert != s else 0
    return distance_dict

#主函数。
def dijkstra(graph, s):
    #初始化2个中间变量和2个结果变量。
    queue_list = PriorityQueue()
    queue_list.put((0,s))
    visited_list = set()
    parent_dict = {s:None}
    distance_dict = init_distence(graph, s)

    #遍历队列，直到队列为空，表示所有点都遍历结束。
    #如需遍历到目的点结束，在代码中加个if判断和break跳出循环即可。
    while queue_list.qsize() > 0:
        #取出队首点。
        dis, point = queue_list.get()
        #如果该点没有访问过。
        if point not in visited_list:
            visited_list.add(point)
            #从图中取出当前点所有的邻接点，开始循环这些点。
            nodes = graph[point]
            for node in nodes:
                if node not in visited_list:
                    #计算从起点经过当前点到达邻接点的距离。
                    #如果该距离小于已保存的距离，即代表找到了一条更短的路径，
                    #更新父亲点和这个更短的距离。
                    now_dist = dis + graph[point][node]
                    if now_dist < distance_dict[node]:
                        queue_list.put((now_dist,node))
                        parent_dict[node] = point
                        distance_dict[node] = now_dist
    return parent_dict, distance_dict

if __name__=='__main__':    # **程序入口**
    #将有权图表示为字典结构。
    #比如：和A相连的点是B和C,距离为5和1,即可表示为'A':{'B':5,'C':1}。
    graph = {
        'A':{'B':5,'C':1},
        'B':{'A':5,'C':2,'D':1},
        'C':{'A':1,'B':2,'D':4,'E':8},
        'D':{'B':1,'C':4,'E':3,'F':6},
        'E':{'C':8,'D':3},
        'F':{'D':6}
        }
    #调用主函数，得到2个结果。
    parent, distence = dijkstra(graph, 'A')
    print(parent)
    print(distence)
```

---

### 附录

1. B站 [从BFS到Dijkstra算法](https://www.bilibili.com/video/BV1ts41157Sy/?spm_id_from=333.788.recommend_more_video.-1)