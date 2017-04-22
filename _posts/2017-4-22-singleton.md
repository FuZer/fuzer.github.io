---
layout: post
title: Singleton Pattern
category: programming
---
<img src="https://developer.apple.com/library/content/documentation/General/Conceptual/DevPedia-CocoaCore/Art/singleton_2x.png" width="600" height="200"><br>
<font color="gray"> developer.apple.com </font>

일반적인 클래스는 인스턴스 요청을 받으면 생성자를 통해 새로운 객체를 만들어
넘겨주는 방식이다. 싱글턴 디자인 패턴은 인스턴스 요청을 받을때 무조건 하나의 인스턴스만
만들고 나머지 요청은 이 인스턴스를 넘겨주는 방식이다.

이런 싱글턴 패턴은 `하나의 자원에서 여러 접근이 필요한` 상황에서 주로 사용된다.
이를 Python 에서는 다양한 방법 으로 구현 가능하다.

### 1. 클래스에서 instance 변수로 관리
```python
class Singleton(object):
    def __new__(cls):  # singleton
        if not isinstance(cls._instance, cls):
            cls._instance = object.__new__(cls)
        return cls._instance
```

### 2. Decorator
```python
def singleton(cls):
    instances = {}

    def getinstance():
        if cls not in instances:
            instances[cls] = cls()
        return instances[cls]

    return getinstance

@singleton
class MyClass:
    pass
```

### 3. \_\_metaclass\_\_
```python
class SingletonType(type):
    def __call__(cls, *args, **kwargs):
        try:
            return cls.__instance
        except AttributeError:
            cls.__instance = super(SingletonType, cls).__call__(*args, **kwargs)
            return cls.__instance

class MySingleton(object):
    __metaclass__ = SingletonType
```
