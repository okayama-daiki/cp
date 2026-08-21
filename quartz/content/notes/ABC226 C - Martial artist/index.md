---
title: "ABC226 C - Martial artist"
date: 2022-05-02
modified: 2022-05-02
tags:
  - "ABC"
  - "DFS"
  - "Tips_集"
---
https://atcoder.jp/contests/abc226/tasks/abc226_c

茶色下位．グラフ探索．

どうやら依存関係ときたらグラフを考えると良さそうな感じがする．
ただ今回は技1を始点とするのではなく，技Nを始点としてグラフ探索することが思いつかなかった．

```python
n = int(input())
graph = [[] for _ in range(n)]
time = []
for i in range(n):
    t, _, *a = map(int, input().split())
    time.append(t)
    for a in a:
        a -= 1
        graph[i].append(a)

visited = set()
que = [n - 1]
while que:
    now = que.pop()
    visited.add(now)
    for nex in graph[now]:
        if nex in visited:
            continue
        que.append(nex)
ans = 0
for node in visited:
    ans += time[node]
print(ans)
```

#ABC #Tips_集 #DFS
