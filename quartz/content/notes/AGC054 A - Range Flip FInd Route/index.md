---
title: "AGC054 A - Range Flip FInd Route"
date: 2022-11-19
modified: 2022-11-19
tags:
  - "AGC"
  - "DP"
---
https://atcoder.jp/contests/agc043/tasks/agc043_a

「スタートする前に予め盤面をいじっておくことができる」系の問題には定石がある．

- まずは経路を一つ固定してみる
- その経路が条件を満たすようにするための最小操作回数を求めてみる
- それがわかれば、元の問題の解き方も自然とわかってくる

以上を踏まえ問題を考察すると，ある経路についての最小操作回数は連続しないブロック数と一致する．

よってこの問題はdpで解ける．
直前のブロックが壁なら操作回数を引き継ぐ．

```python
INF = float("inf")

h, w = map(int, input().split())
s = [list(input()) for _ in range(h)]

dp = [[INF] * w for _ in range(h)]
# dp[r][c] := (r, c)に到達するための最小移動回数

dp[0][0] = s[0][0] == "#"

for r in range(h):
    for c in range(w):
        if r + 1 < h:
            dp[r + 1][c] = min(
                dp[r + 1][c], dp[r][c] + (s[r][c] == "." and s[r + 1][c] == "#")
            )
        if c + 1 < w:
            dp[r][c + 1] = min(
                dp[r][c + 1], dp[r][c] + (s[r][c] == "." and s[r][c + 1] == "#")
            )

print(dp[h - 1][w - 1])
```

#DP #AGC
