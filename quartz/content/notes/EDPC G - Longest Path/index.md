---
title: "EDPC G - Longest Path"
date: 2022-10-22
modified: 2022-10-22
tags:
  - "DP"
  - "EDPC"
---
https://atcoder.jp/contests/dp/tasks/dp_g

「Gは**有向閉路を含みません**」からグラフがDAGであると分かる．

## 考え方

頂点がn個なので，DPテーブルで頂点番号を添え字にしたい

## DPテーブル

`dp[x]` := xから始まる最長経路長

## DP遷移

`dp[x] = max(dp[y] + 1)` (但し，`x`から`y`への有向辺が存在する)
-> xから直接辺が伸びている頂点y全ての`dp[y]`のmaxをとる

Tips:: 次の状況下ではメモ化再帰が楽
1．漸化式は立った
2．良い遷移の順番は分からないけど，なんかありそう

`dp[x]`を関数化すればよい．

```python
import sys

sys.setrecursionlimit(10**6)

n, m = map(int, input().split())
graph = [[] for _ in range(n)]
for _ in range(m):
    x, y = map(int, input().split())
    graph[x - 1].append(y - 1)

memo = [None for _ in range(n)]


def dp(x):
    if memo[x] is not None:
        return memo[x]
    memo[x] = max((dp(y) for y in graph[x]), default=-1) + 1
    return memo[x]


print(max(dp(x) for x in range(n)))
```

なぜかPythonで提出するとTLEする．

#EDPC  #DP
