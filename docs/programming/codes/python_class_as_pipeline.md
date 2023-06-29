---
layout: default
title: Python 클래스를 통한 파이프라인
parent: Codes
grand_parent : Programming
---
# 클래스를 통한 파이프라인 구성

- TOC
{:toc}
---

## Why
>   - 기업연계프로젝트를 진행하며, 프로젝트가 실제 업무에 유용하게 사용되기 위해서는
>       - 예외처리 할 것이 많다.
>       - 비즈니스 로직상 데이터를 하드코딩하여 편집할 부분이 생긴다.
>       - 처리할 것들이 많아지고 코드가 nested해지기 쉽다.

&rarr; *결론적으로 코드가 길어진다!*
>   - 길어지는 코드를 관리하는 과정에서 class를 활용하게 되었다.
>       - 관련하여 class를 pipeline으로 사용하는 방법론을 남겨두려고 한다.


## How
### Naive한 예시
#### Naive 파이프라인 (1개)

```
class MyClass1:
    @staticmethod
    def print_a():
        print("a")

    @staticmethod
    def print_b():
        print("b")

    def __call__(self):
        self.print_a()
        self.print_b()
        return "MyClass1"
---
my_class_1 = MyClass1()
arg = my_class_1()
---
a
b
```

#### Naive 파이프라인(2개)

```
class MyClass1:
    @staticmethod
    def print_a():
        print("a")

    @staticmethod
    def print_b():
        print("b")

    def __call__(self):
        self.print_a()
        self.print_b()
        return "MyClass1"


class MyClass2(MyClass1):
    @staticmethod
    def print_c():
        print("c")

    @staticmethod
    def print_d():
        print("d")

    def __call__(self):
        arg = super().__call__()
        print(arg)
        self.print_c()
        self.print_d()
        return "MyClass2"
---
my_class_2 = MyClass2()
arg = my_class_2()
---
a
b
MyClass1
c
d
```

#### Naive 파이프라인(3개)

```
class MyClass1:
    @staticmethod
    def print_a():
        print("a")

    @staticmethod
    def print_b():
        print("b")

    def __call__(self):
        self.print_a()
        self.print_b()
        return "MyClass1"


class MyClass2(MyClass1):
    @staticmethod
    def print_c():
        print("c")

    @staticmethod
    def print_d():
        print("d")

    def __call__(self):
        arg = super().__call__()
        print(arg)
        self.print_c()
        self.print_d()
        return "MyClass2"


class MyClass3(MyClass2):
    @staticmethod
    def print_e():
        print("e")

    @staticmethod
    def print_f():
        print("f")

    def __call__(self):
        arg = super().__call__()
        print(arg)
        self.print_e()
        self.print_f()
        return "MyClass3"
---
my_class_3 = MyClass3()
arg = my_class_3()
---
a
b
MyClass1
c
d
MyClass2
e
f
```


### Data-Engnieering에서 pipeline 예시
>   - 굳이 뭐 꼭 `__call__`에서 실행 해야하는것은 아님
>       - `__init__`에서 실행하고 진행되어도 양호하다.

```
from sklearn.linear_model import LinearRegression


class Preprocessor:
    def __init__(self, df) -> None:
        self.raw_df = df
        self.df = df

    def append_mean_col(self):
        df = self.df
        df["mean"] = df.mean(axis=1)
        self.df = df
        return None

    def append_high_low_diff_col(self):
        df = self.df
        df["high_low_diff"] = df.apply(lambda x: x.high - x.low, axis=1)
        self.df = df
        return None

    def __call__(self):
        self.append_mean_col()
        self.append_high_low_diff_col()
        return self.df


class Model(Preprocessor):
    def __init__(self, df, model) -> None:
        super().__init__(df)
        self.df = super().__call__()
        self.model = model
        self.run_model()

    def run_model(self):
        model_obj = self.model()
        df = self.df

        feature_cols = [col for col in df.columns if col != "mean"]

        x = df.loc[:, feature_cols]
        y = df.loc[:, "mean"]
        model_obj.fit(x, y)
        self.model_obj = model_obj
        return model_obj

    def __call__(self, x_test):
        model = self.model_obj
        y_pred = model.predict(x_test)
        return y_pred


model = Model(df, LinearRegression)
model([[1, 2, 3]])
```
## Naming Convention에 관련한 얘기
### Generally 함수 

```
def my_function_a():
    ...
---
함수는 snake_case
```
### Generally 클래스

```
class MyClassA:
    def __init__(self,*args):
        ...
---
클래스는 UpperCamelCase
```
### Pipeline 클래스
>   - 내가 만든 concept의 클래스를 일반적인 객체지향을 위한 클래스와 다른 네이밍 컨벤션이 필요
>       - WHY: class 이름이 길어짐
```
class MY_PIPELINE_CLASS:
    ...
---
내가 사용할 
파이프라인을 위한 클래스는 SCREAM_SNAKE_CASE를 사용하겠다.
```


## Conclusion
>   - 이런식으로 파이프라인 구성한다면 데이터 엔지니어링에선
>   1. DataLoader / Loader / ETL_pipeline
>   2. DataPreprocessor / Preprocessing / PPS_pipeline
>   3. DataModel / Model / MODEL_pipeline
>   4. DataWriter / Writer / RESULT_pipeline  

&rarr; 요런식으로 구성하면 될 것이다.
