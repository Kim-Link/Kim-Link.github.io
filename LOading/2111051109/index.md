---
layout: post
type: LOading
date: 2021-11-05 11:09
category: Computer Science
title: MVC Design Pattern
subtitle: Front 와 Back은 어떤 관계일까?
writer: 000000
post-header: true
header-img: img/about.jpeg
hash-tag: [기술면접, MVC]
---

분명 배웠는데,

기술면접에서 제대로 대답하지 못해 정리해 본다.

MVC Design Pattern에 대해서 대해서 알아보자.

<img src="img/1.jpeg" alt="1" style="zoom:80%;" />

# MVC 패턴

MVC는 소프트웨어 공학에서 사용되는 소프트웨어 디자인 패턴이다.

MVC 는 Model, View, Controller의 약자 이다.

모델-뷰-컨트롤러는 응용 프로그램을 세 가지의 구성요소로 나눈다. 각각의 구성요소들 사이에는 다음과 같은 관계가 있다.

<img src="img/2.jpeg" alt="2" style="zoom:50%;" />

<br>

## **컨트롤러**

- **컨트롤러**는 모델에 명령을 보냄으로써 모델의 상태를 변경할 수 있다. (예: 워드 프로세서에서 문서를 편집하는 것) 또, 컨트롤러가 관련된 뷰에 명령을 보냄으로써 모델의 표시 방법을 바꿀 수 있다. (문서를 스크롤하는 것)
- MVC의 뷰는 여러 개의 컨트롤러(controller)를 가지고 있다. 사용자는 컨트롤러를 사용하여 모델의 상태를 바꾼다. 컨트롤러는 모델의 mutator 함수를 호출하여 상태를 바꾼다. 이때 모델의 상태가 바뀌면 모델은 등록된 뷰에 자신의 상태가 바뀌었다는 것을 알리고 뷰는 거기에 맞게 사용자에게 모델의 상태를 보여 준다.

<br>

## **모델**

- **모델**은 모델의 상태에 변화가 있을 때 컨트롤러와 뷰에 이를 통보한다. 이와 같은 통보를 통해서 뷰는 최신의 결과를 보여줄 수 있고, 컨트롤러는 모델의 변화에 따른 적용 가능한 명령을 추가·제거·수정할 수 있다. 어떤 MVC 구현에서는 통보 대신 뷰나 컨트롤러가 직접 모델의 상태를 [읽어 오기](https://ko.wikipedia.org/wiki/폴링_(컴퓨터_과학))도 한다.
- 모델(model)이란 어떠한 동작을 수행하는 코드를 말한다. 표시 형식에 의존하지 않는다. 다시 말해, 사용자에게 어떻게 보일지에 대해 신경쓰지 않아도 된다. 모델은 순수하게 public 함수로만 이루어진다. 몇몇 함수들은 사용자의 질의(query)에 대해 상태 정보를 제공하고 나머지 함수들은 상태를 수정한다.

<br>

## **뷰**

- **뷰**는 사용자가 볼 결과물을 생성하기 위해 모델로부터 정보를 얻어 온다.
- MVC에서 모델은 여러 개의 뷰(view)를 가질 수 있다. 뷰는 모델에게 질의를 하여 모델로 부터 값을 가져와 사용자에게 보여준다.

<br>

## 작동 순서

1. 사용자가 브라우저를 통해 controller 조작
2. controller가 model을 통해서 데이터를 가져옴
3. 가져온 데이터 정보를 바탕으로 View에 update된 정보 표시
4. View를 통해서 사용자에게 전달
