---
title: "[TDD-DRF] 1. windows에서 가상 환경 설정"
description: "windows 10에서 django 프로젝트를 진행하기 위해 virtualenv 를 설치하고, 가상 환경을 설정한다."
categories:
- Python
tags:
- python
- virtualenv
- virtualenvwrapper
- windows10
- 가상환경
- 파이썬
---

> <sub>본 글은 학습한 내용을 정리하기 위해 작성한 것으로 오류가 있을 수 있는 점 염두에 두고 읽어주시기 바랍니다.\
> 제가 잘못 알고 있는 부분에 대해 알려주시면 공부에 도움이 많이 될 것입니다. 감사합니다.</sub>

# Windows 10 에 Virtualenv 설치 #

[클린 코드를 위한 테스트 주도 개발](http://www.obeythetestinggoat.com/, "2판을 온라인에서 무료로 읽을 수 있다.")의 내용을 윈도우에서 따라 진행해보기 위해 가상 환경을 설정하기로 했다. 그동안은 공부를 하면서 ubuntu 혹은 mint 리눅스에서 실습을 진행하여 개발 환경 구축에 어려움이 없었는데, 이번에는 윈도우에서 실습을 진행하려다보니 처음부터 막히는 것 같아 관련 내용을 간단히 정리해보려고 한다. (사실 이렇게 환경을 구축하는 부분에서 낯섦이 있어 윈도우 환경에서 실습하길 꺼려했었다.)

python 3.3 이후로 [venv](https://docs.python.org/3/library/venv.html)가 공식적으로 제공되고 있지만 지금껏 [virtualenv](https://virtualenv.pypa.io/en/stable/)를 사용해왔으므로 이번에도 virtualenv로 가상 환경을 구성하고 [virtualenvwrapper](http://virtualenvwrapper.readthedocs.io/en/latest/index.html)를 활용한다.

---

## 0. 실습 환경 ##

- Windows 10
- python 3.7
- pip 10.0.1
- vscode

## 1. [Virtualenv](http://virtualenvwrapper.readthedocs.io/en/latest/index.html) 설치 ##

윈도우에 python 최신 버전을 설치하면 pip가 함께 설치되어 있을 것이다.(([pip 설치](https://pip.pypa.io/en/stable/installing/) 문서에 따르면 `Python 2>=2.7.9` 또는 `Python 3 >= 3.4` 버전을 [python.org](https://python.org)를 통해 설치하면 pip 가 함께 설치된다. 이 경우 설치가 종료되는 시점에 python의 경로를 윈도우 환경 변수 중 PATH에 등록하는 과정까지 한 번에 진행된다.)
virtualenv는 다음의 pip 명령을 통해 설치할 수 있다.

```bash
pip install virtualenv
```

이렇게 virtualenv 가 설치되면 다음과 같이 지정된 폴더에 가상 환경을 설정할 수 있다.

```bash
virtualenv ENV
```

여기서 `ENV`는 가상 환경이 설정될 폴더를 가르킨다.(`ENV` 라는 폴더가 존재하지 않으면 `virtualenv` 명령어와 함께 해당 폴더가 생성된다.)

이후

```bash
ENV\Scripts\activate
```

를 통해 가상 환경을 활성화 할 수 있으며, 작업이 끝나면 언제든

```bash
deactivate
```

를 커맨드 라인에 입력함으로써 가상 환경을 종료할 수 있다.

## 2. [Virtualenvwrapper](http://virtualenvwrapper.readthedocs.io/en/latest/) 설치 ##

> virtualenvwrapper is a set of extensions to Ian Bicking’s virtualenv tool.

virtualenvwrapper 의 문서 가장 첫 머리에는 virtualenvwrapper 가 virtualenv 의 extension 이라고 되어 있는데, virtualenvwrapper 를 사용하면 virtalenv 를 좀 더 편리하게 사용할 수 있다.

virtualenvwrapper 또한 pip 명령으로 설치 할 수 있다.

```bash
pip install virtualenvwrapper-win
```

([virtualenvwrapper-win](https://pypi.org/project/virtualenvwrapper-win/)은 기존의 virtualenvwrapper 를 Windows batch scripts 로 porting 한 것이다.)

virtalenvwrapper 를 사용하면 다음과 같은 명령어를 사용하게 된다.

먼저 새로운 가상 환경을 만들기 위해서는

```bash
mkvirtualenv myproject
```

이렇게 가상 환경을 설정하면 `C:\Users\Username\Envs` 에 `myproject` 라는 폴더가 생성되면서 가상 환경에 필요한 파일들이 해당 폴더에 복사 된다.

이후

```bash
workon myproject
```

라는 명령어로 기존의 `activate` 명령을 대신할 수 있다. (가상 환경 activate 시 가상 환경 설정 파일이 들어있는 폴더의 경로를 직접 입력할 필요가 없다.)

가상 환경을 종료하기 위해서는 역시 `deactivate` 명령을 사용한다.

```bash
deactivate
```

## 3. [Virtualenvwrapper](http://virtualenvwrapper.readthedocs.io/en/latest/) 활용 ##

virtualenvwrapper 를 활용하면 가상 환경 생성, 사용, 종료 등의 상황에서 필요한 작업을 자동화 할 수 있다.

여기서는

1. 가상 환경이 저장되는 폴더(기본 값이 `%USERPROFILE%\Envs` 로 되어 있다.)를 `C:\Projects\Envs` 로 수정
2. 새로운 가상 환경을 만들 때, 해당 가상 환경 이름에 대응하는 프로젝트 폴더를 생성하도록 설정

하는 작업을 진행한다.

### 3.1 `WORKON_HOME` 폴더 수정 ###

환경 변수 `WORKON_HOME`이 설정되어있지 않는 경우 가상 환경 파일이 저장되는 폴더의 기본 값은 `%USERPROFILE%\Envs` 로 설정되어 있는데, 이를 수정하기 위해서는 윈도우 커맨드 라인에서 환경 변수 `WORKON_HOME` 을 설정해주면 된다.

```bash
setx WORKON_HOME "C:\Project\Envs"
```

(임시로 `WORKON_HOME` 을 설정하기 위해서는 `set` 명령어를 사용 할 수 있다. 여기서는 향후 작업을 위해 지정된 값이 유지될 수 있도록 `setx` 명령어를 사용하였다. ([참고](https://superuser.com/questions/916649/what-is-the-difference-between-setx-and-set-in-environment-variables-in-windows, "set 과 setx 의 차이")))

> 성공: 지정한 값을 저장했습니다.

라는 문구가 뜨면 성공적으로 환경 변수를 저장한 것이다.

```bash
echo %WORKON_HOME%
```

명령을 통해 다시 한 번 변수 값이 잘 지정 되었는지 확인할 수 있다. (`%WORKON_HOME%` 라고 출력 되는 경우 터미널을 닫았다 다시 열어 보자.)

이후 다음과 같이 가상 환경을 생성하면 `WORKON_HOME` 으로 지정된 폴더 아래 가상 환경 이름과 같은 폴더가 만들어지고 파일이 복사됨을 알 수 있다.

```bash
mkvirtualenv myproject
```

### 3.2 가상 환경에 대응하는 프로젝트 폴더 생성 ###

`mkvirtualenv` 명령어 대신 `mkproject` 명령어를 사용하면 가상 환경 생성시 가상 환경의 이름과 같은 이름의 프로젝트 폴더를 자동으로 생성할 수 있다. 다만 아무런 설정 없이 이 명령어를 진행하면

> ERROR: set environment variable PROJECT_HOME to the directory where projects are stored.

와 같은 에러 메시지를 볼 수 있다. 이를 피하기 위해 위의 `WORKON_HOME` 환경 변수 지정에서와 같이 `PROJECT_HOME` 환경 변수를 지정한다.

```bash
setx PROJECT_HOME "C:\Project"
echo %PROJECT_HOME%
```

환경 변수가 저장 된 것을 확인 한 후 `mkproject` 명령어를 사용하면 가상 환경이 만들어질 때 같은 이름의 프로젝트 폴더가 만들어지는 것을 확인할 수 있다.

```bash
mkproject myproject
```
