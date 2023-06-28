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
### 파이프라인(1)

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

### 파이프라인(2)

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

### 파이프라인(3)

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

## Conclusion
>   - 이런식으로 파이프라인 구성한다면 데이터 엔지니어링에선
>   1. DataLoader / Loader / ETL_pipeline
>   2. DataPreprocessor / Preprocessing / PPS_pipeline
>   3. DataModel / Model / MODEL_pipeline
>   4. DataWriter / Writer / RESULT_pipeline  

&rarr; 요런식으로 구성하면 될 것이다.