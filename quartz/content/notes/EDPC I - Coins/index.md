---
title: "EDPC I - Coins"
date: 2022-10-22
modified: 2022-10-22
tags:
  - "DP"
  - "EDPC"
---
https://atcoder.jp/contests/dp/tasks/dp_i

## 考え方

確率に関するDPでは，求める確率の事象を細かく分解すれば上手く漸化式が立つ可能性がある．

## 配列

`dp[i][j]` := `i`番目までコインを投げて，表がちょうど`j`枚出る確率

## 遷移

`dp[i][j] = dp[i-1][j] * (1 - p[j]) + df[i-1][j-1] * p[j]`
- `i-1`枚目までで表が`j`回出た→`j`枚目は裏
- `i-1`枚目までで表が`j-1`回出た→`j`枚目は表

```python
n = int(input())
p = list(map(float, input().split()))

dp = [[0 for _ in range(n + 1)] for _ in range(n + 1)]
dp[0][0] = 1

for i in range(n):
    for j in range(n + 1):
        dp[i + 1][j] += dp[i][j] * (1 - p[i])
        dp[i + 1][j] += dp[i][j - 1] * p[i] * (j > 0)

print(sum(dp[-1][n // 2 + 1 :]))
```

#EDPC #DP
