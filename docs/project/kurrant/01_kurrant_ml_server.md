---
layout: default
title: kurrant_ml_server
parent: Kurrant
grand_parent : Project
nav_order: 1
---
# ML모델을 배포를 위한 서버 개발
{: .no_toc }
ML모델을 배포할 수 있는 서버를 개발한다.
{: .fs-6 .fw-300 }

## Table of contents
{: .no_toc .text-delta }

- TOC
{:toc}
---
## TOOL
> - Python
>   - Django
>   - mysqlclient
>   - pandas

## Structure

```
├── dashboard
│   ├── client
│   ├── foodschedule
│   └── makers
├── models
│   ├── foodassignment
│   └── makersassignment
├── static
└── templates
    ├── base.html
    ├── navbar.html
    ├── dashboard
    │   ├── client
    │   ├── foodschedule
    │   └── makers
    └── models
        ├── foodassignment
        └── makersassignment
```

## ToDo
>   - DashBoard 구현
>       - 상용 DB에서는 변경하기는 비즈니스적으로 허용되지 않지만, 모델에 넣을때는 변경해야하는 데이터들이 존재
>       - 이를 하드코딩으로 대체 해야하는데 이를 DashBoard를 구현하여 가능토록 한다.
>           - 고객사 DashBoard
>           - 음식점 DashBoard
>           - 식단현황 DashBoard
>               - 식단 현황
>               - 식단 추천 현황
>               - 유저 개인화 추천 현황
>   - MODEL 구현
>       - 식단 자동화 추천
>       - 유저 개인화 추천

## Problems & Learned
### APP 내부 APP을 구현하기

```
urlpatterns = [
    path("", index),
    path("makersassignment/", include("models.makersassignment.urls")),
    path("foodassignment/", include("models.foodassignment.urls")),
]
```


### 수정한 데이터를 저장하는 javascript
>   - 웹으로 보여준 데이터를 수정 가능하도록 하고 저장 가능하도록 해야한다.
>       - Django를 사용하여 rest-api정도만 구현해 보았지 javascript를 사용한것이 처음이었다.
>       - 배워서 진행한다

```
function saveTableData(tableid,dining_type) {    
    var table = document.getElementById(tableid);
    var rows = table.getElementsByTagName("tr");
    var columnNames = table.getElementsByTagName("th");
    var data = [];

    for (var i = 1; i < rows.length; i++) {
        var cells = rows[i].cells;
        var rowData = {};
        for (var j = 0; j < cells.length; j++) {
            var columnName = columnNames[j].textContent;
            var cellValue = cells[j].textContent;
            rowData[columnName] = cellValue;
        }
        data.push(rowData);
    }

    // Send the data to the view using AJAX
    var csrfToken = document.querySelector('[name=csrfmiddlewaretoken]').value;
    var url = 'update/'+dining_type;
    var xhr = new XMLHttpRequest();

    xhr.open("POST", url, true);
    xhr.setRequestHeader("Content-Type", "application/json");
    xhr.setRequestHeader("X-CSRFToken", csrfToken);
    xhr.onreadystatechange = function () {
        if (xhr.readyState === 4 && xhr.status === 200) {
            // Handle the response from the view if needed
            console.log(xhr.responseText);
        }
    };
    xhr.send(JSON.stringify(data));
}
```

### 데이터는 수정 하지만 DB update는 제한하기
>   - 데이터를 수정해야하지만 그것이 DB에 저장되면 안됨
>       - 이 프로젝트는 상용-DB를 주기적으로 dump해 오는 ML-DB와 연결된다.
>       - 그러므로 웹에서 데이터를 수정받아 DB말고 local에 저장해야한다.

```
A) [상용 DB] -> [ML DB]
B)             [Local DATA]

A + B => WEB DATA
```


## Conclusion
> - Django기반의 데이터 처리 및 모델 배포 방식 습득
> - base.html <-> templates의 조합 방식에 익숙해지는 계기
> - java_spring에서 사용하던 MVC pattern에 service_layer를 추가한 방식의 아키텍처를 통한 장고 프로젝트 구현
