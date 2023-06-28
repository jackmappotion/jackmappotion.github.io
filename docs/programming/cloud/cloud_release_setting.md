---
layout: default
title: Cloud 배포 과정
parent: Cloud
grand_parent : Programming
---
# Cloud 환경에서 배포할때 체크할 것을 알아보자

- TOC
{:toc}
---
## Cloud에서의 세팅
>   - Cloud의 공인 ip를 확인한다.
>   - 서버를 열기로 계획한 port를 open한다.
>       - 필요하다면 port-fowarding을 진행한다.

## 배포할 프로젝트의 세팅
>   - 프로젝트를 Cloud에서 open한 port로 실행한다.
>       - 혹은 port-fowarding 세팅에 맞게 open 한다.
>   - 프로젝트 tool 에 따라서 외부접속 허용 과 같은 부분들을 설정한다.  
>
> EX)
>```
>  Django
>  python manage.py runserver 0.0.0.0:8000 (0.0.0.0 -> 모든 ip 접속을 허용한다.)
>```

## 배포할 프로젝트의 세팅
>   - 프로젝트를 Cloud에서 open한 port로 실행한다.
>       - 혹은 port-fowarding 세팅에 맞게 open 한다.
>   - 프로젝트 tool 에 따라서 외부접속 허용 과 같은 부분들을 설정한다.  
>
> EX)
>```
>  Django
>  python manage.py runserver 0.0.0.0:8000 (0.0.0.0 -> 모든 ip 접속을 허용한다.)
>```

### 만약 도커를 통한 배포를 진행한다면?

```
FROM python:3

WORKDIR /Users/jack/my_project

COPY requirements.txt ./
RUN pip install -r requirements.txt
COPY . .

EXPOSE 8000

CMD ["python","manage.py","runserver","0.0.0.0:8000"]
---
EXPOSE 8000 처럼 특정 포트를 열어주는 작업을 image에서도 진행하는것이 필요하다
```
