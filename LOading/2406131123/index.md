---
layout: post
type: LOading
date: 2024-06-13 11:23
category: NestJS
title: "[NestJS]DataSource 활용하기"
subtitle: forRoot vs forRootAsync
writer: 0
post-header: true
header-img: ../nestJS.png
hash-tag:
  - NestJS
  - 저
---
# Intro
묵은 숙원이었던 `dataSource.ts`를 사용해서 database연결하기에 성공 했다.

`dataSource`의 사용은 개인적으로 database에 Seed 작업을 하거나 schema sync 작업을 할때 사용 한다.

(TypeORM 0.2 ver 에서는 ormconfig.ts 혹은 ormconfig.js를 통해서 연결하는 자료만 많았지만 나는 TypeORM 0.3 ver 을 사용하기 때문에 ormcofig를 사용하지 않았다.)

기존 코드의 문제점은 DB 연결을 `app.module.ts`에서 따로 db 설정을 하고, 동일한 설정을 `dataSource.ts`에서도 해놓고 사용했었다.

이번에 스터디를 하면서 코드를 깔끔하게 정리 하게 됐다.

<br>

# dataSource.ts setting

``` ts
// src/dataSource.ts  
import { DataSourceOptions } from 'typeorm';  
import { ConfigModule, ConfigService } from '@nestjs/config';    
  
// ConfigModule 설정  
ConfigModule.forRoot({  
  isGlobal: true,  
  envFilePath: `.env.${process.env.NODE_ENV}`,  
});  
  
const config = new ConfigService();  
  
const host = config.get<string>('NODE_ENV');  
  
console.log('This is Data Source >> ', host);  
  
const dataSourceOptions: DataSourceOptions = {  
  type: 'mysql',  
  host: config.get<string>('DB_HOST'),  
  port: config.get<number>('DB_PORT'),  
  username: config.get<string>('DB_USERNAME'),  
  password: config.get<string>('DB_PASSWORD'),  
  database: config.get<string>('DB_DATABASE'),
  entities: [__dirname + '/**/**/*.entity.*'],  
  migrations: [__dirname + '/src/migrations/*.ts'],  
  timezone: 'Asia/Seoul',  
  charset: 'utf8mb4_general_ci',  
  synchronize: false,  
  logging: config.get<any>('LOGGING') === 'true',  
};  
  
export const configOptions = dataSourceOptions;
```

dataSource.ts 코드를  살펴보면
1. ConfigModule 설정
2. dataSourceOptions 설정
3. dataSourceOptions export
   단계로 구성된다.
   ConfigModule는 .env 파일을 통해 key 값들을 가져 오기 위해서 설정한다.
   그 뒤 dataSourceOptions를 설정하고 export 한다.

<br>

# app.module.ts setting

- 기존 방식 : `.forRootAsync` 사용, 직접 입력.
    - 직접 입력을 할때 `ConfigService`의 주입을 위해서 `.forRootAsync`사용

- 개선 방식 : `.forRootAsync` 사용, `configOptions`를 받아와서 입력
``` ts  
@Module({  
  imports: [  
    ConfigModule.forRoot({  
      isGlobal: true,  
      envFilePath: `.env.${process.env.NODE_ENV}`,  
    }),  
    TypeOrmModule.forRoot(configOptions),  
    GraphQLModule.forRoot<ApolloDriverConfig>({  
      playground: process.env.PLAYGROUND !== 'false',  
      driver: ApolloDriver,  
      autoSchemaFile: 'src/common/graphql/schema.gql',  
      context: ({ req, res }) => ({ req, res }),  
    }),  
    // ... 
  ],  
  controllers: [HealthCheckController],  
  providers: [  
    {  
      provide: APP_PIPE,  
      useClass: ValidationPipe,  
    },  
  ],  
})  
export class AppModule {}
```

`app.module.ts` 코드에서 달라진 부분은 `TypeOrmModule.forRoot(configOptions)` 이다.

기존 방식은  `ConfigService`를 주입 받기 위해 `TypeOrmModule.forRootAsync`를 써왔다.

이는 TypeOrmModule에 직접적으로 ConfigService를 받아와야 하기 때문에 비동기 처리가 되어야 했다.

반면 `dataSource.ts`에서 `configOptions`(db 연결 옵션)를 받아와서 처리해야 하는 경우

`TypeOrmModule`에서 비동기 처리가 필요하지는 않고, `dataSource.ts` 파일 안에서

```ts
// dataSource.ts
...
ConfigModule.forRoot({  
  isGlobal: true,  
  envFilePath: `.env.${process.env.NODE_ENV}`,  
});  
  
const config = new ConfigService();
...
```
이렇게 `ConfigService` 설정을 해 주어야 한다.

<br>

## forRoot vs forRootAsync
NestJS에서 `TypeOrmModule.forRoot`와 `TypeOrmModule.forRootAsync`는 TypeORM을 사용하여 데이터베이스 연결을 설정하는 두 가지 방법이다.

- **`TypeOrmModule.forRoot`**
  `TypeOrmModule.forRoot`는 동기식으로 데이터베이스 연결을 설정한다.<br>
  이 메서드는 설정 객체를 직접 제공하여 TypeORM이 초기화되도록 한다.
    -  특성:
        - **간단한 설정**: 설정 객체를 직접 전달 →  간단한 설정
        - **동기 초기화**: 애플리케이션이 시작될 때 데이터베이스 연결 설정이 동기적으로 처리
        - **정적 설정**: 설정 정보가 애플리케이션 코드에 고정되어 있음
- **`TypeOrmModule.forRootAsync`**:
  `TypeOrmModule.forRootAsync`는 비동기식으로 데이터베이스 연결을 설정한다.<br>
  이 메서드는 비동기 설정을 지원하여, 설정 객체를 동적으로 생성할 수 있게 한다.
    - 특성:
        - **비동기 초기화**: 데이터베이스 설정을 비동기적으로 처리
        - **동적 설정**: 설정 객체를 동적으로 생성할 수 있어, 환경 변수나 다른 비동기 소스에서 설정 값을 가져올 수 있음
        - **의존성 주입 지원**: 설정 값을 제공하기 위해 다른 서비스나 모듈에 의존할 수 있음

<br>

# 요약
1. `dataSource.ts`에 옵션 세팅
    - ConfigModule 설정 → dataSourceOptions 설정 → dataSourceOptions export
2. `app.module.ts`에서 `TypeOrmModule`로 DB 연결
3. `TypeOrmModule`로 DB 연결할때 export된 dataSourceOptions 사용

<br>

*해당 글은 chatGPT를 기반으로 작성되었습니다.*
