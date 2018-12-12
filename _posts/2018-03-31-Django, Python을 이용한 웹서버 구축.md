---
layout: post
title: Django, Python을 이용한 웹서버 구축
categories: work
tags: web
---
### 배경

지금 글을 쓰고 있는 github 사이트를 만들때 누군가 만들어놓은 이 스킨(?)을 사용했다. 코드에 대해 잘 몰랐지만 얼추 수정해서 사용했다. **CSS** 개념이 살짝있었지만 이해는 안(못)했다.

온습도 측정시스템 통계그래프를 WEB에서 뿌려주는 javascript(?)를 사용했었다. 코드에 대해 완벽히 이해하는 것은 아니었지만 그래도 얼추 통밥으로 수정해서 사용했다. 그 안에 CSS, javascript개념이 조금 있었다. 

**생활코딩**이라는 온라인 코드 학습 사이트(?)를통해서 HTML, CCS에 대한 학습을 하고 있었다. 그냥 시간날때 가볍게 듣는 정도였다. HTML은 흔히 우리가 알고있는 그거.. 그리고 CSS는 꾸며주는 부분을 따로 빼낸 부분이라는 정도?

저번주 영준과장님이 "자네 코딩할 생각없나?" 라며 장고를 공부하라고 했다. 그게 뭐냐고 하니 python을 사용한 WEB서버 같은거라나?...

그냥 이쯤되면 공부해보고 실제 만들어봐야겠다는 생각이 들었다.

### 무엇을 만들것인가?

일단은 영준과장이 추진중인 *로그원(?)*이라는 프로그램을 만들어야한다. 타팀에서 만든것을 보니 php기반의 웹페이지,서버를 통해서 각각의 서버의 로그를 긁어와 저장하고 이쁘게 뿌려주는것이다. 뭐 거기에 노하우를 더 넣어야겠지? 암튼 만들어 보자.

### 뭐부터 할까?

- 생활코딩 WEB2-CSS, WEB2-Javascript, WEB2-Python 다 듣기 (기초개념 잡기 좋다)
- Django가 무엇인지 개념잡기 (유튜브에서 찾아보자)
- Docker로 Linux&Django 컨터이너 설치하고 만들어보자
> 왜그런지 모르겠는데 최근에 Docker를 지웠다가 18 버젼을까니 갑자기 된다!! ㅜㅜ 기쁘다.
- 뭘 만들지 밑그림 그리기

### 생활코딩 WEB2-CSS 노트

- snippet: 템플릿, 자주쓰는 코드 같은거.. vscode 확장프로그램으로 HTML snippet을 설치했다.
- emmet
- vetur
- HTML Tag: grid, div, @media, link
- https://caniuse.com : html기술을 입력하면 각각의 웹브라우져에서 사용이 가능한지 알려준다 grid를 입력해보자
- WEB 브라우져의 캐싱 : CSS파일을 캐싱하여 빠르게 웹페이지를 보여준다.

### 생활코딩 WEB2-java 노트

- 동적으로 사용자와 상호반응
- script
- input : value, event (onclick, onchange, onkeydown)
- console : web브라우저 검사 클릭 -> console -> javascript 실행 (Elements에서 esc를 클릭하면 console이랑 같이 뜬다)