---
title: "謎AC集"
date: 2022-08-24
modified: 2022-08-24
---
https://atcoder.jp/contests/abc170/tasks/abc170_d

aのmaxsizeを制約に従って10**6にするとTLEしたが，入力に基づいて動的に計算するとACした問題．
実際に入力をみると，nが大きい時はAの各値は小さい傾向にあった．

https://atcoder.jp/contests/abc170/submissions/34301916
https://atcoder.jp/contests/abc170/submissions/34301935

入力の最大値を求めるのはせいぜい$O(N)$なので，逐次計算しましょう．
