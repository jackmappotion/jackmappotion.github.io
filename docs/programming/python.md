---
layout: default
title: Python
nav_order: 1
parent: Programming
---

# Python
{: .no_toc }

{: .fs-6 .fw-300 }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## DefaultDict

```
from collections import defaultdict

my_default_dict = defaultdict(lambda : [], dict())

for i in range(10):
    for j in range(10):
        my_default_dict[i].append(j)

뭐 작업하다 보면 defaultdict를 사용하게 될일이 종종 있는데

이때 만약 key가 없으면 넣은 key값을 그대로 배출하고 싶을때가 있다
-> 이때 defaultdict로 작업하기 좀 까다로운데
class CustomDict(dict):
    def __missing__(self,key):
        return key
요렇게 하나 만들어서 쓰니 유용하다
```

## Django - pandas 

```
my_df = pd.DataFrame(...)

def index(request):
    my_df = pd.DataFrame(...)
    context = {
        "my_df_html":my_df.to_html()
    }
    return render(
            request=request,
            template_name="/my_template/template.html",
            context=context)
```

## Airflow 

```
db_dump_dag.py


import pendulum
from datetime import datetime
from airflow.models import DAG
from airflow.operators.bash import BashOperator
from airflow.operators.dummy import DummyOperator


BASE_DIR = os.path.dirname(__file__)

KST = pendulum.timezone("Asia/Seoul")
args = {'owner': 'kurrant'}

dag  = DAG(dag_id='my_dag',
        default_args=args,
        start_date=datetime(2021, 2, 8, tzinfo=KST),
        schedule_interval='0 0 * * *',
        catchup = False
        )

task_0 = BashOperator(
    task_id = "path",
    bash_command=f"echo {BASE_DIR} : ; pwd",
    dag=dag,
        )

task_1 = BashOperator(
    task_id="dump_db",
    bash_command=f'mysqldump -h localhost -u admin -p1234 kurrant_de > {BASE_DIR+"/db_dumps/"}"$(date +\%Y_\%m_\%d).sql"',
    dag=dag,
        )
task_2 = BashOperator(
    task_id="load_dump",
    bash_command=f'mysql -u admin -p1234 airflow_checker <  {BASE_DIR+"/db_dumps/"}"$(date +\%Y_\%m_\%d).sql"',
    dag=dag,
        )

task_0>>task_1 >> task_2
```

## 멋진 코드

```
df['col_1']=df['col_2'].map(my_dict).fillna(df['col_1'])
```

## Pandas Rolling

```
rolling은 좀 난해한 느낌이 있는데,

df["Open_diff"] = df["Open"].rolling(window=2).apply(lambda x: x.iloc[1] - x.iloc[0])
df["High_diff"] = df["High"].rolling(window=2).apply(lambda x: x.iloc[1] - x.iloc[0])
df["Low_diff"] = df["Low"].rolling(window=2).apply(lambda x: x.iloc[1] - x.iloc[0])
df["Close_diff"] = df["Close"].rolling(window=2).apply(lambda x: x.iloc[1] - x.iloc[0])

이렇게 되면 cur_row + 1 - cur_row 값을 계산한다.

직과적으로 다가오지 않는데, 한번 쯤 체크해야할듯
```
