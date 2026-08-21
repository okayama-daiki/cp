---
title: "ABC262 D - I Hate Non-integer Number"
date: 2022-12-29
modified: 2022-12-29
tags:
  - "ABD"
  - "D"
  - "DP"
---
水色下位

4重DP

当初のDPテーブルは
`dp[i][j][k] := a_1, ..., a_iの中でj(j <= i)個選択した時に，それらの和がk (mod j)となる選び方の個数`
であったが，これの問題点は
- `j`が増えた時に遷移を考えることが出来ない
  - 例えば，`4 + 1`個選択するときに，それまでに求めたものは全て`mod 4`の値である．（必要なのは`mod 5`の値）
そのため，`mod`を取る値を固定して考える必要がある．

考えるDPテーブルは
`dp[i][j][k][l] := Aの先頭j項からk個の項を選ぶ方法であって，選んだ項の総和をiで割った余りがlとなるようなものの個数`

遷移は，`a[j]` を取る・取らないの2通り．
`dp[i][n][i][0] (i = 1 ~ n)`の総和が求めたい答え．

ハマった点は，
- `k == i`，即ち既にこれ以上選択出来ない場合でもそれまでの結果を`j + 1`に伝える必要があること

```python
n = int(input())
a = list(map(int, input().split()))


ans = 0
for i in range(1, n + 1):
    # dp[j][k][remainder] := Aの先頭j項からk個を選び，
    # 選んだ項の総和をiで割った余りがremainderとなる方法の個数
    dp = [[[0 for _ in range(i)] for _ in range(i + 1)] for _ in range(n + 1)]
    dp[0][0][0] = 1

    for j in range(n):
        for k in range(i + 1):
            for remainder in range(i):
                dp[j + 1][k][remainder] += dp[j][k][remainder]
                dp[j + 1][k][remainder] %= 998244353
                if k == i:
                    continue
                dp[j + 1][k + 1][(remainder + a[j]) % i] += dp[j][k][remainder]
                dp[j + 1][k + 1][(remainder + a[j]) % i] %= 998244353

    ans += dp[n][i][0]

print(ans % 998244353)
```

#DP #ABD #D
