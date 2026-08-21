---
title: "ABC213  C - Reorder Cards"
date: 2022-05-05
modified: 2022-05-05
tags:
  - "ABC"
  - "C"
  - "座標圧縮"
---
https://atcoder.jp/contests/abc213/tasks/abc213_c

茶色下位．座標圧縮．

操作は片方の軸に対してのみ影響するため，$x$と$y$で独立させて考える．

以下では$x$ のみ考える，
入力が$10, 3, 4, 4, 2, 9$であったとすると，これは操作後$5, 2, 3, 3, 1, 4$となる．

このように，元の数列の大小関係を維持しつつ，値を小さくすることを座標圧縮という．

Pythonでは辞書型を使うと簡潔に処理を書ける．

① 集合型にすることで重複を排除する
② 小さい順にソートする
③ その要素が何番目の要素なのかを辞書型で管理する

```python
def compress(seq):
    dic = {v: i for i, v in enumerate(sorted(set(seq)))}
    return [dic[v] for v in seq]
```

つまり座標圧縮後の数列をそのまま出力すればよいことになる．

```python
def compress(seq):
    dic = {v: i for i, v in enumerate(sorted(set(seq)), 1)}
    return [dic[v] for v in seq]


*_, n = map(int, input().split())
ab = [list(map(int, input().split())) for _ in range(n)]
a = [a for a, _ in ab]
b = [b for _, b in ab]

for x, y in zip(compress(a), compress(b)):
    print(x, y)
```

#ABC #C #座標圧縮
