---
title: "ABC268 C - Chinese Restaurant"
date: 2022-12-29
modified: 2022-12-29
tags:
  - "ABC"
  - "C"
---
https://atcoder.jp/contests/abc268/tasks/abc268_c
茶色上位

実際に動かそうとするとTLEする．
ポイントは回転方向が一定であることである．
各人について，喜ぶためのテーブルの回転回数を求め適宜それを記録していく．これは$O(1\times N)$で可能．

そうして最も喜ぶ人が多かった回転数について，喜んだ人の数が答えとなる．

```python
n = int(input())
p = list(map(int, input().split()))

ans = [0] * n
for i in range(n):
    ans[(p[i] - i) % n] += 1
    ans[(p[i] - i + 1) % n] += 1
    ans[(p[i] - i - 1) % n] += 1

print(max(ans))
```

#ABC #C
