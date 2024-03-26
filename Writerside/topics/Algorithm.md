# Algorithm

## LeetCode每日一题

## 2642. 设计可以求最短路径的图类

20240326
![2642](2642.png)
思路：

将图用邻接表表示，减少内存的使用
code
```Python
from typing import List

inf = 10 ** 9


def min_heapify(lb, start, end):
    dad = start
    son = dad * 2 + 1
    while son <= end:
        if son + 1 <= end and lb[son + 1][0] < lb[son][0]:
            son = son + 1
        if lb[dad][0] < lb[son][0]:
            break
        else:
            temp = lb[dad]
            lb[dad] = lb[son]
            lb[son] = temp
            dad = son
            son = dad * 2 + 1
    return lb


def pop(lb):
    temp = lb[0]
    lb = lb[1:]
    for i in range(int(len(lb) / 2) - 1, -1, -1):
        lb = min_heapify(lb, i, len(lb) - 1)
    return temp, lb


def push(lb, el):
    lb.append(el)
    for i in range(int(len(lb) / 2) - 1, -1, -1):
        lb = min_heapify(lb, i, len(lb) - 1)
    return lb


class Graph:

    def __init__(self, n: int, edges: List[List[int]]):
        table = [[] for _ in range(n)]
        for f, t, v in edges:
            table[f].append([t, v])
        self.table = table

    def addEdge(self, edge: List[int]) -> None:
        f, t, v = edge
        self.table[f].append([t, v])

    def shortestPath(self, node1: int, node2: int) -> int:
        dis = [inf] * len(self.table)
        dis[node1] = 0
        h = [[0, node1]]
        h = min_heapify(h, 0, len(h) - 1)
        while h:
            (d, x), h = pop(h)
            if x == node2:
                return d
            if d > dis[x]:
                continue
            for y, w in self.table[x]:
                if d + w < dis[y]:
                    dis[y] = d + w
                    h = push(h, [dis[y], y])
        return -1


if __name__ == '__main__':
    edge = [[7, 2, 131570], [9, 4, 622890], [9, 1, 812365], [1, 3, 399349], [10, 2, 407736], [6, 7, 880509],
            [1, 4, 289656], [8, 0, 802664], [6, 4, 826732], [10, 3, 567982], [5, 6, 434340], [4, 7, 833968],
            [12, 1, 578047], [8, 5, 739814], [10, 9, 648073], [1, 6, 679167], [3, 6, 933017], [0, 10, 399226],
            [1, 11, 915959], [0, 12, 393037], [11, 5, 811057], [6, 2, 100832], [5, 1, 731872], [3, 8, 741455],
            [2, 9, 835397], [7, 0, 516610], [11, 8, 680504], [3, 11, 455056], [1, 0, 252721]]
    graph = Graph(13, edge)
    print(edge)

    # print(graph.shortestPath(9, 3))
    graph.addEdge([11, 1, 873094])
    print(graph.shortestPath(3, 10))

```