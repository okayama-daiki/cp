---
title: "EDPC U - Grouping"
date: 2023-01-27
modified: 2023-01-27
tags:
  - "EDPC"
  - "bitDP"
---
https://atcoder.jp/contests/dp/tasks/dp_u
bitDP．

部分集合の部分集合に関する2重for文は$O(N^3)$であることが知られている．

$N\le16$ときたら$O(N^3)$アルゴリズムを疑おう

`dp[S] = うさぎの集合Sをグループ分けしたときのスコアの最大値`
`dp[φ] = 0`
`dp[S] = max(dp[S \ T] + score(T) | T ⊂ S)`

**POINT** `s`の部分集合`t`を効率的に列挙するテクニック
```python
def bit_subset(s: int):
    """
    sの部分集合tを効率的に列挙します．
    >>> print(*bit_subset(0b1101))
    0b1101, 0b1100, 0b1001, 0b1000, 0b0101, 0b0100, 0b0001
    """
    t = s
    while t > 0:
        yield t
        t = (t - 1) & s
```

```python
import sys
import functools

sys.setrecursionlimit(10**7)


INF = float("inf")

n = int(input())
a = [list(map(int, input().split())) for _ in range(n)]


cost = [0] * (1 << n)
for s in range((1 << n) + 1):
    for i in range(n):
        for j in range(i + 1, n):
            if s >> i & 1 and s >> j & 1:
                cost[s] += a[i][j]


@functools.lru_cache(maxsize=None)
def dp(s):
    if s == 0:
        return 0
    ret = -INF
    t = s
    while t > 0:
        ret = max(ret, dp(s & ~t) + cost[t])
        t = (t - 1) & s
    return ret


print(dp((1 << n) - 1))
```

#bitDP #EDPC
