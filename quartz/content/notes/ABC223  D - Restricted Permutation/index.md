---
title: "ABC223  D - Restricted Permutation"
date: 2023-01-02
modified: 2023-01-02
tags:
  - "ABC"
  - "D"
  - "トポロジカルソート"
---
https://atcoder.jp/contests/abc223/tasks/abc223_d
緑上位．

入力として与えられる$A_{i}, B_{i}$は有向辺$E(A_{i}, B_{i})$に対応する．
閉路が存在する場合は`-1`を出力，存在しない場合はトポロジカル順序を出力すればよい．

但し辞書順最小を満たすために，最小の頂点から順に出力する必要がある．双方向キューの代わりに優先度付きキューを用いること．

```python
import collections
import heapq

n, m = map(int, input().split())

graph = collections.defaultdict(list)
indegree = [0] * n

for _ in range(m):
    a, b = map(lambda x: int(x) - 1, input().split())
    graph[a].append(b)
    indegree[b] += 1

que = list()

for vertex in range(n):
    if indegree[vertex] == 0:
        heapq.heappush(que, vertex)

ans = []
while que:
    now = heapq.heappop(que)
    ans.append(now + 1)
    for nex in graph[now]:
        indegree[nex] -= 1
        if indegree[nex] == 0:
            heapq.heappush(que, nex)

if any(indegree):
    print(-1)
else:
    print(*ans)
```

#トポロジカルソート #ABC #D
