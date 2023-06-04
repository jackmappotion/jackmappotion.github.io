---
layout: default
title: reco_corporation_kurrant
parent: Project
---
# reco_corporation_kurrant
{: .no_toc }
기업들의 식단현황을 기반으로 기업 식단배정을 자동화하는 시스템을 구축한다.
{: .fs-6 .fw-300 }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---
## Why

- 기존 시스템: 운영팀에서 회의를 통해 기업들의 식단을 배정하는 시스템

서비스 사용 기업의 수가 늘어날 수록 고려해야할 사안은 지수적으로 증가합니다.

- 고려할 사안:
  1. 음식점의 특성이 적당한 중복을 피했는가?
  2. 배송 거리 및 경로를 최적화할 수 있는 음식점을 선택했는가?.
  3. 기업의 지원금 및 음식점별 캐파를 고려해야 했는가?
  4. 각 기업의 요구사항을 만족하는 배정이었는가?

->
**ML기반으로 자동화를 진행하자!**

## How

### 1. DataBase를 구축한다.
1. 기업내부에 아직까지 데이터팀이 없는 상태에서 프로젝트를 시작하게 되었다.
    - 백엔드에서 사용하는 상용DB가 있지만 이 DB를 직접 활용하는것은 안전하지 않다.
    - 데이터엔지니어링을 위해 사용할 DB를 구축한다.
    ```
    DB Config
    location : Local 
    OS : Window
    Tool_DB : MySQL (상용 DB와 같은 종류의 DB를 사용한다.)
    
    ps. 사내 IP에 포트포워딩을 통해 내부 IP에서만 접속 가능하도록 하고 먼저 진행한다.
    ```

### 2. DataBase Analysis를 진행한다.
1. 사용 DataBase는 정규화가 많이 진행되어 있으며 많은 Mapping이 되어있다.
    - 이를 적절하게 해석하여 Data-Engineer용 DB에 넣을 데이터 및 테이블을 설계한다.
    - 테이블의 description을 보아도 좋지만, Backend 자원과 지속적인 의사소통을 통해 설계를 진행한다.
    ```
    Ex)
    query = """
        SELECT
            oo.id as order_id,
            oo.user_id,
            fdf.group_id,
            fdf.food_id,
            fdf.service_date
        FROM
            order__order oo
        JOIN(
        SELECT
            id as order_item_id,
            order_id
        FROM
            order__order_item
        ) ooi
        ON
            oo.id = ooi.order_id
        JOIN(
            ...
        )
        --
        이런식으로 테이블 명만으로는 이해하기 힘든 부분은 Backend 분들의 도움이 필요하다.
    ```


### 3. DataPipeline을 구축한다.
1. 데이터 엔지니어링을 위한 DB를 구축했다면 상용 DB의 데이터중 필요한 데이터를 가져올 수 있는 DataPipeline을 구축해야 한다.
    - 수동이 아닌 자동으로 실행되는 DataPipeline을 구축하는것은 처음하는 일이었다.
    - Apache airflow를 통해 DataPipeline을 구축하기로 한다.
    ```
    PipeLine Config
    location : Cloud (NaverCloud)
    OS : Ubuntu 18.04
    Tool_Pipeline : Airflow
        schedule_interval : 0 5 * * 1 (매주 5AM 진행)
    ```

### 4. Data EDA를 진행한다.
1. 데이터 엔지니어링을 통한 의사결정이 처음 진행되는 부분이 있기 때문에 아직까지는 데이터의 정합성이 상당히 부족했다.
    - 데이터 EDA를 통해 의심가는 데이터들에 대한 확인을 운영팀과 함께 진행한다.
    - 문제가 있는 부분은 데이터를 수정하거나, 비즈니스 문제가 엮여있을 경우 데이터 엔지니어링시 하드코딩 할 수 있도록 기록한다.
    ```
    EDA Checklist
    - 음식점
        - 음식점의 주소 
        - 음식점의 Capacity(일일 가능 수량)
        - 음식점의 상태 (계약중/ 계약종료)
        - 음식점의 샐러드 가능 여부
            - 샐러드를 필수로 제공하야하는 기업이 있기에 체크한다.
    - 기업
        - 기업의 주소
        - 기업의 Capacity(인원 수)
        - 기업의 상태 (계약중/ 계약종료)
    ... 
    ```
### 5. 문제의 특성에 따른 분리
1. 큰 문제를 해결할 때 작은 문제들의 특성이 모두 동일 하지 않다.
```
    - 데이터 엔지니어링으로 해결해야하는 문제
        - Ex)
            - 배송경로최적화 
            - 기업의 구성원의 특징에 따른 식단추천
    - 알고리즘으로 해결해야하는 문제
        - Ex)
            - Capa 관련 문제 (음식점의 일일 가능 수량 > 기업 총 구성원의 수)
            - 사용일 관련 문제 (음식점이 제공 가능 요일, 기업 서비스 사용 요일)
```    

### 6. 개발진행
1. 이제 개발을 진행한다.
```
    1. ipynb에서 러프하게 진행
    2. ipynb에서 클린하게 진행
    3 필요한 것들 모듈화
    4. py파일로 변환
```
이런식으로 진행하는것이 가장 나에게 가능한 방식이었다.