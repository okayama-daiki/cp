---
title: "ABC220 D - FG operation"
date: 2022-05-07
modified: 2022-05-07
tags:
  - "ABC"
  - "D"
  - "DP"
---
https://atcoder.jp/contests/abc220/tasks/abc220_d

茶色上位．DP．

まず問題を見て，「操作を行うことにより，数列が右端から左に向かって小さくなっていっている」ということに気付く必要がある．ここでDPかなという予想がつく．

また今までになかったのが，DPテーブルの更新時に数列Aの情報を必要とすることである．
その情報をもとに遷移先を決定する．

```python
MOD = 998244353
n = int(input())
a = list(map(int, input().split()))

dp = [[0 for _ in range(10)] for _ in range(len(a))]
dp[0][a[0]] += 1
for col in range(len(a) - 1):
    for row in range(10):
        dp[col + 1][(row * a[col + 1]) % 10] += dp[col][row]
        dp[col + 1][(row * a[col + 1]) % 10] %= MOD
        dp[col + 1][(row + a[col + 1]) % 10] += dp[col][row]
        dp[col + 1][(row + a[col + 1]) % 10] %= MOD
print(*dp[-1], sep="\n")
```

例のごとくMODを取り忘れて，TLE＆WAの嵐だったので注意．

#ABC #D #DP
