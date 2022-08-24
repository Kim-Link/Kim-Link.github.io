---
layout: post
type: TIL
date: 2022-08-24 18:30
draft: false
TIL : true
category: TIL
title: "[크롬익스텐션] 구조 개선 파악"
subtitle: 어떻게..했었더라..?
writer: 000000
post-header: true
header-img : img/about.jpeg
hash-tag: [기능개선]
---


## 문제

---

- 크롬 익스텐션 실행시 간헐적으로 크롬익스텐션 인식이 되지 않음

## 원인 분석

---

- 페이지 이벤트리스너를 통해 크롬익스텐션을 실행 시키고 있었음
- 이벤트 인식 방식에 대한 개선 작업이 필요


## 해결

---

- 기존 방식
  1. 크롬익스텐션이 필요한 페이지 들어오게 되면 크롬 익스텐션 실행
  2. 이벤트 발생시 이벤트리스너를 통해 키워드 인식 후 크롬익스텐션 실행

- 개선 방식
  1. front에서 sendmessage로 'test' 보냄
  2. back에서는 background.js에서 chrome.runtime.onMessageExternal.addListener를 통해 메세지 를 받아서 sendResponse()로 { test: true } 회신
  3. true를 받은 front는 크롬익스텐션이 설치되었다고 판단하고 다시 sendMessage로 message와 data 전송
  4. back에서는 message 검증후 전달받은 message와 data를 script.js로 보낼 함수로 넘김
  5. 전달받은 함수에서 어느 script.js로 보낼지 data안의 url을 통해 결정
  6. 결정된 script.js로 chrome.tabs.sendMessage를 통해 전달
  7. script.js에서 받은 data를 가지고 함수 실행
  8. 함수 결과(result)를 sendResponse로 background.js에 회신
  9. background.js에서 받은 result를 특정 변수에 할당 
  10. chrome.runtime.onMessageExternal.addListener의 sendResponse로 front로 회신

 개선 방식이 조금 복잡해 보일수 있지만, 메세지 전달을 통해 기존 방식에 비해 정확하게 크롬익스텐션을 실행 시킬수 있다.
 
 기존방식도 localStorage.setItem("hashTags", JSON.stringify(hashTags)) 사용 등 훨씬 복잡한 과정이 있지만 생략하였다.