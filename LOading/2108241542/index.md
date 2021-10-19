---
layout: post
type: LOading
date: 2021-08-24 15:42
draft: false
category: github
title: git remote 설정법
subtitle: 시작중의 시작, 세팅은 항상 어렵다.
writer: 000000
post-header: true
header-img: img/about.jpeg
hash-tag: [github, git, setting]
---

처음 git으로 repository(레포)를 만들고 프로젝트를 진행할때,

프로젝트 시작시 git에 대해 레포에 대해 규칙을 만들면 헷갈리지 않고 편한다.

내가 프로젝트 할때 진행했던 규칙을 공유하고자 한다.

### Git Branch 규칙

------

- Master : 배포 시 유일하게 Team Leader가 merge할 권한을 갖는다. 배포 상황이 아니거나 member가 실수로 merge할 경우 능지처참형에 처한다.
- Dev : 개발이 이루어지는 공통 스트림
- feat/기능명 : 개발 목표 기능을 담아 브랜치를 생성하여 작업한다.

<details>
<summary>git branch name 설명</summary>
<div markdow>
  Master, Dev, feat/기능명 : branch 이름이다. Master는 배포전까지 건드리지 않는것으로 하고 포크 후 각자 branch이름은 feat/기능명으로 하고 push후 merge를 Dev로 해서 branch 관리를 한다.<br>
이때 branch를 터미널을 사용해서 git clone을 하지 않고 .zip 파일로 다운받을 경우 연결이 끊어서 merge가 되지 않는 경우가 발생할수도 있으니 귀찮더라도 터미널을 이용하여 다운받는것을 추천한다.<br>
왜 이렇게 branch 이름을 설정 하는지는 차후 git-flow에 대해 다시한번 블로깅 하겠다...만,<br>
간추려 말하면 효율적으로 branch를 관리하기 위함이며 엄밀히 말하면 프로젝트 팀 내에서 정한 규칙이다.</div>



### 첫 작업 가이드

------

1. Fork Repo
2. $ git clone (fork repo URL)
3. $ git remote add pair (main repo URL)
4. $ git remote -v
5. $ git add .
6. $ git commit -m 'first set'
7. $ git checkout -b dev
8. $ git pull pair dev

<details>
<summary>첫 작업 시 주의 사항</summary>
<div markdow>
1. Fork Repo : Repo를 내 github으로 포크를 먼저 해준다.<br>
2. git clone (fork repo URL) : 터미널을 사용하여 다운받을 장소(폴더)로 이동한뒤 해당 git clone을 한다. 이때 적어야할 URL은 퍼온 내 주소가 적힌 Fork가 된 주소이다.( 하..이거 존나 중요한데 접기안이라 강조가 안되네...나중에 안된다고 징징거리지 말고 여기에 니꺼주소 제대로 적었는지 확인하자 제발...)<br>
3. git remote add pair (main repo URL) : pair라는 이름으로 main repo URL을 연결된다. 이때 pair는 규칙으로 정한건데 한번 정한걸로 쭉 사용하게 되니 오타 나지 않도록 하자(pari 라던지...piar 라던지... 이거 한번 잘못하면 리모트를 다시하거나 프로젝트 끝날때 까지 저대로 사용해야되서 굉장히 불편한 일이 생긴다..)<br>
4. git remote -v : 연결된 remote 현황을 확인할수 있다.<br>
5 ~ 6. git add 와 git commit 하는 것이다. 정확한 정보는 git 사용법 자체에 대해서 설명하는 문서를 찾아보자.<br>
7. git checkout -b dev : 새로운 branch를 생성하게 된다. 이때 만들어진 repo는 1번에서 내 github으로 연결되어 있으니 push할때 내 github에도 repo가 생성되게 된다. 그리고 branch를 만들때의 코드를 그대로 가지고 오기 때문에 직전의 repo상태에 대해서 유의하자.(문제 많은 repo에서 새 branch를 만들고 그 위에 pull을 실행하면 conflict가 많이 나게되어 귀찮아 지는 경우가 생긴다.) 처음 실행하는 경우에는 코드가 없는 상태라 상관없긴하다.<br>
8. git pull pair dev: 생성된 새 branch에 pair로 연결된 repo의 dev 브랜치에서 pull을 당겨 온다.(pair가 되어 있지 않다면 piar자리에 URL을 직접 넣어도 되지만, 매번 URL을 직접 넣기가 힘들어 remote처리를 하는것이다.)<br>
  </div>



### 반복 작업 가이드

------

1. (작업 착수) $ git checkout -b feat/기능명
2. (작업 완료) $ git add . $ git commit -m '[서버/클라이언트/CSS] 작업내용'
3. (업로드) $ git push origin (브랜치명)
4. (로컬 초기화) $ git checkout dev
5. 깃허브에서 메인 repo로 PR을 전송합니다
6. 다음날 merge가 완료된 상태임을 확인합니다
7. (최신화) $ git pull pair dev
8. 작업 착수를 위해 1번부터 반복합니다

<details>
<summary>반복 작업 시 주의사항</summary>
<div markdow>
  반복작업시 주의할 점은 새 branch 생성하는것을 잊어먹지 말자. <br>  처음에는 적응이 되지 않아 conflict가 많이 발생할것이다.  <br> 조금만 주의하면 conflict로 인한 귀찮음을 줄일수 있다.
  </div>



깔끔한 github 환경을 유지하려고 하는데 쉽지가 않다.

더 큰 귀찮음을 방지하기 위해 처음에 조금만 귀찮고자 하는것이다.

처음할때는 왜 이렇게 하지 했는데,

알면알수록 개발자들을 위한 개발툴을 개발하신 분들이 존경스럽다.
