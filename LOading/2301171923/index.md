---
layout: post
type: LOading
date: 2023-01-17 19:23
category: Python
title: "장고 설치 및 프로젝트 생성(+ .env)"
subtitle: 장고 시작하기 - 2
writer: 000000
post-header: true
header-img: ../django.png
hash-tag: [Python,Django]
---

<!-- 양식

[사진 첨부]

<img src="img/1.png" alt="1" style="zoom:80%;" />

[참고 문헌]

[파이썬 딕셔너리 사용하기 dict](https://jvvp.tistory.com/998) 

-->

<br>

## Intro

---

시작이 반이다.

개발하면서 가장 와닿는 말인것 같다.

그만큼 시작하는데 있어서 환경을 설정하고 세팅하는게 가장 번거롭고,

반면 한번 세팅만 제대로 해놓으면 뭔가 든든하고 편한 느낌이 많이 든다.

가상환경에 관한 포스팅을 지난번에 했으니 이이서 장소 설치 및 프로젝트 시작하는것에 대해 기록해보자 한다.

<br>

## 장고 설치

---

- 장고를 설치 하기 전에 먼저 가상환경이 켜져있는지 확인후 설치한다.

<img src="img/1.png" alt="1" style="zoom:80%;" />

- `pip install django` `(django~=3.0.0)`
    - 뒤에 버젼을 붙이면 최신 장고 버젼이 아닌 원하는 버전을 설치할수 있다.
    - 설치후 가상 환경을 한번 껏다 켜서 가상환경에 제대로 설치가 되어 있는지 확인한다.
        
        ⇒ `which django-admin`
        

<br>

## 프로젝트 생성하기

---

- 장고를 설치 하기 전에 먼저 가상환경이 켜져있는지 확인후 설치한다.
- `django-admin startproject (projectName)` 실행
    - 폴더가 projectName으로 만들어지면서 생성됨
    - 폴더 생성이 싫다면 현재 경로를 설정해주기 위해 마지막에 `.` 을 찍음
        
        ⇒ `django-admin startproject (projectName) .` 
        
- 명령어를 실행하면 프로젝트 이름으로 폴더가 생성된것을 확인할수 있다.

<img src="img/2.png" alt="1" style="zoom:80%;" />

<br>

## 서버 가동하기

---

- 프로젝트 폴더 안으로 이동한다.
- `manage.py` 가 있는지 확인하고 명령어를 실행한다.
- `python manage.py runserver`

<img src="img/3.png" alt="1" style="zoom:80%;" />

- 빨간글씨가 뜨는 이유는 마이그레이션을 아직 하지 않았기 때문이다.
- 서버가 잘 구동되는지 로컬 주소로 접속해 본다.

<img src="img/4.png" alt="1" style="zoom:80%;" />



<br>


## .gitignore .env 파일 생성

---

- .gitignore 파일과 .env을 생성한다.
- .gitignore 파일을 생성해서 git에 올릴 필요가 없는 파일들을 제외시킨다.

```python
# .gitignore
### Django ###
/**/*.log
/**/*.pot
/**/*.pyc
/**/__pycache__/
/**/migrations/
db.sqlite3
db.sqlite3-journal
media
static
.env
```

- .env을 생성해서 환경변수들을 따로 보관한다.
    - 환경 변수에는 해킹에 취약한 SECRET_KEY들을 모아놓고 git에 공유되지 않게 보안을 유지한다.
    - .env 파일을 읽기 위해서 `pip install django-dotenv` 를 설치한다.
    - 설치한 라이브러리를 읽기 위해서 `manage.py`에 dotenv를 import 한다.

```python
# manage.py

"""Django's command-line utility for administrative tasks."""
import os
import sys
import dotenv

def main():
    """Run administrative tasks."""
    os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'kristinComp.settings')
    try:
        from django.core.management import execute_from_command_line
    except ImportError as exc:
        raise ImportError(
            "Couldn't import Django. Are you sure it's installed and "
            "available on your PYTHONPATH environment variable? Did you "
            "forget to activate a virtual environment?"
        ) from exc
    execute_from_command_line(sys.argv)

if __name__ == '__main__':
    dotenv.read_dotenv()
    main()
```

```python
# .env

SECRET_KEY = 'django SECRET_KEY'

SOCIAL_AUTH_GOOGLE_CLIENT_ID = "GOOGLE_CLIENT_ID"
SOCIAL_AUTH_GOOGLE_SECRET = "GOOGLE_SECRET"
STATE = "STATE"

SOCIAL_AUTH_KAKAO_CLIENT_ID = "KAKAO_CLIENT_ID"
SOCIAL_AUTH_KAKAO_SECRET = "KAKAO_SECRET"
```

- settings.py에서 SECRET_KEY를 .env에 옮겨 놓고 .env로 연결 시켜 놓는다.

```python
# setting.py

from pathlib import Path
import os

...

# SECURITY WARNING: keep the secret key used in production secret!
SECRET_KEY = os.environ.get("SECRET_KEY") # 이부분에 있던 키 값을 .env로 이동!!
```


<br>

### 마이그레이션

---

- `python manage.py makemigrations`
    - 모델에 따라 마이그레이션 파일을 만드는 것이다.
- `python manage.py migrate`
    - 만들어진 마이그레이션 파일을 기반으로 실제 DB에 마이그레이트 한다.


<br>

## outro

---

환경 세팅은 필요에 따라서 바뀔수 있다.

.env는 사용하지 않으면 굳이 세팅할 필요가 없지만, 보안키값들을 좀더 안전하게 지키기 위해서는 세팅해 두는것이 좋은것 같다.

나는 settings.py 자체를 gitignore에 포함시킬수 있지만, 생각보다 settings.py도 바뀌는 부분이 많기 때문에 원활한 공유를 위해 .env를 사용하기로 했다.

<br>


[참고 문헌]

[Django - django-dotenv 사용하기](https://kangraemin.github.io/django/2020/10/09/dotenv/)