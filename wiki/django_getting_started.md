# Django 시작하기

## Django 소개
> Django makes it easier to build better Web apps more quickly and with less code.

Django는 빠른 개발과 깨끗하고 실용적인 디자인을 장려하는 고급 python 웹 프레임워크다.  
2003년 시작된 파이썬 기반 오픈소르 웹 어플리케이션 프레임워크로 MVC(혹은 MVT)패턴을 따른다.  
2005년 오픈소스로 공개되어 구글앱엔진, 인스타그램 등에서 채택되어 사용되고있다.  

### MVC

### Django vs Flask

## Django 설치
1. 파이썬설치  
    - 파이썬 다운로드 url : https://www.python.org/downloads  
1. Django 패키지 설치
    ```
    $ pip install django
    ```
    ```
    $ pip install --proxy http://70.10.15.10:8080 django
    Collecting django
    Downloading https://files.pythonhosted.org/packages/eb/4b/743d5008fc7432c714d753e1fc7ee56c6a776dc566cc6cfb4136d46cdcbb/Django-2.2.2-py3-none-any.whl (7.4MB)
        100% |████████████████████████████████| 7.5MB 479kB/s
    Collecting sqlparse (from django)
    Downloading https://files.pythonhosted.org/packages/ef/53/900f7d2a54557c6a37886585a91336520e5539e3ae2423ff1102daf4f3a7/sqlparse-0.3.0-py2.py3-none-any.whl
    Collecting pytz (from django)
    Downloading https://files.pythonhosted.org/packages/3d/73/fe30c2daaaa0713420d0382b16fbb761409f532c56bdcc514bf7b6262bb6/pytz-2019.1-py2.py3-none-any.whl (510kB)
        100% |████████████████████████████████| 512kB 603kB/s
    Installing collected packages: sqlparse, pytz, django
    Successfully installed django-2.2.2 pytz-2019.1 sqlparse-0.3.0
    You are using pip version 18.1, however version 19.1.1 is available.
    You should consider upgrading via the 'python -m pip install --upgrade pip' command.
    ```

## Django 프로젝트 생성
1. 장고프로젝트를 생성한다.  
    ```
    $ django-admin startproject mysite
    ```
     디렉토리에서 mysite라는 디렉토리를 생성한다.
1. mysite 폴더로 이동한다.  
    ```
    $ cd mysite
    ```
1. django server 실행  
    python manage.py 명령어를 사용하여 django server를 실행.  
    ```
    $ python manage.py runserver
    ```
    browser에서 "http://localhost:8000"를 실행.  
    ![0](image/welcome.png)
1. 포트 변경하기  
    장고는 기본적으로 내부 IP에  8000번 포트를 사용하여 개발서버를 띄운다. 아래와 같이 커맨드라인 파라미터로 포트를 변경할 수 있다.
    ```
    $ python manage.py runserver 8080
    ```
## Django 프로젝트 구조
<pre>
mysite/
    manage.py
    mysite/
        __init__.py
        settings.py
        urls.py
        wsgi.py
</pre>
- mysite/ 디렉토리 바깥의 디렉토리는 단순히 프로젝트를 담는 공간입니다. 이 이름은 Django 와 아무 상관이 없으니, 원하는 이름으로 변경하셔도 됩니다.  
- manage.py: Django 프로젝트와 다양한 방법으로 상호작용 하는 커맨드라인의 유틸리티 입니다. manage.py 에 대한 자세한 정보는 django-admin and manage.py 에서 확인할 수 있습니다.  
- mysite/ 디렉토리 내부에는 프로젝트를 위한 실제 Python 패키지들이 저장됩니다. 이 디렉토리 내의 이름을 이용하여, (mysite.urls 와 같은 식으로) 프로젝트의 어디서나 Python 패키지들을 임포트할 수 있습니다.  
- mysite/__init__.py: Python으로 하여금 이 디렉토리를 패키지처럼 다루라고 알려주는 용도의 단순한 빈 파일입니다. Python 초심자라면, Python 공식 홈페이지의 패키지를 읽어보세요.  
- mysite/settings.py: 현재 Django 프로젝트의 환경 및 구성을 저장합니다. Django settings에서 환경 설정이 어떻게 동작하는지 확인할 수 있습니다.  
- mysite/urls.py: 현재 Django project 의 URL 선언을 저장합니다. Django 로 작성된 사이트의 "목차" 라고 할 수 있습니다. URL dispatcher 에서 URL 에 대한 자세한 내용을 읽어보세요.  
- mysite/wsgi.py: 현재 프로젝트를 서비스하기 위한 WSGI 호환 웹 서버의 진입점입니다. How to deploy with WSGI를 읽어보세요.  

