---
layout: post
type: LOading
date: 2021-10-20 14:23
category: Programmers
title: 영어 끝말잇기
subtitle: 끝말잇기 판정이 이렇게 짜증날줄 몰랐지..
writer: 000000
post-header: true
header-img: img/about.jpeg

hash-tag: [Programmers]
---

###### 문제 설명

1부터 n까지 번호가 붙어있는 n명의 사람이 영어 끝말잇기를 하고 있습니다. 영어 끝말잇기는 다음과 같은 규칙으로 진행됩니다.

1. 1번부터 번호 순서대로 한 사람씩 차례대로 단어를 말합니다.
2. 마지막 사람이 단어를 말한 다음에는 다시 1번부터 시작합니다.
3. 앞사람이 말한 단어의 마지막 문자로 시작하는 단어를 말해야 합니다.
4. 이전에 등장했던 단어는 사용할 수 없습니다.
5. 한 글자인 단어는 인정되지 않습니다.

다음은 3명이 끝말잇기를 하는 상황을 나타냅니다.

tank → kick → know → wheel → land → dream → mother → robot → tank

위 끝말잇기는 다음과 같이 진행됩니다.

- 1번 사람이 자신의 첫 번째 차례에 tank를 말합니다.
- 2번 사람이 자신의 첫 번째 차례에 kick을 말합니다.
- 3번 사람이 자신의 첫 번째 차례에 know를 말합니다.
- 1번 사람이 자신의 두 번째 차례에 wheel을 말합니다.
- (계속 진행)

끝말잇기를 계속 진행해 나가다 보면, 3번 사람이 자신의 세 번째 차례에 말한 tank 라는 단어는 이전에 등장했던 단어이므로 탈락하게 됩니다.

사람의 수 n과 사람들이 순서대로 말한 단어 words 가 매개변수로 주어질 때, 가장 먼저 탈락하는 사람의 번호와 그 사람이 자신의 몇 번째 차례에 탈락하는지를 구해서 return 하도록 solution 함수를 완성해주세요.

<br>

##### 제한 사항

- 끝말잇기에 참여하는 사람의 수 n은 2 이상 10 이하의 자연수입니다.
- words는 끝말잇기에 사용한 단어들이 순서대로 들어있는 배열이며, 길이는 n 이상 100 이하입니다.
- 단어의 길이는 2 이상 50 이하입니다.
- 모든 단어는 알파벳 소문자로만 이루어져 있습니다.
- 끝말잇기에 사용되는 단어의 뜻(의미)은 신경 쓰지 않으셔도 됩니다.
- 정답은 [ 번호, 차례 ] 형태로 return 해주세요.
- 만약 주어진 단어들로 탈락자가 생기지 않는다면, [0, 0]을 return 해주세요.<br>

------

##### 입출력 예

| n    | words                                                        | result |
| ---- | ------------------------------------------------------------ | ------ |
| 3    | ["tank", "kick", "know", "wheel", "land", "dream", "mother", "robot", "tank"] | [3,3]  |
| 5    | ["hello", "observe", "effect", "take", "either", "recognize", "encourage", "ensure", "establish", "hang", "gather", "refer", "reference", "estimate", "executive"] | [0,0]  |
| 2    | ["hello", "one", "even", "never", "now", "world", "draw"]    | [1,3]  |

<br>

##### 입출력 예 설명

입출력 예 #1
3명의 사람이 끝말잇기에 참여하고 있습니다.

- 1번 사람 : tank, wheel, mother
- 2번 사람 : kick, land, robot
- 3번 사람 : know, dream, `tank`

와 같은 순서로 말을 하게 되며, 3번 사람이 자신의 세 번째 차례에 말한 `tank`라는 단어가 1번 사람이 자신의 첫 번째 차례에 말한 `tank`와 같으므로 3번 사람이 자신의 세 번째 차례로 말을 할 때 처음 탈락자가 나오게 됩니다.

입출력 예 #2
5명의 사람이 끝말잇기에 참여하고 있습니다.

- 1번 사람 : hello, recognize, gather
- 2번 사람 : observe, encourage, refer
- 3번 사람 : effect, ensure, reference
- 4번 사람 : take, establish, estimate
- 5번 사람 : either, hang, executive

와 같은 순서로 말을 하게 되며, 이 경우는 주어진 단어로만으로는 탈락자가 발생하지 않습니다. 따라서 [0, 0]을 return하면 됩니다.

입출력 예 #3
2명의 사람이 끝말잇기에 참여하고 있습니다.

- 1번 사람 : hello, even, `now`, draw
- 2번 사람 : one, never, world

와 같은 순서로 말을 하게 되며, 1번 사람이 자신의 세 번째 차례에 'r'로 시작하는 단어 대신, n으로 시작하는 `now`를 말했기 때문에 이때 처음 탈락자가 나오게 됩니다.

<br>

### 내가 푼 방식

------

```jsx
function solution(n, words) {
    let countNum = 0
    let countTurn = 0
    let resultIndex = 0
    let arr = []
    for(let i = 1; i < words.length ; i++){
       let word1 = words[i -1]
       let word2 = words[i]
       arr.push(word1)
        if(resultIndex === 0){
            if(word2[0] !== word1[word1.length-1] || arr.includes(word2)){
            resultIndex = i;
            break;
            }   
        }
    }
    if(resultIndex > 0){
     countTurn = parseInt(resultIndex / n) + 1 
     countNum = (resultIndex + 1) % n || n
    }
    
    return [countNum, countTurn]
}
```

- for문을 사용해서 순서대로 알파벳 첫 글자와 마지막 글자를 비교한다.
- 처음에 빈 배열 arr에 첫번째 단어를 넣어 두고, 두번째 단어 부터 비교해 나간다.
- includes를 활용해서 word2의 단어가 arr안에 있는 단어인지 확인한다.
- 틀린 단어를 발견했을때, 그 index를 resultIndex안에 넣어둔다.
- 틀린 Index를 가지고 몇번째 사람이 몇번째 턴에서 틀렸는지 유추해 낸다.

<br>

### 리팩토링 한다면?

- 따로 resultIndex에 담지 않고 바로 return 을 사용해서 변수 할당을 줄일수 있다.

<br>

### 다른사람이 푼 방식

------

```jsx
function solution(n, words) {
    let answer = 0;
    words.reduce((prev, now, idx) => {
        answer = answer || ((words.slice(0, idx).indexOf(now) !== -1 || prev !== now[0]) ? idx : answer);
        return now[now.length-1];
    }, "")

    return answer ? [answer%n+1, Math.floor(answer/n)+1] : [0,0];
}
```

- reduce를 활용하였다.
- 첫글자와 마지막글자 비교와 이전 사용 단어 유무를 이중 삼항연산자로 처리하여 바로 index값을 찾아 내었다.

[링크] : https://programmers.co.kr/learn/courses/30/lessons/12981#
