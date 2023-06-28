---
layout: default
title: Shellscript 변수 넘겨주기
parent: Codes
grand_parent : Programming
---
---
# 쉘스크립트에서 변수를 넘겨주는 방법

- TOC
{:toc}
---

## Why
> - Airflow DB ETL pipeline을 만드는 과정에서
> - Bashoperator를 사용하는데 필요한 부분이 생겼다.

## Code
```
-- my_script.sh --
echo "First arg : $1"
echo "Second arg : $2"


> sh my_script.sh number_1 number_2
First arg : number_1
Second arg : number_2
```