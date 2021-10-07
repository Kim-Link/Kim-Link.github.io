---
layout: post
type: LOading
date: 2021-10-07 12:35
category: Typescript
title: Typescript실행하기
subtitle: 타입스크립트를 실행해 보자
writer: 000000
post-header: true
header-img: img/about.jpeg
hash-tag: [Typescript, 단축키]

---

>

### [ Intro ]

사이드 프로젝트를 위해 타입스크립트 공부를 시작했다.

자바스크립트를 잘 쓰다가 왜 타입스크립트를 쓰게 됬을까?

사람들은 왜 타입스크립트를 쓸까?

 

쬐끔 공부해보니 장점이 보였다.

백엔드 기준으로 자바스크립트는 에러에 대한 인식을 node를 실행시킬때 알수 있다.

반면 타입스크립트는 코드를 입력하면서 바로바로 에러를 인식할수 있다.

타입스크립트를 사용하면 에러핸들링을 하기 위해 속도가 조금 느리게 느껴질수도 있지만,

결과적으로 봤을때 에러가 발생할 확률이 낮아지기 때문에 완성도가 더 높은것 같다.

*(아직 공부하는 단계라 단편적인 느낌이겠지만..)*

 

 

### [ Typescript 실행하기 ]

타입스크립트는 기본적으로 컴파일을 통해서 자바스크립트를 만든 다음에 node.js로 실행하는 방법이 있다. 컴파일을 한 뒤 노드를 사용(node src/1.js)해서 실행할수 있다.

타입스크립트의 실행 순서 : 타입스크립트 코드 작성 → 컴파일을 통해 자바스크립트 생성 → Node를 사용하여 코드 실행

예) 1.ts → compile → 1.js → node 1.js

 

 

#### [ 필요한 vscode 익스텐션 설치 ] : Code Runner

 Code Runner는 vscode에서 실행시키고 싶은 라인을 블럭하여 constrol + option + n 누르면 해당 라인만 실행이 가능하게 하는 익스텐션이다.

익스텐션을 설치하고 나서 약간의 옵션 설정이 필요하다.

 

\1. vscode 익스텐션 검색

\2. Code Runner 설치 터미널에서 ts-node 설치하기( 터미널에 " npm install ts-node "입력)

\3. "cmd" + " , "을 누르면 검색창이 하나 뜸. executorMap 검색, 제일 위에것을 클릭( 자동으로 "code-runner.executorMap": {} 의 객체로 이동됨)

\4. "code-runner.executorMap": {} 안에 "typescript": "node_modules/.bin/ts-node" 작성 후 저장

\5. 타입스크립트 코드에 실행시키고 싶은 라인을 블럭하여 constrol + option + n

 

#### [ 타입스크립트의 플레이그라운드 사이트를 이용하기 ]

https://www.typescriptlang.org/play

이 홈페이지에서 코드를 작성하면 컴파일결과, 실행결과, 에러를 확인할수 있다.

 

#### [ 단축키 활용하기 ]

1. Auto Import : cmd + .
2. 멀티 선택 : cmd + d / 취소 : esc
3. 멀티 커서 : cmd + option + 화살표 / 취소 : es