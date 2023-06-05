---
layout: default
title: reco_personal_kurrant
parent: Project
---
# 유저주문기반 ML기반 개인화 추천
{: .no_toc }

유저들의 주문정보를 기반으로 개인화 추천 시스템을 구축한다.
{: .fs-6 .fw-300 }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---
## 왜
- 기존 시스템: None

```
- 기업중식의 경우 주문을 실수로 하지못한 경우가 아니라면 대체로 주문을 진행한다.
- 하지만 실수로 주문을 하지 못하는 경우가 생겨 이는 소비자에게도 회사에게도 손실로 다가온다.

이를 실수로 주문하지 못하는 경우를 대비하여
개인화된 추천시스템을 구축하여 주문을 하지 않았을때 자동구매가 가능한 설정을 추가하여 소비자와 회사에게 도움이 되게한다.
```
## 어떻게
- 1 ~ 4까지는 (기업식단현황 ML기반 최적화 배정)과 동일

[기업식단현황 ML기반 최적화 배정][reco_corporation_kurrant_detail]{: .btn .fs-5 .mb-4 .mb-md-0}

[reco_corporation_kurrant_detail]: /docs/project/reco_corporation_kurrant/

## 한계
> - 백엔드를 제대로 이해하지 못한다면 순간순간 추천결과물을 만들 수 있을 뿐 지속적인 ml service 제공이 불가.
> - 결국 개발이 뭔지 알아야함.
