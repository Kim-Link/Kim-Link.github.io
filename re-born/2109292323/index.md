---
layout: post
type: re-born
date: 2021-09-29 23:23
category: HTML/CSS
title: CSS 두번째
subtitle: 미적 감각, 아주 중요합니다.
writer: 000000
post-header: true
header-img : img/about.png
hash-tag: [Javascript, HTML, CSS, flex, grid]
---
> 난 그래도 예대를 나왔으니 화면에 예쁘게 색칠할 수 있을거야


---





### 박스

CSS에서 박스를 구성하는 요소는 `border`, `padding`, `margin` 까지 총 3가지가 있다

```css
body {
  border : 1px solid #color;
  margin : 10px 20px 30px 40px;
  padding : 5px
}
```



- **border : 테두리**
  - 테두리는 심미적으로도 필요하지만, 개발 시 각 영역이 해당하는 크기를 알기 위해서 보통 레이아웃을 만들 때 크기를 시각적으로 보이도록 만든다.
- **padding : 안쪽 여백**
  - 한번에 입력할 때는 위, 오른쪽, 아래, 왼쪽 (시계방향) 순서로 입력한다.
  - 값을 2개 넣으면 (위,아래 ), (왼쪽, 오른쪽)으로 적용된다.
  - 값을 1개 넣으면 모든 방향에 적용된다.
  - 음수도 적용 가능하다. 다른 엘리먼트와 겹치거나 화면 밖으로 뺄 수도 있다.
- **margin : 바깥쪽 여백**
  - padding과 규칙은 동일하다.



> #### 박스 사이즈 처리
>
> 박스의 너비는 **콘텐츠 영역 + 테두리 두께 + 여백**으로 구성된다.
> 이 때문에 안쪽 박스에 100%를 값으로 주게 되면 내용이 밖으로 다 튀어나가게 된다.
>
> ```css
> #container {
> width : 300px;
> padding : 10px;
> background-color : yellow;
> border : 2px solid red;
> }
> 
> #inner {
> width : 100%;
> height : 200px;
> border : 2px solid green;
> background-color : lightgreen;
> padding : 30px
> }
> ```
>
> 이렇게 사이즈를 설정했다면 `container`는 324px크기를, `inner`는 364px의 크기를 갖는다.
>
> 안쪽 박스가 컨테이너보다 더 커서 사이즈가 엉망진창이 되는데, 이럴 때 의도대로 박스 사이즈를 설정하려면
>
> ```css
> * {
>   box-sizing : border-box;
> }
> ```
>
> 이렇게 전역이나 상위 항목에 `border-box` 옵션을 추가해주면 모든 여백과 테두리를 포함한 값을 300px(예시)로 계산한다.





### 넘치는 컨텐츠 처리

주어진 영역보다 컨텐츠가 더 크면 빠져나오는 것이 기본 동작이다. 그럴 땐 셀렉터 내용에 몇가지 속성을 추가하면 해결할 수 있다.

- `overflow : auto` 상하좌우 자동 스크롤
- `overflow : hidden` 스크롤하지 않고 내용을 가림
- `overflow-x : scroll`, `overflow-y : scroll` x축만 스크롤, y축만 스크롤





### Flex

`flex`는 상당히 편리한 방식으로 박스를 분할하고 정렬하고 제어할 수 있는데, 부모 엘리먼트에 주어지는 속성과 자식 엘리먼트에 주어지는 속성이 서로 다르다. 이 점에 유의하여 코드를 작성하도록 한다.



- **부모 엘리먼트에 주어지는 속성**

  - **display** : `flex`, `flex-box`, `inline-flex` 등 여러 옵션을 쓸 수 있다

  

  - **flex-direction** : 아이템 배치 방향을 결정하는 속성

    - **row** (기본값) : 아이템들이 가로 방향으로 배치됨
    - **column** : 아이템들이 세로 방향으로 배치됨 (block 요소를 쌓아 놓은 것처럼 보임)
    - **column-reverse, row-reverse** : 역방향

    

  - **flex-wrap** : 컨테이너가 더 이상 아이템을 한 줄에 담을 공간이 부족할 경우 아이템 줄바꿈을 어떻게 할 지 결정하는 속성

    - **nowrap** (기본값) : 줄바꿈하지 않음. 넘치면 그대로 밖으로 빠져나감
    - **wrap** : 줄바꿈 함. float나 inline-block으로 배치한 요소들과 비슷하게 동작함
    - **wrap-reverse** : 줄바꿈을 하지만 아이템을 역순으로 배치함.

    

  - **flex-flow** : `flex-direction`과 `flex-wrap`을 한꺼번에 지정할 수 있는 단축 속성. direction, wrap 순으로 입력한다)

  

  - **justify-content** : 메인 축 방향 정렬

    - **flex-start** (기본값) : 아이템들을 시작점으로 정렬. direction이 row일 때는 왼쪽, column일 때는 위쪽으로 정렬됨.
    - **flex-end** : 아이템들을 끝점으로 정렬. direction이 row일 때는 오른쪽, column일 때는 아래로 정렬됨.
    - **center** : 아이템들을 가운데로 정렬
    - **spact-between** : 아이템들의 사이에 균일한 간격을 만듬
    - **space-around** : 아이템들의 둘레에 균일한 간격을 만듬
    - **space-evenly** : 아이템의 사이와 양 끝에 균일한 간격을 만듬 ***MS 브라우저에서는 지원하지 않음***

    

  - **align-items** : 수직 축 방향 정렬

    - **stretch** (기본값) : 아이템들이 수직 축 방향으로 끝까지 늘어남
    - **flex-start** : 아이템들을 시작점으로 정렬. direction이 row일 때는 위, column일 때는 왼쪽으로 정렬됨.
    - **flex-end** : 아이템들을 끝으로 정렬. direction이 row일 때는 아래, column일 때는 오른쪽으로 정렬됨.
    - **center** : 아이템들을 가운데로 정렬
    - **baseline** : 아이템들을 텍스트 베이스라인 기준으로 정렬

    

  - **align-content** : 여러 행 정렬. `flex-wrap : wrap`이 설정된 상태에서 아이템들의 행이 2줄 이상 되었을 때 수직축 방향 정렬을 결정한다.





- **자식 엘리먼트에 주어지는 속성**
  - **order** : 각 아이템의 시각적 나열 순서를 결정하는 속성. 작은 숫자일 수록 먼저 배치되나, 시각적 순서일 뿐 HTML 구조를 바꾸는 것은 아니므로 접근성 측면에서 사용에 주의해야 함. (시각장애인의 스크린 리더로 화면을 읽을 시 order로 변경한 순서는 아무 의미가 없음)
  - **flex-grow** : 아이템이 `flex-basis`의 값보다 커질 수 있는지를 결정하는 속성 (일단 값이 0보다 클 경우 해당 아이템이 flexible 박스로 변하고, 원래의 크기보다 커지며 빈 공감을 메우게 됨. 기본값 0)
  - **flex-shrink** : 아이템이 `flex-basis`의 값보다 작아질 수 있는지를 결정하는 속성 (일단 값이 0보다 클 경우 해당 아이템이 flexible 박스로 변하고, 원래의 크기보다 작아짐. 기본값 1)
  - **flex-basis** : flex 아이템의 기본 크기를 설정(direction이 row일 때는 너비, column일때는 높이. 기본값 auto)
  - **align-self** : 아이템의 수직 축 방향 정렬 (기본값 auto. `align-items` 설정 상속받음)
  - **flex** : `flex : 0 1 auto`와 같이 grow shrink basis 순서로 입력한다.
  - **z-index** : z축 정렬을 할 수 있음. (position에서의 z-index와 동일한 역할)





### Grid

CSS 레이아웃의 결정체이다. `Flex`는 1차원적인 한 방향 레이아웃 시스템이었다면, `Grid`는 가로-세로(2차원)의 두 방향 레이아웃 시스템이라는 것이 가장 큰 차이이다.

flex와 마찬가지로 grid도 부모 엘리먼트와 자식 엘리먼트에 주어지는 속성이 서로 다르기 때문에 이 점에 주의하여 코드를 작성하도록 한다.

IE에서는 Grid를 지원하지 않거나 제대로 표현되지 않는 경우가 많으므로 유의한다.

> #### 용어 정리
>
> - **그리드 컨테이너**
>   
>   `display : grid`를 적용하는 그리드의 전체 영역이다(부모 엘리먼트). 컨테이너 안의 요소들(자식 엘리먼트)이 grid 규칙의 영향을 받아 정렬된다.
>
> 
>
> - **그리드 아이템**
>   
>   그리드 컨테이너의 자식 요소들이다.
>
> 
>
> - **그리드 트랙**
>   
>   그리드의 행 또는 열
>
> 
>
> - **그리드 셀**
>   
>   그리드의 한 칸을 가리키는 용어이다.
>
> 
>
> - **그리드 라인**
>   
>   그리드 셀을 구분하는 선이다
>
> 
>
> - **그리드 번호**
>   
>   그리드 라인의 각 번호
>
> 
>
> - **그리드 갭**
>   
>   그리드 셀 사이의 간격
>
> 
>
> - **그리드 영역**
>   
>   그리드 라인으로 둘러싸인 사각형 영역. 그리드 셀의 집합이다.



- 부모 엘리먼트에 주어지는 속성

  - **display** : `grid`, `inline-grid` 등 여러 옵션을 쓸 수 있다.
  - **grid-template-rows / columns** : 컨테이너에 트랙의 크기를 지정하는 속성.
    - 값을 입력하는 만큼 row나 column에 트랙이 생성된다. px나 fr 등 다양한 단위를 사용할 수도 있고, 함수를 사용할 수도 있다.
      - **repeat 함수** : `repeat(반복횟수, 반복값)`을 입력하면 반복되는 값을 자동으로 처리할 수 있다. 반복값을 다양하게 삽입하여 패턴으로 만들 수도 있다.
      - **minmax 함수** : `minmax(최소, 최대)`을 입력하면 아무리 양이 적더라도 최소값만큼 높이나 너비를 확보하고, 내용이 많아 최소값을 넘어가게 되면 최대값까지 자동으로 늘어나도록 처리해 준다.
      - **auto-fill, auto-fit** : column의 개수를 미리 정하지 않고 설정된 너비가 허용하는 한 최대한 셀을 채운다. 단, auto-fil은 셀의 개수가 모자라면 공간이 남지만 auto-fit은 셀 크기를 늘려 남는 공간을 자동으로 채운다.

  

  - **row-gap / column-gap / gap** : 그리드 셀 사이의 간격을 설정하는 속성.
  - **grid-auto-rows / columns** : `grid-template-rows/columns`의 통제를 벗어난 위치에 있는 트랙의 크기를 지정하는 속성이다. `grid-template` 속성에서 설정한 트랙 수 보다 실제 생성되는 트랙이 많아지면 설정되지 않아서 제멋대로 표시되게 되는데, 트랙이 얼마나 생성될 지 확실하지 않다면 이 속성을 써서 자동으로 처리되도록 할 수 있다.
  - **grid-column / grid-column-start / grid-column-end / grid-row / grid-row-start / grid-row-end** : 
    각 셀의 영역을 지정하는 속성이다.
  - **grid-template-areas** : 각 영역에 이름을 붙이고, 그 이름을 이용하여 배치하는 속성이다. 각 영역 매칭은 자식 엘리먼트의 해당 요소에 `grid-area` 속성으로 이름을 지정해 주면 된다.
  - **grid-auto-flow** : 아이템이 자동 배치되는 흐름을 결정하는 속성.
  - **align-items** : 세로 방향 정렬
  - **justify-items** : 가로 방향 정렬
  - **place-items** : 정렬 속성 2개를 같이 쓸 수 있다. 세로-가로 순으로 작성하고, 하나만 쓰면 두 속성 모두 적용된다.
  - **align-content** : 아이템들의 높이를 모두 합한 값이 컨테이너 높이보다 작을 때 아이템들을 통째로 정렬한다.
  - **justify-content** : 아이템들의 너비를 모두 합한 값이 컨테이너 너비보다 작을 때 아이템들을 통째로 정렬한다.
  - **place-content** : 정렬 속성 2개를 같이 쓸 수 있다. 세로-가로 순으로 작성하고, 하나만 쓰면 두 속성 모두 적용된다.



- **자식 엘리먼트에 주어지는 속성**
  - **align-self** : 해당 아이템을 세로 방향으로 정렬한다
  - **justify-self** : 해당 아이템을 가로 방향으로 정렬한다
  - **place-self** : 정렬 속성 2개를 같이 쓸 수 있다. 세로-가로 순으로 작성하고, 하나만 쓰면 두 속성 모두 적영된다.
  - **order** : flex의 속성과 동일하다
  - **z-index** : z축 정렬을 할 수 있다 (position에서의 역할과 동일하다)

