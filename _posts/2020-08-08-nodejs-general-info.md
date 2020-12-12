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
Node.js를 공부하면서 알게된 잡다한 내용들을 담았습니다.
<br>

<br>

## Node.js란?
---
Node.js는 서버사이드 자바스크립트고, 구글의 자바스크립트 엔진인 V8을 기반으로 구성되었습니다.
또, 자바스크립트 표준 라이브러리 프로젝트인 CommonJS의 스펙을 따라가고 있습니다.
<br>
<br>
## 예약어
---
키워드 혹은 예약어들은 제어, 조작 부분으로 사용하기 위해 사전에 정의해둔 내용입니다.
미리 예약된 키워드, 예약어들은 변수, 함수 등에 정의해서 사용할 수 없기 때문에 유의해야 합니다.
<br>
정의된 키워드, 예약어에 대한 예시들입니다.
<br>
```javascript
break
case
catch
continue
default
delete
do
else
false
finally
for
function
if
in
instanceof
new
null
return
switch
this
throw
try
true
typeof
var
void
while
with 
```
<br>
<br>
## 조건 연산자 사용 방법
---
다들 아시겠지만 조건 연산자는 조건을 비교하여 참인 경우와 거짓인 경우에 대해 실행을 다르게 하는 방법입니다.
이에 대해 간단한 사용 예시를 아래에서 확인해보도록 하겠습니다.
<br>

```javascript
var a = 5;
var b = 1;
var c = 3, d = 4;
 
(a > b)? (console.log("a가 더 크다")):(console.log("b가 더 크다"));
// 위 문구 해석은 (조건)? A:B 형태의 조건 연산자로 조건이 참일 때는 A를 실행하고 거짓일 때는 B를 실행하라는 것입니다.
```
<br>
<br>
## 스코프 체인(Scope Chain) 개념
-----------------------------------------------------------------------------------------------
아래 사용 예시를 기준으로 설명하겠습니다.
 - inner 함수가 가장 내부 함수고, 이 함수의 경우 자신을 선언한 바깥 함수 내에 있는 지역 변수도 사용 가능합니다. 
   (당연하게도 전역변수도 사용 가능하며, 변수를 찾을 때는 자신의 함수 내부 > 자신을 포함한 외부 함수 > 전역 변수 순으로 찾게 됩니다.)
 - outer 함수는 inner 함수 내부 변수를 사용할 수 없다. (안쪽에서 밖으로만 가능합니다.)
 - 체인처럼 꼬리를 물고 상위 스코프를 참조한다고 해서 스코프 체인이라고 불리고 있습니다.
<br>
<br>

```javascript
사용 예시)
var a = 2;
 
function outer() {
	var b = 5;
	console.log(a); // 2
	
	function inner() {
		var c = 3;
		console.log(b);
		console.log(a); 
	}
	
	inner();  // 5 2
}
outer();
 
console.log(c);  // not defined
```
<br>
<br>
## 호이스팅(Hoisting)
---
호이스팅의 개념은 함수 안에서 변수를 선언할 때 어떤 위치에서 선언이 되던지 함수의 시작 위치에서 선언한 것처럼 만들어주는 것입니다.
단, 선언 부분만 시작 위치 선언으로 만들어주고 값은 실제 선언 위치에서 반영이 됩니다. (사용 예시1 참고)
<br>
<br>
 
```javascript
사용 예시1) // 변수 호이스팅
function test1() {
    // 사실 이 위치에 var a; 라고 정의된 것과 같은 효과를 주는 것입니다.
	console.log(a);  // 이 때, a라는 변수가 없음에도 불구하고 에러가 떨어지지 않고 값이 정의되지 않았다고 메시지가 나옵니다.
	var a = 1;
	console.log(a);  // 1
}
test1();
 
 
사용 예시2) // 함수 호이스팅이고 에러는 없습니다.
test2();
 
function test2() {
	console.log('log');
}
 
 
사용 예시3) // 함수 호이스팅이고 에러가 발생합니다.
// 에러가 발생하는 이유는 실제 이 코드의 경우 현재 위치에 var test3;이라고 정의된 것과 같기 때문에 함수가 아닌 변수로 선언이 되어 있는 상태라 함수가 아니라고 에러를 표시하는 것입니다.
test3();  
 
var test3 = function() {
	console.log('log');
};
```
<br>
<br>

## 마치며
새로운 언어를 접하면 항상 사용 방법이 어색하고 어렵게 느껴지는 것 같습니다.
그래도 하나하나 찾아서 해보면 재미도 있고 장문의 코딩을 해야 하는 개발자들의 노고도 조금 알게되는 것 같고 더 많이 알고 싶은 욕구도 생겨 기분이 좋습니다.
<br>
사실 이번 포스팅에 넣어야 할 내용이 많은데 시간이 부족해 <b>2편</b>으로 추가하도록 하겠습니다.
<br>
감사합니다.
