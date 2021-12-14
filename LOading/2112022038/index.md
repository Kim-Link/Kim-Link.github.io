---
layout: post
type: LOading
date: 2021-12-02 20:38
category: JavaScript
title: callback, promise, async
subtitle: 가깝고도 먼...
writer: 000000
post-header: true
header-img: img/about.jpeg
hash-tag: [callback, promise, async]
---

동기, 비동기, 블로킹, 논블로킹에 대해서는 이미 앞에서 다루었다.

앞에서 다룬것은 이론적인 부분이었고, 실제 사용하려하니 개념부터 다시 흔들리기 시작했다.

역시 실전은 많이 다른것 같다.

# 내가 잘못생각했던 부분

내가 실제 코드에서 놓치고 있던 부분이 뭘까?

<aside> 💡 자바스크립트는 비동기, 논블로킹 이라는 사실. </aside>

물론 자바스크립트가 비동기, 논블로킹 이라는 것은 알고 있었다. 그냥 그렇다고 알고 있었다.
하지만 이 의미를 안다는 것은 조금 더 다르다. 자바스크립트의 디폴트 값이 비동기 이기 때문에, 내가 실행하는 모든 함수에 대해서 따로 동기 처리를 해주지 않으면 결과를 기다리지 않고 그냥 지나친다.

<aside> 💡 awiat에 대한 이해가 부족. </aside>

밑에서 다시 한번 설명 할테지만, await 은 await 뒤에 쓰여지는 함수에 대해서 동기 처리 하는것이며, 그 함수가 리턴값을 가져 올때까지 기다린다.

동기처리는 함수 시간이 오래걸리는 서버측 코드에서 반드시 해야 하는 작업이며, axios는 기본적으로 Promise 처리를 한다.

<aside> 💡 함수를 거시적으로 보지 못해, 어느 함수가 동기적으로 일어 나야 하는지 제대로 인식하지 못함. </aside>

함수안에 함수를 작성 하다보니 내가 적는 이 함수가 async/await 처리가 되었는지 Promise 처리가 되었는지, 아니면 그냥 비동기로 처리되고 있는지에 대한 인지가 부족했다.

함수를 작성할때 좀더 생각하면서 거시적인 관점에서 이해하고 작성할 필요를 느꼈다.

----------
<br>

# Callback

비동기로 작동하는 자바스크립트에서 동기적으로 바꿔주는 방법중 하나...라고만 생각하면 안되고, 자바스크립트에서 콜백 함수는 어떤 함수에서 매개변수로 함수를 전달하고, 이벤트가 발생한 뒤 `함수가 다시 호출`되는것을 의미한다.

여기서 중요한것은 `이벤트가 발생한 뒤` 이다.

비동기 방식은 이벤트가 발생하는 중이라 할지라도 다음 함수를 진행시키지만, 콜백을 사용하면 함수가 끝나기를 기다려야 한다. 그렇기 때문에 비동기로 작용하는 자바스크립트를 동기적으로 바꿔줄수가 있다.

<br>

### Async 가 좋은 것은 알겠는데..순서를 제어하고 싶은 경우엔 어떻게?

```jsx
const printString = (string) => {
    setTimeout(
      () => {
        console.log(string)
      }, 
      Math.floor(Math.random() * 100) + 1
    )
  }

  const printAll = () => {
    printString("A")
    printString("B")
    printString("C")
  }
  printAll() // what do you expect?

```

위 예제의 결과는 어떻게 될까?

정답은 랜덤으로 나온다.

왜그럴까?

printAll 함수안에 printStirng 함수가 있고, printString 안에는 Math.random 함수가 있다. 때문에 함수를 실행시킬때 마다 나타나는 결과가 다르다.

그렇다면 이 순서를 제어하기 위해서는 어떻게 해야 할까?

<br>


### Callback

```jsx
const printString = (string, callback) => {
    setTimeout(
      () => {
        console.log(string)
        callback()
      }, 
      Math.floor(Math.random() * 100) + 1
    )
  }

  const printAll = () => {
    printString("A", () => {
      printString("B", () => {
        printString("C", () => {})
      })
    })
  }
  printAll() // now what do you expect?

```

위 코드의 결과는 어떻게 될까?

정답은 A,B,C 이다.

왜그럴까?

printAll 함수안에 똑같이 printString이 있는데, 이번에는 그냥 나열된 것이 아니라 callback으로 연결 되어 있다.

무슨 뜻이냐 하면, 첫번째 진행된 함수가 끝날때 까지 기다렸다가 다음 함수를 진행 한다.

그렇기 때문에 printStirng 안에 함수가 몇초가 걸리든 기다렸다가 실행되기 때문에 항상 A,B,C 로 나타나게 된다.

<br>


### Callback error handling Design

그렇다면 함수를 순차적으로 진행하다가 중간에 에러가 나면 어떻게 해야 할까?

```jsx
const somethingGonnaHappen = callback => {
    waitingUntilSomethingHappens()

    if (isSomethingGood) {
        callback(null, something)
    }

    if (isSomethingBad) {
        callback(something, null)
    }
}

```

콜백함수의 경우 위의 코드 처럼 if문 처리를 통해서 에러가 아닌경우와 에러가 난경우를 분기시켜 다음 함수를 진행한다. 이렇게 적을 경우 함수의 양도 많아지고, if문도 여러번 적여야 하는 번거로움이 있다.

<br>

### 오류 우선 콜백 (Error-First Callback)

```jsx
somethingGonnaHappen((err, data) => {
    if (err) {
        console.log('ERR!!');
        return;
    }
    return data;
})

```

콜백을 사용하면 에러처리가 힘들져 `콜백의 첫 번째 매개변수에 에러 객체를 쓰자` 고 약속한것이 오류 우선 콜백이다.

오류 우선 콜백 스타일을 사용해서 에러가 났을때, 뒤에 return을 사용해서 에러가 났음에도 그대로 진행되게 할수도 있고, return을 넣지 않고 그대로 멈추게 할수도 있다.

-   콜백의 첫 번째 매개변수에 에러 객체를 사용
-   에러가 null이나 undefined이면 정상이라고 판단

<br>

# Promise

**Promise** 객체는 비동기 작업이 맞이할 미래의 완료 또는 실패와 그 결과 값을 나타낸다.

Promise 객체는 세 가지 상태를 가질 수 있다.

-   대기(_pending)_: 이행하거나 거부되지 않은 초기 상태. 로직이 아직 처리 되지 않은 상태.
    
-   이행(_fulfilled)_: 연산이 성공적으로 완료됨.
    
-   거부(_rejected)_: 연산이 실패함.

<details>
<summary>세가지 상태</summary>
<div markdown="1">  

    ### 대기(_pending)_
    
    `new Promise()` 메서드를 호출하면 대기(Pending) 상태가 된다.
    
    ```jsx
    new Promise();
    
    ```
    
    `new Promise()` 메서드를 호출할 때 콜백 함수를 선언할 수 있고, 콜백 함수의 인자는 `resolve`, `reject` 이다.
    
    ```jsx
    new Promise(function(resolve, reject) {
    // ...
    });
    
    ```
    
    ### 이행(_fulfilled)_
    
    콜백 함수의 인자 `resolve`를 아래와 같이 실행하면 이행(Fulfilled) 상태가 된다.
    
    ```jsx
    new Promise(function(resolve, reject) {
      resolve();
    });
    
    ```
    
    그리고 이행 상태가 되면 아래와 같이 `then()`을 이용하여 처리 결과 값을 받을 수 있다.
    
    ```
    function getData() {
      return new Promise(function(resolve, reject) {
        var data = 100;
        resolve(data);
      });
    }
    
    // resolve()의 결과 값 data를 resolvedData로 받음getData().then(function(resolvedData) {
      console.log(resolvedData);// 100});
    
    ```
    
    ### 거부(_rejected)_
    
    `reject`를 아래와 같이 호출하면 실패(Rejected) 상태가 된다.
    
    ```jsx
    new Promise(function(resolve, reject) {
      reject();
    });
    
    ```
    
    그리고, 실패 상태가 되면 실패한 이유(실패 처리의 결과 값)를 `catch()`로 받을 수 있다.
    
    ```jsx
    function getData() {
      return new Promise(function(resolve, reject) {
        reject(new Error("Request is failed"));
      });
    }
    
    // reject()의 결과 값 Error를 err에 받음
    getData().then().catch(function(err) {
      console.log(err);// Error: Request is failed
    });
    
    ```
    
  </div>
</details>
    
<br>

### Promise

앞에서 봤던 랜덤한 경우를 컨트롤 할때, 아래와 같이 표현할수 있다.

```jsx
const printString = (string) => {
    return new Promise((resolve, reject) => {
      setTimeout(
        () => {
         console.log(string)
         resolve()
        }, 
       Math.floor(Math.random() * 100) + 1
      )
    })
  }

  const printAll = () => {
    printString("A")
    .then(() => {
      return printString("B")
    })
    .then(() => {
      return printString("C")
    })
  }
  printAll()

```

<br>

### Promise의 에러 처리

서비스를 구현하다 보면 네트워크 연결, 서버 문제 등으로 인해 오류가 발생한다.

Promise의 에러 처리 방법에는 2가지가 있다.

1.`then()`의 두 번째 인자로 에러를 처리하는 방법

```jsx
getData().then(
  handleSuccess,
  handleError
);

```

2.`catch()`를 이용하는 방법

```jsx
getData().then().catch();

```

일반적으로 catch를 사용한 방식을 더 선호한다. 그 이유는 catch를 사용할때 더 많은 예외 처리 상황을 잡아 낼수 있기 때문이다.

<br>

### Promise Hell

Promise도 Hell이 존재 한다.

이렇게 작성하게 되면 가독성이 떨어진다.

```jsx
function gotoCodestates() {
    return new Promise((resolve, reject) => {
        setTimeout(() => { resolve('1. go to office') }, Math.floor(Math.random() * 100) + 1)
    })
}

function sitAndCode() {
    return new Promise((resolve, reject) => {
        setTimeout(() => { resolve('2. sit and code') }, Math.floor(Math.random() * 100) + 1)
    })
}

function eatLunch() {
    return new Promise((resolve, reject) => {
        setTimeout(() => { resolve('3. eat lunch') }, Math.floor(Math.random() * 100) + 1)
    })
}

function goToBed() {
    return new Promise((resolve, reject) => {
        setTimeout(() => { resolve('4. go home') }, Math.floor(Math.random() * 100) + 1)
    })
}

gotoCodestates()
.then(data => {
    console.log(data)

    sitAndCode()
    .then(data => {
        console.log(data)

        eatLunch()
        .then(data => {
            console.log(data)
            
            goToBed()
            .then(data => {
                console.log(data)
        
            })
        })
    })
})

```

<br>

### 여러 개의 프로미스 연결하기 (Promise Chaining)

더 좋은 가독성을 위해 Promise Chaining 방식으로 작성할수 있다.

```jsx
function gotoCodestates() {
    return new Promise((resolve, reject) => {
        setTimeout(() => { resolve('1. go to office') }, Math.floor(Math.random() * 100) + 1)
    })
}

function sitAndCode() {
    return new Promise((resolve, reject) => {
        setTimeout(() => { resolve('2. sit and code') }, Math.floor(Math.random() * 100) + 1)
    })
}

function eatLunch() {
    return new Promise((resolve, reject) => {
        setTimeout(() => { resolve('3. eat lunch') }, Math.floor(Math.random() * 100) + 1)
    })
}

function goToBed() {
    return new Promise((resolve, reject) => {
        setTimeout(() => { resolve('4. go home') }, Math.floor(Math.random() * 100) + 1)
    })
}

gotoCodestates()
.then(data => {
    console.log(data)
    return sitAndCode()
})
.then(data => {
    console.log(data)
    return eatLunch()
})
.then(data => {
    console.log(data)
    return goToBed()
})
.then(data => {
    console.log(data)
})

```

<br>

# Async/await

Javascript에 대한 최신 추가 사항인 ECMAScript 2017 JavaScript 에디션의 일부로 [`async`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function), `[await](<https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/await>)` 키워드가 추가되었다. 이 키워드는 기본적으로 비동기 코드를 쓰고 Promise를 더 읽기 더 쉽도록 만들어준다.

<br>


### async await

async await를 사용하면 아래와 같이 표현할수 있다.

```jsx
function gotoCodestates() {
    return new Promise((resolve, reject) => {
        setTimeout(() => { resolve('1. go to office') }, Math.floor(Math.random() * 100) + 1)
    })
}

function sitAndCode() {
    return new Promise((resolve, reject) => {
        setTimeout(() => { resolve('2. sit and code') }, Math.floor(Math.random() * 100) + 1)
    })
}

function eatLunch() {
    return new Promise((resolve, reject) => {
        setTimeout(() => { resolve('3. eat lunch') }, Math.floor(Math.random() * 100) + 1)
    })
}

function goToBed() {
    return new Promise((resolve, reject) => {
        setTimeout(() => { resolve('4. go home') }, Math.floor(Math.random() * 100) + 1)
    })
}

const result = async () => {
    const one = await gotoCodestates();
    console.log(one)

    const two = await sitAndCode();
    console.log(two)

    const three = await eatLunch();
    console.log(three)

    const four = await goToBed();
    console.log(four)
}

result();

```

콜백 함수에 비해 Promise chaining을 사용하면 코드가 간결해 지지만,

async await는 훨씬 더 간결한것을 확인할수 있다.

여기서 주의하셔야 할 점은 비동기 처리 메서드가 반드시 프로미스 객체를 반환해야 한다.

일반적으로 `await`의 대상인 [Axios](https://github.com/axios/axios) 등의 비동기 처리 코드는 프로미스를 반환하는 API 호출 함수이다.

위 코드에서도 각 함수안에서 프로미스 객체로 반환하는것을 확인할수 있다.

<br>

### async & await 예외 처리

async & await에서 예외를 처리를 위해 try catch를 사용한다.

```jsx
async function logTodoTitle() {
  try {
    const user = await fetchUser();
    if (user.id === 1) {
      const todo = await fetchTodo();
      console.log(todo.title);// delectus aut autem
		}
  } catch (error) {
    console.log(error);
  }
}

```

<br><br>


# 마무리

promise의 경우 앞에 있는 함수의 return값을 then으로 받아 와서 그 값을 다시 사용하는 방식이라면,

async await의 경우에는 변수를 선언해서 해당 함수를 할당하는데, 할당하는 값이 나올때 까지 기다리면서 다음 코드로 진행되지 않는 방식이다.

확실히 async await이 promise보다 더 쓰기 간편한것은 사실이지만, 때에 따라서는 promise로 쓰는것이 좋을때가 있을것 같다.

코드를 작성하면서 어떤것이 더 가독성이 좋고 효율적인지 고민하면서 사용해야 할듯 하다.