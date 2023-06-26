---
layout: default
title: kurrant_user
parent: Kurrant
grand_parent : Project
nav_order: 3
---
# 유저주문기반 ML기반 개인화 추천 (4인)
{: .no_toc }
유저들의 주문정보를 기반으로 개인화 추천 시스템을 구축한다.
SK-Planet 기업연계 프로젝트로 데이터엔지니어링 3인의 교육생과 같이 추천시스템을 구축한다.

### WHY
{: .no_toc }
- 기존 시스템: None
    - 유저 만족도 향상과 이후 유저들의 옵션으로 점심 자동 구매 선택할 수 있게 하여 주문누락 실수를 막을 수 있게 된다.
        - 유저 : 실수로 주문하지 못해도 점심식사 가능
        - 기업 : 실수로 주문하지 못해도 점심식사 제공으로 매출 증대 가능 


## Table of contents
{: .no_toc .text-delta }

- TOC
{:toc}

---

## TOOL
> - LANG : Python
>   - DB : mysqlclient / pymysql
>   - DE : pandas
>   - MODEL : sklearn, torch, numpy

## Structure

```

└── Model
    ├── Content_Based_Model
    ├── Hyrid_Based_Model
    │    ├── Content_Based_Model
    │    └── Boosting_Based_Model
    │        └── CTR_Based_Model
    └── DeepLearning_Based_Model
         ├── Neural Collaboarative Filtering
         └── CTR_Based_Model

```

## ToDo
### 요구사항
>   - 유저의 앱사용 만족도를 높이는 추천 시스템을 제공한다.
>       - 추천하는 메뉴가 너무 수렴하지 않도록 한다.
>       - 다양한 메뉴를 추천할 수 있으며 또한 유저의 취향을 반영한다.

### 추천시스템에 사용할 모델들을 찾고 필요 데이터를 선택한다.
>   - 모든 모델은 장단점이 있고 필요한 데이터가 다를 수 있다.
>       - 각자 사용하기에 적합하다고 생각하는 모델들을 찾아본다.
>           - Content_Based : 빠른 속도 보장 ,직접적인 연관성 확인 가능 / 추천이 단순하여 수렴가능성이 증가, 시계열적인 해석 불가
>           - Hybrid_Based : 추천의 다양성 확보 가능, Cold_Start 문제에 대해서도 어느정도 cover 가능 
>           - DeepLearning_Based : 추천의 다양성 확보 가능, 시계열적인 해석 가능 / Cold_start 문제에 취약

### 같이 사용할 DataBase를 구성한다.
>   - 상용 DB에 직접 접근 하는 인원이 많아지는것은 위험 할 수 있다.
>       - DataEnginner용 DB를 따로 구성한다.
>       - NaverCloud의 Ubunutu 서버를 활용하도록 한다.
>   - 상용 DB의 데이터 중 필요한것으로 선택된 데이터들을 Airflow를 활용하여 가져온다.


### 데이터 전처리하여 제공한다.
>   - Backend에서도 static하게 활용되고 있는 mapper들을 받아서 전처리 하여 팀원들에게 제공한다.

### 모델의 결과물의 format을 고정한다.
>   - 다양한 모델들의 결과물 형식을 미리 정해주지 않으면 결과물을 함께 활용하기 까다롭다.
>       - 각각의 모델들을 동시에 사용하여 의사결정하기 위해 결과물의 format을 고정한다.


## Problems & Learned

### Airflow M1 관련 문제
>   - M1 local에서 airflow를 돌리는 과정에서 시간이 거의 소모되지 않을 dag도 굉장히 오랜시간이 걸렸다.
>       - M1에서 airflow를 돌리는것이 가능하나 아직 최적화가 되지 않았던 때였던것 같다.

&rarr; 클라우드 서버를 하나 구해 ubuntu amd64 에서 돌리니 합리적인 속도로 돌아갔다.

### AWS 쿼리 비용 문제
>   - 초기 데이터 EDA 과정에서 DB를 살펴보는 과정에서 상용 DB에 너무 많은 쿼리를 사용하는 일이 생겼다.
>       - 많이 들었던, DB 폭탄을 직접 겪게 되는 문제가 생겼다.
>       - 항상 상용 DB에 query를 던지는 것은 주의 해야한다.

&rarr; 이후 따로 서버를 하나 구성하여 DE-DB를 만들어 이곳에서 DB를 분석하는것으로 문제를 해결했다.

### 모델 제공에 대한 문제
>   - 데이터 엔지니어링 프로젝트로 진행하였으나, 현업에 Python을 사용하는 backend 엔지니어가 없었다.
>       - 그럼 Java spring 서버 내부에 python code를 불러서 사용해야 하는데 이것은 보안의 문제가 많았다.
>       - 모델의 결과물을 제공하기 위해서는 아무래도 python backend가 가능한 자원이 필요했다.

&rarr; 내가 Django를 공부하고 이를 바탕으로 parameter 수정 및 모델 배포가 가능토록 했다.

## Conclusion
>   - 모델 배포를 위해서 백엔드 역량은 필수적이다.
>       - 또한 모델 배포과정을 알아야지 두 번 일하지 않게 모델을 구현할 수 있다.
>   - 상용에 적용할때 모델 학습 및 적용 시간은 아주 중요한 factor이다.
>       - 실제 서비스는 time-dependent하다.

