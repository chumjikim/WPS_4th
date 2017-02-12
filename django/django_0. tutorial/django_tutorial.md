#Django

#Tutorial
## part 1.   
### 1.1  virtualenv 설정하고 Django 설치하기
```
* mkdir django_tutorial	
* pyenv virtualenv 3.4.3 tutorial
* pyenv local tutorial
* pip freeze > requirements.txt
* pip install django
* mysite 디렉토리 이름 변경
* git 저장소 초기화
* .gitignore작성(python, pycharm, django)
    -추천 : .gitignore 파일에 .idea/추가
```

###1.2 프로젝트 생성
* django-admin startproject mysite
	* 해당경로에서 mysite라는 디렉토리 생성
	
### 1.3 startproject 에서 생성된 파일들

```
mysite/
    manage.py
    mysite/
        __init__.py
        settings.py
        urls.py
        wsgi.py
```

* `mysite/` : 단순히 프로젝트를 담는 공간
* `manage.py` : Django 프로젝트와 다양한 방법으로 상호작용 하는 커맨드라인의 유틸리티
* `mysite/` : Project를 위한 실제 Python 패키지들이 저장되는 공간. 이 디렉토리 내의 이름을 이용하여 project 어디서나 Python 패키지들을 import할 수 있음
* `mysite/__init__.py` : Python 으로 하여금 이 디렉토리를 패키지 처럼 다루라고 알려주는 용도의 단순한 빈 파일
* `mysite/settings.py` : 현재 Django project의 환경/구성을 저장
* `mysite/urls.py` : 현재 Django project 의 URL 선언을 저장. Django 로 작성된 사이트의 "목차"라고 할 수 있음
* `mysite/wsgi.py` : 현재 project를 서비스 하기 위한 WSGI 호환 웹 서버의 진입점

###1.4 개발 서버

`./manage.py runserver`   

* 해당명령어로 서버를 동작, 웹 브라우져에서 http://127.0.0.1:8000/ 를 통해 접속할 수 있다.
*  정상작동시 다음과같은 명령어가 출력된다.

```
Performing system checks...

System check identified no issues (0 silenced).

You have unapplied migrations; your app may not work properly until they are applied.
Run 'python manage.py migrate' to apply them.

February 03, 2017 - 15:50:53
Django version 1.10, using settings 'mysite.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.
```



###1.5 설문조사 앱만들기
- app 생성하기   

`./manage.py startapp polls`

<출력결과 다음과 같은 디렉토리 구조가 생성된다.>

```polls/
    __init__.py
    admin.py
    apps.py
    migrations/
        __init__.py
    models.py
    tests.py
    views.py
```

###1.6 첫번째 뷰 작성하기
"polls/view.py" 파일을 열어 다음 코드를 입력한다.

```
from django.http import HttpResponse


def index(request):
    return HttpResponse("Hello, world. You're at the polls index.")
```

view를 호출하려면 이와 연결된 URL이 있어야 한다.  
이를 위해 URLconf가 사용 된다.  
`polls/urls.py` 파일을 열어 코드 입력/확인

```
from django.conf.urls import url

from . import views

urlpatterns = [
	url(r'^$', views.index, name='index'),
]
```
--
project 최상단의 URLconf에서 `polls/urls` 모듈을 바라보도록 설정  
`mysite/urls.py` 파일 오픈  
다음과 같은 코드 추가

```
from django.conf.urls import include, url
from django.contrib import admin

urlpatterns = [
	url(r'^polls/', include('polls.urls')),


