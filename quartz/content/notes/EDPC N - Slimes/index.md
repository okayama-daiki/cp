---
title: "EDPC N - Slimes"
date: 2023-01-25
modified: 2023-01-25
tags:
  - "DP"
  - "区間DP"
---
https://atcoder.jp/contests/dp/tasks/dp_n

あるスライムはある範囲を一匹にしたものであるから，DPが使える．

`l`から`r`のスライムが1匹であるとき，その直前には，`l~m`のスライムと，`m~r`のスライムの2匹いた場合のみが考えられる（排反に場合分け）

`dp[l][r] := l以上r未満のスライムを1体にするときのコストの最小値`
`dp[i][i+1] = 0`
`dp[l][r] = min(dp[l][m] + dp[m][r] + l~mスライムとm~rスライムの合成コスト | l < m < r)`

`l~m`及び`m~r`スライムの大きさはそれぞれ`a_{l} ~ a_{m-1}`，`a_{m} ~ a_{r-1}`の和に等しく，合成コストは，事前に累積和を求めておくことで$O(1)$で計算が可能．

```python
# TLEします
import sys
import functools
import itertools

sys.setrecursionlimit(10**7)

n = int(input())
a = list(map(int, input().split()))

acc = list(itertools.accumulate(a, initial=1))


@functools.lru_cache(maxsize=None)
def dp(left, right):
    if not __debug__:
        print("called", left, right)
    if left + 1 == right:
        return 0
    return min(
        dp(left, middle) + dp(middle, right) + acc[right] - acc[left]
        for middle in range(left + 1, right)
    )


print(dp(0, n))
```

#区間DP #DP
