---
title: "configparserの使い方"
date: 2022-06-22
modified: 2022-06-22
---
```ini
[DEFAULT]
user = default_name
id = 123
```

```python
import configparser

config = configparser.ConfigParser(interpolation=None)
config.read(r"config.ini")

# read
print(config["DEFAULT"]["user"])  # default_name
print(config["DEFAULT"]["id"])  # 123

# write
config["DEFAULT"]["user"] = "update_name"
with open(r"config.ini", "w") as f:
    config.write(f)
```
