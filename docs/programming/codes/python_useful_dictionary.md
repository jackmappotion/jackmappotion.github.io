---
layout: default
title: Python 유용한 dictionary
parent: Codes
grand_parent : Programming
---

## defaultdict

```
from collections import defaultdict
```

> key가 없으면 0 을 반환하는 dictionary

```
default_zero_dict = defaultdict(lambda : 0, dict())
```

> key가 없으면 list를 반환하는 dictionary

```
default_list_dict = defaultdict(lambda : [], dict())
```

> key가 없으면 value중 가장 높은 값 + 1을 반환하는 dictionarys

```
default_custom_dict = defaultdict(lambda : max(default_custom_dict.values())+1, dict)
```

## customdict

> 만약 key가 없으면 넣은 key값을 반환하는 dictionary

```
class CustomDict(dict):
    def __missing__(self,key):
        return key

custom_dict = CustomDict()
```
