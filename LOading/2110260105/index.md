---
layout: post
type: LOading
date: 2021-10-26 01:05
category: Programmers
title: 튜플
subtitle: 튜플 자체를 이해하기 힘들었...
writer: 000000
post-header: true
header-img: img/about.jpeg

hash-tag: [Programmers]
---



### 문제 설명**

셀수있는 수량의 순서있는 열거 또는 어떤 순서를 따르는 요소들의 모음을 튜플(tuple)이라고 합니다. n개의 요소를 가진 튜플을 n-튜플(n-tuple)이라고 하며, 다음과 같이 표현할 수 있습니다.

- (a1, a2, a3, ..., an)

튜플은 다음과 같은 성질을 가지고 있습니다.

1. 중복된 원소가 있을 수 있습니다. ex : (2, 3, 1, 2)
2. 원소에 정해진 순서가 있으며, 원소의 순서가 다르면 서로 다른 튜플입니다. ex : (1, 2, 3) ≠ (1, 3, 2)
3. 튜플의 원소 개수는 유한합니다.

원소의 개수가 n개이고, **중복되는 원소가 없는** 튜플 `(a1, a2, a3, ..., an)`이 주어질 때(단, a1, a2, ..., an은 자연수), 이는 다음과 같이 집합 기호 '{', '}'를 이용해 표현할 수 있습니다.

<br>

### **[제한사항]**

- s의 길이는 5 이상 1,000,000 이하입니다.
- s는 숫자와 '{', '}', ',' 로만 이루어져 있습니다.
- 숫자가 0으로 시작하는 경우는 없습니다.
- s는 항상 중복되는 원소가 없는 튜플을 올바르게 표현하고 있습니다.
- s가 표현하는 튜플의 원소는 1 이상 100,000 이하인 자연수입니다.
- return 하는 배열의 길이가 1 이상 500 이하인 경우만 입력으로 주어집니다.

<br>

<br>

<br>

### 내가푼 방식

```jsx
function solution(s) {
    s= s.replace(/[{}]/g,'').split(',')
    
    //객에 숫자를 카운팅 해서 넣는다.
    let obj = {}
    for(let i = 0; i < s.length; i++){
        if(obj[s[i]]){
            obj[s[i]] += 1
        } else {
            obj[s[i]] = 1
        }
    }
		//value 값으로 sort
	  //key 값으로 result push
    let result = []
    for (const prop in obj){

        let arr =[]
        arr.push(prop,obj[prop])
        result.push(arr)
    }
    result = result.sort((a,b) => b[1]-a[1]).map((el) => Number(el[0]) )
    return result
    
    
}
```

- 객체에 각 숫자를 카운팅 해서 넣는다.
- 카운팅된 숫자를 이차원 객체로 하여 객체안에 넣는다.
- sort를 통해 내림차순으로 정렬을 하고 map을 통해 숫자만 남도록 바꿔준다.

<br>

<br>

### 문제 추가 설명
**앞에서부터 추가되는 숫자 순으로 튜플을 유추할 수 있다.**