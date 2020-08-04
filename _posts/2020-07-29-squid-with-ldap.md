---
layout: post
title:  "Configuration Squid with LDAP (CentOS7)"
author: Sejin
tags: squid integration ldap openldap user management configuration install centos7 opensource
categories: Etc
sitemap:
 changefreq: daily
 priority: 1.0
---

## 개요
---
`참고 링크 :`
<br>
[NameServer 설치하기](https://yoosejin.github.io/etc/2020/07/24/How-to-install-dns.html)
[OpenLDAP 설치하기](https://yoosejin.github.io/etc/2020/07/26/How-to-install-openldap.html)


<br>
오늘 포스팅은 이전 포스팅에서 설치했던 DNS, LDAP을 활용하여 인증된 유저만 특정 사이트에 접속할 수 있도록 하는 내용입니다.
<br>
기업 내에 같은 임직원이라도 보안 정책에 따라 접근할 수 있는 서비스가 다를 수 있습니다.
<br>
이런 경우에 사용하면 좋을 것 같습니다.
<br>
혹시 이해가 어려운 부분에 대해서는 **sejin_yoo@naver.com**으로 메일주시면 답장 드리겠습니다.
<br>
<br>

## 설치 과정
---
<br>

`패키지 설치:`

```bash
# install squid
$ yum install squid -y

```
<br>

`Squid 설정:`

```bash
# Configuration squid
$ vi /etc/squid/squid.conf

# Ldap 인증을 받도록 하는 설정입니다. (LDAP Authentication)
auth_param basic program /usr/lib64/squid/basic_ldap_auth -b "dc=sejin,dc=com" -f "uid=%s" -h sejin.com

# 특정 사이트에 대해서만 접근이 가능하도록 하기 위해 도메인 정보를 담은 파일을 생성하여 등록하는 설정입니다. (Setup whitelist with squid proxy)
acl domain_whitelist dstdomain "/etc/squid/domain_whitelist.txt"

# proxy를 이용할 때 ldap 유저로 인증된 사람만 허용하는 설정입니다. (Integrate LDAP)
acl ldap-auth proxy_auth REQUIRED

## 접근을 제한하는 설정입니다. (Restrict squid access)
http_access deny !Safe_ports !ldap-auth
http_access deny CONNECT !SSL_ports !ldap-auth
http_access deny manager
http_access deny all

## Ldap 유저와 whitelist에 등록된 도메인만 허용하도록 하는 설정입니다.
http_access allow !ldap-auth
http_access allow domain_whitelist

## 로컬 네트워크 통신을 제한하는 설정입니다. (기존에는 주석이 없으나 주석 설정을 추가함)
#http_access allow localnet
```
<br>
이제 위에서 설정한 화이트 리스트 파일을 작성합니다.
<br>
`도메인 화이트리스트 파일 생성:`

```bash
# add domain_whitelist.txt
$ vi /etc/squid/domain_whitelist.txt

.naver.com
google.com
# 도메인 앞에 .이 붙으면 *.naver.com과 동일한 효과가 있습니다. (Enable wildcard domains)
```
<br>
이제 설정은 완료되었고 방화벽에 Squid 포트를 등록합니다.
<br>
`방화벽 설정 & 서비스 시작:`

```bash
$ firewall-cmd --permanent --add-port=3128/tcp
$ firewall-cmd --reload

$ systemctl restart squid
$ systemctl enable squid
$ systemctl status squid

````
<br>
이제 모든 설정이 완료되었습니다.
<br>
이제 curl을 활용하여 로컬에서 잘 동작하는지 확인합니다.
<br>

`Test: `

```bash
$ curl -O -L "naver.com" -x "username:password@squid-server-ip:3128"
```
<br>
LDAP 계정 정보를 넣어 인증이 정상적으로 진행되면 테스트는 완료입니다.
<br>
<br>

## Browser Test (Firefox)
---
<br>
브라우저 테스트를 진행하려면 아래 순서대로 진행합니다. (Firefox 기준)
<br>


<figure>
  <img data-action="zoom" src='{{ "/assets/img/squid1.PNG" | relative_url }}' alt='absolute'>
  <figcaption>Firefox 설정 화면1</figcaption>
</figure>

<br>
<br>
<figure>
  <img data-action="zoom" src='{{ "/assets/img/squid2.PNG" | relative_url }}' alt='absolute'>
  <figcaption>Firefox 설정 화면2</figcaption>
</figure>
<br>
<br>
<figure>
  <img data-action="zoom" src='{{ "/assets/img/squid3.PNG" | relative_url }}' alt='absolute'>
  <figcaption>Firefox 설정 화면3</figcaption>
</figure>
<br>
<br>
설정이 완료되면 모든 페이지 접근시 Squid Proxy 설정을 적용받아 LDAP 인증을 요구하게 됩니다.
<br>
이미지에서 확인할 수 있듯이 Whitelist를 별도로 적용하시면 됩니다.
<br>
<br>
문의사항은 댓글이나 이메일로 남겨주세요.
<br>
감사합니다.
