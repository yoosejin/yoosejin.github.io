---
layout: post
title:  "Node.js에 관한 잡다한 내용-1"
author: Sejin
tags: nodejs development
categories: NodeJS
sitemap:
 changefreq: daily
 priority: 1.0
---

## 개요
---
[Node.js에 관한 잡다한 내용1]에 이은 2번째 시리즈물입니다.

[Node.js에 관한 잡다한 내용1]: https://yoosejin.github.io/nodejs/2020/08/08/nodejs-general-info.html
<br>

<br>

## Socket.io에 대한 설명
---
웹소켓을 사용하려 하다보니 웹소켓이 HTML5의 기술이라 오래된 버전의 웹 브라우저가 지원하지 않는다는 사실을 알게 되었습니다.
<br>
그래서 찾아보니 node.js 기반으로 만들어진 Socket.io라는기술을 확인했는데 Socket.io는 현존하는 거의 모든 웹 브라우저와 모바일 장치를 지원하는 실시간 웹 어플리케이션 라이브러리입니다.
<br>
100% 자바스크립트로 작성되어 있고, 대부분의 웹 기술을 사용할 수 있습니다.
<br>
사용해보면서 가장 좋았던 점은 웹 브라우저와 서버의 종류와 버전을 스스로 파악하여 가장 적합한 기술을 선택하여 사용하도록 되어 있던 점입니다.
<br>
<br>
## 설치 방법과 사용 가능한 기능들
---
설치는 npm으로 간단하게 진행되고 설치 후 사용가능한 기능들에 대해 나열합니다.
<br>
```javascript
io.to(socket.id).emit
// 서버가 해당 socket id에만 이벤트를 전달. (채팅 기준으로는 변경을 요청한 유저만 변경되도록)

io.on('connection'), 함수() : 
// io.on에서 선언되는 connection은 기본 이벤트로 사용자가 웹사이트를 열면 자동으로 발생하는 이벤트

socket.emit
// socket.emit은 이벤트를 발생시키는 함수. 서버에서 이벤트를 발생하면 클라이언트 페이지의 이벤트 리스너에서 처리

socket.on
// client login 이벤트를 만들어서 콘솔 창에 해당 데이터를 표기

```
<br>
<br>
## 간단한 사용 예시
---
위에 나열한 기능들을 활용해보는 예시들입니다.
<br>

```javascript
//server.js
// connection 안에 이벤트를 작성할 때 socket.on('이벤트 명', 함수) 형식으로 작성

var app = require('http').createServer(handler),
    io = require('socket.io').listen(app),          // socket.io 선언
    fs = require('fs');

app.listen(3000);

function handler (req, res) {
	fs.readFile('index.html', function (err, data) {
		if (err) {
			res.writeHead(500);
			return res.end('Error loading index.html');
		}
		res.writeHead(200);
		res.end(data);
	});
}

io.on('connection', function (socket) {
	socket.emit('news', { serverData : "서버 작동" });
	socket.on('client login', function (data) {
		console.log(data);
	});
		
	socket.on('disconnect', function(){
		console.log('접속이 종료되었습니다.');
	});

});
 
```
<br>
<br>

## 마치며
오랜만에 진행한 포스팅입니다.
<br>
새로운 환경에 적응하는 시간이 필요했다고 생각도 들지만 지나고 보니 핑계인 것 같네요.
<br>
2020년이 가기전에 새로운 포스팅을 하고 싶었는데 작은 목표를 이뤄 기분이 좋습니다.
<br>
포스팅을 할 내용은 꾸준히 정리하여 어느새 20개 이상의 포스팅 내용이 확보되어 있는데 앞으로 꾸준히 업데이트 하도록 노력해야겠네요.
<br>

### 시리즈는 계속됩니다.
너무 늦지않게 <b>3편</b>으로 돌아오도록 하겠습니다.
<br>
감사합니다.

[Node.js에 관한 잡다한 내용1]: https://yoosejin.github.io/nodejs/2020/08/08/nodejs-general-info.html
