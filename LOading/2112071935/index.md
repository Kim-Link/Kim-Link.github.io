---
layout: post
type: LOading
date: 2021-12-07 19:35
category: NodeJs
title: package.json에 대해서
subtitle: package.json 너의 정체는...? 
writer: 000000
post-header: true
header-img: img/about.jpeg
hash-tag: [package.json]
---

package.json은 뭐하는 녀석일까?

항상 init을 하게 되면 자동으로 생성 되는 파일이고, 대충보면 버전관리를 해준다고 생각할수 있다.

하지만 이 package.json을 제대로 활용하지 못하면 불필요한 행위를 하게되는 꼴이 될수 있다.

코드를 칠때, 이유없는것은 없다. 하나하나 다 뜯어봐야 하고, 귀찮은것을 극도로 싫어하는 개발자들이 귀찮음을 뒤로하고 설정하도록 한것은 이유가 있을것이다.

package.json을 하나하나 다 뜯어 보면 좋겠지만, 반드시 숙지해야 할 몇가지 사항에 대해서 짚고 넘어가고자 한다.

### 예시

```json
{
  "name": "HelloWorld",
  "version": "0.1.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "start": "nodemon -e yaml,js index.js",
    "test": "exit"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "amqplib": "^0.8.0",
    "aws-sdk": "^2.988.0",
    "axios": "^0.21.1",
    "bcrypt": "^5.0.1",
    "cheerio": "^1.0.0-rc.10",
    "cors": "^2.8.5",
    "dotenv": "^10.0.0",
    "easy-soap-request": "^4.2.0",
    "express": "^4.17.1",
    "helmet": "^4.6.0",
    "json2csv": "^5.0.6",
    "json2xls": "^0.1.2",
    "jsonwebtoken": "^8.5.1",
    "mongodb": "^3.6.0",
    "morgan": "^1.10.0",
    "multer": "^1.4.3",
    "multer-s3": "^2.9.0",
    "mysql2": "^2.2.5",
    "nodemailer": "^6.6.3",
    "python-shell": "^3.0.0",
    "swagger-ui-express": "^4.1.6",
    "tunnel-ssh": "^4.1.6",
    "winston": "^3.3.3",
    "winston-mongodb": "^5.0.7",
    "winston-slack-webhook-transport": "^2.1.0",
    "xml-js": "^1.6.11",
    "yamljs": "^0.3.0"
  },
  "devDependencies": {
    "eslint": "^7.31.0",
    "eslint-config-airbnb-base": "^14.2.1",
    "eslint-config-prettier": "^8.3.0",
    "eslint-plugin-import": "^2.23.4",
    "eslint-plugin-prettier": "^3.4.0",
    "nodemon": "^2.0.12",
    "prettier": "^2.3.2",
    "swagger-jsdoc": "^6.1.0",
    "swagger-ui-express": "^4.1.6"
  }
}

```

### 1. "scripts"

스크립트는 npm이나 yarn등의 명령어를 통해 실행시킬수 있는 항목이다.

```json
 "scripts": {
    "start": "nodemon -e yaml,js index.js",
    "test": "exit"
  },

```

배열로 되어 있고, `npm run start` 를 터미널에서 치게되면 "nodemon -e yaml,js index.js" 를 실행하게 된다.

### 2. "dependencies"

```json
"dependencies": {
    "amqplib": "^0.8.0",
    "aws-sdk": "^2.988.0",
    "axios": "^0.21.1",
				.
				.
				.
    "xml-js": "^1.6.11",
    "yamljs": "^0.3.0"
  }

```

dependencies는 프로젝트의 의존성 관리를 위한 부분이다. 이 부분이 프로젝트에서 사용하는 확장모듈을 확인할수 있고, 사용할 버젼을 결정할수 있다.

`npm install <module's name> —save`을 하게 되면 여기에 기록되며, git clone을 진행 할때 npm install을 하게 되면 기록된 모듈들이 설치된다.

### 3. "devDependencies"

```json
"devDependencies": {
    "eslint": "^7.31.0",
    "eslint-config-airbnb-base": "^14.2.1",
    "eslint-config-prettier": "^8.3.0",
    "eslint-plugin-import": "^2.23.4",
    "eslint-plugin-prettier": "^3.4.0",
    "nodemon": "^2.0.12",
    "prettier": "^2.3.2",
    "swagger-jsdoc": "^6.1.0",
    "swagger-ui-express": "^4.1.6"
  }

```

devDependencies는 dependencies와 유사하게 확장 모듈을 관리하지만, 차이점은 개발에서만 사용된다. 예시에 적힌 모듈들을 보면 eslint, nodemon 등 개발을 할때만 필요한 모듈들이다. 이렇게 배포후에도 사용될 모듈들과, 개발에서만 사용할 모듈들을 구분해서 실제 배포환경에서는 불필요한 설치를 하지 않도록 막아준다.

devDependencies에는 `npm install <module's name> --save -dev` 로 기록할수 있다.

<br>
[참고 링크] [](https://edu.goorm.io/learn/lecture/557/%ED%95%9C-%EB%88%88%EC%97%90-%EB%81%9D%EB%82%B4%EB%8A%94-node-js/lesson/174371/package-json)[https://edu.goorm.io/learn/lecture/557/한-눈에-끝내는-node-js/lesson/174371/package-json](https://edu.goorm.io/learn/lecture/557/%ED%95%9C-%EB%88%88%EC%97%90-%EB%81%9D%EB%82%B4%EB%8A%94-node-js/lesson/174371/package-json)