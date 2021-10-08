---
layout: post
type: LOading
date: 2021-10-07 19:02
category: Programmers
title: JadenCase 문자열 만들
subtitle: 벽... 벽이 있음이 분명해....
writer: 000000
post-header: true
header-img: img/about.jpeg
hash-tag: [Programmers]
---

### 문제 설명

------

JadenCase란 모든 단어의 첫 문자가 대문자이고, 그 외의 알파벳은 소문자인 문자열입니다. 문자열 s가 주어졌을 때, s를 JadenCase로 바꾼 문자열을 리턴하는 함수, solution을 완성해주세요.

### 제한 조건

- s는 길이 1 이상인 문자열입니다.
- s는 알파벳과 공백문자(" ")로 이루어져 있습니다.
- 첫 문자가 영문이 아닐때에는 이어지는 영문은 소문자로 씁니다. ( 첫번째 입출력 예 참고 )

### 입출력 예

| s                       |         return          |
| ----------------------- | :---------------------: |
| "3people unFollowed me" | "3people Unfollowed Me" |
| "for the last week"     |   "For The Last Week"   |

### 

### 시도한 방식

------

```jsx
function solution(s) {
    //제일 첫글자만 대문자로 변환하는 함수 작성
    function makeUpperFirstword (word){
        return word.charAt(0).toUpperCase() + word.slice(1)
    }
    //전체 소문자로 변환
    let str = s.toLowerCase();
    let arr = str.split(' ');
    let result = ''
    for(let i = 0 ; i < arr.length ; i++){
       result = result + ' ' + makeUpperFirstword(arr[i])
    }
    return result.slice(1);
}
```

- 접근방식

1. 모두 소문자로 전환
2. split을 사용하면 문자열을 배열로 바뀌는 특성으로 공백(' ')을 기준으로 모두 자름
3. 제일 첫 글자만 대문자로 변환해주는 util함수를 작성
4. 반복문을 통해 각 배열값마다 함수 적용을 하면서 result값에 다시 문자열로 합성
5. 합성할때 제일 앞에 공백이 남아서 slice로 제거

### 다른사람의 풀이

------

```jsx
function solution(s) {
    return s.split(" ").map(v => v.charAt(0).toUpperCase() + v.substring(1).toLowerCase()).join(" ");
}
```

### 느낀점

------

벽...벽이 있다...

나도 한줄로 코드를 적어 내는 간지를 뿜어 내고 싶다...

반면 함수의 유틸화를 시키려고 생각을 많이 한다.

문제를 많이 푸는것도 좋지만, 함수를 유틸화 시켜서 여러군데 사용할수 있다면,

한문제를 봤을때는 둘러가는것 같지만 여기저기 쓸때는 이득이지 않을까... 라며 위로해본다..쩝...