---
layout: post
type: LOading
date: 2022-12-28 20:24
category: Django
title: Parsing, Parser and paser_classes in Django 
subtitle: Dj 췌키라우 파서
writer: 000000
post-header: true
header-img: ../django.png
hash-tag: [Django]
---

## Intro

---

최근 Django를 계속해서 공부하고 있는데 기존 공부했던 NodeJS와 많이 달라서 어려움을 겪고 있다.

무엇보다도 NodeJS를 사용할때 함수 기반으로 코드를 많이 짜왔는데, 현재는 대부분 클래스 기반으로 구성되어 있어 스타일 차이에서 오는 구성 차이로 인한 어려움이 가장 큰것 같다.

swagger구성 중 첨부파일을 넣은 양식이 있는데, 필드값도 제대로 세팅되어 있는데 swagger에 뜨지 않았고, 그 원인이 parser_classes를 설정하지 않아 생긴 문제였다.

이참에 미루왔던, 파싱과 파서를 포함해서 DRF에서 parser를 설정하는 방법을 정리하고자 한다.

<br>

## Parsing

---

파싱이란 어떤 데이터를 원하는 모양으로 만들어 내는걸 말한다.

1. 특정문서(XML 따위)를 읽어 들여서 이를 다른 프로그램이나 서브루틴이 사용할 수 있는 내부 의 표현 방식으로 변환시켜 주는 것이다. XML 문서를 보시면 HTML처럼 <>태그가 보일 것입니다. 사용자가 이렇게 입력하지만 컴터가 알아 볼 수 있도록 바꿔주는 과정을 의미합니다.
2. 컴파일러의 일부로써 원시 프로그램의 명령문이나 온라인 명령문, HTML 문서등에서 마크업태그등을 입력으로 받아들여서 구문을 해석 할수 있는 단위로 여러부분으로 분할해 주는 역할을 한다.


<br>


## Parser

---

Parsing을 하는 processor이다. Paser를 통해 Parsing을 할 수있다.

조금더 자세히 설명하면, 파서(parser)는 컴파일러의 일부로서 원시 프로그램 즉, 컴퍼일러나 인터프리터에서 원시 프로그램을 읽어 들여, 그 문장의 구조를 알아내는 구문 분석(parsing)을 행하는 프로그램을 말한다.

<br>

## **Content-Type이란?**

---

Http 통신에서 전송되는 데이터의 타입을 명시하기 위해 header에 실리는 정보다. 즉, api 요청 시 request에 실어 보내는 데이터(body)의 타입 정보다.

Content-Type을 설명하는 이유는 Content-Type에 따라 DRF에서 Parser를 설정해야 하기 때문이다.

클라이언트 응용 프로그램을 개발할 때는 항상 HTTP request로 데이터를 보낼때 `Content-Type`헤더를 설정해야 한다.

Content-Type을 설정하지 않으면 대부분의 클라이언트는 `'application/x-www-form-urlencoded'`
를 기본값으로 사용한다. 하지만 사용자는 다른 Type을 사용해야 하는 경우도 있다.

예를 들어, `.ajax()`메서드로 jQuery를 사용하여 `json`으로 인코딩 된 데이터를 보내는 경우, `contentType: 'application/json'`설정을 해야 한다.

<br>

## Parser in DRF

---

REST 프레임워크에는 `Parser`클래스가 내장되어 있어 다양한 미디어 타입으로 requests를 수락할 수 있다. 

또한 `custom parser`를 정의 할 수 있어서 API에서 허용하는 미디어 타입을 유연하게 디자인 할 수 있다.

**How the parser is determined**

뷰에 대한 유효한 parser set은 항상 클래스 목록으로 정의된다. `request.data`에 액서스하면 REST 프레임워크는 들어오는 request의 헤더에서 `Content-Type` 검사하고 request 내용을 parse하는데 사용할 `parser`를 결정한다.

<br>

## Setting the parsers in DRF

---

### 1. `**parser`의 Default 값**

`parser`의 기본 set은 `DEFAULT_PARSER_CLASSES` 설정을 사용하여 전역으로 설정할 수 있다. 예를 들어, 다음 설정은 기본 JSON 이나 formdata 대신 JSON 컨텐트가 있는 requests만 허용한다.

```python
REST_FRAMEWORK = {
    'DEFAULT_PARSER_CLASSES': (
        'rest_framework.parsers.JSONParser',
    )
}
```

<br>

### 2. **클래스 기반 views(CBV)**

`APIView`클래스의 기본 views를 사용하여 개별 view나 viewset에 사용되는 `parser`를 설정할 수도 있습니다.

`parser_classes = (JSONParser,)` 를 class 안에 삽입하여 parser설정을 한다.

```python
from rest_framework.parsers import JSONParser
from rest_framework.response import Response
from rest_framework.views import APIView

class ExampleView(APIView):
    """
    A view that can accept POST requests with JSON content.
    """
    parser_classes = (JSONParser,)

    def post(self, request, format=None):
        return Response({'received data': request.data})
```

<br>

### 3. **함수 기반 views(FBV)**

FBV와 함께 `@api_view`데코레이터를 사용하는 경우 `@parser_classes((JSONParser,))` 데코레이터를 함수 위에 추가한다.

```python
from rest_framework.decorators import api_view
from rest_framework.decorators import parser_classes

@api_view(['POST'])
@parser_classes((JSONParser,))
def example_view(request, format=None):
    """
    A view that can accept POST requests with JSON content.
    """
    return Response({'received data': request.data})
```

<br>

## T**ype of parsing**

---

### 1. **JSONParser**

**`JSON 요청 컨텐츠`**를 파싱한다. request.data는 딕셔너리 형태로 구성된다.

**.media_type** : **`application/json`**

<br>

### 2. **FormParser**

`**HTML form 컨텐츠**`를 파싱한다. request.data는 데이터 QueryDict로 구성된다.

HTML form 데이터를 완벽하게 지원하려면 일반적으로 FormParser과 MultiPartParser를 같이 사용해야 한다.

**.media_type**: **`application/x-www-form-urlencoded`**

<br>

### 3. **MultiPartParser**

파일 업로드를 지원하는 `**멀티파트 HTML form 컨텐츠**`를 파싱 한다. request.data 모두 QueryDict로 구성된다.

HTML form 데이터를 완벽하게 지원하려면 일반적으로 FormParser과 MultiPartParser를 함께 사용해야 한다.

**.media_type**: **`multipart/form-data`**

<br>

### 4. **FileUploadParser**

`**raw 파일 업로드 컨텐츠**`를 파싱합니다. request.data 속성은 업로드된 파일이 포함된 단일 키 'file'이 있는 딕셔너리가 된다.

FileUploadParser와 함께 사용되는 view가 filename URL 키워드 아규먼트와 함께 호출되는 경우 해당 아규먼트가 filename으로 사용됩니다.

파일 이름으로 된 URL 없이 filename이 호출되는 경우 클라이언트는 content-Disposition HTTP header에서 filename을 설정해야합니다.

예시 : `Content-Disposition : attachment; filename=upload.jpg`

**.media_type**: **`*/*`**

<br>

## Outro

---

아직 적응의 단계인지… 왜 저거 한줄 띡 적었다고 swagger에서 읽을수 있는건지…

구조적으로 눈으로 바로바로 들어오지 않아 이해하는데 어려움이 있다.

다른언어도 다 마찬가지 겠지만, 공부해 나가는 과정에서 뭘 어디까지 공부해나야가 하는지 감이 안잡혀서 어렵다.

계속 하다 보면 되겟지 뭐.

<br>

[참고]

[Django 공식 문서 - Parsers](https://www.django-rest-framework.org/api-guide/parsers/#formparser)

[Django REST Framework - Parsers](https://kimdoky.github.io/django/2018/07/09/drf-Parsers/)

[파싱(Parsing)과 파서(Parser)](https://blog.binple.net/98)