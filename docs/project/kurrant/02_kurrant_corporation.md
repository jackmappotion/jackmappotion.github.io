---
layout: default
title: kurrant_corporation
parent: Kurrant
grand_parent : Project
nav_order: 2
---
# 기업식단 ML기반 최적화 배정
{: .no_toc }
기업 식단배정을 자동화하는 시스템을 구축한다.

### WHY
{: .no_toc }
- 기존 시스템: 운영팀에서 회의를 통해 기업들의 식단을 배정하는 시스템
    - 서비스 사용 기업의 수가 늘어날 수록 고려해야할 사안은 지수적으로 증가

{: .fs-6 .fw-300 }

## Table of contents
{: .no_toc .text-delta }

- TOC
{:toc}

---

## TOOL
> - LANG : Python
>   - DB : mysqlclient / pymysql
>   - DE : pandas
>   - MODEL : sklearn, numpy, collections, itertools, geopy ...
>   - API : requests

## Structure

```
├── load_data
│   ├── 고객사 정보
│   ├── 음식점 정보
│   ├── 음식 정보
│   ├── 식단현황 정보
│   └── 식단추천현황 정보
│
├── preprocess_data
│   ├── 고객사 정보
│   │    └── 데이터 수정 및 추가 (하드코딩)
│   │        ├── DB 데이터를 수정할 수 없으나 모델에 넣을 때 수정해야하는 데이터 (이후 -> ml_server에서 수정 가능토록함)
│   │        └── DB에 없는 데이터이지만 모델에서 고려되어야 하는 데이터  (이후 -> ml_server에서 추가 가능토록함)
│   ├── 음식점 정보
│   │    └── 데이터 수정 및 추가 (하드코딩)
│   │        ├── DB 데이터를 수정할 수 없으나 모델에 넣을 때 수정해야하는 데이터 (이후 -> ml_server에서 수정 가능토록함)
│   │        └── DB에 없는 데이터이지만 모델에서 고려되어야 하는 데이터 (이후 -> ml_server에서 추가 가능토록함)
│   └─── General
│        ├── 중복 제거
│        ├── bit -> int
│        ├── null값 처리
│        ├── String -> list -> multilabelbinarizer
│        └── ...
│
├── Model
│   ├── 거리 기반 모델 (배송거리 최적화 기반 모델)
│   │    ├── ver_1
│   │    │   ├── 주소 -> 좌표 (Kakao map API)
│   │    │   └── 좌표 -> 거리계산 (geopy)
│   │    └── ver_2
│   │        └── 주소 -> 배송최적화 (SK T-MAP API)
│   │
│   ├── 고객사 음식 선호도 기반 모델 
│   │    ├── 고객사
│   │    │   └── 고객사 식단 현황을 통한 고객사 음식 성향 수치화
│   │    └── 음식점
│   │        └── 음식점 가능한 음식정보를 통한 음식점 음식 성향 수치화
│   │
│   └── 식단 중복도 기반 모델
│        ├── 과거 식단현황 기반 중복도 
│        │   └── 음식점들이 최근에 식단현황에 배치되었다면 disadvantage를 적절하게 부여
│        │       ├── 최근 배치된 날짜
│        │       └── 음식점들이 제공가능한 음식의 다양성
│        ├── 당일 추천현황에 따른 중복도
│        │   └── 음식점들이 당일 식단현황에 배치되었다면 disadvantage를 적절하게 부여
│        │       ├── 음식점들의 일일 가능 수량
│        │       ├── 음식점들의 신뢰도 (음식준비완료 시간에 대한 신뢰도)
│        │       └── 음식점들의 일일 가능 한 메뉴 다양성
│        │
│        └── 음식점
│            └── 음식점 가능한 음식정보를 통한 음식점 음식 성향 수치화
├── Assignment
│   ├── Parameter 
│   │    ├── 날짜 
│   ├── Filtering
│   │    ├── 날짜 
│   │    │   ├── 고객사의 사용요일
│   │    │   └── 음식점의 가능요일
│   │    ├── 고객사 <-> 음식점
│   │    │   ├── 고객사가 배제를 부탁한 음식점
│   │    │   ├── 고객사가 필수로 요구한 음식점
│   │    │   ├── 고객사가 필요하다고 한 foodtype을 가진 음식점 (샐러드, 한식, 국물요리 등등..)
│   │    │   └── 고객사와 음식점 간의 거리가 limit 이하인지 확인
│   ├── RunModel
│   │    ├── 거리기반 모델 
│   │    ├── 음식선호도 기반 모델
│   │    └── 중복도 기반 모델
├── Visualize
│   ├── Pivot Table
└── Save
    └── 식단 현황 최신화 (배정한 것을 이후 배정에 가져와 사용하도록)

```

## ToDo
### DataBase 구축
>       - 기업내부에 아직까지 데이터팀이 없는 상태에서 프로젝트를 시작하게 되었다.
>           - 백엔드에서 사용하는 상용DB가 있지만 이 DB를 직접 활용하는것은 안전하지 않다.
>           - 데이터엔지니어링을 위해 사용할 DB를 구축한다.

```
    DB Config
    location : Local 
    OS : Window
    Tool_DB : MySQL (상용 DB와 같은 종류의 DB를 사용한다.)
    
    ps. 사내 IP에 포트포워딩을 통해 내부 IP에서만 접속 가능하도록 하고 먼저 진행한다.
```

<hr>

### DataBase Analysis
>       - 사용 DataBase는 정규화가 많이 진행되어 있으며 많은 Mapping이 되어있다.
>           - 이를 적절하게 해석하여 Data-Engineer용 DB에 넣을 데이터 및 테이블을 설계한다.
>           - 테이블의 description을 보아도 좋지만, Backend 자원과 지속적인 의사소통을 통해 설계를 진행한다.

```
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

<hr>

### DataPipeline 구축
>       - 데이터 엔지니어링을 위한 DB를 구축했다면 상용 DB의 데이터중 필요한 데이터를 가져올 수 있는 DataPipeline을 구축해야 한다.
>           - 수동이 아닌 자동으로 실행되는 DataPipeline을 구축하는것은 처음하는 일이었다.
>           - Apache airflow를 통해 DataPipeline을 구축하기로 한다.

```
pipline_config
    service : Cloud (NaverCloud)
    OS : Ubuntu 18.04
    pipeline_tool : Airflow
        schedule_interval : 0 5 * * 1 (매주 5AM 진행)
```

> - 추가적으로 Naver cloud에서 airflow를 진행하기에, local db를 cloud로 mirgrate한다.

```
    DB Config
    location : Cloud
    OS : Ubuntu 18.04
    Tool_DB : MySQL (상용 DB와 같은 종류의 DB를 사용한다.)
    
    ps. 사내 IP에 포트포워딩을 통해 내부 IP에서만 접속 가능하도록 한다.
```


<hr>

### Data EDA 진행
>       - 데이터 엔지니어링을 통한 의사결정이 처음 진행되는 부분이 있기 때문에 아직까지는 데이터의 정합성이 상당히 부족했다.
>           - 데이터 EDA를 통해 의심가는 데이터들에 대한 확인을 운영팀과 함께 진행한다.
>           - 문제가 있는 부분은 데이터를 수정하거나, 비즈니스 문제가 엮여있을 경우 데이터 엔지니어링시 하드코딩 할 수 있도록 기록한다.


<hr>

### 모델 구현
>       - 한 개 이상의 모델들을 구현하고 이들의 결과물을 적당히 결합하여 최종 의사결정을 진행한다.
>           - 거리기반 모델
>               - limit 이하에 대해서는 penalty 부여
>               - limit 이상은 배제
>           - 선호도 기반 모델
>               - 선호도가 높을 수록 advantage 부여
>           - 중복도 기반 모델
>               - 음식점이 제공 가능한 음식 다양성에 따른 과거중복도 penalty 계산
>               - 음식점이 일일 가능량에 따른 일일중복도 penalty 계산

<hr>

## Problems & Learned
### DB 해석 및 이해
>   - 실제 기업에서 사용하는 상용-DB를 본것은 처음이었다.
>       - 테이블이 정말 많아서 필요한 데이터들을 찾기가 까다로웠다.
>          - Description을 통한 테이블 이해하는 능력
>          - 백엔드 분들과 DB를 설계할때 생각하신 방식들을 얘기하는것이 이해에 큰 도움이 되었다.

### 길어지는 코드
>   - 실제 비즈니스에 적용하기 위해서는 상당히 많은 예외처리와 상황 제어가 필요했다.
>       - 이렇게 긴 코드를 작성하고 관리하는 경험이 처음이라 나의 코드의 질 떨어짐을 많이 느꼈다.
>          - 유지보수와 가능한 코드를 위해 다양한 프로그래밍 패러다임을 공부하는 계기가 되었다.
>             - java spring을 공부하며 배운 코딩 스타일
>             - 객체지향 프로그래밍
>             - 함수형 프로그래밍

### 내가 없을때 돌아갈 수 있는 코드
>   - 내가 실행하지 않아도 돌아가는 코드
>   - 파라미터가 변해도 돌아가는 코드
>       - ML이 메인이었던 내게 이러한 부분 큰 부담으로 다가왔다.
>          - 운영팀에서 내 모델을 이용한다고 하기엔 너무나도 코드들이 물렁해 보였다.
>          - 단단하고, robust한 스타일의 코딩을 지향해야 한다는것을 뼈저리게 느낄 수 있는 좋은 계기였다.

## Conclusion
> - 길어지는 코드를 manage하는 방법에 대한 고민을 하는 계기
>   - 단단한 파이썬 코드를 쓰자
>   - 직관적으로 기능을 알 수 있는 함수명을 쓰자.
>   - class를 잘 활용하자.

> - 상용-DB에서 필요한 데이터들을 추출하는 까다로운? query들을 작성해보는 계기
>   - 역시나 query는 중요했다.
>   - 또한 DB를 효과적으로 이해하기 위해서는 설계를 하신 backend분들과의 소통이 중요하다.