---
title: "Tips"
date: 2022-05-02
modified: 2022-08-23
---
[[https://gyazo.com/6c4e53bb3465d9ddf45ab1bf696385f2.png]]
## 昇順＆降順ソート

```python
sort(key=lambda x: (x[0], -x[1]))
```

[[https://gyazo.com/6c4e53bb3465d9ddf45ab1bf696385f2.png]]
## 2つの閉区間の重なりを判定する

```python
def check_overlap(a1, b1, a2, b2):
    return max(a1, a2) <= min(b1, b2)
```

[[https://gyazo.com/6c4e53bb3465d9ddf45ab1bf696385f2.png]]
## 閉区間・開区間混合問題に対して

開区間`(a,b)`などは，それぞれ0.1くらい足し引きしてやると，閉区間として扱える．`[a+.1,b-.1]`
[[https://gyazo.com/6c4e53bb3465d9ddf45ab1bf696385f2.png]]
## 二次元配列を回転させる

```python
def rotate(array):
    return list(zip(*array[::-1]))
```

[[https://gyazo.com/6c4e53bb3465d9ddf45ab1bf696385f2.png]]
## 二次元の図形を左上に寄せる

```python
def move(array):
    while all(e == "." for e in array[0]):
        array = array[1:] + [["." for _ in range(n)]]
    while all(array[i][0] == "." for i in range(n)):
        array = [col[1:] + ["."] for col in array]
    return array
```

ABC218 C - Shapes (https://atcoder.jp/contests/abc218/tasks/abc218_c) で使用．
[[https://gyazo.com/6c4e53bb3465d9ddf45ab1bf696385f2.png]]
## 座標圧縮

```python
def compress(seq):
    dic = {v: i for i, v in enumerate(sorted(set(seq)))}
    return [dic[v] for v in seq]
```

[[https://gyazo.com/6c4e53bb3465d9ddf45ab1bf696385f2.png]]
## 大きい数の演算

数が大きくなると計算も遅くなるため，MODの導入を検討しましょう．
[[https://gyazo.com/6c4e53bb3465d9ddf45ab1bf696385f2.png]]
## title

```python
```

[[https://gyazo.com/6c4e53bb3465d9ddf45ab1bf696385f2.png]]
## title

```python
```
