---
layout: post
type: LOading
date: 2021-10-14 19:49
category: Programmers
title: 올바른 괄호
subtitle: 괄호과 괄호괄호 하네...
writer: 000000
post-header: true
header-img: img/about.jpeg
hash-tag: [Programmers]
---

### **문제 설명**

------

괄호가 바르게 짝지어졌다는 것은 '(' 문자로 열렸으면 반드시 짝지어서 ')' 문자로 닫혀야 한다는 뜻입니다. 예를 들어

- "()()" 또는 "(())()" 는 올바른 괄호입니다.
- ")()(" 또는 "(()(" 는 올바르지 않은 괄호입니다.

'(' 또는 ')' 로만 이루어진 문자열 s가 주어졌을 때, 문자열 s가 올바른 괄호이면 true를 return 하고, 올바르지 않은 괄호이면 false를 return 하는 solution 함수를 완성해 주세요.

### 제한사항

------

- 문자열 s의 길이 : 100,000 이하의 자연수
- 문자열 s는 '(' 또는 ')' 로만 이루어져 있습니다.

### 입출력 예

------

| s        | answer |
| -------- | ------ |
| "()()"   | true   |
| "(())()" | true   |
| ")()("   | false  |
| "(()("   | false  |



### 내가 푼 방식

------

```tsx
function solution(s){
    s = s.split("")
    if(s[0] !== '(' || s[s.length-1] !== ')') return false;
    
    let result = false
    let open = s.filter(el => el === '(')
    let close = s.filter(el => el === ')')
    if(open.length === close.length) {
        result = true
    }
    
    return result;
}
```

- 처음에 괄호를 열거나 나중에 괄호를 닫지 않을 경우 false.
- 여는 괄호와 닫는 괄호의 숫자가 같으면 true 로 설정하였다.
- 문제점 : "())))((()" → 이런 케이스를 통과 하지 못했다.

### 다시 푼 방식

------

```tsx
function solution(s){
    s = s.split("")
    if(s[0] !== '(' || s[s.length-1] !== ')') return false;
    let result = true
    function dis ( str){
        if(str === '(') return 1
        else return -1
    }
    let counter = 0
    for(let i = 0; i < s.length; i++){
        counter += dis(s[i])
        if(counter < 0){
            result = false;
            break;
        }
    }
    if(counter !==0){
        result = false
    }
    return result;
}
```

- 열리는 괄호를 1 로, 닫히는 괄호를 -1로 바꾼뒤, 앞에서 부터 순서대로 더한다.
- 앞에서 부터 더해 가면서 더한 합이 0보다 작으면 false, 끝까지 갈수 있으면 true로 한다.

[링크] : https://programmers.co.kr/learn/courses/30/lessons/12909
