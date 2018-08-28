---
title: "Anaconda Install"
description: "python을 이용해 data analysis를 하기 위해 anaconda 배포판을 설치한다."
categories:
- Data Analysis
tags:
- python
- anaconda
- data analysis
- 파이썬
- 데이터분석
photos:
- https://www.anaconda.com/wp-content/themes/anaconda/images/logo-dark.png
---

> <sub>본 글은 학습한 내용을 정리하기 위해 작성한 것으로 오류가 있을 수 있는 점 염두에 두고 읽어주시기 바랍니다.<br />
> 제가 잘못 알고 있는 부분에 대해 알려주시면 공부에 도움이 많이 될 것입니다. 감사합니다.</sub>

# 데이터 분석 공부를 위한 [Anaconda](https://www.anaconda.com/) 설치 #

데이터 분석 공부를 위해 Windows 10 환경에 [Anaconda 배포판](https://www.anaconda.com/)을 설치한다.

## 0. 실습 환경 ##

- windows 10

## 1. [Anaconda](https://www.anaconda.com/) 설치 ##

Anaconda 배포판 [다운로드 페이지](https://www.anaconda.com/download/)에서 환경에 맞는 설치 파일을 다운로드 받는다.

![Anaconda 다운로드 페이지](/assets/images/post_img/anaconda-install-01.png "Anaconda 다운로드 페이지")
*Anaconda 다운로드 페이지*

여기서 우리는 Python 3.6 를 사용하는 64비트 버전을 다운 받도록 한다.

다운받은 파일을 실행하면 아래와 같이 설치 화면이 나온다. **Next**를 선택하여 다음으로 넘어가도록 하자.

![Anaconda 설치 화면 01 - 초기화면](/assets/images/post_img/anaconda-install-02.png "Anaconda 설치 화면 01 - 초기화면")
*Anaconda 설치 화면 01 - 초기화면*

![Anaconda 설치 화면 02 - 라이센스 동의](/assets/images/post_img/anaconda-install-03.png "Anaconda 설치 화면 02 - 라이센스 동의")
*Anaconda 설치 화면 02 - 라이센스 동의*

라이센스까지 동의하고 나면 모든 유저가 사용할 수 있도록 설치할지, 나만을 위해 설치할지 결정하는 화면이 나오는데, 개인이 사용하는 컴퓨터라는 전제하에 모든 사용자를 선택하였다.

![Anaconda 설치 화면 03 - 대상 유저 범위](/assets/images/post_img/anaconda-install-04.png "Anaconda 설치 화면 03 - 대상 유저 범위")
*Anaconda 설치 화면 03 - 대상 유저 범위*

설치 경로는 원하는 곳으로 변경할 수 있지만 여기서는 기본값으로 지정된 범위로 하였다.

![Anaconda 설치 화면 04 - 설치 경로](/assets/images/post_img/anaconda-install-05.png "Anaconda 설치 화면 04 - 설치 경로")
*Anaconda 설치 화면 04 - 설치 경로*

Anaconda 를 환경 변수에 추가하여 아나콘다에서 제공하는 파이썬을 기본으로 사용하도록 지정하는 과정이다. 여기서는 둘 다 체크하였다.

![Anaconda 설치 화면 05 - 환경 변수 등록](/assets/images/post_img/anaconda-install-06.png "Anaconda 설치 화면 05 - 환경 변수 등록")
*Anaconda 설치 화면 05 - 환경 변수 등록*

![Anaconda 설치 화면 06 - 설치 중](/assets/images/post_img/anaconda-install-07.png "Anaconda 설치 화면 06 - 설치 중")
*Anaconda 설치 화면 06 - 설치 중*

![Anaconda 설치 화면 07 - 설치 완료](/assets/images/post_img/anaconda-install-08.png "Anaconda 설치 화면 07 - 설치 완료")
*Anaconda 설치 화면 07 - 설치 완료*

Anaconda를 다 설치하면 VSCode([Visual Studio Code](https://code.visualstudio.com/) - Microsoft 사에서 무료로 제공하는 IDE)를 설치할 수 있는 옵션이 나온다. 선택해서 설치할 수 있지만, 추후 필요시 설치 가능하므로 **Skip**을 눌러 넘어간다.

![Anaconda 설치 화면 08 - VSCode 설치](/assets/images/post_img/anaconda-install-09.png "Anaconda 설치 화면 08 - VSCode 설치")
*Anaconda 설치 화면 08 - VSCode 설치*

![Anaconda 설치 화면 09 - 마무리 화면](/assets/images/post_img/anaconda-install-10.png "Anaconda 설치 화면 09 - 마무리 화면")
*Anaconda 설치 화면 09 - 마무리 화면*

필요한 정보를 추가로 볼 수 있는 옵션을 모두 해제하고 설치를 종료한다.

윈도우 시작 버튼을 누르면 아래 그림과 같이 새롭게 설치된 **Anaconda** 폴더와 그 아래 설치된 프로그램들을 확인할 수 있다.

![Anaconda 설치 화면 10 - 시작 메뉴](/assets/images/post_img/anaconda-install-11.png "Anaconda 설치 화면 10 - 시작 메뉴")
*Anaconda 설치 화면 10 - 시작 메뉴*

여기서 **Anaconda Navigator**를 선택하면 처음 실행시 아래와 같이 버그 등 프로그램 개선을 위한 정보 수집에 동의해달라는 문구가 나오는데, 일단 체크를 해제하고 녹색 버튼을 눌러 해당 화면을 더이상 보지 않도록 하자.

![Anaconda 설치 화면 11 - 오류 보고 동의 화면](/assets/images/post_img/anaconda-install-12.png "Anaconda 설치 화면 11 - 오류 보고 동의 화면")
*Anaconda 설치 화면 11 - 오류 보고 동의 화면*

이후 아래와 같은 **Anaconda Navigator** 화면을 볼 수 있다.

Anaconda는 가상 환경을 만들고 각 가상 환경에 따른 패키지를 관리하는 등의 작업을 손쉽게 할 수 있도록 도와주는데, **Anaconda Prompt**를 통해 CLI 환경에서도 사용할 수 있지만 **Anaconda Navigator**를 통해 GUI 환경에서도 작업할 수 있으므로 앞으로의 공부 과정에서 이를 적극적으로 이용하도록 할 예정이다.

![Anaconda 설치 화면 12 - Anaconda Navigator 화면](/assets/images/post_img/anaconda-install-13.png "Anaconda 설치 화면 12 - Anaconda Navigator 화면")
*Anaconda 설치 화면 12 - Anaconda Navigator 화면*
