---
title: "ABC096 D - Five, Five Everywhere"
date: 2023-04-15
modified: 2023-04-25
tags:
  - "ABC"
  - "D"
  - "合成数"
  - "素数"
---
https://atcoder.jp/contests/abc096/tasks/abc096_d

水色下位．構築系

任意の5つの和が合成数となるような素数の組み合わせを求めたい．

合成数を5の倍数であると考えれば，条件は各素数のmod5が1であることと等しい．

$55555$以下の素数は数千個存在し，その中からmod5が1である$N\le55$個を選ぶ．

```python
def create_sieve(n):
    sieve = [True] * n
    sieve[:2] = [False] * 2
    for p in range(2, n):
        if not sieve[p]:
            continue
        i = p
        while i + p < n:
            i += p
            sieve[i] = False
    return sieve


n = int(input())

sieve = create_sieve(55555)
prime = []
for i in range(55555):
    if sieve[i] and i % 5 == 1:
        prime.append(i)

print(*prime[:n])
```

#合成数 #素数 #ABC #D
