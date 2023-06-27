---
layout: default
title: Django config 세팅
parent: Backend
grand_parent : Programming
---
- TOC
{:toc}
---
## Generally

### DEBUG 관련 세팅

```
DEBUG = True
DEBUG = False
---
배포시에는 DEBUG = False 필요
```

### ALLOWED_HOST

```
ALLOWED_HOSTS = []
ALLOWED_HOSTS = ["*"]
---
배포시에는 ALLOWED_HOST = ["*"]
혹은 더 specific 하게 정의 필요
```

### TEMPLATES 

```
TEMPLATES = [
    {
        "BACKEND": "django.template.backends.django.DjangoTemplates",
        "DIRS": [BASE_DIR / "templates"],
        "APP_DIRS": True,
        "OPTIONS": {
            "context_processors": [
                "django.template.context_processors.debug",
                "django.template.context_processors.request",
                "django.contrib.auth.context_processors.auth",
                "django.contrib.messages.context_processors.messages",
            ],
        },
    },
]

"DIRS": []
"DIRS": [BASE_DIR / "templates"]
---
일반적인으로 추천되는 Django-project 구조를 위해선  "DIRS": [BASE_DIR / "templates"] 필요
```

### STATIC

```
STATIC_URL = "static/"
STATICFILES_DIRS = [BASE_DIR / 'static']
---
일반적인으로 추천되는 Django-project 구조를 위해선  STATICFILES_DIRS = [BASE_DIR / 'static'] 필요
```

### LANGUAGE_CODE & TIME_ZONE

```
LANGUAGE_CODE = "ko-kr"
TIME_ZONE = "Asia/Seoul"
---
필요하다면...
```

<hr>

## MySQL

```
DATABASES = {
    "default": {
        "ENGINE": "django.db.backends.mysql",
        "HOST": "localhost",
        "PORT": "3306",
        "NAME": "my_django_db",
        "USER": "jackmappotion",
        "PASSWORD": "1234",
    }
}
---
host localhost의
port 3306의
database my_django_db에
user jackmappotion과
password 1234로 접속하자.
```