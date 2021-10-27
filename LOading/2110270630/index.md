---
layout: post
type: LOading
date: 2021-10-27 06:30
category: Programmers
title: REST API 버전관리는 어떻게 해야하나?
subtitle: 버전도 있었냐...
writer: 000000
post-header: true
header-img: img/about.jpeg
hash-tag: [기술면접, C/S]
---

<br>

## REST API 와 버전

RESTful API를 완벽하게 지키기는 힘들다.

회사나 내가 정한 규칙에서 좀 일관적이게 규율을 따라 갈수 있으면 어떤 api 를 쓰던지 예상하기가 쉽다.

api가 버전이 있다. 이버전을 어떻게 할것인가?

api 버전을 넣는 방법중 적어도 옳은 방법은 없고, 3가지 틀린 방법이 있다.

*( 각 방법마다 단점이 뚜렷하기 때문에 틀렸다고 표현한다.)*

1. api앞에 번호를 넣는것.
2. URL은 똑같은데 req를 보낼때 header를 보통 만든다. 이 header안에 자기만의 entry(key,value)를 넣는다.
3. http의 accept header가 있다. 거기에 버전을 표기를 한다.

⇒ 가장 좋은것은 회사에서 쓰는 것을 따라 쓰는것이 좋다.

내가 혼자 한다면 선택할 규약은 두번째 방법이다.

그 이유는 일단 URL이 바뀌지 않는다. url이 바뀌지 않으니까 api가 아니라 웹사이트 api가 되더라도 search optimagation이 깨지지 않는다. 

그리고 클라이언트에게 눈에 보이지 않고 숨겨져 있기 때문에 더 깔끔하다고 생각된다.

<br>

<br>

많은 사람들이 REST API를 사용한다면  REST API version control 또한 라이브러리로 관리하는 tool이 따로 개발되지 않을까 생각한다.

[참고 링크] : https://www.troyhunt.com/your-api-versioning-is-wrong-which-is/
