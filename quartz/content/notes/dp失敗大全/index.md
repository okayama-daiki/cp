---
title: "dp失敗大全"
date: 2022-08-24
modified: 2022-10-29
---
- 0 <= i <= Nの範囲で遷移．
`range(n)`とすると，`i=n`の時の遷移がなくなる(貰うDPのとき)

- iとjの取り違え
- +=と=の取り違え

桁DP
- or を | とする
- `for x in range(10 if smaller else n_(i)): ...` としてしまう．正しくは`for x in range(10 if smaller else n_(i) + 1): ...`
- nをそのまま使ってしまう
