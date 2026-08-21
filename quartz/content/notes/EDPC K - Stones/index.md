---
title: "EDPC K - Stones"
date: 2022-10-22
modified: 2023-01-26
tags:
  - "DP"
  - "EDPC"
---
https://atcoder.jp/contests/dp/tasks/dp_k

類題：https://techful-programming.com/user/skill/problem/coding/12217

## 考え方

残りの石の個数によって勝敗は決定しそう．
先行視点と後攻視点を一緒に考えるとややこしいので，先行の視点に立って考えよう．

## DPテーブル

`dp[i]` := 石の残り個数が`i`個のとき，先手が勝つかどうか

## 遷移

`dp[0] = False`(負け)
`dp[i] = any(not dp[i - a])`
-> i番目から遷移出来る手の中に，負け手(False)一つでもあれば勝ち(True)
-> i番目から遷移出来る手が全て，勝ち手(True)なら負け => 相手に勝ち手を渡すということ

- 後手からみると，いずれかの`i - a_i`が`False`であればそれを選ぶことで勝ちとなる
- 先手からみると，`i - a_i`がすべて`True`であれば勝ちとなる（`i - a_i`が一つでも`False`となると，後手はその手を必ず取るため）

```python
n, k = map(int, input().split())
a = list(map(int, input().split()))

dp = [None for _ in range(k + 1)]
dp[0] = False

for i in range(1, k + 1):
    dp[i] = any(not dp[i - e] for e in a if 0 <= i - e <= k)

if dp[-1]:
    print("First")
else:
    print("Second")
```

```python
# メモ化再帰
import sys
import functools

sys.setrecursionlimit(10**7)

n, k = map(int, input().split())
a = list(map(int, input().split()))


@functools.lru_cache(maxsize=None)
def dp(i):
    if i == 0:
        return False
    return any(not dp(i - e) for e in a if i - e >= 0)


print("First" if dp(k) else "Second")
```

#EDPC #DP
