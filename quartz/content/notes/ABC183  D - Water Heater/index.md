---
title: "ABC183  D - Water Heater"
date: 2022-05-05
modified: 2022-05-05
tags:
  - "ABC"
  - "D"
  - "imos法"
---
https://atcoder.jp/contests/abc183/tasks/abc183_d
茶色上位．いもす法．

```python
time[x]: 時刻xでの供給量
```

で話を進める．

$S_i$から$T_i$までの区間で一つずつ愚直に$P_i$ずつ足していくと当然の如くTLEする．

ここで，$S_i$と$T_i$のところにそれぞれ$+P_i$，$-P_i$を記録しておき，後にそれらの累積和を取れば，先程作りたかった配列が完成する．この考え方は結構重宝しそうな気がする．

ただし，このやり方はあくまで最大$T_i$が小さいからであり，そうでない場合は$S_i$と$T_i$でソートを行い，シュミレーションする必要が出てくることに留意したい．

これをimos法と呼ぶらしい．何やら発案は競技プログラマによるものらしく．

```python
from itertools import accumulate

n, w = map(int, input().split())
timecard = [0 for _ in range(2 * 10**5 + 1)]
for _ in range(n):
    s, t, p = map(int, input().split())
    timecard[s] += p
    timecard[t] -= p

acc = accumulate(timecard)
print("Yes" if max(acc) <= w else "No")
```

#ABC #D #imos法
