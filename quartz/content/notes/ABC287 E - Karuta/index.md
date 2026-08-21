---
title: "ABC287 E - Karuta"
date: 2023-01-28
modified: 2023-03-14
tags:
  - "ABC"
  - "E"
  - "MultiSet"
  - "多重集合"
  - "文字列"
---
https://atcoder.jp/contests/abc287/tasks/abc287_e

緑上位．文字列．

## POINT

- **文字列をhash値の和で表現**することで，文字列間の一致判定を$O(1)$で行う．
- 各文字列について，考えられる接頭辞（先頭からの連続部分文字列）のhash値を算出しておき，多重集合で管理する

[[hr.icon]]
## 重要な点：文字列をhashの和で表現する

ここではhash関数にPython組み込み関数の`hash()`を利用して考える．
例) `'abc'`
```python
S = "abc"
value = 0
for i in range(len(S)):
    value += hash(f"{i}{S[i]}")
```

大事な部分は，`hash()`に与える文字列の先頭に「今何文字目か」を表す`i`を与えた上でhash値を計算していることである．これにより，並び順のみが異なる文字列（例: `'acb', 'cba'` 等）と区別がされる．

（補足）
`'abc'`のhash値は`hash('1a') + hash('2b') + hash('3c')`であり，これは`'acb'`のhash値 `hash('1a') + hash('2c') + hash('3b')`とは異なる．

[[hr.icon]]
各文字列で考えられる全ての接頭辞についてhash値を計算し，適宜多重集合へ値を追加する．

その後改めて各文字列について見ていき，自身の接頭辞が多重集合内に2つ以上存在すれば，他の文字列に一致するものが存在すると判定できる．（1つはその文字列自身の接頭辞がカウントされているため）

```python
# 全部貼り付けると長くなるので適宜多重集合のコードを張ってください
from atcoder.multiset import MultiSet

n = int(input())
s = [input() for _ in range(n)]

mset = MultiSet()

# 各文字列について接頭辞のhash値を計算，同時に多重集合へ追加
for i in range(n):
    v = 0
    for j in range(len(s[i])):
        v += hash(f"{j}{s[i][j]}")
        mset.add(v)

# 各文字列について再度接頭辞のhash値を計算し，一致した最長接頭辞の長さを出力
for i in range(n):
    v = 0
    ans = 0
    for j in range(len(s[i])):
        v += hash(f"{j}{s[i][j]}")
        if mset.count(v) > 1:
            ans += 1
        else:
            break
    print(ans)
```

[提出コード(PyPy3, 1509ms)](https://atcoder.jp/contests/abc287/submissions/38430765)

## 補足：接頭辞の列挙とhash値の計算

例えば，文字列`atcoder`は次のように各接頭辞のhash値を連続的に計算出来る．
```python
S = "atcoder"
value = 0
hashes = []
for i in range(len(S)):
    value += hash(f"{i}{S[i]}")
    hashes.append(value)

print(hashes)  # [1文字目がaのhash値,
#  1文字目がaのhash値 + 2文字目がtのhash値,
#  1文字目がaのhash値 + 2文字目がtのhash値 + 3文字目がcのhash値,
#                           :
# ]
```

追記：別にMultiSetじゃなくてもよい．https://blog.uoh-dakken.com/dike/191/
#文字列 #ABC #E #MultiSet #多重集合
