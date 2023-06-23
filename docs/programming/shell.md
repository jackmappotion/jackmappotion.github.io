---
layout: default
title: Shell
nav_order: 2
parent: Programming
---

# Shell
{: .no_toc }

{: .fs-6 .fw-300 }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

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