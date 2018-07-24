---
title: "django REST framework 과 Vue.js 를 이용한 Todo 앱 만들기 (1)"
description: "django 및 django REST framework 과 Vue.js 를 활용하여 Todo 앱을 만들어본다."
categories:
- Django
tags:
- python
- django
- djangorestframework
- vuejs
- todoapp
- 파이썬
photos:
- https://www.djangoproject.com/s/img/logos/django-logo-negative.png
- http://www.django-rest-framework.org/img/logo.png
- https://vuejs.org/images/logo.png
---

> <sub>본 글은 학습한 내용을 정리하기 위해 작성한 것으로 오류가 있을 수 있는 점 염두에 두고 읽어주시기 바랍니다.<br />
> 제가 잘못 알고 있는 부분에 대해 알려주시면 공부에 도움이 많이 될 것입니다. 감사합니다.</sub>

# [django REST framework](http://www.django-rest-framework.org/) 과 [Vue.js](https://kr.vuejs.org/) 를 이용한 Todo 앱 만들기 (1) #

**Todo 앱**을 만드는 프로젝트는 사용자의 입력을 받아 할 일을 생성하고(**Create**), 할 일 목록을 보여주는(**Read** 또는 **Retrieve**) 작업을 비롯해 기존의 입력 값을 수정하고(**Update**), 삭제하는(**Delete**) 작업까지 아우르기에 프레임워크를 공부할 때 활용하기 좋은 주제다. 게다가 웹상에 다양한 Todo 앱 만들기 튜토리얼이 있어 이를 바탕으로 조금씩 변형해 보는 것도 많은 공부가 된다.

이번 글에서는 **django REST framework** 과 **Vue.js** 를 활용하여 간단한 **Todo 앱** 을 만들어보고자 한다.

> 본 글은 다음 자료들을 참고하여 작성했습니다.
> - (Article) [CRUD App using Vue.js and Django](https://medium.com/quick-code/crud-app-using-vue-js-and-django-516edf4e4217)
> - (Youtube) [The Vue Tutorial for 2018 - Learn Vue 2 in 65 Minutes](https://youtu.be/78tNYZUS-ps)

## 0. 실습 환경 ##

- windows 10
- python 3.7
- django 2.0.7
- djangorestframework
- vue.js
- vscode

## 1. [django](https://www.djangoproject.com/) 프로젝트 설정 ##

### 1.1 `mkproject` 로 가상 환경 만들기 ###

django project 를 설정하기 위해 가장 먼저 해야 할 작업은 새로운 가상 환경을 만드는 일이다. 프로젝트 폴더가 위치할 곳에서 새로운 가상 환경을 만들자.

```bash
> mkproject todos
```

따로 설정을 변경하지 않았다면 가상 환경이 만들어지면서 프로젝트 이름과 동일한 프로젝트 폴더가 만들어지고 가상 환경이 `activate` 되면서 `cd` 명령어로 해당 폴더로 이동할 것이다. (또는 `workon todos` 로 가상 환경을 `activate` 할 수 있다.)

### 1.2 `django` 설치 및 프로젝트 만들기 ###

이제 **django** 를 설치하자.

```bash
> pip install django
```

django 가 정상적으로 설치되면 다음 명령어를 통해 현재 폴더에 django 프로젝트를 생성한다.

```bash
> django-admin starproject todos .
```

'`.`' 을 빼먹으면 현재 폴더 하위에 새로운 프로젝트 폴더가 생기니 주의하자. (큰 문제는 아니지만 폴더 트리가 더욱 복잡해진다.)

**django**는 프로젝트를 기능별로 작은 앱들로 나누도록 설계되어 있으니 프로젝트 폴더에서 `todoapp` 이라는 이름의 앱을 만들자.

```bash
> python manage.py startapp todoapp
```

여기까지 작업한 후의 폴더 트리는 대략 다음과 같다. (`__pycache__` 폴더가 있다면 무시하고 넘어가자.)

```bash
/todos
├─ manage.py
│
├─ /todoapp
│  ├─ admin.py
│  ├─ apps.py
│  ├─ models.py
│  ├─ tests.py
│  ├─ views.py
│  ├─ __init__.py
│  │
│  └─ /migrations
│     └─ __init__.py
│
└─ /todos
   ├─ settings.py
   ├─ urls.py
   ├─ wsgi.py
   └─ __init__.py

```

`todoapp` 을 프로젝트에서 사용하기 위해 `/todos/settings.py` 파일에 다음과 같이 `todoapp`을 추가한다.

```python
# /todos/settings.py 중 INSTALLED_APPS
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'todoapp',      # todoapp 추가
]
```

### 1.3 django 시간대 설정 ###

`settings.py` 파일을 연 김에 `TIME_ZONE` 설정을 변경하자.

```python
# /todos/settings.py

TIME_ZONE = 'Asia/Seoul'        # 'UTC' -> 'Asia/Seoul' 로 변경
```

### 1.4 Mode 작성 및 Migrations ###

이제 `todoapp` 에 필요한 django model 을 작성하고 해당 모델을 DB에 적용하기 위해 Migration 한다.

**할 일**에 대한 모델은 다음과 같이 정의할 수 있다.

| 변수 | 필드 타입 | 설명 |
|---|---|---|
| title | CharField | 할 일 명 |
| complete | BooleanField | 완료여부 |
| created | DateTimeField | 생성시간 |

`id` 변수는 다른 변수를 `primary_key`로 할당하지 않는 이상 자동으로 할당되며, 첫 번째 변수 `user` 는 `django.contrib.auth` 에서 제공되는 `User` 모델에 연결하여 사용하도록 한다.

위의 정의에 따라 실제 모델을 작성해보면 아래와 같다.

```python
# /todoapp/models.py
from django.db import models

class Todo(models.Model):
    title = models.CharField(max_length=100)
    complete = models.BooleanField(default=False)
    created = models.DateTimeField(auto_now_add=True)

    class Meta:
        ordering = ['created']

    def __str__(self):
        return self.title
```

모델을 작성한 후 이를 바탕으로 데이터베이스 테이블을 만들기 위해 `makemigrations` 와 `migrate` 명령을 실행한다.

```bash
> python manage.py makemigrations

> python manage.py migrate
```

## 2. django REST framework ##

### 2.1 [django-rest-framework](http://www.django-rest-framework.org/) 설치 ###

모델을 작성하고 migration 까지 끝냈다면  django-rest-framework를 설치하자.

```bash
> pip install djangorestframework
```

django-rest-framework를 정상적으로 설치한 후에는 `todoapp` 과 마찬가지로  `/todos/settings.py` 파일의 `INSTALLED_APPS` 에 `rest_framework` 를 추가해야 한다.

```python
# /todos/settings.py
INSTALLED_APPS = [
    ...
    'rest_framework',
]
```

### 2.2 Serializer, Viewset 및 Routers 만들기 ###

django-rest-framework를 설치하고 django 프로젝트에 연결한 후 다음과 같이 django 모델을 직렬화 한다.

```python
# /todoapp/serializers.py
from rest_framework import serializers

from todoapp.models import Todo

class TodoSerializer(serializers.ModelSerializer):
    class Meta:
        model = Todo
        fields = '__all__'
```

View 는 django-rest-framework 에서 제공하는 viewset 을 활용하면 다음과 같이 간단히 작성할 수 있다.

```python
# /todoapp/views.py
from rest_framework import viewsets
from .models import Todo
from .serializers import TodoSerializer


class TodoViewSet(viewsets.ModelViewSet):
    queryset = Todo.objects.all()
    serializer_class = TodoSerializer
```

이제 todoapp 폴더에 `urls.py` 파일을 다음과 같이 작성하자.

```python
# /todoapp/urls.py
from django.urls import path, include
from rest_framework.routers import DefaultRouter
from .views import TodoViewSet

router = DefaultRouter()
router.register(r'todos', TodoViewSet)

urlpatterns = [
    path('', include(router.urls)),
]
```

위의 `urls.py` 의 설정을 다음과 같이 프로젝트 폴더에 위치한 `urls.py` 에 연결함으로써 간단한 **api 서버**를 완성할 수 있다.

```python
# /todos/urls.py
from django.urls import path, include

urlpatterns = [
    # path('admin/', admin.site.urls),
    path('api/', include('todoapp.urls')),
]
```

터미널에서 다음과 같이 django 개발 서버를 가동하고

```bash
> python manage.py runserver
```

웹 브라우저를 켜서 `localhost:8000/api/` 에 접속해서 아래와 같은 화면이 나오는 것을 확인하자.

![Todos REST API](/assets/images/todos-rest-api-01.png "Todos REST API root 화면")

## 3. 다음 글 계획 ##

이어지는 글에서는 **Vue.js**를 활용하여 todoapp 의 기본적인 기능을 완성한다.