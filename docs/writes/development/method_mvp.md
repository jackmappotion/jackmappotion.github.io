---
layout: default
title: 방법론_MVP
parent: Development
grand_parent : Writes
---
# Minimum Viable Product

- TOC
{:toc}
---

## Minimum Viable Product의 개념
>   - 최소 기능 제품
>       - 프로젝트를 헤비하게 개발하기 전에 최소 필요 기능을 갖춘 제품을 만든다.  

&rarr; `이것이 내가 본능적으로 프로젝트를 진행한 방식과 유사한 부분이 있는것 같다.`

## 나의 Work-Flow

### Project README를 작성한다. (MVP_method_1)
>   - Project의 목표를 작성한다.
>       - Data Source -> Data ETL -> Data Preprocess -> Model 정도 까지의 outline을 생각한다.


### Project의 main_test.ipynb를 만든다. (MVP_method_2)
>   - 여기서 README에 작성한 토대로 러프하게 프로젝트를 Model의 결과물을 볼 수 있는 pipeline을 구현한다.

### Project의 main_test.ipynb를 토대로 코드들을 모듈화하고 나눈다.
>   - 여기서 `__main__.py`를 만든다는 생각으로 코드가 하는 행위들을 리팩토링한다.

### Project의 `__main__.py` 를 실행하고 디버깅한다.


## Conclusion
>   - 내가 진행하는 방식이 이 개념에 부합하는 방식일까?
>       - 꽤나 그러한것 같다.
>   - 이러한 방식을 사용하려면 git-branch 전략을 잘 사용하는 것이 중요할 것이다.

