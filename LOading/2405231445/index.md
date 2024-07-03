---
layout: post
type: LOading
date: 2024-05-23 14:45
category: ElasticSearch
title: ElasticSearch 활용(2) - Index Setting
subtitle: ElasticSearch Index Setting
writer: 0
post-header: true
header-img: ../elastic.png
hash-tag:
  - ElasticSearch
  - OpenSearch
  - AWS
  - 저
---


## Index Setting

### Index 생성

- Index 생성은 아래 예시와 같다. (3개는 모두 동일하게 작동)

```
PUT my_index
{
  "settings": {
    "index": {
      "number_of_shards": 3,
      "number_of_replicas": 1
    }
  }
}

PUT my_index
{
  "settings": {
    "index.number_of_shards": 3,
    "index.number_of_replicas": 1
  }
}

PUT my_index
{
  "settings": {
    "number_of_shards": 3,
    "number_of_replicas": 1
  }
}
```

<br>
 
### number_of_shards, number_of_replicas

- **number_of_shards** : 프라이머리 샤드 수(기본값 1)
- **number_of_replicas** : 리플리카 수(기본값 1)


> 주의 사항
> 
> **number_of_shards** 설정은 인덱스를 처음 생성할 때 한번 지정하면 바꿀 수 없습니다.
> 
> 샤드 수를 바꾸려면 새로 인덱스를 정의하고 기존 인덱스의 데이터를 재색인 해야 합니다.

**number_of_replicas**는 변경이 가능하다.
- 인덱스명 + `_settings API`로 접근해서 변경

```
PUT my_index/_settings
{
  "number_of_replicas": 2
}
```

<br>

### analyzer, tokenizer, filter(동의어 사전과 사용자 사전 세팅)

- Elasticsearch는 **텍스트 분석(Text Analysis)** 과정을 통해 검색어 토큰을 저장한다.
- 텍스트 분석 과정에서 처리하는 기능을 **애널라이저(Analyzer)** 라고 한다.
- 애널라이저는 **캐릭터 필터(Character Filter)**, **토크나이저(Tokenizer)**,  **토큰 필터(Token Filter)** 로 구성된다.

<img src="img/Pasted image 20240530173849.png" alt="1" style="zoom:80%;" />

<br>


#### 1. 캐릭터 필터(Character Filter)

캐릭터 필터는 입력 텍스트를 처리하여 특정 문자를 추가하거나 제거하는 등의 변환을 수행한다. 일반적으로 입력 텍스트의 전처리 과정에서 사용된다.

<br>

##### 사용 예시
- HTML 제거: 입력 텍스트에서 HTML 태그를 제거하여 순수한 텍스트만을 추출한다.
- 특수 문자 제거: 입력 텍스트에서 특정 특수 문자를 제거하여 데이터를 정제한다.
- 대소문자 변환: 입력 텍스트의 대소문자를 통일하거나, 특정 부분만 대문자로 변환하는 등의 작업 한다.

<br>

#### 2. 토크나이저(Tokenizer)

토크나이저는 캐릭터 필터가 처리한 텍스트를 일련의 토큰(token)으로 분할하는 역할을 한다. 토큰은 의미 있는 최소 단위로, Elasticsearch에서 검색할 때 기본 단위로 사용된다.

<br>

##### 사용 예시
- 공백 분할: 입력 텍스트를 공백을 기준으로 분할하여 개별 단어를 토큰으로 생성한다.
- 구두점 분할: 입력 텍스트를 구두점(마침표, 쉼표 등)을 기준으로 분할하여 개별 토큰을 생성한다.
- n-gram 생성: 입력 텍스트에서 연속된 n개의 문자를 추출하여 n-gram 토큰을 생성한다.

<br>

#### 3. 토큰 필터(Token Filter)

분리된 텀 들을 하나씩 가공하는 과정을 거치는데 이 과정을 담당하는 기능이 **토큰 필터**의 역활이다.
토큰 필터는 토크나이저에서 생성된 토큰에 대해 추가적인 처리를 수행한다. 주로 토큰의 형태소 분석, 정규화, 변환 등의 작업을 수행하여 검색 결과를 향상시킨다.

<br>

##### 사용 예시
- 스탑워드 필터링: 불용어(예: and, or, the 등)를 제거하여 색인에 불필요한 단어를 제외한다.
- 형태소 분석: 각 토큰을 형태소 단위로 분석하여 어간 추출(Stemming) 또는 표제어 추출(Lemmatization)을 한다.
- 동의어 확장: 특정 단어의 동의어를 추가하여 검색의 정확성과 유연성을 높인다.

<br>

### 실제 서비스 적용

#### 실사용 예시

```
PUT /index_name_v1.0.0
{
    "settings": {
      "index": {
          "analysis": {
            "analyzer": {
              "korean": {
                  "type": "custom",
                  "tokenizer": "seunjeon",
                  "filter":["synonym"]
              }
            },
            "filter": {
                "synonym": {
                  "type": "synonym",
                  "synonyms_path": "analyzers/F142454713"
                }
            },
            "tokenizer": {
              "seunjeon": {
                "user_dict_path": "analyzers/F164792853",
                "index_eojeol": "true",
                "index_poses": [
                    "UNK",
                    "EP",
                    "I",
                    "J",
                    "M",
                    "N",
                    "SL",
                    "SH",
                    "SN",
                    "VCP",
                    "XP",
                    "XS",
                    "XR"
                ],
                "decompound": "true",
                "type": "seunjeon_tokenizer"
              }
            }
          }
        }
    },
    "mappings": {
        "properties": {
          "title": {
                "type": "text",
                "analyzer": "standard",
                "search_analyzer" : "korean"
            },
          "brand": {
                "type": "text",
                "analyzer": "korean"
            },
            "content": {
                "type": "text",
                "analyzer": "korean"
            }
        }
    }
}
```


<br>

#### 애널라이저의 구성 요소

1. **토크나이저(Tokenizers)**: "seunjeon" 토크나이저
    - **seunjeon 토크나이저**: 한국어 형태소 분석기인 Seunjeon을 사용하여 텍스트를 토큰화 한다. 이 토크나이저는 사용자 사전을 사용하고, 어절 분리, 품사 태깅, 복합어 분리 등의 기능을 수행한다..
2. **토큰 필터(Token Filters)**: "synonym" 필터
    - **synonym 필터**: 유의어 처리를 수행합니다. "synonyms_path" 매개변수를 통해 지정된 파일에 저장된 유의어 사전을 사용하여 텍스트 내의 유의어를 교체합니다.

<br>

> analyzer구성안에 있는 요소들에 대한 세팅값이  아래 나열이 되어야 한다.
> 
> 이 구조가 지켜 지지 않으면 index생성이 되지 않는다.
> 
> ~~(이것도 개고생함..)~~****

<br>

#### 주의사항

- 사용자는 실제 유의어 사전 파일이 "analyzers/F222513712" 경로에 존재해야 한다.
- Seunjeon 토크나이저는 사용자 사전 파일이 "analyzers/F164670752" 경로에 존재해야 한다. 
- AWS Opensearch 서비스의 장점이다. AWS S3에 올려놓고 경로 지정만 해주면 된다.
    - *~~자세한 사용법은 다음번에 기회가 되면 다루도록 하겠다.~~*

<br>

마지막으로...

> 반드시 정적 맵핑을 해야 한다.
> 동적 맵핑을 맵핑을 하면 인덱싱 할때 token 설정을 하지 않기 때문에
> 동의어, 사용자 사전이 적용되지 않는다!!
> (제일중요함. 실컷 세팅하고 안되면 개빡침)

<br>

## 참고

[elasticsearch 가이드북](https://esbook.kimjmin.net/)
