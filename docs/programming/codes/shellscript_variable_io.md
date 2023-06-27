---
layout: default
title: Shellscript 변수 넘겨주기
parent: Codes
grand_parent : Programming
---

## ShellScript with parameter

```
-- my_script.sh --
echo "First arg : $1"
echo "Second arg : $2"


> sh my_script.sh number_1 number_2
First arg : number_1
Second arg : number_2
```