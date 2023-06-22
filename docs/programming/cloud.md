---
layout: default
title: Cloud
nav_order: 5
parent: Programming
---

# Cloud
{: .no_toc }

{: .fs-6 .fw-300 }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

# GCP
```
GCP vm machine 

root 접속 하려면 설정을 좀 해야함

1. sudo passwd 
    - password 설정
2. su - 
    - root 계정 접속
3. vi /etc/ssh/sshd_config

    - PermitRootLogin yes
    - PasswordAuthentication yes
```

# 배포를 위한 setting
## Cloud 배포를 위해서는 
> 1. server 공인 ip에 대한 portforwarding
> 2. 각 application을 열때 오픈하기   
>   - ex) django : python manage.py runserver 0.0.0.0:8000
> 3. DataBase open 하기
>   - []
