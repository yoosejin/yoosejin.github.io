---
layout: post
title:  "Node.js 개발 환경 구축"
author: Sejin
tags: node nodejs configuration install ubuntu ubuntu18 opensource
categories: NodeJS
sitemap:
 changefreq: daily
 priority: 1.0
---

## 개요
---
오늘 포스팅은 Ubuntu 서버에 Node.js를 설치하는 내용입니다.
<br>
구글 확장 플러그인 개발 건으로 Javascript를 사용하게 되었는데 추후에도 꾸준히 포스팅할 것 같습니다.
<br>
혹시 이해가 어려운 부분에 대해서는 **sejin_yoo@naver.com**으로 메일주시면 답장 드리겠습니다.
<br>
<br>

## 설치 과정
---
설치는 Ubuntu 18.04에서 진행하였습니다.
<br>

```bash

`패키지 업그레이드:  `

```bash

# 업데이트
$ apt update

# 업데이트 리스트 확인
$ apt list --upgradable

# 업그레이드
# apt -y upgrade


```
<br>
패키지 업그레이드가 끝나면 node.js를 설치합니다.
<br>
`Install Node.js: `

```bash
# add node.js repo
$ apt -y install curl dirmngr apt-transport-https lsb-release ca-certificates

$ curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -


# install node.js
$ apt install nodejs -y

# install dependency packages
$ apt install gcc g++ make -y

```
<br>
설치가 완료되면 설치된 버전이 맞는지 확인합니다.
<br>
`버전 확인: `

```bash
$ node --version
v12.18.3

$ npm -- version
6.14.6
```
<br>
<br>
개발의 시작은 `Hello World`와 함께이기 때문에 간단하게 테스트를 진행하도록 하겠습니다.
<br>

`Test Node.js:  `

```javascript
$ mkdir /app/node/test -p

$ cd /app/node/test

$ vi hello.js
# 파일 시작
const http = require('http');

const hostname = '127.0.0.1';
const port = 3000;

const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('Hello, World!\n');
});

server.listen(port, hostname, () => {
  console.log(`Server running at http://${hostname}:${port}/`);
});
# 파일 끝


# 노드 실행
$ node hello.js

# 웹 페이지 확인
$ curl -v http://localhost:3000
```

<br>
<br>

문의사항은 댓글이나 이메일을 남겨주세요.
<br>
감사합니다.
