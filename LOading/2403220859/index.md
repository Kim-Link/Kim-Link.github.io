---
layout: post
type: LOading
date: 2024-03-22 08:58
category: JavaScript
title: map과 Promise.all
subtitle: 둘이 무슨 관계인가...
writer: 0
post-header: true
header-img: ../js.jpeg
hash-tag: [술, JavaScript, Promise]
---

# map
## 정의
**`map()`** 메서드는 배열 내의 모든 요소 각각에 대하여 주어진 함수를 호출한 결과를 모아 새로운 배열을 반환합니다.

## 구문
```js
arr.map(function(element, index, array){  }, this);
```
`element` :배열에서 처리 중인 현재 요소.
`index` : 배열에서 처리 중인 현재 요소의 인덱스.
`array` : `map()`가 호출된 배열.

## 설명`
어떤 배열에 있는 모든 요소들의 값을 변경해서 만든 `새로운 배열`을 써야 할 때가 사용합니다.
여기서 중요한 점은 결과 값을 얻고 싶다면 새로운 변수에 할당 받아야 하지만,
map 안에 callback 함수만 실행 시켜도 됩니다.

```js
let arr = [2, 3, 5, 7]

const result = arr.map(function(element, index, array){
    console.log(element);
    console.log(index);
    console.log(array);
    return element+1;
}, 80);
console.log(arr, result)
```

대부분의 경우 나머지는 무시하고 콜백 함수 내부의 `element` 인수만 사용합니다.
```js
let arr = [2, 3, 5, 7]

const result = arr.map(function(element){
    return element+1;
}, 80);
console.log('input arr: ', arr)
console.log('output arr: ', result)
```


## 예시
```js 
// map-example.js
function test1() {  
    let arr = [2, 3, 5, 7]  
  
    const result = arr.map(function(element, index, array){  
        console.log('====시작====');  
        console.log(element);  
        console.log(index);  
        console.log(array);  
        console.log(this + 2);  
        console.log('==== 끝 ====');  
        return element+1;  
    }, 80);  
  
    console.log('input arr: ', arr)  
    console.log('output arr: ', result)  
}  
  
function test2() {  
    let arr = [2, 3, 5, 7]  
  
    const result = arr.map(function(element){  
        return element+1;  
    }, 80);  
    console.log('input arr: ', arr)  
    console.log('output arr: ', result)  
}  
  
test2();
```

# Promise.all
## 정의
**`Promise.all()`** 메서드는 순회 가능한 객체에 주어진 모든 프로미스가 이행한 후, 혹은 프로미스가 주어지지 않았을 때 이행하는 [`Promise`](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Promise)를 반환합니다. 주어진 프로미스 중 하나가 거부하는 경우, 첫 번째로 거절한 프로미스의 이유로 `promise.all` 전체가 `reject`됩니다.

## 구문

```js
Promise.all(iterable);
```
[`iterable`](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Promise/all#iterable): `Array`와 같이 [순회 가능](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Iteration_protocols#the_iterable_protocol)한(iterable) 객체.


## 설명
- Promise.all() 은 여러 개의 `Promise` 들을 비동기적으로 실행하여 처리할 수 있다.
- Promise.all() 은 여러 개의 `Promise` 들 중 하나라도 reject 를 반환하거나 에러가 날 경우, 모든 Promise 들을 reject 시킨다.

## 예시

```js 
// map-promise-1.js
async function sampleFunc(){  
    // time start  
    console.time('promise all example');  
  
    // first promise  
    const fetchNameList = async ()=> {  
        return new Promise((resolve, reject) => {  
            setTimeout(() => {  
                const result= ['Jack', 'Joe', 'Beck'];  
                resolve(result);  
            }, 300);  
        });  
    };  
  
    // second promise  
    const fetchFruits = async ()=> {  
        return new Promise((resolve, reject) => {  
            setTimeout(() => {  
                const result  = ['Apple', 'Orange', 'Banana'];  
                resolve(result);  
            }, 200);  
        });  
    };  
  
    // third promise  
    const fetchTechCompanies = async ()=> {  
        return new Promise((resolve, reject) => {  
            setTimeout(() => {  
                const result = ['Apple', 'Google', 'Amazon'];  
                resolve(result);  
            }, 400);  
        });  
    };  
  
  
    // promise all  
    const result = await Promise.all([  
        fetchNameList(),  
        fetchFruits(),  
        fetchTechCompanies(),  
    ]);  
  
    // time end  
    console.timeEnd('promise all example');  
  
    console.log(result);  
}  
  
// execute  
sampleFunc();
```


# map과 Promise.all
## map 만 가지고 요청 보내면 안되나요~?
### 응 안돼~
- map 만 가지고 진행했을때 에러 발생합니다.
```js
// map-promise-3.js
async function sampleFunc(){  
    // time start  
    console.time('map example');  
  
    // 비동기 작업을 수행하는 함수 예시  
    function asyncTask(item) {  
        return new Promise((resolve, reject) => {  
            // 비동기 작업 수행 (여기서는 간단히 setTimeout으로 대체)  
            setTimeout(() => {  
                resolve(item * 2); // 작업 완료 후 결과 반환  
            }, item * 100); // 시간 대기  
        });  
    }  
  
// 비동기 작업을 처리할 데이터 배열  
    const data = [1, 2, 3, 4, 5];  
  
// 각 항목에 대해 asyncTask 함수를 적용하여 비동기 작업을 수행하고 결과를 모읍니다.  
    const result = data.map(asyncTask);  
  
    // time end  
    console.timeEnd('map example');  
  
    console.log(result);  
}  
  
// execute  
sampleFunc();
```

### 그럼 어떻게 해??
- `promise.all`도 같이쓰렴~
```js
// map-promise-4.js
async function sampleFunc(){  
    // time start  
    console.time('map example');  
  
    // 비동기 작업을 수행하는 함수 예시  
    function asyncTask(item) {  
        return new Promise((resolve, reject) => {  
            // 비동기 작업 수행 (여기서는 간단히 setTimeout으로 대체)  
            setTimeout(() => {  
                resolve(item * 2); // 작업 완료 후 결과 반환  
            }, item * 100); // 시간 대기  
        });  
    }  
  
// 비동기 작업을 처리할 데이터 배열  
    const data = [1, 2, 3, 4, 5];  
  
// 각 항목에 대해 asyncTask 함수를 적용하여 비동기 작업을 수행하고 결과를 모읍니다.  
    const promises = data.map(asyncTask);  
  
    const result = await Promise.all(promises)  
  
  
    // time end  
    console.timeEnd('map example');  
  
    console.log(result);  
}  
  
// execute  
sampleFunc();
```

### 근데 되는건 뭐야?
- 근데 이건 왜 돼??
```js
// map-promise-5.js
async function sampleFunc(){  
    // time start  
    console.time('map example');  
  
    // 비동기 작업을 수행하는 함수 예시  
    function asyncTask(item) {  
        // 비동기 작업 수행 (여기서는 간단히 setTimeout으로 대체)  
        setTimeout(() => {  
            console.log('짜란~ : ',item)  
        }, item * 1000);// 시간 대기  
        return item * 2  
    }  
  
// 비동기 작업을 처리할 데이터 배열  
    const data = [1, 2, 3, 4, 5];  
  
// 각 항목에 대해 asyncTask 함수를 적용하여 비동기 작업을 수행하고 결과를 모읍니다.  
    const result = data.map(asyncTask);  
  
    // time end  
    console.timeEnd('map example');  
  
    console.log(result);  
}  
  
// execute  
sampleFunc();
```
- 이건 `return`이 Promise가 아니잖아~

## 결론
대부분의 요청 방식(fetch, axios 등)은 promise방식으로 통신하고 있기 때문에 promise.all을 사용해야 한다.



### 참고 문헌
- https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Promise/all
- https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Promise
- https://velog.io/@jay2u8809/Promise.all-%EB%A1%9C-%EB%B9%84%EB%8F%99%EA%B8%B0-%EC%B2%98%EB%A6%AC%EB%A5%BC-%EA%B5%AC%ED%98%84%ED%95%B4-%EB%B3%B4%EC%9E%90