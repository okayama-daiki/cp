---
title: "ABC235 E - MST + 1"
date: 2023-03-04
modified: 2023-03-04
tags:
  - "ABC"
  - "E"
  - "クエリ先読み"
  - "最小全域木"
---
https://atcoder.jp/contests/abc235/tasks/abc235_e

水色下位．最小全域木．

> このように全てのクエリの答えを並列に求めるテクニックは
> 500 点以上の問題でよく出てくるテクニックなので、クエリを与えられる問題が出たときは頭の片隅に入れておきましょう。
とのこと．

初見ではまず元の辺のみを利用してMSTを作成し，各クエリで辺を追加した際に生じる閉路のいずれかの辺のうち，クエリで追加された辺より小さいものが存在すればYesと出力すればよいかと考えたが，閉路検出に$O(N)$のため現実でない．

想定解はクエリ並行処理を利用するもので，クエリで追加される辺も元の辺も同時に利用してMSTを構成することを考える．クラスカル法を利用し，MSTに追加せんとする辺がクエリの辺であれば，そのクエリにはYesと答え，実際のMSTには追加しないという方法で全てのクエリに回答することが可能．

```python
from atcoder.dsu import DSU


n, m, q = map(int, input().split())
es = []

for _ in range(m):
    a, b, c = map(int, input().split())
    es.append((a, b, c, -1))

for i in range(q):
    u, v, w = map(int, input().split())
    es.append((u, v, w, i))

es.sort(key=lambda e: e[2])

ans = ["No"] * q
dsu = DSU(n + 1)

for a, b, c, i in es:
    if i != -1:
        if not dsu.same(a, b):
            ans[i] = "Yes"
        continue
    dsu.merge(a, b)

print(*ans, sep="\n")
```

#ABC #E #最小全域木 #クエリ先読み
