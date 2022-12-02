## 搜索

在求解一个问题时，要找到一个合适的表示方法。如何快速的搜索到合适的求解方法，是搜索要解决的问题。

### 搜索策略

1. 盲目搜索

   在问题不具有任何特征，任何有效消息的条件下，按固定的步骤进行的搜索。

2. 启发式搜索

   考虑特定的问题可以应用于相应的知识，动态的确定调用操作算子的步骤，尽量减少不必要的搜索。

### 状态空间表示方法

首先，在进行搜索之前，我们要定义一个状态空间。利用**状态变量**和**操作符号**，表示系统的符号体系，从而可以进行搜索操作。

- 状态

  表示系统状态，可以用一组变量来表示：Q = [q1, q2, ... , qn]

- 操作

  表示引起状态变化的一组函数： F = {f1, f2, ... , fm}

- 状态空间

  状态空间是一个四元组: (S, O, S0, G)

  - S: 状态集合
  - O: 操作算子集合
  - S0: 初始状态
  - G: 目的状态

- 求解路径

  从S0到G的路Introduction to artificial intelligence径

- 状态空间解

  一个有限的操作算子序列: {O1, O2, O3, ... , Ok}，根据这个序列，可以从S0到G。

### 状态空间图描述

用**有向图**来描述状态空间，节点表示状态，边表示求解步骤。初始节点为根节点。

例：设有2个修道士和2个野人来到河边，打算乘⼀条索道从河的左岸渡到河的右岸。但索道只有⼀个轿箱，每次只能装载1人，在任何岸边野人的数目都不得超过修道士的人数，否则修道士就会被野人吃掉。野人和修道士都服从你的过河安排。请问如何规划过河计划才能把所有人都安全地渡过河去。请画出状态空间图，并给出深度优先的搜索求解过程。

<img src="C:\Users\陈鸿煜\AppData\Roaming\Typora\typora-user-images\image-20220102161409811.png" alt="image-20220102161409811" style="zoom: 80%;" />

### 盲目搜索

#### 回溯策略

[五大常见算法策略之——回溯策略 - 头发是我最后的倔强 - 博客园 (cnblogs.com)](https://www.cnblogs.com/vfdxvffd/p/12484932.html)

回溯策略表示：在搜索过程中，如果遇到不可解节点就回溯到路径中最近的父节点，并查看该节点是否还有其他子节点未被扩展。若有，则沿这些子节点继续搜索；直到找到目标。

回溯策略有三张表：

1. PS(path state)表：保存当前搜索路径上的状态
2. NPS(new path state)表：新的路径状态表
3. NSS(no solvable states)表：不可解状态表

图搜索算法的回溯思想：

1. 用NPS来使算法可以回宿到任意一个当前未处理的状态
2. 用NSS来避免算法重复搜索
3. 用PS记录当前搜索路径的状态，当找到结果时可以将PS作为求解路径返回

#### 宽度优先搜索和深度优先搜索

太简单了，直接上代码

```python
# -*- coding: utf-8 -*-
 
from collections import deque    # 导入双向队列
 
# 首先定义一个创建图的类，使用邻接矩阵
class Graph(object):
    def __init__(self, *args, **kwargs):
        self.order = []  # visited order
        self.neighbor = {}
 
    def add_node(self, node):
        key, val = node
        if not isinstance(val, list):
            print('节点输入时应该为一个线性表')    # 避免不正确的输入
        self.neighbor[key] = val
 
    # 宽度优先算法的实现
    def BFS(self, root):
        #首先判断根节点是否为空节点
        if root != None:
            search_queue = deque()
            search_queue.append(root)
            visited = []
        else:
            print('root is None')
            return -1
 
        while search_queue:
            person = search_queue.popleft()
            self.order.append(person)
 
            if (not person in visited) and (person in self.neighbor.keys()):
                search_queue += self.neighbor[person]
                visited.append(person)
 
    # 深度优先算法的实现
    def DFS(self, root):
        # 首先判断根节点是否为空节点
        if root != None:
            search_queue = deque()
            search_queue.append(root)
 
            visited = []
        else:
            print('root is None')
            return -1
 
        while search_queue:
            person = search_queue.popleft()
            self.order.append(person)
 
            if (not person in visited) and (person in self.neighbor.keys()):
                tmp = self.neighbor[person]
                tmp.reverse()
 
                for index in tmp:
                    search_queue.appendleft(index)
 
                visited.append(person)
 
    def clear(self):
        self.order = []
 
    def node_print(self):
        for index in self.order:
            print(index, end='  ')
 
 
if __name__ == '__main__':
    # 创建一个二叉树图
    g = Graph()
    g.add_node(('A', ['B', 'C']))
    g.add_node(('B', ['D', 'E']))
    g.add_node(('C', ['F']))
 
    # 进行宽度优先搜索
    g.BFS('A')
    print('宽度优先搜索:')
    print('  ', end='  ')
    g.node_print()
    g.clear()
 
    # 进行深度优先搜索
    print('\n\n深度优先搜索:')
    print('  ', end='  ')
    g.DFS('A')
    g.node_print()
    print()

```

### 启发式搜索

- 启发式信息：用来简化搜索过程，有关具体问题的信息。
- 启发式图搜索策略的特点：重排OPEN表，选择最有希望的节点扩展

- 估价函数：估算节点”希望程度“的函数

  f(n) = g(n) + h(n)

  - g(n): 实际代价
  - h(n): 估计代价，称为启发函数

#### A搜索

使用估价函数f的最佳优先搜索。

- 最佳优先搜索：是一种启发式算法，基于宽度优先，每次遍历一整层，找到估价函数最小的节点，选择它，然后遍历它的子节点，计算估价函数，循环。

A搜索有两个表：

- OPEN:保留所有已生成而未扩展的状态
- CLOSED:记录已经扩展过的节点

#### A*搜索

A*搜索其实是A搜索的一种特殊情况

   一种具有f(n)=g(n)+h(n)策略的启发式算法能成为A*算法的充分条件是：
      1、搜索树上存在着从起始点到终了点的最优路径。
      2、问题域是有限的。
      3、所有结点的子结点的搜索代价值>0。
      4、h(n)=<h*(n) （h*(n)为实际问题的代价值）。


























