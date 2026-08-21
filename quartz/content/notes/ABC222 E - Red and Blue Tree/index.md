---
title: "ABC222 E - Red and Blue Tree"
date: 2023-05-03
modified: 2023-05-03
tags:
  - "ABC"
  - "DP"
  - "E"
  - "木"
  - "配列の再利用"
---
https://atcoder.jp/contests/abc222/tasks/abc222_e
水色上位．DP．

## 知見

木上では任意の2頂点の最短経路は一つに定まる
木上での任意の2頂点の最短経路はDFSにより求められる（BFSよりも計算量は小さそう？）
$a_1,a_2,\ldots,a_n$の各要素から任意に選ぶ時，選んだ要素の合計が$j$になるような選び方の場合の和は1次元のDPにより求められる
内側のforループを逆順に回せば良い
$a_1,a_2,\ldots,a_n$の要素を二つに分けそれぞれの和を$x,y$とするとき，$x-y=k$を満たすような分け方は何通りあるかという問題は，$s=a_1+a_2+\dots+a_n$として，$\frac{s+k}{2}$となる選び方が何通りあるかという問題に帰着できる
連立方程式$x-y=k,x+y=s$を解く要領
勿論$\frac{s-k}{2}$となる選び方の総数と一致する
$s+k$が偶数，0以上であることは保証されないことに注意

## 解法

木のままでは問題が考えにくいため，実際に操作を行った際にどの辺が使われるのかを考える．各操作について最短経路を求めていくが，木の上での最短経路は一意に定まることを考えると，どの辺が何回使われるのかという情報は事前に求めることができる．

```python
def dfs(u, p=-1):
    if u == target:
        return True
    for v, i in tree[u]:
        if v == p:
            continue
        if dfs(v, u):
            use[i] += 1
            return True
    return False


use = [0] * (n - 1)
for i in range(m - 1):
    target = a[i + 1]
    dfs(a[i])
```

あとは`use`の各要素を2つの部分集合に分け，それらの差が$k$となるような分け方を求めれば良い．

（はまやんの解法では`dp[i][j] := use[:i]を利用して，和がjになるような選び方(jは整数)`と力ずくで求めていたが，バグらせてACできなかったため，以下は公式解法）

2つの部分集合の和を$a,b$とすると，$k=a-b$と表現できる（$a,b$は入れ替えても一般性を失わない）．ここで$s=a+b$を導入すると，$a=\frac{s+k}{2}$により，`use`の各要素から和が$\frac{s+k}{2}$となるような選び方の総数が解となる．

以上から`use`の各要素を利用してある数$x$がいくつ作れるかをDPによって求めればよい．
だがしかし，`dp[i][j] := use[:i]を利用して，和がjになるような選び方(j>=0)`とすると$j$は最大$100,000$となり，$100\times100,000=10,000,000$サイズのDPでは少し計算時間が怪しいらしい．（実際にPyPyではTLE）
（$j=100,000$となるのはパスグラフで両端を行ったり来たりする例）

そこで配列の再利用を行うことにより，空間計算量$O(100,000)$でDPを行う．
2本の配列を利用すれば（ギリギリ）問題ないが，2つ目のループの更新を逆順にすれば1本の配列でも（貰う）DPが可能となる．

## （追記）2周目のループの上限を動的に変更することにより，配列の再利用を行うことなくACした例もある．

https://atcoder.jp/contests/abc222/submissions/26464265

```python
import sys

sys.setrecursionlimit(10**6)

MOD = 998244353

n, m, k = map(int, input().split())
a = list(map(lambda x: int(x) - 1, input().split()))

tree = [[] for _ in range(n)]
for i in range(n - 1):
    u, v = map(lambda x: int(x) - 1, input().split())
    tree[u].append((v, i))
    tree[v].append((u, i))


def dfs(u, p=-1):
    if u == target:
        return True
    for v, i in tree[u]:
        if v == p:
            continue
        if dfs(v, u):
            use[i] += 1
            return True
    return False


use = [0] * (n - 1)
for i in range(m - 1):
    target = a[i + 1]
    dfs(a[i])

max_k = sum(use)

# dp[i] := useの各要素のうち，選んだ要素の和がiになる場合の数
dp = [0] * (max_k + 1)
dp[0] = 1

for i in range(n - 1):
    for j in reversed(range(max_k + 1)):
        if j - use[i] >= 0:
            dp[j] += dp[j - use[i]]
            dp[j] %= MOD

if (max_k - k) % 2 == 0 and 0 <= (max_k - k) // 2 <= max_k:
    print(dp[(k + max_k) // 2])
else:
    print(0)
```

#配列の再利用 #DP #木 #ABC #E
