---
title: "ABC D - 8 Puzzle on Graph"
date: 2023-05-04
modified: 2023-05-04
tags:
  - "ABC"
  - "BFS"
  - "D"
  - "最短経路"
---
https://atcoder.jp/contests/abc224/tasks/abc224_d
水色下位．最短経路．

ゲーム木を知っていると，なんとなく各状態とその遷移方法がグラフに見えてくる．

盤面の状態数が$9!$であるからBFSにより探索ができる．

PyPyでのACには定数倍高速化が重要で，いくつか実装のポイントがある
- 目的の盤面が見つかったら探索を打ち切る
- 既に訪れた頂点をqueに入れない
  - 今回は内側のループ（遷移先を計算するフェーズ）の計算量がかなり大きいため，特に**同じ頂点をqueに入れない**努力が必要である（詳しくは実装例を参照）
- dists配列を持たずに，queの要素直接に格納する（queの2つ目の要素）
- 内側のループで辺を全探索することを避けるために，予めどの頂点が空いているのかを保持しておく（queの3つ目の要素）

特に1つ目と2つ目の足切りの効果が絶大．
```python
INF = float("inf")

m = int(input())
graph = [[] for _ in range(9)]
for _ in range(m):
    u, v = map(lambda x: int(x) - 1, input().split())
    graph[u].append(v)
    graph[v].append(u)
p = tuple(map(lambda x: int(x) - 1, input().split()))
for i in range(9):
    if i not in p:
        empty = i

que = [(p, 0, empty)]
visited = set()

target = tuple(range(8))
for u, dist, empty in que:
    if u == target:
        print(dist)
        break
    for sub in graph[empty]:
        translate = {sub: empty}
        v = tuple(translate.get(e, e) for e in u)
        if v in visited:
            continue
        que.append((v, dist + 1, sub))
        visited.add(v)
else:
    print(-1)
```

#BFS #ABC #D #最短経路
