---
layout: post
type: LOading
date: 2021-10-11 00:10
category: Programmers
title: 조이스틱
subtitle: 함정이 있었어...
writer: 000000
post-header: true
header-img: img/about.jpeg
hash-tag: [Programmers]
---



###### 문제 설명

조이스틱으로 알파벳 이름을 완성하세요. 맨 처음엔 A로만 이루어져 있습니다.
ex) 완성해야 하는 이름이 세 글자면 AAA, 네 글자면 AAAA

조이스틱을 각 방향으로 움직이면 아래와 같습니다.

```
▲ - 다음 알파벳
▼ - 이전 알파벳 (A에서 아래쪽으로 이동하면 Z로)
◀ - 커서를 왼쪽으로 이동 (첫 번째 위치에서 왼쪽으로 이동하면 마지막 문자에 커서)
▶ - 커서를 오른쪽으로 이동
```

예를 들어 아래의 방법으로 "JAZ"를 만들 수 있습니다.

```
- 첫 번째 위치에서 조이스틱을 위로 9번 조작하여 J를 완성합니다.
- 조이스틱을 왼쪽으로 1번 조작하여 커서를 마지막 문자 위치로 이동시킵니다.
- 마지막 위치에서 조이스틱을 아래로 1번 조작하여 Z를 완성합니다.
따라서 11번 이동시켜 "JAZ"를 만들 수 있고, 이때가 최소 이동입니다.
```

만들고자 하는 이름 name이 매개변수로 주어질 때, 이름에 대해 조이스틱 조작 횟수의 최솟값을 return 하도록 solution 함수를 만드세요.

##### 제한 사항

- name은 알파벳 대문자로만 이루어져 있습니다.
- name의 길이는 1 이상 20 이하입니다.

##### 입출력 예

| name     | return |
| -------- | ------ |
| "JEROEN" | 56     |
| "JAN"    | 23     |



### 시도한 방식

------

```js
function solution(name) {
    function codeNum(str){
        if(str === 'A'){return 0
        } else if (str === 'B' || str === 'Z'){ return 1
        } else if (str === 'C' || str === 'Y'){ return 2
        } else if (str === 'D' || str === 'X'){ return 3
        } else if (str === 'E' || str === 'W'){ return 4
        } else if (str === 'F' || str === 'V'){ return 5
        } else if (str === 'G' || str === 'U'){ return 6
        } else if (str === 'H' || str === 'T'){ return 7
        } else if (str === 'I' || str === 'S'){ return 8
        } else if (str === 'J' || str === 'R'){ return 9
        } else if (str === 'K' || str === 'Q'){ return 10
        } else if (str === 'L' || str === 'P'){ return 11
        } else if (str === 'M' || str === 'O'){ return 12
        } else if (str === 'N'){ return 13}
    }
    let result = 0;
    let side = 0;
    if(name[1] === 'A'){
        
        side = side + name.length -2;
    } else {
      side = side + name.length - 1 ;    
    }
    for( let i = 0 ; i < name.length ; i++){
        result = result + codeNum(name[i]);
    }
    return result + side; 
}
```

- 먼저 상하로 움직이는 경우와 좌우로 움직이는 경우를 따로 분리해서 생각함.
- countNum 함수를 만들어서 위아래로 움직이는 횟수를 카운트 함.
- 좌우로 움직이는 경우 오른쪽이 우선적으로 움직인다고 생각하고 두번째 칸에 A가 있는 경우만 좌로 움직이고 나머지 경우 우로 움직인다고 계산함.
- 채점결과 2개 통과하지 못함

⇒ 통과 하지  못한 이유는 ZZAAAAAZZ처럼 갔다가 돌아와야 하는 경우를 생각하지 않음.

### 통과한 방식

------

```js
function solution(name) {
    function codeNum(str){
        if(str === 'A'){return 0
        } else if (str === 'B' || str === 'Z'){ return 1
        } else if (str === 'C' || str === 'Y'){ return 2
        } else if (str === 'D' || str === 'X'){ return 3
        } else if (str === 'E' || str === 'W'){ return 4
        } else if (str === 'F' || str === 'V'){ return 5
        } else if (str === 'G' || str === 'U'){ return 6
        } else if (str === 'H' || str === 'T'){ return 7
        } else if (str === 'I' || str === 'S'){ return 8
        } else if (str === 'J' || str === 'R'){ return 9
        } else if (str === 'K' || str === 'Q'){ return 10
        } else if (str === 'L' || str === 'P'){ return 11
        } else if (str === 'M' || str === 'O'){ return 12
        } else if (str === 'N'){ return 13}
    }
    let result = 0;
    for( let i = 0 ; i < name.length ; i++){
        result = result + codeNum(name[i]);
    }

   let side=name.length-1;
    for(let i=1;i<name.length;i++){
        if(name[i]==='A'){
            for(var j=i+1;j<name.length;j++){
                if(name[j]!=='A') break;
            }
            const left=i-1;
            const right=name.length-j;
            side=Math.min(side,left>right ? left+right*2:left*2+right);
            i=j;
        } 
    }

    return result + side; 
}
```

- name의 중간에 A가 나올 때, 그대로 쭉 우측으로 갔을때와, 무조건 좌측으로 돌아간 경우를 비교해서 더 작은 수가 나오는 경우를 채택한다.
- 무조건 좌측으로 돌아간 경우 A가 중복으로 나왔을때, A가 나온 수 만큼 좌측으로 이동 하지 않아도 된다.
- 우측으로 이동한 만큼 좌측으로 돌아가야 하기 때문에 *2 처리를 해주어야 한다.
