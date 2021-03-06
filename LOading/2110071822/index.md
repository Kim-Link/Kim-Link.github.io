---
layout: post
type: LOading
date: 2021-10-07 18:22
category: Programmers
title: H-index
subtitle: 프로그래머스...왜틀렸는지 안알랴쥼...
writer: 000000
post-header: true
header-img: img/about.jpeg
hash-tag: [Programmers]

---

### **문제 설명**

H-Index는 과학자의 생산성과 영향력을 나타내는 지표입니다. 어느 과학자의 H-Index를 나타내는 값인 h를 구하려고 합니다. 위키백과[1](https://programmers.co.kr/learn/courses/30/lessons/42747#fn1)에 따르면, H-Index는 다음과 같이 구합니다.

어떤 과학자가 발표한 논문 `n`편 중, `h`번 이상 인용된 논문이 `h`편 이상이고 나머지 논문이 h번 이하 인용되었다면 `h`의 최댓값이 이 과학자의 H-Index입니다.

어떤 과학자가 발표한 논문의 인용 횟수를 담은 배열 citations가 매개변수로 주어질 때, 이 과학자의 H-Index를 return 하도록 solution 함수를 작성해주세요.

### 제한사항

- 과학자가 발표한 논문의 수는 1편 이상 1,000편 이하입니다.
- 논문별 인용 횟수는 0회 이상 10,000회 이하입니다.

### 입출력 예

| citations       | return |
| --------------- | ------ |
| [3, 0, 6, 1, 5] | 3      |

### 입출력 예 설명

이 과학자가 발표한 논문의 수는 5편이고, 그중 3편의 논문은 3회 이상 인용되었습니다. 그리고 나머지 2편의 논문은 3회 이하 인용되었기 때문에 이 과학자의 H-Index는 3입니다.



### 처음에 시도한 방식

------

```js
function solution(citations) {
    // 비교함수 작성 => 숫자와 배열을 입력해서 입력받은 숫자보다 큰 배열의 숫자의 갯수를 구하는 함수
    function hCounter ( num, arr ){
        let result = arr.filter(x => {return x >= num})
        return result.length;
    }
    let result = 0
    for(let i = 1; i < citations.length ; i++){
        let count = hCounter(i,citations);
        if(i === count){
            result = i;
            break;
        }
    }
    return result;
    
}
```

이렇게 했을때 테스트중 2개가 통과과 안된다. 하.. 빨리 풀려고 대충 생각한게 문젠가..

<img src="/Users/wildrays/Desktop/Kim-Link.github.io/LOading/2110071822/img/1.jpeg" alt="1" style="zoom:50%;" />



util함수를 만들어서 풀려고 했던게 이사단을 만든거 같다..



### 리팩토링 한 방식

------

```jsx
function solution(citations) {
    //오름차순으로 먼저 정렬한다.
   citations = citations.sort((a,b) => b-a)
    //index의 숫자가 index 번호보다 같거나 크면 된다.
    //인용된 횟수보다 논문 편이 더 중요하다. 이것은 index 번호를 return 값으로 받아야 하는것이다.
    let num = 0
    while(num <= citations.length){
        if(num + 1 <= citations[num]){
            num++
        } else {
            break;
        }
    }
    return num
}
```

오류가 발생했는데 왜 그런지 이해가 안되서 다 엎었다.

먼저 오름차순으로 정렬을 한다. 오름차순으로 정렬한 이유는 큰숫자를 앞에 오게해서 점수와 횟수를 배열의 index 번호를 이용하여 쉽게 판단하기 위해서 이다.

위 코드에서 num는 index number를 뜻한다.

초기값 num을 0으로 셋팅하고 while문으로 citations.length 만큼 반복한다. ⇒ while(num <= citations.length)

num은 index number인데 실제 논문 횟수는 +1 이 되어야 하므로 num +1 과 index 숫자의 크기를 비교한다. ⇒ if(num + 1 <= citations[num])

비교 했을때 true라면 다음 index로 넘어가야 하므로 num++를 해준다.

그렇게 반복하다가 논문횟수가 점수보다 작아지는 순간 멈춰야 하고, 그때의 index number가 H-index 값이 된다.

### 느낀점

------

빨리 풀려다가 돌아갔다.

항상 문제를 만나면 처음에 막막해서 답을 풀려는데 집착하는것 같다.

다른사람들 문제 풀이를 보니 많은 사람들이 이렇게 푼것 같다.

코딩테스트를 잘보려면 더 빨리 잘 풀어야 하는데 큰일이다.

* 문제 링크 : https://programmers.co.kr/learn/courses/30/lessons/42747%5D