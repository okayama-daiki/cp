---
title: "部分列DP"
date: 2023-05-17
modified: 2023-05-17
---
参考：https://qiita.com/drken/items/a207e5ae3ea2cf17f4bd

```python
MOD = 10**9 + 7

s = "abcde"
n = len(s)

# nex[i][j] := k | a[k] == j, i < k
nex = [[n] * 26 for _ in range(n + 1)]

for i in reversed(range(n)):
    for j in range(26):
        nex[i][j] = nex[i + 1][j]
    nex[i][ord(s[i]) - ord("a")] = i

print(*nex, sep="\n")

# dp[i] := s[i]は必ず使うとしたとき，s[:i]を利用して作られる部分文字列の総数
dp = [0] * (n + 1)
dp[0] = 1

# 実際の要素
real_dp = [set() for _ in range(n + 1)]
real_dp[0].add("")

for i in range(n):
    for j in range(26):
        if nex[i][j] < n:
            dp[nex[i][j] + 1] += dp[i]
            dp[nex[i][j] + 1] %= MOD
            for e in real_dp[i]:
                real_dp[nex[i][j] + 1].add(e + chr(j + ord("a")))

print(*dp)
print(*real_dp, sep="\n")

print(sum(dp))
```
