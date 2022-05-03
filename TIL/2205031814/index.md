---
layout: post
type: TIL
date: 2022-05-03 18:14
draft: false
studing : true
category: TIL
title: [크롤러] mongoDB error
subtitle: 왜 몽고DB에 입력을 못하는 거니...
writer: 000000
post-header: true
header-img : img/about.jpeg
hash-tag: [오류개선]
---


## 문제

---

![스크린샷 2022-05-03 오후 4.54.40.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4c89bbd8-4ccb-41d4-a8b5-29cc94ea7d13/스크린샷_2022-05-03_오후_4.54.40.png)

![스크린샷 2022-05-03 오후 4.58.06.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/06039ca0-1b78-4980-abd1-80a12045067b/스크린샷_2022-05-03_오후_4.58.06.png)

- No operations to execute 에러 발생

## 원인 분석

---

- No operations to execute 에러는 몽고DB에 빈배열을 넣을때 발생함
    - 왜 빈배열인지 찾아봐야 됨.
- 최근 크롤링 함수를 수정했던 부분중 몽고DB에서 데이터를 가져 온 다음 다시 최신 정보를 업데이트 해주는 함수가 있음.
- 데이터를 가져 올때 기존 데이터가 존재 하지 않아서 발생함.

## 해결

---

- 기존 데이터가 없을시 새로운 데이터를 만들어서 진행할수 있도록 수정