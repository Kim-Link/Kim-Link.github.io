---
layout: post
type: LOading
date: 2024-03-13 14:55
category: nestJS
title: Custom interceptor 만들기
subtitle: interceptor는 끝이 없지...
writer: 0
post-header: true
header-img: ../nestJS.jpeg
hash-tag:
  - NestJS
  - custom_interceptor
  - 편
---

# Interceptor
Interceptor는 들어오는 요청이나 나가는 요청을 가로채는데 사용한다.
다른 프레임워크의 미들웨어와 비슷한 역활을 한다.
Intorceptor는 각각의 API(route handler)나 Controller, 혹은 global 단위로 사용이 가능하다.


# Custom Interceptor 만들기
### 1. 폴더 생성
src안에 `interceptors`라는 폴더 생성

### 2. 파일 생성
`serialize.interceptor.ts` 파일을 `interceptors` 폴더 안에 생성
파일 이름을 보면 알겠지만 객체를 가져와서 json으로 직렬화 시켜주는 역활을 한다.
역활에 맞게 파일 이름을 설정하면 된다.

### 3. 코드 작성
예시 코드는 service에서 user 데이터를 조회한뒤
특정 정보만 client로 넘길수 있게 하는 interceptor를 만드는것을 목표로 한다.

```ts
import { CallHandler, ExecutionContext, NestInterceptor } from '@nestjs/common';  
import { map, Observable } from 'rxjs';  
  
export class SerializeInterceptor implements NestInterceptor {  
  intercept(  
    context: ExecutionContext,  
    handler: CallHandler<any>,  
  ): Observable<any> {  
    //   Run something before a request is handled  
    //   by the request handler    
    console.log('Im running before the handler', context);  
  
    return handler.handle().pipe(  
      map((data: any) => {  
        //   Run something before the response is sent out  
        console.log('Im running before response is sent out', data);  
      }),  
    );  
  }  
}
```
extends는 존재하는 클래스의 하위 클래스를 만들 때 사용한다.
반면, implements는  추상 클래스나 인터페이스의 모든 조건을 만족하는 새로운 클래스를 구현하고자 할때 사용한다.
implements NestInterceptor라고 쓰면 타입스크립트가 이 NestInterceptor 인터페이스에 존재하는 모든 메서드를 확인한다.
즉, 새로운 클래스를 만드는데 implements로 받은 class안에 있는 모든 메소드를 정의해야 오류가 발생하지 않는다.

NestInterceptor에는 `intercept`라는 메소드가 하나 있는데,
`context`,`handler`를 인자로 받는다.
들어오는 요청에 대한 처리는 `return`전에 코드를 작성하고,
나가는 요청에 대한 처리는 `return > handel.handle().pipe(map(()=>{}))`안에서 처리를 하면 된다.
이때 들어올때 데이터는 context에 담겨 있고,
나갈때 데이터는 handel에 담겨 있는데 나갈때 데이터를 쉽게 처리하기 위해서 `.handle().pipe(map(()=>{})` 처리 한다.
위 예시 코드를 적용하면 응답이 오지 않을건데, 이는 map에서 return을 하지 않았기 때문이다.

client에 보여주지 않을 데이터를 정리하기 위해 dto를 생성한다.
```ts
import { Expose } from 'class-transformer';  
  
export class UserDto {  
  @Expose()  
  id: number;  
  
  @Expose()  
  email: string;  
}
```
Exclude와 다르게 Expose는 해당 속성을 공유하라는 것을 의미한다.

이 dto를 적용한 최종 interceptor 코드는 아래와 같다.
```ts
import { CallHandler, ExecutionContext, NestInterceptor } from '@nestjs/common';  
import { map, Observable } from 'rxjs';  
import { plainToClass } from 'class-transformer';  
import { UserDto } from '../users/dtos/user.dto';  
  
export class SerializeInterceptor implements NestInterceptor {  
  intercept(  
    context: ExecutionContext,  
    handler: CallHandler<any>,  
  ): Observable<any> {  
    return handler.handle().pipe(  
      map((data: any) => {   
        return plainToInstance(UserDto, data, {  
          excludeExtraneousValues: true,  
        });  
      }),  
    );  
  }  
}
```
UserDto 인스턴스로 변환하기 위해 plainToInstance 사용한다.
map에서 return 해주고 plainToClass에
첫번째 인자로 UserDto,
두번째 인자로 사용자 엔티티를 넘긴다.
그리고 마지막 옵션 설정 항목에서 객체로
excludeExtraneousValues를 true로 설정한다.
이 설정을 추가하면
UserDto 인스턴스가 있고 기본 JSON으로 변환하려고 할 때마다
Expose라고 표시된 속성만 공유하거나 노출한다.



### 4. 적용
```ts
// controller

@UseInterceptors(SerializeInterceptor)  
@Get('/:id')  
async findUser(@Param('id') id: string) {  
  console.log('handler is running');  
  const user = await this.usersService.findOne(parseInt(id));  
  if (!user) {  
    throw new NotFoundException('user not found');  
  }  
  return user;  
}

```
Intercepter를 만들고 나면 `UseInterceptors` 데코레이터를 사용하여 `SerializeInterceptor`를 적용시킬수 있다.


### 5. 응용
위 방법대로 하면 발생하는 문제가 있다.
이렇게 하게 되면 interceptor를 userDto 하나에만 귀속된다는 점이다.
해당 intorceptor의 재사용성을 높이는 방법은 아래와 같다.

```ts
// controller

@UseInterceptors(new SerializeInterceptor(UserDto))  
@Get('/:id')  
async findUser(@Param('id') id: string) {  
  const user = await this.usersService.findOne(parseInt(id));  
  if (!user) {  
    throw new NotFoundException('user not found');  
  }  
  return user;  
}

```

```ts
// interceptor

import { CallHandler, ExecutionContext, NestInterceptor } from '@nestjs/common';  
import { map, Observable } from 'rxjs';  
import { plainToInstance } from 'class-transformer';  
  
export class SerializeInterceptor implements NestInterceptor {  
  constructor(private dto: any) {}  
  intercept(  
    context: ExecutionContext,  
    handler: CallHandler<any>,  
  ): Observable<any> {  
    return handler.handle().pipe(  
      map((data: any) => {  
        return plainToInstance(this.dto, data, {  
          excludeExtraneousValues: true,  
        });  
      }),  
    );  
  }  
}

```

간단히 설명하면 UserDto를 interceptor에서 넣는것이 아닌 controller에서 넣어 interceptor에서 constructor로 받아서 사용한다.

### 6. 리팩토링
마지막 응용코드에서 controller의 데코레이터 부분이 너무 길다.
한줄에 import가 3개나 붙어있다.
이것을 컴팩트 하게 줄여보자.

```ts
// controller

@Get('/:id')  
@Serialize(UserDto)  
async findUser(@Param('id') id: string) {  
  const user = await this.usersService.findOne(parseInt(id));  
  if (!user) {  
    throw new NotFoundException('user not found');  
  }  
  return user;  
}

```
기존의 UseInterceptors 데코레이터 대신에 Serialize라는 데코레이터가 붙었다.

Serialize라는 데코레이터는 interceptor 안에서
`@UseInterceptors(new SerializeInterceptor(UserDto))`의 역활을 하는 함수이다.
```ts
// interceptor

import {  
  CallHandler,  
  ExecutionContext,  
  NestInterceptor,  
  UseInterceptors,  
} from '@nestjs/common';  
import { map, Observable } from 'rxjs';  
import { plainToInstance } from 'class-transformer';  
  
interface ClassConstructor {  
  new (...args: any[]): NonNullable<unknown>;  
}  
export function Serialize(dto: ClassConstructor) {  
  return UseInterceptors(new SerializeInterceptor(dto));  
}  
  
export class SerializeInterceptor implements NestInterceptor {  
  constructor(private dto: ClassConstructor) {}  
  intercept(  
    context: ExecutionContext,  
    handler: CallHandler<any>,  
  ): Observable<any> {  
    return handler.handle().pipe(  
      map((data: any) => {  
        return plainToInstance(this.dto, data, {  
          excludeExtraneousValues: true,  
        });  
      }),  
    );  
  }  
}

```
Serialize 함수를 만들어주고
any 타입을 방지 하기 위해 최소한의 타입인 객체로 받기 위해 ClassConstructor를 만들어 준다.
