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

2차원 행렬 arr1과 arr2를 입력받아, arr1에 arr2를 곱한 결과를 반환하는 함수, solution을 완성해주세요.

### 제한 조건

------

- 행렬 arr1, arr2의 행과 열의 길이는 2 이상 100 이하입니다.
- 행렬 arr1, arr2의 원소는 -10 이상 20 이하인 자연수입니다.
- 곱할 수 있는 배열만 주어집니다.

### 입출력 예

------

| arr1                              | arr2                              | return                                     |
| --------------------------------- | --------------------------------- | ------------------------------------------ |
| [[1, 4], [3, 2], [4, 1]]          | [[3, 3], [3, 3]]                  | [[15, 15], [15, 15], [15, 15]]             |
| [[2, 3, 2], [4, 2, 4], [3, 1, 4]] | [[5, 4, 3], [2, 4, 1], [3, 1, 1]] | [[22, 22, 11], [36, 28, 18], [29, 20, 14]] |



### 내가 푼 방식

------

```jsx
function solution(arr1, arr2) {
    let result = [];
    for(let i = 0 ; i < arr1.length ; i++){
    let column = [];    
        for(let j = 0 ; j < arr2[0].length ; j++){
            let row = 0;
            for(let k = 0; k < arr2.length ; k++){
                row += arr1[i][k] * arr2[k][j];
            }
            column.push(row);
        }
    result.push(column);
    }
    return result;
}
```

- 행렬 곱셈에 대한 원리를 잘 이해 해야한다.
- 처음에 이중 for 문을 사용 했었는데 안풀려서 손코딩 진행 했다.
- 주의 할점은 2번째 for문에서 length를 arr1에서 가지고 오면 안되고 arr2에서 가지고 와야한다.



[링크] : https://programmers.co.kr/learn/courses/30/lessons/12949
