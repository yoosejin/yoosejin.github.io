---
layout: post
title:  "Connect to MySQL using Node.js"
author: Sejin
tags: mysql connect nodejs integration configuration install centos7 opensource
categories: NodeJS
sitemap:
 changefreq: daily
 priority: 1.0
---

## 개요
---
Node.js와 함께 어울릴 수 있는 Database는 많지만 그 중 MySQL을 사용하는 방법에 대한 내용입니다.
<br>
MySQL을 설치하는 방법에 대해서는 간단해서 별도로 기술하지는 않으나, 대략적인 순서는 MySQL Yum repository 등록 후, yum을 사용하여 MySQL을 설치하면 됩니다.
<br>
Yum repository 정보는 Google에서 "MySQL yum repo" 이런 키워드로 검색하면 Oracle에서 제공하는 공식 홈페이지에서 얻을 수 있습니다. 
<br>

혹시 이해가 어려운 부분에 대해서는 **sejin_yoo@naver.com**으로 메일주시면 답장 드리겠습니다.
<br>
<br>

## 설치 과정
---
<br>

`패키지 설치:`

```bash
# create directory
$ mkdir /app/node/web1
$ cd /app/node/web1

# npm initialize
$ npn init --yes

# initializing messages
Write to /app/node/web1/package.json:

{
  "name": "web1",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}

# 수동으로 입력 가능한 항목들
This utility will walk you through creating a package.json file.
It only covers the most common items, and tries to guess sensible defaults.

See `npm help init` for definitive documentation on these fields
and exactly what they do.

Use `npm install <pkg>` afterwards to install a package and
save it as a dependency in the package.json file.

Press ^C at any time to quit.
package name: (web1)
version: (1.0.0)
description: First Web App
entry point: (index.js)
test command:
git repository:
keywords:
author:
license: (ISC)
About to write to /app/node/web1/package.json:

{
  "name": "web1",
  "version": "1.0.0",
  "description": "First Web App",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC"
}


Is this OK? (yes)
// 모두 완료 후, package.json 파일이 생성됨.
[root@nodered-test web1]# ls
package.json

```
<br>

이제 MySQL 모듈을 설치합니다.
<br>

`Install Mudule: `

```bash
$ npm install express mysql

// 아래는 설치 메시지
npm notice created a lockfile as package-lock.json. You should commit this file.
npm WARN web1@1.0.0 No description
npm WARN web1@1.0.0 No repository field.

+ express@4.17.1
+ mysql@2.18.1
added 59 packages from 48 contributors and audited 59 packages in 7.336s
found 0 vulnerabilities
```
<br>

모듈이 설치되면 자동으로 생성됐던 package.json 파일을 수정해야 합니다.
<br>

`Edit package.json: `

```bash
$ vi package.json
# AS-IS
{
    "name": "web1",
    "version": "1.0.0",
    "description": "",
    "main": "index.js",
    "scripts": {
      "test": "echo \"Error: no test specified\" && exit 1"
    },
    "author": "",
    "license": "ISC"
  }

# TO-BE
{
    "name": "web1",
    "version": "1.0.0",
    "description": "",
    "main": "index.js",
    "scripts": {
      "start": "node index"
    },
    "dependencies": {
        "express": "^4.17.1",
        "mysql": "^2.18.1"
    }
  }

```
<br>

파일을 수정하고 npm 명령을 사용하여 구성에 문제가 없는지 테스트합니다.
<br>

```bash
$ npm start
```
<br>

구성에 문제가 없다고 판단되면 MySQL과의 연결을 포함한  라우팅을 작성합니다.
<br>
```bash
// index.js 를 변경
const express    = require('express');
const mysql      = require('mysql');
const dbconfig   = require('./config/database.js');
const connection = mysql.createConnection(dbconfig);

const app = express();

// configuration =========================
app.set('port', process.env.PORT || 3000);

app.get('/', (req, res) => {
  res.send('Root');
});

app.get('/users', (req, res) => {
  connection.query('SELECT * from Users', (error, rows) => {
    if (error) throw error;
    console.log('User info is: ', rows);
    res.send(rows);
  });
});

app.listen(app.get('port'), () => {
  console.log('Express server listening on port ' + app.get('port'));
});



// 여기서부터는 config/database.js
module.exports = {
    host     : 'localhost',
    user     : '< MySQL username >',
    password : '< MySQL password >',
    database : 'my_db'
  };
```
<br>
이제 데몬을 실행합니다.
<br>
```bash
$ forever start index.js

# 이후 http://server-ip:port/users를 호출하여 정상 확인
```

<br>
문의 사항이 있으시면 댓글이나 메일을 남겨주세요.
<br>
감사합니다.
