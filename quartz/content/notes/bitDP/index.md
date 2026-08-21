---
title: "bitDP"
date: 2022-10-30
modified: 2023-06-09
---
ビットで表現した集合を添え字に持つDPをbitDPという．

`dp[S] := 部分集合Sに対して，|S|!通りの順序の中から最適なものを選んだときの，何かしらの値`

遷移式は次のようなものが多い．
`dp[S ⋃ {v}] = dp[S] + cost(S, v)`

この集合に対するDPにより，n個の物に対して，n!通りの順序の中から最適なものを計算するのを高速化出来る場合がある．

**例1** 巡回セールスマン問題

```python
INF = float("inf")
n: int


def dist(u, v):
    """cost from u to v"""
    ...


dp = [[INF] * k for _ in range(1 << k)]
for u in range(n):
    dp[1 << u][u] = 0

for s in range(1 << n):
    for u in range(n):
        if not (1 << u) & s:
            continue
        for v in range(n):
            if (1 << v) & s:
                continue
            dp[s | (1 << v)][v] = min(dp[s | (1 << v)][v], dp[s][u] + dist(u, v))
```
