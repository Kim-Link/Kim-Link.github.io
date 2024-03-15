---
layout: post
type: LOading
date: 2022-11-21 18:19
category: MacOS
title: xcrun error
subtitle: 업데이트 마다 왜이러니
writer: 000000
post-header: true
header-img: ../macOS.png
hash-tag: [Error]
---

## Intro

---

git add . 를 하는데 

갑자기 못보던 오류가 나왔다.

<img src="img/1.png" alt="1" style="zoom:80%;" />

뭐지??

## 해결

---

xcode-select --install 을 하면 해결

<img src="img/3.png" alt="1" style="zoom:80%;" />

해당 명령어로 설치하면 

<img src="img/2.png" alt="1" style="zoom:80%;" />

이렇게 뭔가 설치 하게 되고

<img src="img/4.png" alt="1" style="zoom:80%;" />

설치가 끝나면

<img src="img/5.png" alt="1" style="zoom:80%;" />

해결 된다.

## Outro

---

원인은 아까전에 즐겁게 업데이트 했던 것 때문이다.

mac OS를 업데이트 할때마다 xcode에 관련된 것을 업데이트 해 주어야 하는것 같다.

별거 아닌것 같지만,

환경을 세팅하고 버전을 맞추는게 가장 번거롭고 어려운 일인것 같다.
