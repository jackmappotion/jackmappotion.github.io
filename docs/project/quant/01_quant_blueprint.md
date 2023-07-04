---
layout: default
title: Quant_Blueprint
parent: Quant
grand_parent : Project
nav_order: 1
---
# Quant 프로젝트 청사진을 설계한다. 
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

- TOC
{:toc}
---

# Minimal Viable Product를 구현한다.

## Data Source는 어떻게 구현할것인가
>   - 일단 pyupbit library로 진행
>       - 이후 open_api를 통해 직접 구현하고 외부라이브러리 의존을 낮춘다.

## 어떤 종목을 선택할 것인가?
>   - 일단 pass 
>       - bitcoin으로 진행

## 어떤 데이터로 어떤 데이터를 예측할 것인가?

```
DATE  |  close  | close_diff | close_diff_level
day_1 |   100   |   None     |       None
day_2 |   101   |   1        |        상방
day_3 |   103   |   2        |        상방
day_4 |   100   |   -3       |        하방
day_5 |   99    |   -1       |        하방
---
<!-- 여기서 close_diff 를 regression 으로 -->
여기서 close_diff_level 을 classification 으로 예측하는것으로 한다.
```

## 어떤 모델을 사용하는가
>   - sklearn.linear_model.LogisticRegression
>   - sklearn.tree.DecisionTreeClassifier
>   - sklearn.svm.LinearSVC

## 어떤 것을 보여줄 것인가.
>   1. 모든 모델에 대한 heatmap graph
>   2. 승률 top3 모델에 대한 heatmap graph 

```
실제 하락 : (예측 하락 / 예측 상승)
실제 상승 : (예측 하락 / 예측 상승) 

예측 하락 : (실제 하락 / 실제 상승)
예측 상승 : (실제 하락 / 실제 상승)

예측 상승 중 실제 상승 비율 (승률)
```

## 승률 통계에 대한 detail한 설정
>   1. train이 많아지는건 별로 안좋아 보임
>   2. 30 개 row train
>       1. 30 개 row train, column : window
>       2. 예측 1개 -> (True/False)
>       - iter 10
>       - 승률 확인


```
실제 하락 : (예측 하락 / 예측 상승)
실제 상승 : (예측 하락 / 예측 상승) 

예측 하락 : (실제 하락 / 실제 상승)
예측 상승 : (실제 하락 / 실제 상승)

예측 상승 중 실제 상승 비율 (승률)
```

