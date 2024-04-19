---
layout: post
type: LOading
date: 2024-04-17 13:22
category: NestJS
title: 의존성 주입(DI, IoC)
subtitle: 
writer: 0
post-header: true
header-img: ../nestJS.png
hash-tag:
  - 편
  - NestJS
  - DI
  - IoC
  - 의존성주입
  - 제어역전
---
<!-- 양식 참고
[사진 첨부]
<img src="img/1.png" alt="1" style="zoom:80%;" />

[참고 문헌]
[파이썬 딕셔너리 사용하기 dict](https://jvvp.tistory.com/998)
-->

## 의존성 주입

### 의존성
하나의 코드가 다른 코드에 의존하는 상태

```ts
class MessageService {
 // 생략
}

class MessageController {
	messageService : MessageService;
	constructor(){
		this.messageService = new MessageService()
	}
	
	// 생략
}
```

예를 들어 Message라는 모듈을 만든다고 했을때,
코드를 보면 MessageController는 MessageService에 의존하고 있다고 볼수 있다.

### 주입
넣어주는 것

### 의존성 주입
**의존성이 있는 객체(코드)를 넣어주는것**
의존성 주입은 객체가 직접 자신이 필요로 하는 의존성을 만들거나 관리하는 것이 아니라,
**외부에서 의존성을 주입받는 것**을 의미한다.

이는 객체가 자신의 동작을 완수하는 데 필요한 리소스를 스스로 관리하지 않고,
외부에서 주입받아 사용함으로써 객체 간의 결합도를 낮추고 유연성을 높인다.

```ts
class MessageService {
 // 생략
}

class MessageController {
	messageService : MessageService;
	constructor(service : MessageService){
		this.messageService = service
	}
	
	// 생략
}
```

코드를 보면 앞에 봤던 것과 달리 Controller 안에서 새로운 new Service를 생성하지 않는다.
대신 constructor 에서 service로 주입 받는것을 확인할수 있다.

`어떻게 이게 가능할까?`
이걸 알려면 제어 역전(IoC)에 대해 알아야 한다.
### 제어 역전(IoC: Inversion of Controller)
제어의 역전은 의존성 주입을 통해 구현되는 디자인 패턴 중 하나이다.

<img src="img/Pasted image 20240417143439.png" alt="1" style="zoom:80%;" />

일반적인 경우에는 Class Controller에서 Class Service에 있는 것을 직접 참고 하여 사용하였다.
제어 역전의 경우에는 Class Controller 와 Class Service 중간에 매개체를 두고 매개체를 통해서 사용한다.
화살표 방향은 제어의(코드 흐름) 방향이다.
화살표 방향만 보면 일반적인 경우에는 화살표가 위에서 아래로 흐르던 것이 제어역전을 사용하게 되면 화살표가 서로를 마주보게 되어 제어를 나타내던 방향이 달라진 것을 확인 할수 있다.

즉, 개발자가 직접 의존성을 제어 하던것을
어떠한 매개체에게 제어권을 일임 또는 빼앗기게 되어
**더이상 제어의 주체가 개발자가 아니게 되기 때문**에
제어의 역전이 발생했다고 보면 된다.


<img src="img/Pasted image 20240417143752.png" alt="1" style="zoom:80%;" />

여기서 매개체가 IoC Container(혹은 nestJS Container) 라고 한다.
IoC Container는 개발자에게서 일임받은 제어권을 사용하여 의존성을 관리하고 인스턴스를 생성하여 주입해주고 나중에는 메모리를 해제하는 역활까지 한다.

이 IoC Container는 주로 프레임워크(NestJS, Spring)가 역활을 담당한다.

이렇게 함으로써 코드의 유연성과 확장성이 향상되고, 유지보수가 용이해진다.
기존에는 의존성을 모두 직접 제어 했다면
제어 역전이 발생하면 제어권이 역전 되어 있기 때문에 직접 제어 하지 않는다.


### 실제 사용

#### Module - Controller - Service 예시

```ts
//module.ts
@Module(
	import:[UserModule],
	providers:[MessageController, MessageService],
	exports:[MessageService]
)
export class MessageModule {}



// Controller.ts
@Controller()
export class MessageController {
	constructor(public service : MessageService)
	{}
	// 생략
}


// Service.ts
@Injectable()
export class MessageService {
 // 생략
}

```

#### Module.ts
- `@Module` : 데코레이터로 표시된 NestJS 모듈
- `imports` : 현재 모듈이 의존하는 다른 모듈 등록
    - 현재 모듈에서 사용되는 클래스나 기능이 다른 모듈에서 **제공**되는 경우, 해당 모듈을 `Import` 배열에 추가하여 NestJS가 이를 인식하고 사용할 수 있도록 해준다.
- `providers` : 배열에는 해당 모듈에서 사용할 공급자(서비스, 컨트롤러 등)를 등록
    - 여기서는 `MessageController`와 `MessageService`를 등록함.

>[!HINT] IoC 컨테이너는 어디서 작동하는가?
>IoC 컨테이너가 작동하려면 `@Module()` 데코레이터에 있는 `providers` 배열에 해당하는 클래스들을 등록해야 합니다.
>이렇게 함으로써 NestJS는 컨테이너에 등록된 클래스들을 관리하고, 필요한 곳에 주입합니다.
>일반적으로 서비스, 컨트롤러 등의 클래스들이 `providers` 배열에 등록됩니다.

- `exports` 배열은 이 모듈에서 공유하려는 공급자 등록
    - 다른 모듈에서 이 모듈에서 등록한 공급자를 사용할 수 있도록 내보냄



#### Controller.ts
- `@Controller` 데코레이터로 표시된 NestJS 컨트롤러 클래스
- `constructor`에서 `public readonly service: MessageService`를 인자로 받음
    - `readonly` 키워드를 사용하면 변수를 읽기 전용(immutable)으로 만들어 준다.
    - `public`은 객체의 속성이나 메서드를 외부에서 직접적으로 접근할 수 있도록 지정하는 키워드 이다.
        - TypeScript에서는 클래스의 멤버(속성, 메서드)를 선언할 때 별도의 한정자를 지정하지 않으면 기본적으로 `public`으로 간주된다.
        - 반면, `private` 또는 `protected`로 선언된 멤버는 클래스 외부에서 직접 접근할 수 없다. 이러한 가시성 제어를 통해 클래스의 내부 구현을 보호하고 캡슐화(Encapsulation)를 유지할 수 있다.

> [!HINT] 의존성 주입의 핵심
>생성자를 통해 `MessageService`의 인스턴스가 주입되므로 `MessageController`에서 `MessageService`를 사용할 수 있다.
>`MessageController` 클래스의 생성자에서 `MessageService`의 인스턴스를 받아와서 `service` 속성에 할당합니다. 이렇게 함으로써 `MessageController`에서 `MessageService`의 메서드를 호출할 수 있게 됩니다.
>이때 `public` 접근 제어자를 사용하여 외부에서 접근 가능하도록 설정합니다.


#### Service.ts
- `@Injectable` 데코레이터로 표시된 NestJS 서비스 클래스
    -  이 데코레이터는 해당 클래스가 NestJS의 의존성 주입 시스템에 의해 관리되는 클래스임을 나타낸다.
    -  NestJS가 해당 클래스를 식별하고 인스턴스를 생성하며, 필요한 곳에 주입합니다.
    - 이 데코레이터를 사용하면 클래스가 다른 곳에서 주입될 수 있게 됩니다.

#### module간 표시

```ts
// moduleA.ts
import { Module } from '@nestjs/common';
import { User } from './serviceA';
import { ServiceB } from './serviceB';

@Module({
  providers: [UserService, AuthService],
  exports: [UserService], // 다른 모듈에서 UserService를 사용할 수 있도록 내보냄
})
export class UserModule {}
```

```ts
// moduleB.ts
@Module({
  imports: [UserModule], // UserModule에서 제공하는 서비스를 사용하기 위해 UserService를 임포트
  providers: [MessageService],
})
export class MessageModule {}


```

- 내보낼때는 `exports`에  `service`나 `controller`로
- 받을때는 `imports`에 `module`로



## 장점
- 의존성 감소 : 사용하는 클래스에서 직접 생성하는것이 아니라  IoC Container를 통해서 사용하기 때문
    - 변화에 강함 : 의존하고 있는 모듈의 라이프 사이클을 전혀 신경쓰지 않기 때문에 의존하고 있는 모듈이 변경 된다고 해서 사용처에서 신경을 쓸 필요가 없어진다.
    - 재사용성이 더 좋아짐
    - 유지 보수 용이
- 코드양 감소 : 모듈의 생성과 삭제 등을 직접 할 필요가 없기 때문
- 테스트 용이

## 결론
코드의 효율을 높이고 유지보수성을 높이기 위해 강한 결합을 느슨한 결합으로 변경할 필요가 있음.
결합을 느슨하게 하기 위해 의존성 주입이 사용됨.
의존성 주입을 시행하기 위해 제어의 역전이 구현되었음.

### 참고
[nestJS 공식 문서](https://docs.nestjs.com/providers#dependency-injection)
[의존성 주입 3분만에 이해하기 (Dependency Injection, Inversion of Control)](https://youtu.be/1vdeIL2iCcM?si=_OVGiz2eWinP-IVp)
[제대로 이해하는 DI와 DIP](https://www.youtube.com/watch?v=nOHdunGzeRc)
