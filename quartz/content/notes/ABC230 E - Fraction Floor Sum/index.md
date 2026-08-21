---
title: "ABC230 E - Fraction Floor Sum"
date: 2023-01-28
modified: 2023-01-28
tags:
  - "ABC"
  - "E"
  - "整数"
---
https://atcoder.jp/contests/abc230/tasks/abc230_e
緑下位．整数問題

![画像](./63d4bd14a4dd1b001e21734e.png)
実は$y\le \frac{N}{x} \space(x>0,y>0)$の格子点の数を問われている．

![画像](./63d4bd75002426001fe7d24e.png)
上図は$y=x$で対称なので，$y\ge x$となる部分の格子点の個数を求めて2倍してやればよい．
![画像](./63d4be3427233d001eb246e0.png)
このとき$y=x$と$y=\frac{N}{x}$の交点の座標は$x=\sqrt{n}$であり，$x(1\le x \le\sqrt{n})$の範囲の格子点を数え上げればよいことになる．

$y\le\frac{N}{x}$と$y\ge x$に囲まれた部分の格子点の個数は，$\left\lbrack{\frac{N}{x}}\right\rbrack - (x - 1)$で求まるので，$1\le x \le\sqrt{n}$について計算し足し合わせる．

```python
grid = 0
for x in range(1, int(N**0.5) + 1):
    grid += N // x - (x - 1)
```

![画像](./63d4c317897309001dd03567.png)

前述の通り$y=x$で線対称なので値を2倍し，重複して数えられた$y=x$上の格子点を除けば答えとなる．

```python
N = int(input())

grid = 0
for x in range(1, int(N**0.5) + 1):
    grid += N // x - (x - 1)

print(grid * 2 - int(N**0.5))
```

#ABC #E #整数
