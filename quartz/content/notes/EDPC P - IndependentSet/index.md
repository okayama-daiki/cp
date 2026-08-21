---
title: "EDPC P - IndependentSet"
date: 2023-01-25
modified: 2023-01-25
tags:
  - "EDPC"
  - "木DP"
---
https://atcoder.jp/contests/dp/tasks/dp_p

木DP．
DFSの帰りがけ順にDPをするだけ．

`dp[v][WorB] := 頂点vを根とする部分木を塗るとき，頂点vをWhite or Blackに塗る場合の数`
`dp[leaf][:] = 1`
`dp[v][W] = (dp[u1][W] + dp[u1][B]) * (dp[u2][W] + dp[u2][B]) * ...`
`dp[v][B] = dp[u1][W] * dp[u2][W] * ...`
`(u1, u2, ...の親はv)`

木なので自分の好きなノードからDFSを行えばよい．

木におけるDFSはちょっとしたテクニックがあり，今までは`visited[n]`配列を容易して，同じ頂点を2度通らないようにしていたが，木の場合はDFSの引数に自身の親を一緒に渡すだけで良くなる．

```python
import sys

sys.setrecursionlimit(10**7)

MOD = 10**9 + 7

n = int(input())
graph = [[] for _ in range(n)]

for _ in range(n - 1):
    x, y = map(int, input().split())
    x -= 1
    y -= 1
    graph[x].append(y)
    graph[y].append(x)


dp = [[1, 1] for _ in range(n)]


def dfs(u, parent=-1):

    for v in graph[u]:
        if v == parent:
            continue
        dfs(v, u)
        dp[u][0] *= dp[v][0] + dp[v][1]
        dp[u][0] %= MOD
        dp[u][1] *= dp[v][0]
        dp[u][1] %= MOD


dfs(0)
print(sum(dp[0]) % MOD)
```

#木DP #EDPC
