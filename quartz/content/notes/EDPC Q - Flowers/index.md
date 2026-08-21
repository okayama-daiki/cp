---
title: "EDPC Q - Flowers"
date: 2023-01-25
modified: 2023-01-25
tags:
  - "DP"
  - "EDPC"
  - "データ構造で高速化"
---
https://atcoder.jp/contests/dp/tasks/dp_q

`dp[i] := i番目の花を残すとき，iより左の花の美しさの和の最大値`
`(initialize) dp[:] = -INF, dp[0] = 0`
`dp[i] = max(dp[j] | j < i, h[j] < h[i]) + a[i]`

`h[i]`の小さい順から埋めておくと，`dp[i]`を考える際，自分より左の方の最大値を取ってくるだけでよくなる．

```python
from atcoder.segtree import SegTree


INF = float("inf")

n = int(input())
h = list(map(int, input().split()))
a = list(map(int, input().split()))


dp = SegTree(max, -INF, n)
dp.set(0, 0)

for i in sorted(range(n), key=lambda i: h[i]):
    dp.set(i, dp.prod(0, i + 1) + a[i])

print(dp.all_prod())
```

#データ構造で高速化 #DP #EDPC
