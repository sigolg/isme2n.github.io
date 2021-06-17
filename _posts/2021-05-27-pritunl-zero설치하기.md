---
layout: post
title: "pritunl-zero 설치하기"
categories: setup
tags: setup
---

### 설치 배경
어플라이언스 어드민 페이지 2FA 인증을 위해

### 유사한 시스템
- Identity Aware Proxy
- pritunl-zero
- pomerium

### 필요한 부분
- 2FA 인증 연동
- LDAP 연동
- IP제한
- 계정별 role 권한
- 어플라이언스 admin 별도 DNS 부여 필요
- 추후 소스코드 수정 이후 docker로 다시 묶기

### 사전학습

#### MongoDB
- 현존하는 NoSQL 데이터베이스 중 인지도 1위
- NoSQL이란? 기존의 RDBMS의 한계를 극복하기 위해 만들어진 새로운 형태의 데이터저장소. 관계형 DB가 아니므로 고정된 스키마 및 join이 존재하지 않다.
- Document Oriented 데이터베이스: 한개 이상의 키-벨류쌍, json같은 형식
- Collection: 별도의 스키마가 없는 도큐먼트의 동적인 세트

#### Json vs Bson
- https://nahyungmin.tistory.com/100

#### go struct to JSON and BSON

#### freeradius 프로토콜 

#### go 설치하기
- go 과거 버전을 설치하려 하였으나 brew에는 최신버전만 있음
- pritunl 홈페이지에 있는 1.15.6버전 설치명령어를 그대로 수행하였는데 go 명령어가 수행안됨, mac용 패키지가 아니어서 그랬음
- go.1.15.6.darwin-amd64
  ```bash
    # ~/.bash_profile
  export GOPATH="$HOME/go"
  export PATH="/usr/local/go/bin:$PATH"
  ```

#### mongodb 설치하기
- brew tap mongodb/brew
- brew install mongodb-community@4.2
- brew services start mongodb-community@4.4

#### github에서 pritunl-zero 받아서 설치하기
docker 이미지 또는 yum으로 설치하는 것 말고 소스코드를 이용하여 빌드 하는 방법을 알아야한다. 왜냐하면 소스코드 수정이 필요한 작업이기 때문에
- centos 등 특정 도커 이미지 다운
- 소스코드로 설치
- 나만의 도커 이미지 생성

pritunl이 제대로 설치 안된이유: go 버전이 너무 최신이어서 제대로 설치 되지 않음, mac에서 설치하므로 brew를 통해서 특정 버전 설치가 필요해 보임

#### react란?
- 관련 링크: https://react.vlpt.us/

#### 타입스크립트
1. IDE 를 더욱 더 적극적으로 활용 (자동완성, 타입확인)
TypeScript 를 사용하면 자동완성이 굉장히 잘됩니다. 함수를 사용 할 때 해당 함수가 어떤 파라미터를 필요로 하는지, 그리고 어떤 값을 반환하는지 코드를 따로 열어보지 않아도 알 수 있습니다. 추가적으로, 리액트 컴포넌트의 경우 해당 컴포넌트를 사용하게 될 때 props 에는 무엇을 전달해줘야하는지, JSX 를 작성하는 과정에서 바로 알 수 있으며, 컴포넌트 내부에서도 자신의 props 에 어떤 값이 있으며, state 에 어떤 값이 있는지 알 수 있습니다. 또한, 리덕스와 함께 사용하게 될 때에도 스토어 안에 어떤 상태가 들어있는지 바로 조회가 가능해서 굉장히 편리합니다.

2. 실수방지
함수, 컴포넌트 등의 타입 추론이 되다보니, 만약에 우리가 사소한 오타를 만들면 코드를 실행하지 않더라도 IDE 상에서 바로 알 수 있게 됩니다. 그리고, 예를 들어 null 이나 undefined 일 수도 있는 값의 내부 값 혹은 함수를 호출한다면 (예: 배열의 내장함수) 사전에 null 체킹을 하지 않으면 오류를 띄우므로 null 체킹도 확실하게 할 수 있게 됩니다.
  
props