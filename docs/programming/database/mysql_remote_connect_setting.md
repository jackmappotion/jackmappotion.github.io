---
layout: default
title: Mysql 원격접속 허용 세팅
parent: Database
grand_parent : Programming
---

# MySQL
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

- TOC
{:toc}

---

## MySQL 원격 접속 허용하기

> 1. 원격접속 가능한 유저 만들기

```
- CREATE USER 'user1'@'%' IDENTIFIED BY '1234'
- CREATE USER 'user2'@'111.222.%' IDENTIFIED BY '1234'
- CREATE USER 'user3'@'111.222.333' IDENTIFIED BY '1234'
```

> 2. mysql bind-address 설정하기

```
--- ubuntu apt-get installed base ---
/etc/mysql
.
├── conf.d
│   ├── mysql.cnf
│   └── mysqldump.cnf
├── debian-start
├── debian.cnf
├── my.cnf -> /etc/alternatives/my.cnf
├── my.cnf.fallback
├── mysql.cnf
└── mysql.conf.d
    ├── mysql.cnf
    └── mysqld.cnf -> FILE TO EDIT

2 directories, 9 files

--- mac brew installed base---
/opt/homebrew/etc
.
├── my.cnf -> FILE TO EDIT

bind-address = 127.0.0.1 -> bind-address = 0.0.0.0

ps : mysqlx-bind-address 도 있는데 x_plugin과 관련된 것으로 따로 생각하지 않아도 된다.
```

> 3. check port

```
COMMAND     PID            USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
systemd-r   463 systemd-resolve   13u  IPv4  20562      0t0  TCP localhost:53 (LISTEN)
sshd       1725            root    3u  IPv4  30727      0t0  TCP *:22 (LISTEN)
sshd       1725            root    4u  IPv6  30738      0t0  TCP *:22 (LISTEN)
mysqld    28676           mysql   21u  IPv4 295469      0t0  TCP localhost:33060 (LISTEN)
mysqld    28676           mysql   31u  IPv4 295471      0t0  TCP *:3306 (LISTEN)


- mysqld    28676           mysql   31u  IPv4 295471      0t0  TCP localhost:3306 (LISTEN) 
-> mysqld    28676           mysql   31u  IPv4 295471      0t0  TCP *:3306 (LISTEN)

이렇게 포트가 열림
```