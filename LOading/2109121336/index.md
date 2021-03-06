---
layout: post
type: LOading
date: 2021-09-12 13:36
category: Javascript
title: 동기와 비동기

subtitle: 대뇌야 괜찮니..?

writer: 000000
post-header: true
header-img: img/about.jpeg

hash-tag: [Javascript, 상식, SyncAsync]
---





*동기와 비동기는 너무 헷갈린다.*

**동기는 요청과 결과가 동시에 일어나는 거고,**

**비동기는 요청과 결과가 동시에 일어나지 않는거다.**

*(요청과 결과에 대한 인지가 필요하다. 그렇지 않으면 이후에 생각할 블럭과 논블럭, CallBack함수, Promise, Async/awiat에서 개념이 헷갈리기 쉽다.)*

동기와 비동기는 장단점이 뚜렷하다.

**동기는 간단하고 직관적이지만 요청한 뒤 결과가 나올때 까지 기다려야 한다.**

**비동기는 동기에 비해 복잡하지만 요청한뒤 결과가 나올때까지 기다리는 동안 다른 작업도 할수 있다.**

 

카페로 예를 들어보자.

동기적으로 운영하는 카페 A 가 있다고 하자.

아메리카노 한잔 만드는데 3분이 걸릴때,

3명의 손님이 주문한다고 했을때, 세번째 손님은 10분후에 커피를 마실수 있다.

비동기적으로 운영하는 카페B 가 있다.

똑같이 아메리카노 한잔을 만드는데 3분이 걸릴때,

3명의 손님이 주문할때 세번재 손님은 3분후에 커피를 마실수가 있다.

*⇒ 만약 손님이 1000명이 몰렸을때는 어떻게 될까?*

 

결론은 커피를 주문하는것이 요청이 되고, 커피를 받는것이 결과가 될때,

요청후 결과가 나올때 까지 기다려서 요청과 결과가 같이 이루어져야 하는것이 동기,

요청후 다른 요청을 또 추가로 받고 결과가 나오는데로 결과를 얻어갈수 있는 것이 비동기 이다.

#### 동기가 무조건 안좋고 비동기가 무조건 좋을순 없다.

어떤 요청을 받고 난뒤 결과를 가지고 다음 작업을 해야할때는 동기적으로 처리해야하고,

많은양의 데이터를 독립된 과정의 결과가 요구될때는 비동기적으로 처리해야 할것이다.

상황에 따라서 적절하게 사용해야한다.

 

다음 글로는 블럭과 논블럭에 대해서 알아볼건데

위 카페 예제는 정말 쉽게 개념을 이해하기 위한것이다.

실제 사용은 훨씬더 복잡하니

지금은 동기와 비동기의 개념에서 요청과 결과가 있다는것을 인지하고 있도록 하자.