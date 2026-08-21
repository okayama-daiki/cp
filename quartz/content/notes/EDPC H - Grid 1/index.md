---
title: "EDPC H - Grid 1"
date: 2022-10-22
modified: 2022-10-22
tags:
  - "DP"
  - "EDPC"
---
https://atcoder.jp/contests/dp/tasks/dp_h

DAG．

## DPテーブル

`dp[x][y]` := (x, y)から(H, W)への経路の個数

## 遷移

`dp[x][y] = dp[x+1][y] + dp[x][y+1]` (但し，(x, y)が 壁のときは`0`)

```python
MOD = 10**9 + 7
h, w = map(int, input().split())
a = [list(input()) for _ in range(h)]

dp = [[0 for _ in range(w)] for _ in range(h)]
dp[-1][-1] = 1

for i in range(h)[::-1]:
    for j in range(w)[::-1]:
        if a[i][j] == "#":
            dp[i][j] = 0
        dp[i][j] %= MOD
        if i > 0:
            dp[i - 1][j] += dp[i][j]
        if j > 0:
            dp[i][j - 1] += dp[i][j]

print(dp[0][0] % MOD)
```

上の遷移とはちょっと違う

Tips::
最大最小DPの時は**漏れなく**場合分け．
数え上げDPの時は**漏れなく & 重複なく**場合分け(排反)．

#EDPC #DP
