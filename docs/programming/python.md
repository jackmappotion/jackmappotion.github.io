---
layout: default
title: Python
nav_order: 2
parent: Programming
---

# Python
{: .no_toc }

{: .fs-6 .fw-300 }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---
## DefaultDict

```
from collections import defaultdict

my_default_dict = defaultdict(lambda : [], dict())

for i in range(10):
    for j in range(10):
        my_default_dict[i].append(j)

뭐 작업하다 보면 defaultdict를 사용하게 될일이 종종 있는데

이때 만약 key가 없으면 넣은 key값을 그대로 배출하고 싶을때가 있다
-> 이때 defaultdict로 작업하기 좀 까다로운데
class CustomDict(dict):
    def __missing__(self,key):
        return key
요렇게 하나 만들어서 쓰니 유용하다
```

## Django - pandas 
```
my_df = pd.DataFrame(...)

def index(request):
    my_df = pd.DataFrame(...)
    context = {
        "my_df_html":my_df.to_html()
    }
    return render(
            request=request,
            template_name="/my_template/template.html",
            context=context)

my_template/template.html

{% extends 'base.html' %}
{% block title %}
    my web
{% endblock %}
{% block content %}

{{ my_df_html | safe}}

{% endblock %}

```
