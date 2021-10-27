---
layout: post
type: LOading
date: 2021-10-27 06:21
category: Programmers
title: REST API가 뭔가요?
subtitle: 기본중에 기본이었어...
writer: 000000
post-header: true
header-img: img/about.jpeg

hash-tag: [기술면접, C/S]
---

REST api는 정보들이 주고 받아지는데 있어서 개발자들 사이에 널리 쓰이는 일종의 형식이다.

제품이런게 아니라 형식, 포멧이다.

<br>

## API 란?

------

Application Programming Interface. 소프트웨어가 다른 소프트웨어로 부터 지정된 형식으로 요청, 명령을 받을 수 있는 수단이다.

쉽게 말해 "컴퓨터의 기능을 실행시키는 방식" 이다.

<br>

## REST API 란?

------

REST는 Representational State Transfer의 약자이다.

REST API도 API 이기 때문에 컴퓨터를 실행시키는 명령이다.

단,내꺼가 아닌 남의 컴퓨터를 실행 시킨다.

프론트엔드 웹에서 서버에 데이터를 요청하거나, 배달 앱에서 서버에 주문을 넣거나 하는등 이런 서비스들에서 오늘날 널리 사용되는 것이 REST라는 형식의 API이다.

즉, REST api는 기계와 기계가 규격화된 방식으로 인터넷으로 또는 웹을 통해서 통신할수 있도록 돕는 통신규칙이다.

REST의 가장 중요한 특성은 각 요청이 어떤 동작이나 정보를 위한 것인지 요청의 모습 자체로 추론 가능하다는 것이다.

만들려고 하는 서비스에서 기능 자체만을 중요하게 생각한다면 REST고 뭐고 생각할 필요 없이 동작만 하게 만들면 된다.

하지만 개발을 혼자 만드는것이 아니다. 내가 만드는 API를 아무렇게나 만들면 당장 동작은 할수 있겠지만 이 API를 사용해서 다른 제품을 만드는 개발자들은 굉장히 사용하기 힘들것이다.

RESTful하게 만든 API 요청을 보내는 주소만으로도 대략 이게 뭘 하는 요청인지 파악이 가능하다.

REST API는 HTTP 규약에 따라 신호를 전송한다.

HTTP로 요청을 보낼때 사용하는 다양한 메소드가 있다.(GET, HEAD, POST, PUT, DELETE, CONNECT, OPTIONS, TRACE, PATCH)

이중 REST API에서는 GET, POST, PUT, DELETE, PATCH를 사용한다.

POST, PUT,  PATCH는 body 부분이 있어서 정보들을 GET이나 DELETE보다 많이 그리고 비교적 안전하게 감춰서 실어 보낼수 있다.

GET은 정보를 읽는데, POST는 새로운 정보를 추가하는데 사용한다. PUT과 PATCH는 둘다 정보를 변경할때 사용하지만 쓰는 곳마다 다르다.

PUT은 정보를 전부 갈아 끼울때, PATCH는 정보중 일부를 특정 방식으로 변경할때 사용한다.

DELETE는 정보를 삭제할때 사용한다.

결국 REST API란 HTTP 요청을 보낼때 어떤 URI에 어떤 메소드를 사용할지(+기타) 개발자들 사이에서 지켜지는 약속이다.

형식이기 때문에 기술이나 언어에 구애받지 않는다. 앱을 만들든, 웹사이트를 만들든 소프트웨어간 HTTP로 정보를 주고 받는 부분이 있다면 RESTful한 서비스를 만들수가 있다.

<br>

### 그래서 REST API가 뭔가요?

기계와 기계가 규격화된 방식으로 인터넷으로 또는 웹을 통해서 통신할수 있도록 돕는 통신규칙이다.

정보들이 주고 받아지는데 있어서 개발자들 사이에 널리 쓰이는 일종의 형식이다.