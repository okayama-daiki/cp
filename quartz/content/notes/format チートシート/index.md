---
title: "format チートシート"
date: 2022-06-11
modified: 2022-11-12
---
参考: https://gammasoft.jp/blog/python-string-format/#float-point
- precision
```pycon
>>> f'{12.345:.0f}'
'12'
>>> f'{12.345:.1f}'
'12.3'
>>> f'{12.345:.2f}'
'12.35'
>>> f'{12.345:.4f}'
'12.3450'

```

- 寄せ
```pycon
>>> f'{"Python":<10}'
'Python    '
>>> f'{"Python":>10}'
'    Python'
>>> f'{"Python":^10}'
'  Python  '
```

- 数値
```pycon
>>> f'{10-9:+}'
'+1'
>>> f'{9-10:+}'
'-1'
```
