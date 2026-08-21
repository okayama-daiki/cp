---
title: "ABC130 D - Enough Array"
date: 2022-06-12
modified: 2022-06-12
tags:
  - "ABC"
  - "D"
  - "尺取り法"
  - "累積和"
---
https://atcoder.jp/contests/abc130/tasks/abc130_d
緑下位．二分探索 or 尺取り法．

累積和を取ると単調増加な数列が得られるというのがポイント．

左端を固定した上で，その総和がk以上となる右端の最小のインデックスを二分探索すれば，`n-インデックス値`の総和で答えが得られる．

```python
from itertools import accumulate
from bisect import bisect_left

n, k = map(int, input().split())
a = list(map(int, input().split()))

acc = [0] + list(accumulate(a))

ans = 0
for i in range(n + 1):
    b = bisect_left(acc, k + acc[i], lo=i, hi=n + 1)
    ans += n - b + 1
print(ans)
```

※`bisect_left()`の引数`lo=i`は不要だが，若干早くなる(なければlo=0から探索し始めるため．)．
`hi=n+1`はあってもなくても．

## メモ

`bisect_left(acc, k, lo=i, hi=n+1, key=lambda x:x-acc[i])`が直感的だが，`bisect`のオプション引数`key`は，Python3.8.2時点では対応していないらしい．

## つまずいたポイント

- 右端を固定する
別にこれでも出来ないわけではないが，探索するべき値が何かを吟味する必要がある．
加えて探索値と同じ場合は，それより右のインデックスを取得しなければならないことに注意．
```python
bisect_right(a=acc, x=acc[i] - k, lo=0, hi=i)
```

- 累積和の左端に0を挿入していなかった
コレがバグの主要因．累積和を考える時には`[0].extend(accumulate())`としよう．
これがないと，第1項目を無視して計算することになる．

尺取り法でも解けるらしいので，また今度．

#ABC #D #累積和 #尺取り法
