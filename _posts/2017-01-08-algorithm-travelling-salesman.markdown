---
layout: post
category: "python"
title:  "旅行商问题"
tags: [python, algorithm]
---

- TOC
{:toc}

---

### 问题背景

描述：给定一系列城市和每对城市之间的距离，求解访问每一座城市一次并回到起始城市的最短回路。

![travelling_salesman_problem.png](https://i.loli.net/2020/03/05/1H3Eqo6NmxR2d7Y.png)

* travelling salesman problem, TSP，又称为最短路径问题
* 组合优化中的一个NP困难问题：再最坏的情况下，随着城市数量的增加，时间复杂度（n!）是指数级别的增长

---

### 借助itertools暴力破解

```python
import itertools

# 输入一般是各城市之间的距离
dis = [
    [0.00, 24.04, 68.37, 37.66, 58.81, 75.77, 65.20, 57.44, 59.37, 18.61],
    [24.04, 0.00, 89.58, 57.41, 82.04, 95.54, 59.86, 78.53, 73.57, 16.23],
    [68.37, 89.58, 0.00, 69.97, 18.91, 11.62, 86.73, 11.05, 34.42, 75.40],
    [37.66, 57.41, 69.97, 0.00, 52.75, 80.83, 101.03, 61.86, 78.96, 56.26],
    [58.81, 82.04, 18.91, 52.75, 0.00, 30.52, 92.05, 16.56, 45.24, 69.97],
    [75.77, 95.54, 11.62, 80.83, 30.52, 0.00, 85.08, 19.42, 31.47, 80.50],
    [65.20, 59.86, 86.73, 101.03, 92.05, 85.08, 0.00, 78.57, 53.61, 48.83],
    [57.44, 78.53, 11.05, 61.86, 16.56, 19.42, 78.57, 0.00, 28.99, 64.41],
    [59.37, 73.57, 34.42, 78.96, 45.24, 31.47, 53.61, 28.99, 0.00, 57.41],
    [18.61, 16.23, 75.40, 56.26, 69.97, 80.50, 48.83, 64.41, 57.41, 0.00],
]


def getDis(path):
    d = sum([dis[path[i]][path[i + 1]] for i in range(len(path) - 1)] + [dis[path[-1]][path[0]]])
    # print(path, path[-1], path[0], d)

    return d


min_dis = 111111111111
min_path = []
for path in itertools.permutations(range(len(dis))):
    # print(i)
    if min_dis >= getDis(path):
        min_dis = getDis(path)
        min_path.append(path)
        
for p in min_path:
    print(p)
print(min_dis)


(0, 1, 2, 3, 4, 5, 6, 7, 8, 9)
(0, 1, 2, 3, 4, 5, 6, 8, 7, 9)
(0, 1, 2, 3, 4, 5, 7, 6, 8, 9)
(0, 1, 2, 3, 4, 5, 7, 8, 6, 9)
(0, 1, 2, 3, 4, 7, 5, 8, 6, 9)
(0, 1, 2, 4, 3, 7, 5, 8, 6, 9)
(0, 1, 2, 4, 5, 7, 8, 6, 9, 3)
(0, 1, 2, 4, 7, 5, 8, 6, 9, 3)
(0, 1, 3, 2, 4, 5, 7, 8, 6, 9)
(0, 1, 3, 2, 4, 7, 5, 8, 6, 9)
(0, 1, 3, 4, 2, 5, 7, 8, 6, 9)
(0, 1, 3, 4, 7, 2, 5, 8, 6, 9)
(0, 1, 9, 6, 8, 2, 5, 7, 4, 3)
(0, 1, 9, 6, 8, 5, 2, 7, 4, 3)
(2, 5, 8, 6, 9, 1, 0, 3, 4, 7)
(3, 4, 7, 2, 5, 8, 6, 9, 1, 0)
(4, 7, 2, 5, 8, 6, 9, 1, 0, 3)
(5, 2, 7, 4, 3, 0, 1, 9, 6, 8)
303.81999999999994
```

---

### 参考

* [旅行推销员问题](https://zh.wikipedia.org/wiki/%E6%97%85%E8%A1%8C%E6%8E%A8%E9%94%80%E5%91%98%E9%97%AE%E9%A2%98)
* [Python TSP旅行商问题 暴力求解](https://www.codetd.com/article/1331148)