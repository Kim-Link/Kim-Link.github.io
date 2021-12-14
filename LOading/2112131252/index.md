---
layout: post
type: LOading
date: 2021-12-13 12:52
category: TIL
title: "[TIL] morgan, crypto, 429Error"
subtitle: 어설프게 아는것 = 모르는것
writer: 000000
post-header: true
header-img: img/TILabout.jpeg
hash-tag: [morgan, crypto, 429Error]
---

Today I Learn.

잘못한 부분을 반복해서 잘못하지 말자.

<br>

### 오늘 지적 받은 부분

-   morgan 에 대한 이해
    -   이미 mogan처리가 되어 있는데 왜 크롤러에 있는 함수를 가져 올때 logger 함수도 같이 가져 왓냐?
-   crypto가 뭔가?
-   429 에러 처리가 되어 있는가?
-   마감 예상 시간은 내가 코드를 작성끝난 지점이 아니라 검수가 다 끝난 master 배포 시점을 기준으로 잡아야 한다. ⇒ 검수 후 수정시간으로 배포 시간이 늦어지면, 다른분들은 이미 완료 되었다고 생각하는 부분이 서비스에 적용되지 않았다.
-   기존에 작성된 API를 가져다 쓸때, 정확하게 그 API 를 이해하지 않고 가져다 쓰기 때문에 위와 같은 문제가 발생한것이다.
-   기존 API를 가능한 이해하려고 하고, 이해가 되지 않는다면 이해되지 않는 부분에 대해 물어보자.
-   git 브랜치 관리할때 fix에서 찾아야 할 코드를 feat에서 찾음. 정신 차려야 할듯.

<br><br>

### Morgan

Q. Morgan이 뭔가요?

`node.js용 HTTP 요청 로거 미들웨어` 이다. 쉽게 말해서 nodeJS용 [미들웨어](https://velog.io/@wiostz98kr/Express-middleware-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0-morgan)이고, 하는 일을 서버에 요청이 들어올때 log를 기록해주는 것이다.

Q. 왜 사용하나요?

일반적으로 요청에 대한 에러가 발생하거나 현재 요청이 제대로 들어가고 있는지에 대해 확인하고 싶을때 사용한다.

<br><br>

### Crypto

Q. Crypto가 뭔가요?

javascript에서 [해쉬 방식의 암호화](https://kim-link.github.io/LOading/2109082259/)를 할 수 있도록 해주는 Node.js 패키지이다.

<br>

Q. 어떻게 사용되었나요?

naver ad api 호출할때 필요한 암호화 함수를 작성하면서 사용되었다.

```jsx
// naver ad api 호출시 필요한 암호화 함수
function getXSignature(timeStamp, SECRET_KEY, method, uri) {
  const hmac = crypto.createHmac('sha256', SECRET_KEY);
  const signal = `${timeStamp}.${method}.${uri}`;
  const ret = hmac.update(signal).digest('base64');

  return ret;
}

```

```jsx
async function getKeywordAmount(keyword) {
  const [keys] = await readNaverAdAPI();
  const TimeStamp = String(Date.now());
  const headers = {
    'Content-Type': 'application/json; charset=UTF-8',
    'X-API-KEY': keys.API_KEY,
    'X-Customer': keys.CUSTOMER_ID,
    'X-Signature': getXSignature(
      TimeStamp,
      keys.SECRET_KEY,
      'GET',
      '/keywordstool',
    ),
    'X-Timestamp': TimeStamp,
    'User-Agent':
      'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/89.0.4389.90 Safari/537.36',
  };

  try {
    const response = await axios.get(
      `https://api.naver.com/keywordstool?showDetail=1&hintKeywords=${encodeURI(
        keyword,
      )}`,
      { headers },
    );
    const result = "요건 비밀입니다."
    return result;
  } catch (err) {
    if (err.response.status === 400) {
      return [
       "요것도 비밀이겟쥬~??
      ];
    }
    await wait(1000);
    return null;
  }
}

```

위 함수에서 header 부분을 보면 'X-Signature' 가 필요로 한다. 이 X-Signature를 값을 구하기 위해서 readNaverAdAPI함수로 부터 SECRET_KEY를 받아와서 암호화 해서 보낸다.

여러종류의 해쉬암호화 방식이 있는데 보안을 위해 crypto를 사용한다.

<br><br>

### 429 Error

Q. 429에러가 뭔가요?

429에러는 너무 많은 요청이 보내졌을때 발생하는 에러이다.

크롤링을 하게되면 만날수 있는 에러이고, 429 에러에 대한 처리를 반드시 해주어야 한다.

Q. 어떻게 처리 되어야 하나요?

try-catch로 처리한다. 429 에러는 에러 메세지를 응답해주고 끝이 아니라 waait를 사용하여 일정 시간이 지난뒤 재요청을 보내서 에러 처리를 완화해준다.