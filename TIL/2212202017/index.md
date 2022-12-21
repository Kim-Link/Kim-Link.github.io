---
layout: post
type: TIL
date: 2022-12-20 20:17
draft: false
TIL : true
category: TIL
title: "[Django] Migration안될때"
subtitle: 마이마이마이그레이션..!!
writer: 000000
post-header: true
header-img : ../TIL.jpeg
hash-tag: [오류개선]
---

## 문제점

---

- 깃풀을 새로 해서 세팅하는데 마이그레이션을 해도 테이블이 계속 없다고 한다.
    
    <img src="img/1.png" alt="1" style="zoom:80%;" />
    
- 마이그레이션 초기화(삭제후 재생성)을 했는데도 동일하게 테이블이 없다고 한다.
    - `django.db.utils.OperationalError: no such table: user_user`

## 해결

---

1. 기존 db 삭제 ⇒ db.sqlite3 제거
    
    <img src="img/2.png" alt="1" style="zoom:80%;" />
    
2. migration 생성
    1. migration 생성
        
        `python manage.py makemigrations`
        
    2. 테이블 생성
        
        `python manage.py migrate --run-syncdb`
        
3. 계정 생성

    <img src="img/1.png" alt="1" style="zoom:80%;" />


## 결론
---

식겁했다..
