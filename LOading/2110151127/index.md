---
layout: post
type: LOading
date: 2021-10-15 11:27
category: Programmers
title: 숫자의 표현
subtitle: 굳이 이렇게 표현해야할까...허허...
writer: 000000
post-header: true
header-img: img/about.jpeg

hash-tag: [Programmers]
---



### **문제 설명**

------

Finn은 요즘 수학공부에 빠져 있습니다. 수학 공부를 하던 Finn은 자연수 n을 연속한 자연수들로 표현 하는 방법이 여러개라는 사실을 알게 되었습니다. 예를들어 15는 다음과 같이 4가지로 표현 할 수 있습니다.

- 1 + 2 + 3 + 4 + 5 = 15
- 4 + 5 + 6 = 15
- 7 + 8 = 15
- 15 = 15

자연수 n이 매개변수로 주어질 때, 연속된 자연수들로 n을 표현하는 방법의 수를 return하는 solution를 완성해주세요.

### 제한사항

------

- n은 10,000 이하의 자연수 입니다.

------

### 입출력 예

------

| n    | result |
| ---- | ------ |
| 15   | 4      |



### 내가 푼 방식

------

```jsx
function solution(n) {
    let count = 1;
    for( let  i = 1; i < n/2; i++){
        let num = 0
        for(let j = i; j < n;j++){
            num += j
            if(num === n){
            count += 1
            } else if( num > n){
                break;
            }
        }
    }
    return count ;
}
```

- 1부터 시작해서 숫자를 하나씩 더해서 같은 숫자가 나오면 카운트를 하나 올린다.
- 더한 숫자가 n보다 크게 될 경우 break롤 반복문을 빠져 나온다.

[링크] : https://programmers.co.kr/learn/courses/30/lessons/12924