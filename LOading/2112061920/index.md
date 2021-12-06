---
layout: post
type: LOading
date: 2021-12-06 19:20
category: Computer Science
title: redis에 대해서
subtitle: 너 편한거 맞지?? 
writer: 000000
post-header: true
header-img: img/about.jpeg
hash-tag: [redis]
---

입사후 redis라는 놈에 대해서 알게 되었다.
설명을 여러번 들었는데도 이해가 잘 가지 않아서 따로 찾아봤다.
우연히 찾은 링크에서 설명이 굉장히 잘되어 있어 읽어보고 간추려 보았다.

아주 간단히 요약하면 레디스는 캐싱을 위해 사용하는 것이다. DB에 가지 않고 메모리에 캐싱으로 남겨 놓는거다. 이렇게 하게 되면 DB에 트렌젝션을 줄일수 있고 실제 DB에 요청을 보내고 응답받는것 보다 훨씬 빠르다.
*~~(트렌젝션에 대한 뜻과 의미는 다음에 다시 한번 정리해봐야 겠다.)~~*

# redis란?

-   Redis는 Remote Dictionary Server의 약자로서, 키-값(Key-Value) 구조의 비정형 데이터를 저장하고 관리하기 위한 오픈 소스 기반의 비관계형 데이터베이스 관리 시스템이다.
-   Redis는 메모리 기반의 저장소이기 때문에 필요한 정보를 빠르게 저장하고 가져올 수 있는 저장소이다.
-   Redis는 메모리를 캐시처럼 사용한다. 메모리는 휘발성이기 때문에 시스템이 꺼지면 모든 데이터가 날아가게 된다. 따라서 데이터를 지속적으로 유지하기 위해 모든 작업을 로그에 기록해서 디스크에 저장한 후, 시스템을 구동할 때 로그를 기반으로 데이터를 다시 메모리에 올리는 방식을 사용한다.
-   전체 데이터를 영구히 저장하기보다는, 캐시처럼 휘발성이나 임시성 데이터를 저장하는 데 많이 사용된다.

⇒ 메모리를 이용하여 고속으로 <key, value> 스타일의 데이터를 저장하고 불러올 수 있는 시스템이다.

# redis의 특징

1.  Key/Value
    
2.  다양한 데이터 타입
    

-   String - 일반적인 문자열로 최대 512Mbyte 길이까지 지원, Text 문자열뿐만 아니라 Integer와 같은 숫자나 JPEG와 같은 Binary 파일까지 저장 가능
-   Set - String의 집합으로 여러 개의 값을 하나의 Value 내에 넣을 수 있음. 정렬되지 않은 집합형으로, 한 Key에 중복된 데이터는 존재하지 않음. Set에 포함된 요소의 수와 관계없이 일정한 시간으로 체크를 할 수 있는 것이 장점.
-   Sorted Sets - Set에 Score라는 필드가 추가된 데이터형으로, Score는 일종의 가중치. 데이터는 오름차순으로 내부 정렬되며, 정렬하는 기준이 Score. Score 값은 중복될 수 없음.
-   Lists - String의 집합으로 Set과 유사하지만 일종의 양방향 Linked List. List 앞과 뒤에서 push/pop 연산을 사용해 데이터를 추가 및 삭제가 가능.
-   Hashes - value에 field와 string value 쌍으로 이루어진 테이블을 저장. 객체를 나타내는 데 사용 가능. 형태는 List와 비슷하나 필드명, 필드값의 연속으로 이루어짐.

# redis 설치하기

1.  npm install redis
2.  sudo apt-get install redis-server
<br>
<br>




[참고 링크] [](https://edu.goorm.io/learn/lecture/557/%ED%95%9C-%EB%88%88%EC%97%90-%EB%81%9D%EB%82%B4%EB%8A%94-node-js/lesson/174388/redis%EB%9E%80)[https://edu.goorm.io/learn/lecture/557/한-눈에-끝내는-node-js/lesson/174388/redis란](https://edu.goorm.io/learn/lecture/557/%ED%95%9C-%EB%88%88%EC%97%90-%EB%81%9D%EB%82%B4%EB%8A%94-node-js/lesson/174388/redis%EB%9E%80)