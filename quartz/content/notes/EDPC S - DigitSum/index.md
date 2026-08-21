---
title: "EDPC S - DigitSum"
date: 2023-01-27
modified: 2023-01-27
tags:
  - "EDPC"
  - "桁DP"
---
https://atcoder.jp/contests/dp/tasks/dp_s

**Tips**：$X$の上から$i$桁目を$X_i$とする．
$A<B$のとき，
$A_1=B_1$
$A_2=B_2$
$:$
$A_k=B_k$
$A_{k+1}<B_{k+1}$
となる$k\ge0$が一つ存在する．

題意の値を$K$以下とすると，決める値が$K$と等しい場合，各桁の数は対応する$K$の各桁の数と一致していなければならないが，一度その数より小さいものを経験すれば以降の桁では$0-9$の好きな数を利用してよいことになる．
そのため，$K$と同じかというフラグをDPテーブルに組み込む．

基本のDPテーブルとしては，次の形．
`dp[digit][is_smaller] := 上からdigit桁まで見た時に，Kより小さいか`
遷移方法としては3通り．
o `dp[digit + 1][True] = dp[digit][True]`  <-  既にKより小さいのだから，以降もずっと小さい
o `dp[digit + 1][True] = dp[digit][False]` <- これまでKと一致してきたが，`digit + 1`桁目でKより小さくなった
x `dp[digit + 1][False] = dp[digit][True]` <- これまでKより小さかったが，`digit + 1`桁目でKと一致した（有り得ない）
o `dp[digit + 1][False] = dp[digit][False]` <- 暫定的にKと一致している

擬似コード的には次の通り
```python
dp: list[list[int]]
for digit in range(Kの桁数):
    for is_smaller in True, False:  # 別にfor-loopにする必要はないが，DPの遷移っぽいので
        if is_smaller:  # Kより小さいことが確定
            for x in range(10):  # xは実際の値
                dp[digit + 1][(is_smaller := True)] = dp[digit][(is_smaller := True)]
        else:  # Kと暫定一致
            for x in range(Kのdigit桁目の値):
                dp[digit + 1][(is_smaller := True)] = dp[digit][(is_smaller := False)]
            ...
            x = Kのdigit桁目の値
            dp[digit + 1][(is_smaller := False)] = dp[digit][(is_smaller := False)]
```

`x`に関する処理は適宜追加する

[[hr.icon]]
今回は桁の総和のmod D別に個数を求めてやればいい．

```python
MOD = 10**9 + 7

k = input()


def int_k(i):
    return int(k[i])


d = int(input())

dp = [[[0] * d for _ in range(2)] for _ in range(len(k) + 1)]
# dp[digit][is_smaller][mod_d] := 上からdigit桁まで確定し，
# Kより小さいことが確定した状態がis_smallerで，
# 桁の総和をdで割った余りがmod_dであるものの個数
dp[0][0][0] = 1

for dig in range(len(k)):
    for is_smaller in True, False:
        for mod_d in range(d):
            if is_smaller:  # True
                for x in range(10):
                    dp[dig + 1][is_smaller][(mod_d + x) % d] += dp[dig][is_smaller][
                        mod_d
                    ]
                dp[dig + 1][is_smaller][(mod_d + x) % d] %= MOD
            else:  # False
                for x in range(int_k(dig)):
                    dp[dig + 1][not is_smaller][(mod_d + x) % d] += dp[dig][is_smaller][
                        mod_d
                    ]
                dp[dig + 1][not is_smaller][(mod_d + x) % d] %= MOD
                dp[dig + 1][is_smaller][(mod_d + int_k(dig)) % d] += dp[dig][
                    is_smaller
                ][mod_d]
                dp[dig + 1][is_smaller][(mod_d + int_k(dig)) % d] %= MOD

if not __debug__:
    print(*dp, sep="\n")

print((dp[len(k)][True][0] + dp[len(k)][False][0] - 1) % MOD)
```

#桁DP #EDPC
