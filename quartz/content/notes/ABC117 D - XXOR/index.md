---
title: "ABC117 D - XXOR"
date: 2023-02-02
modified: 2023-02-03
tags:
  - "ABC"
  - "XOR"
  - "桁DP"
---
https://atcoder.jp/contests/abc117/tasks/abc117_d
水色上位．XOR．

基本的にはbit独立で見ていけばいい．
## 入力例1

`1: 001`
`6: 110`
`3: 011`
`4: 100` ←解

## 入力例2

`7: 0111`
`4: 0100`
`0: 0000`
`3: 0011`
`9: 1001`

各桁で1の個数と0の個数をカウントするが，このとき各ビットについてXORの和をとるので，数の多い方の逆を答えてやればいい．つまり，各桁が`[1, 0, 0, 1, 1]`であった場合，1の数の方が多いので，逆の0を答えにする．

ただし，$K$より小さくなる必要があるのがポイントで，bitDP的なことを行う必要がある．
桁の大きい方から見てやって，各bitで最適な値を選択すればよいのだが，既に$K$より小さいことが確定しているなら好きな値を，$K$と一致している状況下では値が制限される．

```python
n, k = map(int, input().split())
a = list(map(int, input().split()))

BIT_LENGTH = max(*a, k).bit_length()

x = 0

is_small = False
for digit in range(BIT_LENGTH)[::-1]:
    if is_small or k >> digit & 1:  # 1をおいてもok
        bit1 = 0
        for e in a:
            if e >> digit & 1:
                bit1 += 1
        if bit1 * 2 <= n:
            x += 1 << digit
        else:
            is_small = True

print(sum(map(lambda e: e ^ x, a)))
```

#XOR #桁DP #ABC
