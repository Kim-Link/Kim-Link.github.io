---
layout: post
type: LOading
date: 2024-05-16 16:04
category: ElasticSearch
title: ElasticSearch 활용(1)
subtitle: 일단 써보자...
writer: 0
post-header: true
header-img: ../elastic.png
hash-tag:
  - ElasticSearch
  - OpenSearch
  - AWS
  - 저
---

## Intro

요즘 프로젝트에서 검색엔진(aws open search)을 사용중이다.

프로젝트에서 dash board가 중요했다면 ElasticSearch를 사용 했겠지만,

회사 프로젝트에서 중요한 부분이 **검색엔진 기능** 자체와 **빠른 개발**이었다.

때문에 서버를 따로 띄우지 않고 AWS에서 제공하는 서비스를 활용하기로 결정했다.

<br>

OpenSearch를 사용하면서 ElasticSearch라고 쓴 이유는

이 둘은 엄밀히 말하면 다르지만,

내가 검색을할때 OpenSearch에 대한 자료보다 ElasticSearch의 자료가 월등히 많았고,

내가 필요한 정보들은 대부분 ElasticSearch의 정보들이 동일하게 OpenSearch에 적용이 되었기 때문에 ElasticSearch라고 타이틀을 정해보았다.

<br>

이 글에서는 개념적인 부분보다 프로젝트에서 실제로 사용한 OpenSearch의 주요 기능만 정리하였다.

>elasticsearch의 REST 명령들은 Kibana의 Dev Tools 에서 입력하는 형식으로 설명하도록 하겠습니다.


<br>

## Elastic Search란?

최소한의 개념을 잡자.

자료를 빠르게 쿼리(검색) 해올 수 있는 **DataBase**다.

*~~(역인덱스, 검색엔진서버등 자세한 설명은 생략한다.)~~*

<br>

## Index

### Index 특징

- Elasticsearch 의 **인덱스** :  도큐먼트들이 모여 있는 논리적인 데이터의 집합
- 인덱스는 하나의 노드에만 존재하지 않고 샤드 단위로 구분되어 여러 노드에 걸쳐 저장되어 데이터 무결성의 보장과 검색 성능의 향상을 실현
- 인덱스에서 사용되는 설정이나 도구들은 다른 인덱스에 영향을 미치지 않음

<br>

### Index생성

```
GET my_index
```

기본적으로 이 명령어 만으로도 인덱스는 생성된다.

이것은 매우 기본적인 것으로 이렇게 생성하면 나중에 문제가 될수 있다.

Index생성할때 `settings`와 `mapping`은 지정해주는것이 좋다.

<br>

### Index Settings

Index를 설정할때 크게 다섯가지 정도 설정할수 있다.

- number_of_shards
- number_of_replicas
- analyzer
- tokenizer
- filter

<br>

### Index Mappings

인덱스 맵핑에는 동적 맵핑과 정적 맵핑이 있다.

1. **정적 매핑(Static Mapping)**: 정적 매핑은 인덱스를 생성할 때 미리 정의된 매핑을 사용하는 방법

```
PUT my_index
{
  "mappings": {
    "properties": {
      "name": {
        "type": "text"
      },
      "age": {
        "type": "integer"
      },
      "email": {
        "type": "keyword"
      }
    }
  }
}
```

위의 예시에서는 "name" 필드는 텍스트 유형, "age" 필드는 정수 유형, "email" 필드는 키워드 유형으로 지정되었다.

이렇게 정적으로 매핑을 정의하면 Elasticsearch가 문서를 인덱싱할 때 해당 매핑에 따라 필드를 처리한다.

<br>

2. **동적 매핑(Dynamic Mapping)**:동적 매핑은 인덱싱할 때 자동으로 매핑을 생성하는 방법

```
POST my_index/_doc
{
  "name": "John",
  "age": 30,
  "email": "john@example.com",
  "address": "123 Main St"
}
```

위의 예시에서 "address" 필드는 이전에 정의된 매핑에 없었지만 Elasticsearch는 자동으로 새로운 필드에 대한 매핑을 생성한다. 이러한 방식으로 동적 매핑은 데이터의 다양성에 대응할 수 있다.

> **정적 매핑** : 사전에 정의된 매핑을 사용하여 필드를 정의
> 
> **동적 매핑** : 데이터가 인덱싱될 때마다 필요한 매핑을 자동으로 생성


주의할 점은 이미 만들어진 매핑에 필드를 추가하는것은 가능하지만

이미 만들어진 필드를 삭제하거나 필드의 타입 및 설정값을 변경하는 것은 불가능하다.

Index Settings이나 필드의 변경이 필요한 경우

인덱스를 새로 정의하고 기존 인덱스의 값을 새 인덱스에 모두 재색인 해야 한다.

<br>

## CRUD

### Create(데이터 입력) : `PUT`+`_doc`

```
PUT my_index/_doc/1
{
  "name": "John",
  "age": 30,
  "email": "john@example.com"
}
```

- `PUT` 요청은 Elasticsearch에 새로운 문서를 추가하거나 기존 문서를 덮어쓰는 데 사용
- 문서 ID가 지정되지 않은 경우 Elasticsearch는 새로운 ID를 생성
- 직접 문서 ID를 지정 가능

<br>

### Read(조회) : `GET` + `_doc`, `GET`+ `_search`

- 단일 문서 검색
    - 해당하는 단일 문서 ID의 정보를 가져옴


```
GET my_index/_doc/1
```


- 여러 문서 조회
    - 예시에서는 `my_index` 인덱스에서 이름이 "John"인 문서를 검색
    - `match` 쿼리를 사용하여 검색을 수행
    - 검색 결과로 일치하는 모든 문서가 반환


```
GET my_index/_search
{
  "query": {
    "match": {
      "name": "John"
    }
  }
}
```

<br>

### Update(수정) : `POST`+ `_update`

```
POST my_index/_update/1
{
  "doc": {
    "name": "Updated Name",
    "age": 35
  }
}
```

Elasticsearch에서는 문서를 "수정"하는 것이 실제로는 내부적으로 "업데이트"하는 것이 아니라 **"삭제 후 재색인"** 하는 방식으로 처리된다.

이를 "문서의 업데이트(update)"로 간주할 수 있지만,

Elasticsearch는 실제로는 해당 문서를 삭제하고 새로운 버전의 문서를 색인한다.

이러한 방식으로 인덱스의 일관성을 유지하면서도 데이터를 업데이트할 수 있다.

<br>

### Delete(삭제) : `DELETE`+`_doc`, `DELETE`+`_delete_by_query`

- 단일 문서 삭제

```
DELETE my_index/_doc/1
```


- 일괄 삭제

```
POST my_index/_delete_by_query
{
  "query": {
    "match": {
      "name": "John"
    }
  }
}
```

<br>

## 벌크 API : `POST _bulk` + `json`

Elastic Search를 사용하는 많은 서비스들이 LogStash를 활용한다.

Logstash는 다양한 소스로부터 데이터를 수집하고 전환하여 원하는 대상에 전송할 수 있도록 하는 오픈 소스 데이터 모으기 도구다.

하지만 나는 데이터를 수집하고 전환하는 작업이 필요하지 않았고, 

단지 Json 파일로 구성된 많은 데이터를 한번에 데이터 삽입하는 코드가 사용되었다.

<br>

이때 사용한 방식이 Bulk API 다.

벌크 API인 `_bulk` 엔드포인트는 여러 개의 인덱스 조작 작업을 단일 요청으로 처리하는 데 사용된다.

이를 통해 네트워크 비용과 서버 부하를 줄일 수 있으며, 대량의 데이터를 효율적으로 처리할 수 있다.

<br>

Elastic Search에서 Bulk API의 형태가 조금 독특하다.

`_bulk` 요청은 JSON 형식으로 작성되며, 각 작업은 두 개의 줄로 구성되어 있다.

한줄은 메타데이터를 나타내고, 다른 줄은 작업의 데이터를 나타낸다.

```
POST _bulk
{"index":{"_index":"my_index","_id":"1"}}
{"name":"John","age":30,"email":"john@example.com"}
{"index":{"_index":"my_index","_id":"2"}}
{"name":"Alice","age":25,"email":"alice@example.com"}
{"index":{"_index":"my_index","_id":"3"}}
{"name":"Bob","age":35,"email":"bob@example.com"}
```

예시에서 `_bulk` 요청을 사용하여 `my_index` 인덱스에 세 개의 문서를 동시에 인덱싱하고 있다.

각 문서는 `{}`로 둘러싸인 JSON 형식으로 표현되며, 

각 문서는 인덱싱될 인덱스의 메타데이터와 실제 문서 데이터로 구성된다.

<br>

## 검색 API

위 CRUD에서 설명한 Read방식은 아주 간단하게만 표시한 것이다.

검색엔진인 만큼 훨씬 복잡한 검색이 가능하다.

### Match_all

- 모든 문서를 반환
- `GET my_index/_search` 만 사용하는것과 동일

```
GET my_index/_search
{
  "query": {
    "match_all": {}
  }
}
```

<br>

### Match
- 검색에 사용되는 가장 일반적인 쿼리
- 하나의 필드에서 텍스트를 검색

```
GET my_index/_search
{
  "query": {
    "match": {
      "title": "apple banana"
    }
  }
}
```

위 예시에서는 title 필드에서 apple과 banana가 들어었는 모든 도큐먼트를 검색한다.

이때 apple과 banana는 or 조건으로 검색된다.

이때 만약에 or조건이 아닌 and조건으로 바꾸기 위해서는 operator 옵션을 사용해서 변경할수 있다.


```
GET my_index/_search
{
  "query": {
    "match": {
      "title": {
        "query": "apple banana",
        "operator": "and"
      }
    }
  }
}
```

이때 문법이 조금 달라진다.

기본 방식 : `<필드명>:<검색어>`

옵션 설정 방식 : `<필드명>: { "query":<검색어>, "operator": }`

<br>

### Multi_match

- 여러 필드에 걸쳐 일치하는 검색어를 찾을 때 사용
- 한 번에 여러 필드를 지정하고 하나의 검색어에 대해 검색

```
{
  "query": {
    "multi_match": {
      "query": "apple",
      "fields": ["title", "description"]
    }
  }
}
```

multi-match에서의 옵션 방식에서는 문법이 달라지진 않는다.

```
{
  "query": {
    "multi_match": {
      "query": "apple banana",
      "fields": ["title", "description"],
      "operator": "and"
    }
  }
}
```

<br>

#### match와 multi-match에서의 옵션 : `type`, `operator`

1. **type 옵션**:
    - 검색어의 처리 방법 지정
    - 가능한 값 : `"boolean"`(기본값), `"phrase"`, `"phrase_prefix"`
        - `"boolean"`은 기본 검색 방식으로, 검색어를 개별 단어로 분리하여 각 단어를 개별적으로 일치시킨다.
        - `"phrase"`는 전체 검색어를 정확히 일치하는 구문으로 처리한다.
        - `"phrase_prefix"`는 검색어를 접두사로 처리하여 일치하는 모든 단어를 찾는다.
2. **operator 옵션**:
    - 검색어의 연산자를 지정
    - 가능한 : `"and"`, `"or"`(기본값)가 있습니다.
        - `"and"`는 검색어의 모든 단어가 문서에 함께 존재해야 해당 문서를 반환합니다.
        - `"or"`는 검색어의 하나 이상의 단어가 문서에 존재하면 해당 문서를 반환합니다.

<br>

예를 들어, `"type": "phrase"`과 `"operator": "and"`를 함께 사용하면, 검색어를 정확히 일치하는 구문으로 처리하고 모든 단어가 함께 존재하는 경우에만 해당 문서를 반환한다.

<br>

### Match_phrase
- 검색어가 정확한 순서로 문장 또는 문장의 일부와 일치하는 문서를 찾을 때 사용

```
{
  "query": {
    "match_phrase": {
      "message": "quick brown fox"
    }
  }
}
```

위의 예시에서는 "message" 필드에서 "quick brown fox"라는 구문과 정확히 일치하는 문서를 찾는다.

`match_phrase` 쿼리는 기본적으로 검색어의 각 단어가 연속적으로 나타나야 하지만, 

이 외에도 `slop` 매개변수를 사용하여 단어 사이의 최대 허용 거리를 지정할 수 있다.

이를 통해 단어 사이에 조금의 여유를 두고 일치하는 문서를 찾을 수 있다.

```
{
  "query": {
    "match_phrase": {
      "title": {
        "query": "big banana",
        "slop": 1
      }
    }
  }
}
```

위의 예시에서는 "big"과 "banana" 사이에 최대 1개의 단어가 허용된다.

따라서 "big pink banana"와 "big red banana"와 같은 문서도 검색 결과로 반환될 수 있다.

<br>

### Range

- 지정된 범위에 포함되는 문서를 반환
- 주로 숫자 또는 날짜 필드의 값 범위를 지정하는 데 사용

```
{
  "query": {
    "range": {
      "age": {
        "gte": 30,
        "lte": 50
      }
    }
  }
}
```

<br>

### Bool : `must`, `must_not`, `should`, `filter`

- 여러 개의 다른 쿼리를 조합하여 복잡한 검색 수행
- `must`, `must_not`, `should`,`filter` 를 사용하여 조건 지정

```
{
  "query": {
    "bool": {
      "must": [
        { "match": { "name": "John" }},
        { "range": { "age": { "gte": 30 }}}
      ],
      "must_not": [
        { "match": { "email": "john@example.com" }}
      ],
      "should": [
        { "match": { "city": "New York" }}
      ],
      "filter": [
        { "term": { "status": "active" }}
      ]
    }
  }
}
```

<br>

#### must

- 모든 조건을 충족해야 하는 필수 조건

```
{
  "query": {
    "bool": {
      "must": [
        { "match": { "name": "John" }},
        { "range": { "age": { "gte": 30 }}}
      ]
    }
  }
}

```

#### must_not

- 지정된 조건을 충족하지 않아야 하는 필수 조건

```
{
  "query": {
    "bool": {
      "must_not": [
        { "match": { "email": "john@example.com" }}
      ]
    }
  }
}

```

<br>

#### should

- 하나 이상의 조건 중 하나 이상을 충족해야 하는 선택적인 조건
- 검색 결과의 관련성을 조절할 때 사용

```
{
  "query": {
    "bool": {
      "should": [
        { "match": { "city": "New York" }},
        { "match": { "city": "London" }}
      ]
    }
  }
}
```

검색 결과의 관련성을 조절할 때 사용된다는 것을 활용하여 **match_phrase** 와 함께 유용하게 사용할 수 있다.

```
GET my_index/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "match": {
            "title": {
              "query": "pink banana"
            }
          }
        }
      ],
      "should": [
        {
          "match_phrase": {
            "title": "pink banana"
          }
        }
      ]
    }
  }
}
```

위 예시 처럼 사용하게 되면 **pink** 또는 **banana** 중 하나라 포함된 데이터를 모두 검색하면서 "pink banana"를 정확하게 일치하는 데이터를 가장 위로 가져온다.

**must** 안에 **match** 쿼리로 도큐먼트를 검색하고 **should** 안에 **match_phrase** 쿼리를 써서 스코어 점수를 높인다.

<br>

#### filter

- 검색 결과를 필터링하기 위해 사용되는 조건
- 검색 결과의 관련성에 영향을 미치지 않음
- 특정 조건에 딱 맞는 문서를 찾고자 할 때 사용
- 검색 결과의 순서나 관련성을 고려하지 않음

```
{
  "query": {
    "bool": {
      "filter": [
        { "term": { "status": "active" }},
        { "range": { "age": { "gte": 30 }}}
      ]
    }
  }
}
```

<br>

## 참고

[elasticsearch 가이드북](https://esbook.kimjmin.net/)
