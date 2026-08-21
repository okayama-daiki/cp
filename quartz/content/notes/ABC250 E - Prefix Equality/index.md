---
title: "ABC250 E - Prefix Equality"
date: 2023-03-14
modified: 2023-03-14
tags:
  - "ABC"
  - "E"
  - "Hash"
---
https://atcoder.jp/contests/abc250/tasks/abc250_e
水色上位．

前処理により，一致判定を$O(1)$で行う手法を考える．
このようなprefixの要素集合が一致しているかであったり，prefixそのものが一致しているかという問題は，まともにやると$O(prefix\text{の長さ})$の処理であるが，これをhashを利用して$O(1)$にする．

具体的には$O(N)$の前処理をもって，先頭から任意のインデックスまでの要素の集合をhashとして計算する．
後はクエリに答えるのみである．

類題：[[notes/ABC287 E - Karuta/index|ABC287 E - Karuta]]

```python
n = int(input())
a = list(map(int, input().split()))
b = list(map(int, input().split()))

hash_a = [0]
set_a = set()
for i in range(n):
    if a[i] not in set_a:
        hash_a.append(hash_a[-1] + hash(str(a[i])))
        set_a.add(a[i])
    else:
        hash_a.append(hash_a[-1])

hash_b = [0]
set_b = set()
for i in range(n):
    if b[i] not in set_b:
        hash_b.append(hash_b[-1] + hash(str(b[i])))
        set_b.add(b[i])
    else:
        hash_b.append(hash_b[-1])


q = int(input())
for _ in range(q):
    x, y = map(int, input().split())
    if hash_a[x] == hash_b[y]:
        print("Yes")
    else:
        print("No")
```

#Hash #ABC #E
