---
layout: post
type: LOading
date: 2021-09-25 00:11
category: Javascript
title: 표기법 - 스네이크 케이스(snake case),파스칼 케이스(pascal case), 카멜 케이스 (camel case)
subtitle: 기본인데 잘못쓰면 에러남...
writer: 000000
post-header: true
header-img: img/about.jpeg
hash-tag: [Javascript, 상식, 표기법]
---

코드를 치다보면 글자를 쓸때 띄어쓰기를 할수 없을경우 단어들을 구분해주어야 한다.

이때 3가지 표현 방법이 있다.

### 1. 스네이크 케이스(snake case)

> snake_case
> get_token
> user_id

띄어 쓰기 부분에 언더바(_)로 표현하여 구분하는 방식.

문장의 모양이 뱀처럼 생겨서 스네이크 케이스라고 한다.

### 2. 파스칼 케이스(pascal case)

> PascalCase
> GetToken
> UserId

문장의 첫번째 글자와 띄어 쓰기후 첫번째 글자를 대문자로 표현하여 구분하는 방식.

파스칼 언어의 표기법과 유사하다고 하여 파스칼 케이스라고 한다.

### 3. 카멜 케이스 (camel case)

> camelCase
> getToken
> userId

띄어 쓰기후 첫번째 글자를 대문자로 표현하여 구분하는 방식.

파스칼 케이스와는 달리 띄어쓰기 후에만 대문자로 표기한다.

글자의 형태가 낙타와 모양과 비슷해서 카멜 케이스라고 한다.

 

 

코드 칠때는 보통 스네이크 케이스나 카멜 케이스를 사용하는것 같다.

함께 프로젝트를 진행하는 동료들과 규칙을 정해 하나의 표기법으로 통일하는것이 에러발생을 줄일수 있다.

개인적으로 카멜 케이스를 사용하는것이 에러가 덜나는것 같다.*(순전히 내생각)*