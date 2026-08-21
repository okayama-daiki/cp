---
title: "桁DP"
date: 2022-10-29
modified: 2022-10-29
tags:
  - "DP"
  - "桁DP"
---
桁 DP とは、値の上から i 桁目までを確定させたときに、N 未満で確定しているかどうかを状態に加えた DP を指します。

0以上N以下の整数で，ある条件を満たすものの個数や最大値を求める場合に用いる．特に，Nが非常に大きな数となる場合が多い．

例: 0以上N以下の整数で，条件を満たすものの個数・最大値を求めよ．
N = 456

桁の上から順に確定させていくが，
- 百の位が4: 十の位は0~5の範囲となる
  - 十の位が5: 一の位は0~6の範囲となる
  - 十の位が5未満(0, 1, 2, 3, 4): 一の位の範囲は0~9の範囲となる
- 百の位が4未満(この場合は0, 1, 2, 3): 十の位は0~9の範囲となる
  - 十の位に関わらず，一の位は0~9の範囲となる

つまり自分より上の桁の数字が，制約の対応する桁と一致していたら，自分も制約と対応する桁の範囲でしか動けないが，そうでないならば自由に動けることが分かる．

これは，「今調べている桁より上の桁の数字が，制約と対応する桁の数字と同じになっているかを表すフラグ」を付与することで考える事が出来る．

例えば，N=12345のとき，12***の場合はFalse, 11***の場合はTrueのように状態を持たせる．

見ている桁数を上から`i`，制約より小さいことが確定しているかを`smaller`とすると，
`dp[i][smaller]` := 上から`i`桁まで見た時に，制約以下の数(`smaller = True`) / (今のところ)制約と一致している数(`smaller = False`)の中で，条件を満たす数の個数，最大値と定義できる．

遷移はどうなるだろうか．
`dp[i+1][False] <= dp[i][False]`
`dp[i+1][False] <= dp[i][True]`
`dp[i+1][True] <= dp[i][False]`
`dp[i+1][True] <= dp[i][True]`

`smaller = True`は既に制約より小さくなることが確定している状態なので，この状態から`smaller = False`，即ち制約と一致する可能性はない．よって，`dp[i+1][False] <= dp[i][True]` の遷移は考えられない．

よって，
`dp[i+1][False] <= dp[i][False]`
`dp[i+1][True] <= dp[i][False]`
`dp[i+1][True] <= dp[i][True]`
の3つの遷移式を考えればよい．

---

**例題1** 「0以上12345以下の整数の個数を求める」

```python
str_n = "12345"
len_n = len(str_n)


def n_(i):
    return int(str_n[i])


n = int(str_n)

dp = [[0, 0] for _ in range(len_n + 1)]
# dp[i][smaller] := i桁目まで確定時に，smallerで条件を満たす数の個数

dp[0][0] = 1

for i in range(len_n):
    for smaller in range(2):
        for x in range(10 if smaller else n_(i) + 1):  # i桁目の数のループ
            dp[i + 1][smaller or x < n_(i)] += dp[i][smaller]
            # smaller = 0 <=> 制約と一致している状態 <=> smaller = 1 or 0 へ遷移
            #   smaller = 0 => smaller = 0 への遷移: 桁の数が一緒
            #   smaller = 0 => smaller = 1 への遷移:
            #   桁の数が制約の対応する数字より小さい
            # smaller = 1 <=> 制約より小さいことが確定している状態
            #   smaller = 1のみへ遷移する

print(sum(dp[-1]))
# 12346
```

**例題2** 「0以上12345以下の整数で，各桁に5を含まないものの数の個数」
```python
dp = [[0, 0] for _ in range(len_n + 1)]
# dp[i][smaller] := i桁目まで確定時に，smallerで条件を満たす数の個数

dp[0][0] = 1

for i in range(len_n):
    for smaller in range(2):
        for x in range(10 if smaller else n_(i) + 1):
            if x == 5:
                continue  # 桁の数が5のときは遷移しない
            dp[i + 1][smaller or x < n_(i)] += dp[i][smaller]

print(sum(dp[-1]))
# 8303
```

**例題3**「0から12345の整数で，いずれかの桁に4か9を含むものの数を求める」

```python
str_n = "12345"
len_n = len(str_n)


def n_(i):
    return int(str_n[i])


n = int(str_n)

dp = [[[0, 0] for _ in range(2)] for _ in range(len_n + 1)]
# dp[i][smaller][four_nine] := i桁目まで確定時に，smallerでfour_nineを満たす数の個数

dp[0][0][0] = 1

for i in range(len_n):
    for smaller in range(2):
        for four_nine in range(2):
            for x in range(10 if smaller else n_(i) + 1):
                dp[i + 1][smaller or x < n_(i)][four_nine or x in (4, 9)] += dp[i][
                    smaller
                ][four_nine]

print(dp[len_n][0][1] + dp[len_n][1][1])
# 7002
```

#桁DP #DP
