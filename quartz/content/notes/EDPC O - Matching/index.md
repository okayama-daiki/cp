---
title: "EDPC O - Matching"
date: 2023-01-25
modified: 2023-01-25
tags:
  - "DP"
  - "bitDP"
---
https://atcoder.jp/contests/dp/tasks/dp_o

典型的なbitDP．
取り敢えず任意の男女でペアを作ることが出来ると考えると，男一人に対して任意の女，男二人に対して選ぶときの場合の数は，男一人の時に選ばれなかった女の中から任意の一人を選んだ場合の数の総和になる．これを繰り返すことで男N人に対する場合の数を求める事が出来る．
このとき，男を固定して考えてもよい（例えば番号の若い順番からペアを組ませる）ことがポイントで，番号1の男と任意の女のペアを組ませる，番号1-2の男と任意の女とペアを組ませる，番号1-3の男と...と組ませると女の方は集合を考えればよいという事になり，なんとなくbitDP感が出てくる．

男と女の相性については，ペアを組むときに相性が悪いならペアを組まないことにすればよいのでそこまで重要ではない．

`dp[S] := 左からbitcoutnt[S]人の男性と，女性の部分集合Sでマッチングさせる場合の数`
`dp[φ] = 1  # 0人取ってきてマッチングを組ませる => 何もペアがないという状態が1つある`
`dp[S] = sum(dp[S \ i] * a[bitcount(S)][i])`

```python
# TLEします
import sys
import functools

sys.setrecursionlimit(10**7)


MOD = 10**9 + 7

n = int(input())
a = [list(map(int, input().split())) for _ in range(n)]


@functools.lru_cache(maxsize=None)
def dp(s: int):
    if s == 0:
        return 1
    ret = 0
    bit_count = sum(s >> shift & 1 for shift in range(n))
    for shift in range(n):
        if (s >> shift) & 1 == 1:
            ret += dp(s & ~(1 << shift)) * a[bit_count - 1][shift] % MOD
    return ret % MOD


print(dp((1 << n) - 1) % MOD)
```

#bitDP #DP
