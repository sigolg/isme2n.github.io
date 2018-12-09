---
layout: post
title: "VSCODE에 Github 연동하기"
categories: setup
tags: setup
---

### 1. VSCODE Github 연동하기
- vscode 실행시 git 설치하라는 알림창 발생
- 해당 url접속 후 git 설치
- ctrl+shift+p
- git: initialize repository
- git clone -> https://github.com/sigolg/sigolg.github.io
#### commit 시도시 실패 (*** Please tell me who you are.)
- git config --global user.name "sigolg"
- git config --global user.emai "wee8484@gmail.com"
#### push 시도시 로그인 창이 발생한다.
- login 하면 정상 push 가능

