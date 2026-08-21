---
title: "実践bisect"
date: 2022-08-23
modified: 2022-09-05
---
```python
from bisect import bisect_left, bisect_right
```

`bisect_left(a, x)`は`a`中の要素の内，`x`以上の要素の最大のインデックスを返す．

```pycon
>>> a = [0, 1, 2, 3, 4]
>>> bisect_left(a, 1)
1
>>> a = [0, 1, 1, 1, 2]
>>> bisect_left(a, 1)
1
```

`bisect_right(a, x)`は`a`中の要素の内，`x`以下の要素の最小のインデックスに**1を加えたもの**を返す．
```pycon
>>> a = [0, 1, 2, 3, 4]
>>> bisect_right(a, 1)
2
>>> a = [0, 1, 1, 1, 2]
>>> bisect_right(a, 1)
4
```

※`bisect_right(a, x)-1`が`a`中の要素の内，`x`以下の要素の最大のインデックスとなる．

`a`中に`x`が存在しない場合も上述のように振舞う．
```pycon
>>> a = [0, 2, 3, 4]
>>> bisect_left(a, 1)
1
>>> bisect_right(a, 1)
1
```
