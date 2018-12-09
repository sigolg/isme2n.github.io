---
layout: post
title: "Ubuntu 16.04에서 ATOM 설치 및 Github 연동 하기"
categories: setup
tags: setup
---

### 1. ATOM 설치
- atom.io 접속
- deb 파일 다운로드
- 더블클릭하여 설치


### 2. ATOM git 설정하기
- atom 실행
- package install 이동
- git clone 설치
- git-plus 설치
- git clone 설정에서 Default 폴더 변경
- ctrl+shift+p
- git clone -> https://github.com/sigolg/sigolg.github.io
- git init

- 터미널 open
- sudo apt-get install git
- git config --global --list
- git config --global user.name "sigolg"
- git config --global user.emai "wee8484@gmail.com"

- push가 되지 않았는데 --> 일반 터미널에서 git push를 하고 ID/PW 한번 넣은 이후로는 정사적으로 push가 되고 있다....

### 3. ATOM 기타 설정하기
- `ctrl+,` 설정창으로 이동
- editor > Scroll past end 체크 : 에디터가 마지막 줄이 되어도 스크롤을 더 내릴 수 있게 함.
- editor > soft wrap : 자동 줄 바꿈
- Markdonw-preview-plus 설치
