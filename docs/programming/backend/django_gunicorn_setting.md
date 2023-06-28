---
layout: default
title: Django gunicorn 세팅
parent: Backend
grand_parent : Programming
---
## Django 상용 서버 배포를 위한 몇 가지 세팅을 알아보자.

- TOC
{:toc}
---

## Django 배포
>   - Django 배포를 할때 생각할때 드는 고민들이 있다.
>       - 이거 유저가 접속할떄 http://127.0.0.1:8000/ 
>           - `8000` 포트로 접속하는게 아닌데 어떻게 변경하지?
>       - 이게 배포해도 될 정도로 트래픽을 처리할 수 있나?
>           - WSGI : (Web Server Gateway Interface)
>           - ASGI : (Asynchronous Server Gateway Interface)

## Django 서버 실행
### 1. runserver

```
python manage.py runserver 0.0.0.0:8080
```

### 2. gunicorn by Port
> gunicorn은 port를 이용하여 서버를 열 수 있다.

```
gunicorn --bind 0:8080 config.wsgi:application
```

### 3. gunicorn by Unix socket
> gunicorn은 유닉스 소켓을 이용하여 서버를 열 수 있고, 이는 더 효율적이다.

```
gunicorn --bind unix:/tmp/gunicorn.sock config.wsgi:application
```

> gunicorn을 이용할때는 static이 적용되지 않는데 몇가지 세팅이 필요하다.
> 1. nginx 설치
> 2. nginx setting
```
❯ pwd; tree -d
/etc/nginx
.
├── conf.d
├── modules-available
├── modules-enabled
├── sites-available 
├── sites-enabled
└── snippets
---
여기서 site-available에 A)와 같은 식으로 setting 추가

A) 
server {
  listen 80;
  server_name 34.64.103.28;

  location /static {
    alias  /root/Desktop/things/practice/django_release_setting/static;
  }

  location / {
    include proxy_params;
    proxy_pass http://unix:/tmp/gunicorn.sock;
  }
}
---
이후 
❯ ln -s sites-available/mysite sites-enabled/mysite
이런식으로 sites-enabled에 넘겨준다.
```

> 3. gunicorn --bind unix:/tmp/gunicorn.sock config.wsgi:application