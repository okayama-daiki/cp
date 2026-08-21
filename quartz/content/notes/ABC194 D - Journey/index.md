---
title: "ABC194 D - Journey"
date: 2022-12-27
modified: 2023-03-06
tags:
  - "ABC"
  - "D"
  - "期待値DP"
---
https://atcoder.jp/contests/abc194/tasks/abc194_d
緑上位

## 重要な知見

## 成功確率pの試行を成功するまで行った際の期待値は，$1 / p$

## 証明

求める期待値を$X$とすると，
$X = 1 + (1 / p) \times X$
$X$について解くと，$X = 1 / p$

問題下で考えられる状況は，
- $n - 1$個の頂点に未到達
- $n-2$個の頂点に未到達
- ...
- $1$個の頂点に未到達
であり，各々についてその頂点に辿り着く確率は，$(N - 1) / N$，$(N - 2) / N$，...，$1 / N$となる．各状況についての期待値の総和が解となる．

```python
n = int(input())
print(sum(map(lambda i: n / (n - i), range(1, n))))
```

#期待値DP #ABC #D
