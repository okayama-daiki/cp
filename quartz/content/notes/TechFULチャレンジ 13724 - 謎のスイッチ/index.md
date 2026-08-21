---
title: "TechFULチャレンジ 13724 - 謎のスイッチ"
date: 2023-01-26
modified: 2023-01-26
tags:
  - "DP"
  - "bitDP"
---
https://techful-programming.com/user/skill/problem/coding/13724

$N < 16$よりBitDPであると推測．

`dp[S] := (bit_count(S)回) 押したスイッチがSである状態で，獲得できるスコアの最大値`
`(init) dp[:] = -INF`
`(init) dp[0] = 0`
`(relax) dp[S ⋃ i] = max(dp[S] + a[bit_count(S) + 1][i] | i in V \ S)`
`(ans) max(dp[:])`

$S(i) < S(j) \text{のとき，}i < j$となる性質を利用して，順方向に**配るDP**で遷移を行った．

bit演算のテクニックが沢山出てきたのでメモ．

1. $V - S$： `((1 << n) - 1) & ~s` （最大要素数が`n`）
2. $u$番目のスイッチを押す：`s | 1 << u`
- ここで`s | (u + 1)`や`s | u + 1`とすることは間違いであるので注意

bit演算には関係ないが，`int.bit_count()`は`bin(s).count('1')`で簡潔に書ける．

```python
from __future__ import annotations
from typing import Generator


def bitand(bit: int) -> Generator[int]:
    """
    下位ビットから順に1のインデックスを返します．
    >>> list(bitand(10)) # bin(10) -> 0b1010
    [1, 3]
    """
    for shift in range(bit.bit_length()):
        if bit >> shift & 1:
            yield shift


def bitcount(bit: int) -> int:
    """
    1bitの数を返します．
    """
    return bin(bit).count("1")


INF = float("inf")

n = int(input())
a = [list(map(int, input().split())) for _ in range(n)]


dp = [-INF for _ in range((1 << n) + 1)]
dp[0] = 0

for s in range(1 << n):
    for u in bitand(((1 << n) - 1) & ~s):
        dp[s | 1 << u] = max(dp[s | 1 << u], dp[s] + a[u][bitcount(s)])

print(max(dp))
```

#bitDP #DP
