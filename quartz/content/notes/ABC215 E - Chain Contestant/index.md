---
title: "ABC215 E - Chain Contestant"
date: 2023-05-16
modified: 2023-05-16
tags:
  - "ABC"
  - "E"
  - "bitDP"
---
https://atcoder.jp/contests/abc215/tasks/abc215_e
水色上位．bitDP．

DPテーブルは`dp[i][j][k] := s[:i]までで，利用した文字集合がj，最後の文字がkの場合の数`

遷移方法と初期化にポイントがある．

まず初期化に関して，いつものように`dp[0][0][0]=1`とすると意味が変わってくるので，**ここでは11番目のアルファベットを用意し**，その状態に対し1を与える．つまり，DPテーブルの`k`の添字分を一つ増やし，遷移時もそこからの遷移があるように（つまり最も内側のループをもう1回分増やす）処理すればよい，

また遷移を添字に集合を取るので少し慣れないが，各状態で考えるのは「コンテストに参加するか否か」であり，まず前者は「これまでに参加していない or **直前に参加**している」のいずれかから遷移があり，後者に関しては状態にかかわらず遷移がある．

よってこれらを適切に実装すればよい．

```python
MOD = 998244353
MAX_S = 10

n = int(input())
s = list(map(lambda x: ord(x) - ord("A"), input()))

# dp[i][j][k] := s[:i]までで，利用した文字集合がj，最後の文字がkの場合の数
dp = [[[0] * (MAX_S + 1) for _ in range(1 << MAX_S)] for _ in range(n + 1)]
dp[0][0][10] = 1

for i in range(n):
    for j in range(1 << MAX_S):
        for k in range(MAX_S + 1):
            if True:  # s[i]を選択しない
                dp[i + 1][j][k] += dp[i][j][k]
                dp[i + 1][j][k] %= MOD
            if not 1 << s[i] & j:  # s[i]を初選択
                dp[i + 1][j | 1 << s[i]][s[i]] += dp[i][j][k]
                dp[i + 1][j | 1 << s[i]][s[i]] %= MOD
            if 1 << s[i] & j and s[i] == k:  # s[i]を連続で選択
                dp[i + 1][j][s[i]] += dp[i][j][k]
                dp[i + 1][j][s[i]] %= MOD

# print(*dp, sep='\n')

ans = 0
for j in range(1 << MAX_S):
    for k in range(MAX_S):
        ans += dp[n][j][k]
        ans %= MOD

print(ans)
```

#ABC #E #bitDP
