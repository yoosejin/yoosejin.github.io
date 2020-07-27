---
layout: post
title:  "OpenLDAP 설치 (CentOS7)"
author: Sejin
tags: ldap openldap user management configuration install centos7 opensource
categories: Etc
sitemap:
 changefreq: daily
 priority: 1.0
---

## 개요
---
오늘 포스팅은 CentOS 7에서 OpenLDAP을 설치하고 간략한 설정하는 내용입니다.
<br>
목표는 LDAP 유저를 사용해서 Squid Proxy로 웹 브라우저 접속을 제한하는 것입니다.
<br>
혹시 이해가 어려운 부분에 대해서는 **sejin_yoo@naver.com**으로 메일주시면 답장 드리겠습니다.
<br>
<br>

## 설치 과정
---

`패키지 설치:`

```bash

`서비스 실행:`

```bash
$ systemctl start slapd

$ systemctl enable slapd

# 서비스 확인
$ netstat -nlp | grep 389
OR
$ systemctl status slapd

```
<br>

서비스 실행이 확인되면 설정을 진행합니다.

<br>
`LDAP Configuration: `

```bash
# 관리자 비밀번호 생성
$ slappasswd
New password:
Re-enter new password:
# LDAP (olcSuffix, olcRootDN, olcRootPW) 설정
$ vi Database.ldif
# 파일 생성하는 위치는 상관이 없습니다.
dn: olcDatabase={2}hdb,cn=config
changetype: modify
replace: olcSuffix
olcSuffix: dc=sejin,dc=com

dn: olcDatabase={2}hdb,cn=config
changetype: modify
replace: olcRootDN
olcRootDN: cn=admin,dc=sejin,dc=com

dn: olcDatabase={2}hdb,cn=config
changetype: modify
replace: olcRootPW
olcRootPW: {SSHA}quOmUTsXlYIBJQXpB7SQ2RZukGWTRzpO
# 파일 끝


# LDAP 서버에 설정 반영
$ ldapmodify -Y EXTERNAL -H ldapi:/// -f Database.ldif


# LDAPS 적용
# 만약 SSL 통신이 필요하다면 아래 명령어를 통해 인증서를 생성하고 인증서를 반영시킬 수 있습니다.
# 기존에 사용하는 인증서가 있다면 openssl 명령어는 제외하고 인증서 경로에 파일을 복사하면 됩니다.
$ openssl req -new -x509 -nodes -out \
/etc/openldap/certs/ldap.sejin.com.cert \
-keyout /etc/openldap/certs/ldap.sejin.com.key \
-days 365

# 소유 권한 변경
$ chown -R ldap:ldap /etc/openldap/certs


$ vi certs.ldif
# 파일 시작
dn: cn=config
changetype: modify
replace: olcTLSCertificateFile
olcTLSCertificateFile: /etc/openldap/certs/ldap.sejin.com.cert

dn: cn=config
changetype: modify
replace: olcTLSCertificateKeyFile
olcTLSCertificateKeyFile: /etc/openldap/certs/ldap.sejin.com.key
# 파일 끝


# 인증서 반영
$ ldapmodify -Y EXTERNAL -H ldapi:/// -f certs.ldif


# 구성 테스트
$ slaptest -u


```
<br>

<br>
### LDAP Monitor 구성
---
<br>
이제 모니터링 인터페이스 설정을 진행할텐데 역할은 LDAP Client로 접속했을 때 모니터링 인터페이스에서 제공하는 정보에 대해 액세스 할 수 있습니다.
<br>

`LDAP Monitor Configuration: `

```bash
$ vi monitor.ldif
# 파일 시작
dn: olcDatabase={1}monitor,cn=config
changetype: modify
replace: olcAccess
olcAccess: {0}to * by dn.base="gidNumber=0+uidNumber=0,cn=peercred,cn=external, cn=auth" read by dn.base="cn=admin,dc=sejin,dc=com" read by * none
# 파일 끝

# 설정 반영
$ ldapmodify -Y EXTERNAL  -H ldapi:/// -f monitor.ldif
```
<br>
<br>

### LDAP Database 구성
---
<br>
이제 OpenLDAP에서 사용되는 Database의 설정을 진행하겠습니다.

<br>
`LDAP Database Configuration: `

```bash
# DB 샘플 파일 복사
$ cp /usr/share/openldap-servers/DB_CONFIG.example /var/lib/ldap/DB_CONFIG

# 소유 권한 변경
$ chown -R ldap:ldap /var/lib/ldap

# LDAP Schema 추가
$ ldapadd -Y EXTERNAL -H ldapi:/// -f /etc/openldap/schema/cosine.ldif
$ ldapadd -Y EXTERNAL -H ldapi:/// -f /etc/openldap/schema/nis.ldif
$ ldapadd -Y EXTERNAL -H ldapi:/// -f /etc/openldap/schema/inetorgperson.ldif


# 도메인 설정 파일 생성

$ vi base.ldif
# 파일 시작
dn: dc=sejin,dc=com
dc: field
objectClass: top
objectClass: domain

dn: cn=admin,dc=sejin,dc=com
objectClass: organizationalRole
cn: ldapadm
description: LDAP Administrator

dn: ou=People,dc=sejin,dc=com
objectClass: organizationalUnit
ou: People

dn: ou=Group,dc=sejin,dc=com
objectClass: organizationalUnit
ou: Group
# 파일 끝


# 설정 반영
$ ldapadd -x -W -D "cn=admin,dc=sejin,dc=com" -f base.ldif

# 위 명령어 실행 시 비밀번호 입력창이 나오는데 처음 설정했던 관리자 비밀번호를 입력하시면 됩니다.

```

<br>
문의사항은 댓글이나 메일로 남겨주세요.
<br>
감사합니다.
<br>

