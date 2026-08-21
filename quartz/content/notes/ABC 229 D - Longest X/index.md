---
title: "ABC 229 D - Longest X"
date: 2022-04-18
modified: 2022-04-18
tags:
  - "ABC"
  - "D"
  - "尺取り法"
---
https://atcoder.jp/contests/abc229/tasks/abc229_d

茶色上位．尺取り法．
尺取り法に関しては，
https://qiita.com/drken/items/ecd1a472d3a0e7db8dce#問題-1aoj-course-the-number-of-windows
の1だけでも見るとこの問題は理解できる．

最初は次のように書いていたけど，WAの嵐．
```python
from itertools import accumulate

s = list(map(lambda x: 1 if x == "X" else 0, input()))
n = len(s)
k = int(input())
acc = [0] + list(accumulate(s))

right = 1
ans = 0
zero_count = 0
for left in range(n):
    temp_sum = acc[right] - acc[left]
    while 1:
        # print(f'left:{left}, right:{right}, zero:{zero_count}')
        if right < n and k > zero_count:
            if s[right]:
                temp_sum += 1
            else:
                zero_count += 1
            right += 1
        else:
            break
    ans = max(right - left, ans)
    if left < n - 1 and not s[left + 1]:
        zero_count -= 1
print(ans)
```

解説の擬似コードを見てみると案外シンプルに書けそうな気がする．
```pseudo
r=0, ans=0
for l = 0 to |S| - 1
    while S[l,r+1)をすべて 'X' に変えることが可能
        r = r + 1
    ans = max(ans, r - l)
```

区間に関する問題は例によって，事前に累積和を求めておくと区間和を高速に計算できる．
普通にちょびちょび足していくよりも，そんなに速度も変わらなさそうだし，何より見やすくていい．

擬似コードを基にPythonで書き直したものがこちら．
```python
from itertools import accumulate

s = list(map(lambda x: 0 if x == "X" else 1, input()))
len_s = len(s)
k = int(input())
acc = [0] + list(accumulate(s))

right = 0
ans = 0
for left in range(len(s)):
    while right < len_s and acc[right] - acc[left] <= k:
        right += 1
    if acc[right] - acc[left] > k:
        right -= 1
    ans = max(ans, right - left)
print(ans)
```

とりあえず，`acc[r]-acc[l]`がkを超えるか，rが右端に来るかまで進めて，`acc[r]-acc[l]`がkを超えていたらデクリメントするだけ．

以外に忘れがちだが，`s[len(s)]`はIndexErrorを起こすので注意．

#ABC #D #尺取り法
