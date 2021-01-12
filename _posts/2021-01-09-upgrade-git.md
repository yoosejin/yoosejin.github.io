---
layout: post
title:  "Git 2.29 설치(Install Git 2.29.X)"
author: Sejin
tags: git upgrade gitlab
categories: Etc
sitemap:
 changefreq: daily
 priority: 1.0
---

## 개요
---
안녕하세요, <br>
저는 CentOS를 즐겨 사용하는데(최근 들어서는 Amazon Linux 2를 많이 사용하지만..) 내장된 Git 버전이 낮아 최신 버전의 Gitlab 설치가 되지 않더라구요. <br>
그래서 상위 버전의 Git 설치 포스팅을 가져왔습니다. <br>

Test OS : CentOS Linux release 7.9.2009 (Core)
<br>

[Git 2.29 버전 설치 영상]이 있으니 확인 해보세요

[Git 2.29 버전 설치 영상]: https://youtu.be/JJ9Pqg9GPjI


<br>


<br>

## 현재 버전 확인
---
<br>
```bash
// check verseion
# git --version
git version 1.8.3.1

```
<br>
<br>
## Git 2.29 설치
---
<br>

```bash
// Download source file
wget https://github.com/git/git/archive/v2.29.0.tar.gz
 
// Extract file
tar xvfz v2.29.0.tar.gz

// Change directory
cd git-2.29.0

// make configure
make configure

./configure --prefix=/usr

make

make install

git --version
git version 2.29.0


```
<br>
<br>

## 설치중에 발생할 수 있는 오류
---
간단한 오류들이지만 필요하신 분들을 위해 추가합니다.
<br>
`make configure Command Error: `
<br>
```bash
// Error messages
/bin/sh: autoconf: command not found
make: *** [configure] 오류 127

// Workaround
autoconf 패키지가 없어 발생한 오류입니다.

yum install -y autoconf

```
<br>
`./configure --prefix=/usr Command Error: `
<br>
```bash
// Error messages
configure: Setting lib to 'lib' (the default)
configure: Will try -pthread then -lpthread to enable POSIX Threads.
configure: CHECKS for site configuration
checking for gcc... no
checking for cc... no
checking for cl.exe... no
configure: error: in `/root/git-2.29.0':
configure: error: no acceptable C compiler found in $PATH
See `config.log' for more details

// Workaround
gcc 패키지가 없어 발생한 문제입니다.

yum install -y gcc

```
<br>

`make Command Error: `
<br>
```bash
// Error messages
   * new build flags
    CC fuzz-commit-graph.o
In file included from object-store.h:4:0,
                 from commit-graph.h:5,
                 from fuzz-commit-graph.c:1:
cache.h:21:18: fatal error: zlib.h: 그런 파일이나 디렉터리가 없습니다
 #include <zlib.h>
                  ^
compilation terminated.
make: *** [fuzz-commit-graph.o] 오류 1

// Workaround
zlib-devel 패키지가 없어 발생한 문제입니다.

yum install -y zlib-devel

```

<br>

<br>

<br>
문의는 댓글이나 메일로 남겨주세요. <br>
감사합니다.

