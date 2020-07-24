---
layout: post
title:  "네임서버 구성 (CentOS7)"
author: Sejin
tags: dns nameserver configuration install centos7 opensource
categories: Etc
sitemap:
 changefreq: daily
 priority: 1.0
---

## 개요
---
오늘 포스팅은 CentOS 7에서 네임 서버를 구성하는 내용입니다.

업무 용도로 사용할 도메인이 필요하여 구성하게 되었고 기록용이라 짧게 텍스트로 작성됩니다.

혹시 이해가 어려운 부분에 대해서는 **sejin_yoo@naver.com**으로 메일주시면 답장 드리겠습니다.
<br>
<br> 

## 설치 과정
---

<br>
순서는 패키지 설치 > 설정 > 클라이언트 테스트 입니다.
<br>


### 서버 설정
---

`패키지 설치:` 

```bash

$ yum install bind* -y

```
<br>
패키지 설치가 완료되면 설정을 시작합니다.
<br>
`Configuration: `

```bash
$ vi /etc/named.conf
# 내용 수정
options {
        listen-on port 53 { any; };
        listen-on-v6 port 53 { ::1; };
        directory       "/var/named";
        dump-file       "/var/named/data/cache_dump.db";
        statistics-file "/var/named/data/named_stats.txt";
        memstatistics-file "/var/named/data/named_mem_stats.txt";
        recursing-file  "/var/named/data/named.recursing";
        secroots-file   "/var/named/data/named.secroots";
        allow-query     { any; };


$ vi /etc/named.rfc1912.zones
# 내용 추가
zone "sejin.com" IN {
        type master;
        file "sejin.com.zone";
        allow-update { none; };


$ vi /var/named/sejin.com.zone
# 신규 파일 생성
$TTL 1D
@	IN SOA	@ sejin.com. (
					0	; serial
					1D	; refresh
					1H	; retry
					1W	; expire
					3H )	; minimum
	IN	NS	ns.sejin.com.
	IN	A	server-ip
ns	IN	A	server-ip
www	IN	A	server-ip


```
<br>

이제 설정이 완료되었습니다.
<br>
이제 네임 서버를 재시작하고 클라이언트에서 테스트를 진행합니다.
<br>
`Restart Service:`

```bash
# 서비스 재시작
$ systemctl restart named

# 서비스 확인
$ systemctl status named
```


### 클라이언트 설정
---
클라이언트의 네임 서버 설정을 추가합니다.
<br>

```bash
$ vi /etc/resolv.conf
nameserver server-ip
```

<br>
이제 네임 서버를 바라보도록 설정이 끝났고 실제 도메인 호출이 되는지 확인하면 됩니다.
<br>

```bash
$ nslookup sejin.com
```
<br>
<br>
문의 사항이 있으시면 댓글이나 메일로 남겨주세요
감사합니다.

