---
title: "[TDD-DRF] 2. 첫 번째 테스트와 함께 django 설치"
description: "첫 번째 테스트를 작성하고, django를 설치한다. 이 과정에서 selenium 을 통해 firefox 브라우저를 사용할 수 있게 Geckodriver 를 설치한다."
categories:
- TDD
tags:
- python
- TDD
- django
- selenium
- geckodriver
- 파이썬
photos:
- /assets/images/post_img/django_landing_page.png
---

> <sub>본 글은 학습한 내용을 정리하기 위해 작성한 것으로 오류가 있을 수 있는 점 염두에 두고 읽어주시기 바랍니다.<br />
> 제가 잘못 알고 있는 부분에 대해 알려주시면 공부에 도움이 많이 될 것입니다. 감사합니다.</sub>

# 첫 번째 테스트 작성 및 **django** 설치 #

`django` 를 설치하기에 앞서 [클린 코드를 위한 테스트 주도 개발](http://www.obeythetestinggoat.com/) 의 방법을 참고하여 테스트를 작성한다. 이 테스트는 django 를 설치하여 프로젝트를 만들면 통과할 수 있도록 작성되며, 테스트 작성에서 통과까지의 과정을 거치며 기본적인 설치 작업을 진행한다.

## 0. 실습 환경 ##

- python 3.7
- pip 10.0.1
- virtualenv
- vscode

## 1. 첫 번째 테스트 작성 ##

먼저 `%PROJECT_HOME%` 폴더(여기서는 `C:\Project`, 이후 `PROJECT_HOME` 으로 표기)에 새로 시작할 프로젝트 폴더와 기능 테스트 파일을 저장할 폴더를 만들어준다.

```bash
> mkdir todolist\functional_tests
```

이제 `functional_tests\tests.py` 에 첫 번재 테스트를 작성하자.

```python
# selenium 의 webdriver 를 사용한다.
from selenium import webdriver

browser = webdriver.Firefox()               # 브라우저는 Firefox 를 사용한다.
browser.get('http://localhost:8000')        # 브라우저를 통해 로컬 웹페이지를 연다.

assert 'Django' in browser.title            # 웹 페이지 타이틀에 "Django" 라는 단어가 있는지 확인한다.

browser.quit()                              # 테스트가 끝나면 브라우저를 종료한다.
```

(테스트에서는 책에서와 같이 `Firefox` 브라우저를 사용한다. 따라서 `Firefox` 브라우저가 설치되어 있어야 한다.)

프로젝트 폴더(`C:\Project\todolist`, 이후 `프로젝트 폴더`로 표기)에서 테스트를 진행하고 결과를 확인하면 예상처럼 테스트가 실패하는 것을 알 수 있다.

```bash
> python functional_tests\tests.py

Traceback (most recent call last):
  File "functional_tests\tests.py", line 2, in <module>
    from selenium import webdriver
ModuleNotFoundError: No module named 'selenium'
```

[책](http://www.obeythetestinggoat.com/, "클린 코드를 위한 테스트 주도 개발")과 같은 테스트이지만 `assert` 구문에 가기도 전에 테스트가 실패한 것을 확인할 수 있는데, 이는 selenium 을 설치하지 않았기 때문이다.

## 2. 가상 환경 만들기 ##

`selenium`을 설치하기 전에 프로젝트를 진행 할 가상 환경을 만들어보자. (참고 - [이전 글](2018-07-05-windows-virtualenv.md))

`프로젝트 폴더`에서 아래와 같이 가상 환경 프로젝트를 만든다.

```bash
> mkvirtualenv -a . todolist
```

여기서는 `-a` 옵션을 사용하여 이미 존재하는 프로젝트 폴더를 가상 환경과 연결시켜준다. ([참고](https://pypi.org/project/virtualenvwrapper-win/))

앞으로

```bash
> workon todolist
> deactivate
```

명령어를 사용하여 가상 환경을 실행시키고 종료 할 수 있다.

이후 별도의 언급이 없을 경우 모든 작업은 `workon todolist` 를 통해 가상 환경을 실행 시킨 지점에서 진행한다. (프로젝트 폴더 `C:\Project\todolist`에서 명령 수행)

## 3. [Selenium](http://selenium-python.readthedocs.io/) 설치 ##

이제 `Selenium` 을 설치하고 `Firefox` 브라우저를 사용하기 위한 `Geckodriver` 를 설치한다.

### 3.1 [Selenium with Python](http://selenium-python.readthedocs.io/installation.html) 설치 ###

```bash
> pip install selenium
```

역시 가장 쉬운 설치 방법은 `pip` 를 사용하는 방법이다.

`selenium` 이 설치되었으니 테스트 결과가 어떻게 달라지는지 한 번 확인해보자.

```bash
> python functional_tests\tests.py

Traceback (most recent call last):

...

selenium.common.exceptions.WebDriverException: Message: 'geckodriver' executable needs to be in PATH.
```

긴 에러 메시지가 출력되지만 마지막 줄을 보면 `geckodriver` 가 `PATH` 안에 없다는 내용을 확인 할 수 있다. 

### 3.2 [geckodriver](https://github.com/mozilla/geckodriver/releases) 설정 ###

먼저 [링크](https://github.com/mozilla/geckodriver/releases)에서 최신 `geckodriver` 파일을 받는다. 작성일 기준로 `v0.21.0` 이 최신 버전이므로 `geckodriver-v0.21.0-win64.zip` 파일을 받는다. 다운 받은 `zip`파일의 압축을 풀면 `geckodriver.exe` 라는 파일이 하나 있음을 확인 할 수 있다. 이제 이 `geckodriver.exe` 파일을 윈도우 환경 변수 `PATH`에 등록된 폴더에 넣거나 임의의 폴더에 넣은 후 그 폴더를 `PATH` 에 등록해준다. 필자는 추후 활용할 것을 생각해 `Python` 경로 아래 `Scripts` 폴더에 넣었다.

다시 한 번 테스트를 진행해보자.

```bash
> python functional_tests\tests.py

Traceback (most recent call last):

...

selenium.common.exceptions.WebDriverException: Message: Reached error page: about:neterror?e=connectionFailure&u=http%3A//localhost%3A8000/&c=UTF-8&f=regular&d=localhost%3A8000[...]
```

`geckodriver` 를 찾는 에러 메시지는 사라졌지만, 새로운 에러 메시지가 발생했다. 에러 메시지 중간에서 `... Reached error page ...` 라는 문구를 확인 할 수 있고, 파이어 폭스 화면에서는 `연결 실패`라는 문구를 볼 수 있다.

일단 에러는 해결 할 수 없었지만, `selenium` 이 정상적으로 `Firefox`를 열고 페이지를 탐색하는 걸 확인했으니 이제 `django`를 설치하도록 하자.

## 4. [Django](https://www.djangoproject.com/) 설치 ##

`django` 또한 `pip`를 이용해 설치하면 간단히 설치할 수 있다.

```bash
> pip install django
```

여기서 `django==1.11.14` 나 `django<1.12` 와 같이 `django` 버전을 명시해주면 원하는 버전의 `django` 를 설치할 수 있다. 참고로 `1.11` 버전은 `Python 2.7` 을 지원하는 마지막 `LTS(Long Term Support)` 버전이다.

이상 없이 `django` 가 설치되었다면 다음을 입력하여 `django` 버전을 확인해보자.

```bash
> python -m django --version
2.0.7
```

위와 같이 `django` 버전이 출력되면 `django` 가 잘 설치 된 것을 알 수 있다.

그럼 이제 `프로젝트 폴더` 에서 장고 프로젝트를 생성한다.

```bash
> django-admin startproject todolist .
```

여기서 주의 할 부분은 명령 마지막에 붙어있는 "`.`" 이다. 이 점은 현재의 위치에서 `todolist` 라는 `django` 프로젝트를 만들어 달라는 의미이다. 만약 이 점을 생략한다면 현재의 위치에서 다시 한 번 `todolist` 라는 폴더가 생기고 그 아래 하위 구조로 프로젝트가 생성될 것이다.

이제 테스트를 통과하기 위한 준비가 다 되었다.

```bash
> python manage.py runserver
```

위의 명령어를 실행해 `django` 내장 서버를 실행 시키고 새로운 콘솔창을 열어 테스트를 진행해보자.

```bash
> python functional_tests\tests.py
```

콘솔창에 아무런 메시지가 뜨지 않고 새로운 프롬프트가 뜬다면 테스트가 성공한 것이다. 중간에 잠시 `Firefox` 창이 열렸다 닫히는 것을 볼 수 있다. `assert` 구문 전/후 로 `print(browser.title)` 구문을 삽입하면 콘솔창에 다음과 같은 문장이 찍히는 것을 확인할 수 있다.

> Django: the Web framework for perfectionists with deadlines.

## 5. 추가 사항 ##

### 5.1 [runserver](https://docs.djangoproject.com/en/2.0/ref/django-admin/#django-admin-runserver) 설정 ###

`runserver` 를 통해 실행되는 서버의 `IP주소` 및 `포트 번호` 의 기본값은 `127.0.0.1:8000` (`localhost` 의 `8000` 번 포트를 사용한다) 이지만 `runserver` 명령어 뒤에 `IP 주소` 나 `포트 번호` 를 적어주면 해당 `IP 주소` 혹은 `포트 번호` 로 서버가 실행된다.

### 5.2 `django` 프로젝트의 `settings.py` ###

`django` 프로젝트의 설정이 담기는 `settings.py` 에는 프로젝트 설정값과 함께 `DEBUG 옵션`, `SECRET_KEY`, `DB setting 값` 등 주요 정보가 포함되어 있다. 지금은 학습의 과정이고, 중요한 내용이 담겨있지 않아 그대로 진행하지만, 주요 작업을 배포하기 전에는 해당 정보가 노출되지 않도록 변경해야 한다. 아래의 방법을 통해 배포 작업에서 생길 수 있는 문제를 사전에 확인할 수 있다.

```bash
> python manage.py check --deploy
```

자세한 내용은 [이 곳](https://docs.djangoproject.com/en/2.0/howto/deployment/checklist/, "Deployment checklist")을 참고하자.

### 5.3 테스트를 통과하는 최소한의 방법 ###

다음과 같이 파일을 구성하면 하나의 파일로도 위에서 작성한 테스트를 통과할 수 있다.

```python
# single_server.py
import sys

from django.conf import settings
from django.core.management import execute_from_command_line

settings.configure(
  DEBUG=True,
  ROOT_URLCONF=__name__,
)

urlpatterns = (

)

if __name__ == "__main__":
  execute_from_command_line(sys.argv)
```

서버는 일반적인 `django` 프로젝트에서처럼 아래와 같이 실행한다.

```bash
> python single_server.py runserver
```

테스트는 역시 새로운 콘솔창을 열어서 진행한다.

```bash
> python functional_tests\tests.py
```

### 5.4 여담 ###

`django` 를 `1.4` 버전 정도까지 접하다가 다시 보지 않고 있었는데, 처음 `runserver` 이후에 나오는 페이지가 놀랄만큼 예뻐졌다. 로켓 일러스트가 `django` 가 추구하는 바를 잘 드러내고 있는 것 같다.

![Django 2.0 landing page](/assets/images/post_img/django_landing_page.png "Django landing page")