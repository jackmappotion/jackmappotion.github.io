---
layout: default
title: kurrant_release
parent: Kurrant
grand_parent : Project
nav_order: 4
---
# 프로젝트 배포
{: .no_toc }
ML-MODEL 결합 &rarr; ML-Model 결합 &rarr; ML-server 생성 -> Docker image 구축

## Table of contents
{: .no_toc .text-delta }

- TOC
{:toc}
---

## TOOL
>   - GCP
>       - Compute Engine
>           - VM Instance 
>   - Airflow
>       - DB-ETL pipeline
>   - Docker
>       - Python-Django


## Structure

```
GCP :
    root@mygcp
    OS: Ubuntu 20.04.6 LTS x86_64
    Host: Google Compute Engine
    Kernel: 5.15.0-1036-gcp
    Shell: zsh 5.8
    Terminal: /dev/pts/1
    CPU: Intel Xeon (2) @ 2.200GHz
    Memory: 1124MiB / 7934MiB

Docker :
    python3
    Port 8000

Airflow :
    Port : 8080
```

## ToDo
### GCP 세팅
>   - GCP의 VM instance를 구성한다.
>       - 친숙한 ubuntu를 사용하였다.
>       - Portforwarding 진행 
>           - 3306 -> mysql 용도
>           - 8000 -> Django 용도
>           - 8080 -> Airflow 용도

### MySQL 세팅
>   - Kurrant user를 하나 생성하여 kurrant_de에 접속 가능하도록 한다.
>   - 외부접속 가능하도록 bind-address 도 열어준다.

### Dockerfile 작성
>   - ml-model을 넣고 dashboard와 함께 완성된 ml-server를 docker image로 만든다.
>       - image를 dockerhub에 push 한다.

## Problems & Learned
### GCP 접속
>   - GCP는 vm instance 생성하고 접속하는 방식이 여러가지가 있다.
>       - github 하는것 처럼 ssh key를 업로드 하고 접속
>       - root 계정을 열어서 개인 비밀번호를 생성하여 접속
>       - ...
>   - 이게 클라우드 별로 접속하는 방식이 다양한듯 한데 이런 방식이 default인듯 하다.
>       - AWS : pem을 이용한 접속
>       - GCP : rsa.pub key로 접속 / root password 설정 후 custom password 접속
>       - NaverCloud : 관리자 password를 생성하여, 생성된 password로 접속

&rarr; 첫 GCP vm instance 사용이라 약간 해맸으나 꽤나 편리한듯 하다.

### Dockerfile 이미지 생성
>   - mysqlclient설치에 오류가 발생해서 conda로 설치 했었다.
>       - pip freeze > requirements.txt; pip install -r requirements.txt 로 dockerfile을 구성하려면 결국 pip으로 mysqlclient를 설치해야 했다.  
>        &rarr; apt install python3-dev default-libmysqlclient-dev build-essentia 
>   - docker run os error
>       - m1 mac에서 build 하고 amd64에서 도커 컨테이너를 돌리려고 하니 오류가 발생했다.

```
        docker 
            buildx 
            build 
                --push 
                --platform linux/amd64 
                --tag [asdf]
```

&rarr; 이런식으로 platform을 설정해주어 해결했다.

### Airflow 경로 설정
>   - DB-pipeline 을 만드는 과정에서 airflow dump를 저장하는 path를 구하는데 고생을 했다.
>       - /tmp/airflow..에 잠시 폴더가 생겼다가 사라지는것으로 보이는데
>       - 이를 경로 설정해주고 진행해야지 airflow dag 결과물을 저장할 수 있었다.
>   - 추가적으로 이와 관련하여 

```
my_script.sh

echo "1st param = $1"
echo "2nd param = $2"
echo "3rd param = $3"

--
> sh my_script.sh 11 22 33 
1st param = 11
2nd param = 22
3rd param = 33
```

&rarr; 이런식으로 shell sciprt에 변수를 넘겨줄 수 있어서 bashscript 결과물을 적당한 path로 보내는것이 가능하다.

## Conclusion
>   - buildx를 사용하여 docker-container를 실행할 os에 맞게 이미지를 배포하는것을 배웠다.
>   - 처음 GCP를 사용해 보았는데 AWS보다 내 취향인것 같다 
>       - 이제 AZURE만 써보면 3대 클라우드는 다 접한것 같다 
>   - 좀 더 긴 파이프라인을 airflow로 돌려보고 싶다 아직까지는 crontab으로 관리 가능한 수준의 pipeline인듯 하다.


