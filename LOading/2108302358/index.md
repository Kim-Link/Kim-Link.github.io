---
layout: post
type: LOading
date: 2021-08-30 23:58
category: Algorithm
title: 피보나치 수열 풀기(feat. JavaScript)
subtitle: 외워놓고 쉽게 풀자 피보나치!!
writer: 000000
post-header: true
header-img: img/about.jpeg
hash-tag: [Algorithm, 피보나치 수열]
---

피보나치 수열은

1,1,2,3,5,8,13,21,34... 로 진행 되는 수열이다.

이 피보나치 수열을 처음 코딩테스트로 접했을때 굉장히 당황스러웠다.

여기서 여러가지 방법을 소개 하고자 한다.

 

\1. for문

```
function fibonacci(num) {
  let sum = [0, 1];
  if(num === 0){
    sum = [0]
    return sum
  }
  else if(num === 1){
    return sum
  } else {
    for(i=2; i <= num; i++){
     sum.push(sum[i-1]+ sum[i-2]);
    }
  return sum;
  }
}
function fibonacci(num) {
  let fibNum = [];

  for (let i = 0; i <= num; i++) {
    if (i === 0) {
      fibNum.push(0);
    } else if (i === 1) {
      fibNum.push(1);
    } else {
      fibNum.push(fibNum[i - 2] + fibNum[i - 1]);
    }
  }

  return fibNum;
}
```

\2. 재귀 함수 사용

```
function fibonacci(num) {
  if( num <= 1){
    return num;
  }
  return  fibonacci(num-1) + fibonacci(num-2);
}
```

 

\3. 데이터 저장식

```
function fibonacci(n) {
  let first = 0;
  let second = 1;
  let result;
  if(n === 0){
    return first;
  } else if(n === 1){
    return second;
  }
 
  const fibo = function(num){
    if(num === 1){
      return result;
    }
    else{
      num--;
      result = first + second;
      first = second;
      second = result;
      return fibo(num)
    }
  }
  return fibo(n);
}
```
