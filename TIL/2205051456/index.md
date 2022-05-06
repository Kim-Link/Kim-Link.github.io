---
layout: post
type: TIL
date: 2022-05-05 14:56
draft: false
TIL : true
category: TIL
title: 다중 Sort
subtitle: sort는 복잡해...
writer: 000000
post-header: true
header-img: TIL.jpeg
hash-tag: [sort]
---

## 문제점

---

- 리뷰 댓글을 정렬하려고 한다.
    1. 별점 높은순서로 정렬한다.
    2. 별점이 같다면 최신순서로 정렬한다.

## 해결

---

```jsx
const sortedData = reviewData.sort((a, b) => {
      const s1 = a.score;
      const s2 = b.score;
      const d1 = a.writeAt;
      const d2 = b.writeAt;

      if (s1 < s2) return 1;
      if (s1 > s2) return -1;
      if (d1 < d2) return 1;
      if (d1 > d2) return -1;
      return 0;
    });
```

### 참고

---

[Array.prototype.sort() - JavaScript | MDN]:https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/sort