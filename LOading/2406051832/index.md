---
layout: post
type: LOading
date: 2024-06-05 18:32
category: ElasticSearch
title: ElasticSearch 활용(4) - kNN 검색
subtitle: 방송국 아니다...
writer: 0
post-header: true
header-img: ../elastic.png
hash-tag:
  - ElasticSearch
  - 저
  - kNN
---
# Elastic search k-NN(color index 설정)
- Elasticsearch에서 k-Nearest Neighbors (kNN) 검색은 주어진 쿼리 벡터와 가장 가까운 k개의 벡터를 찾는 방법이다.
- 주로 벡터 검색과 유사성 검색에 사용된다.(예: 이미지 검색, 추천 시스템, 자연어 처리)
-  Elasticsearch의 kNN 검색은 k-NN 알고리즘을 사용하여 고차원 벡터 공간에서 가장 가까운 벡터들을 효율적으로 검색할 수 있도록 지원한다.

<br>

## kNN 검색의 기본 개념

- **벡터**: 각 데이터 포인트를 표현하는 다차원 배열
- **유사성**: 벡터 간의 거리 또는 코사인 유사성을 기반으로 측정
- **k**: 검색할 이웃의 수

<br>

## Elasticsearch에서 kNN 검색을 사용하는 방법

1. **인덱스 생성 및 설정**: kNN 검색을 사용할 인덱스를 생성하고, 벡터 데이터를 저장할 필드를 설정
2. **데이터 색인**: 벡터 데이터를 Elasticsearch에 색인
3. **kNN 검색 쿼리 실행**: 주어진 쿼리 벡터와 가장 가까운 k개의 벡터를 검색

<br>

## 사용 예시
유사 색상을 찾는것을 가정하여 color 검색에 kNN을 적용시켜 보자.

<br>

### 1. 인덱스 생성 및 설정
```
PUT color-knn_v1.0.0
{
  "settings": {
    "index.knn" : true
  }
  ,"mappings": {
    "properties": {
      "rgb_vector":{
        "type": "knn_vector",
        "dimension": 3
      },
      "hex": {
	    "type" : "text"
      }
    }
  }
}
```
위 예시는 3차원 벡터 데이터를 저장할 `rgb_vector` 필드를 가진 인덱스를 생성한다.
이때 `hex`값도 함께 설정한다.

<br>

### 2. 데이터 색인
*예시*
```
POST /my_knn_index/_doc/1
{
  "my_vector": [1.0, 2.0]
}

POST /my_knn_index/_doc/2
{
  "my_vector": [2.0, 3.0]
}

POST /my_knn_index/_doc/3
{
  "my_vector": [3.0, 4.0]
}

```
위 예시는 3개의 문서를 인덱스에 추가하며, 각각 2차원 벡터 데이터를 포함한 것이다.

하지만 내가 사용한 경우는 많은 데이터들을 한번에 넣어야 하기 때문에 bulk형태로 만들어서 데이터를 색인 하였다.
```python
def json_to_bulk_data(entries, index_name):  
    bulk_data = []  
  
    for entry in entries:  
        rgb_color = hex_to_rgb(entry)  
        upsert_line = {  
            "index": {  
                "_index": index_name,  
            }  
        }  
        doc_line = {  
            "rgb_vector": rgb_color,  
            "hex": entry  
        }  
        bulk_data.append(json.dumps(upsert_line))  
        bulk_data.append(json.dumps(doc_line))  
  
    return "\n".join(bulk_data) + "\n"
```
`rgb_color` : `[ 125, 1, 12]` 처럼 `number[]` 타입으로 되어 있다.

<br>

### 3. kNN 검색 쿼리 실행
- 검색 쿼리는 nestJS에 적용 되었다.

```ts
async getColorDataToRgb(rgb: number[]): Promise<string[]> {  
  const { body } = await this.searchClient.search({  
    index: 'color-knn_v1.0.0',  
    size: 200,  
    body: {  
      query: {  
        knn: {  
          rgb_vector: {  
            vector: rgb,  
            k: 100,  
          },  
        },  
      },  
    },  
  });  
  const result = [];  
  body.hits.hits.map((el) => {  
    result.push(el._source.hex);  
  });  
  return result;  
}
```

kNN 검색에서 `k`는 반환할 이웃의 수를 의미하지만, 실제 반환되는 결과는 검색된 벡터와의 유사성(거리)에 따라 달라질 수 있다.

kNN 검색을 사용할 때, 주어진 쿼리 벡터와 유사한 데이터가 충분히 많지 않거나,

유사성 기준(거리가 너무 멀다)에 맞지 않는 경우, 설정된 `k` 값보다 적은 결과가 반환될 수 있다.

<br>

### 4. 결과
Elasticsearch는 가장 가까운 벡터를 찾고, 유사성 점수와 함께 결과를 반환한다.

```
{
  "hits": {
    "total": 2,
    "max_score": 1.0,
    "hits": [
      {
        "_index": "color-knn_v1.0.0",
        "_id": "NzUmwY4B2u02NJGEbEms",
        "_score": 1,
        "_source": {
          "rgb_vector": [
            245,
            245,
            242
          ],
          "hex": "f5f5f2"
        }
      },
      {
        "_index": "color-knn_v1.0.0",
        "_id": "OjUmwY4B2u02NJGEbEms",
        "_score": 1,
        "_source": {
          "rgb_vector": [
            165,
            7,
            13
          ],
          "hex": "a5070d"
        }
      },
      {
        "_index": "color-knn_v1.0.0",
        "_id": "PTUmwY4B2u02NJGEbEms",
        "_score": 1,
        "_source": {
          "rgb_vector": [
            228,
            190,
            185
          ],
          "hex": "e4beb9"
        }
      },
    ]
  }
}

```

<br>

## kNN 검색의 장점

1. **고차원 데이터 처리**: kNN 검색은 고차원 벡터 데이터를 효율적으로 처리할 수 있다.
2. **유연한 응용**: 이미지 검색, 추천 시스템, 자연어 처리 등 다양한 응용 분야에서 활용할 수 있다.
3. **유사성 기반 검색**: 벡터 간의 유사성을 기반으로 하여 더 정교한 검색 결과를 제공한다.

<br>

## kNN 검색의 단점

1. **고비용 연산**: 고차원 데이터에서 kNN 검색은 연산 비용이 높을 수 있다.
    - kNN 검색은 고차원 벡터 공간에서 가장 가까운 이웃을 찾기 위해 많은 계산을 필요로 하므로, 특히 큰 데이터셋에서는 연산 비용이 높아질 수 있다. 이는 응답 시간 증가, 리소스 소모 증가, 스케일링 어려움 등의 문제가 발생할수 있다.
2. **실시간 처리 한계**: 실시간으로 대규모 데이터를 처리하는 데 한계가 있을 수 있다.
    - 연산 비용이 높은 kNN 검색은 실시간으로 데이터를 처리하는 데 어려움을 겪을 수 있다. 이는 추천 시스템, 이미지 검색, 자연어 처리 등에서 실시간으로 관련 결과를 제공하는 데 지연을 발생시킬 수 있다.


알아서 잘쓰자.


<br>

*해당 글은 chatGPT를 기반으로 작성되었습니다.*
