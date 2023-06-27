---
layout: default
title: Mysqldump shellscript 활용
parent: Database
grand_parent : Programming
---

## Mysql dump script example
### All Table
```
mysqldump \
  -h localhost \
  -P 3306 \
  -u jackmappotion \
  -p1234 \
  stock \
  > $(date +%Y_%m_%d.sql)
---
host localhost의
port 3306의
database stock에 
user jackmappotion과
password 1234로 접속하여
stock database를 덤프따서
$(date +%Y_%m_%d.sql)로 저장한다.
```

### Selected Table 

```
mysqldump \
  -h localhost \
  -P 3306 \
  -u jackmappotion \
  -p1234 \
  stock \
  appl \
  > $(date +%Y_%m_%d.sql)
---
host localhost의
port 3306의
database stock에 
user jackmappotion과
password 1234로 접속하여
stock database의 appl 테이블을 덤프따서
$(date +%Y_%m_%d.sql)로 저장한다.
```
