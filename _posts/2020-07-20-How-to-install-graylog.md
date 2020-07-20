---
layout: post
title:  "GrayLog 설치 방법 (CentOS 7.8)"
author: Sejin
tags: elasticsearch graylog yum mongodb configuration install centos7 opensource
categories: OpenSource
sitemap:
 changefreq: daily
 priority: 1.0
---

## 개요
---
서버 로그 수집 도구인 GrayLog 설치 방법에 대한 포스팅입니다.

이해가 어려운 부분에 대해서는 **sejin_yoo@naver.com**으로 메일주시면 답장 드리겠습니다.
<br>
<br> 
## 설치 환경
---
Virtual Machine : `Oracle VirtualBox 6.1`
  
OS : `CentOS 7.8 x86_64`
  
Network : 외부 통신 가능

기타 : 다른 테스트로 yum update가 한번 진행된 서버입니다. 아래 커널 버전을 참고하셔서 진행하시면 좋을 것 같습니다.

Kernel Version : Linux graylog-test 3.10.0-1127.el7.x86_64 #1 SMP Tue Mar 31 23:36:51 UTC 2020 x86_64 x86_64 x86_64 GNU/Linux
  
  <br>
<br>
## 설치 과정
---
  
<br>
먼저 필요 패키지를 설치합니다.
<br>
<br>

`Install packages:` 

```bash
# JDK Version : 1.8.0.252
$ yum install java-1.8.0-openjdk -y

```

<br>
이제 MongoDB를 설치하기 위해 yum repository를 추가합니다.
<br>

`Create repo file:`

```bash
$ vi /etc/yum.repos.d/mongodb.repo
# 아래는 파일 내용입니다.

[mongodb]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/4.0/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-4.0.asc
# 파일 끝


```

<br>
이제 MongoDB를 설치합니다.
<br>

`Install mongodb:`

```bash
# install
$ yum install mongodb-org -y

# systemd에 서비스 등록 과정
$ systemctl daemon-reload

# 재부팅 시 서비스 자동 실행
$ systemctl enable mongod.service

# 서비스 실행
$ systemctl start mongod.service

# 서비스 확인
$ systemctl --type=service --state=active | grep mongod 
OR
$ systemctl status mongod


```

<br>

<br>

### Install Elasticsearch
---

<br>
GrayLog에서 사용되는 검색 엔진을 설치합니다.

Elasticsearch 개념 관련해서는 별도로 포스팅하기로 하고 간단하게 장점만 설명 드리겠습니다.

제가 생각하는 장점은 크게 5가지입니다. 실제 장,단점으로 불릴만한 내용은 많지만 내용이 길어지기 때문에 별도의 포스팅으로 안내드리겠습니다.
1. 오픈소스 (비용 절감)
2. Full text search (내용 전체를 색인하기 때문에 특정 단어가 포함된 문서를 검색하게 됩니다.)
3. 통계 기능
4. 확장성 (분산 시스템 구성이 가능하고 이로써 가용량이 많아집니다.)
5. 역색인 (빠른 검색을 위한 기능인데 간단하게 설명드리면 문서에서 고유한 단어 목록을 만들고 각 단어가 있는 전체 문서를 식별하게 됩니다.)

<br>

`Create repo file:'`
```bash
# download gpg key
$ rpm --import https://artifacts.elastic.co/GPG-KEY-elasticsearch


$ vi /etc/yum.repos.d/elasticsearch.repo
# 아래는 파일 내용
[elasticsearch-6.x]
name=Elasticsearch repository for 6.x packages
baseurl=https://artifacts.elastic.co/packages/oss-6.x/yum
gpgcheck=1
gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
enabled=1
autorefresh=1
type=rpm-md
# 파일 끝

```

<br>

`Install ElasticSearch: `

```bash
# install elasticsearch
$ yum install elasticsearch-oss -y

# systemd에 서비스 등록
$ systemctl daemon-reload

# 서비스 시작
$ systemctl start elasticsearch

# 재부팅 시 서비스 자동 시작
$ systemctl enable elasticsearch```

<br>

### GrayLog 설치

`Install GrayLog:`

```bash

# Download repo
$ rpm -Uvh https://packages.graylog2.org/repo/packages/graylog-3.3-repository_latest.rpm

# Install GrayLog
$ yum install graylog-server graylog-enterprise-plugins graylog-integrations-plugins graylog-enterprise-integrations-plugins -y

# Install Password Generator
$ yum install pwgen -y

# create root password sha2
$ echo -n "Enter Password: " && head -1 </dev/stdin | tr -d '\n' | sha256sum | cut -d" " -f1
Enter Password: sejin
6ed3afe25f53c78a536f8620d49e7d1c69beea930a4e1f6ec404cd3720

# create password_secret
$ pwgen -N 1 -s 96
zKvJsVuRBqbrze2SzgHtQISKh5UryYE5kt8naFGp8hZnNMtueC8jwcScz77mCzJh0sJy90W71Lev0cNCiMlY


```

<br>

이제 위에서 생성한 비밀번호 해시값을 포함하여 설정을 진행합니다.
<br>

`Configuration: `

```bash

$ vi /etc/graylog/server/server.conf
# 아래는 파일 내용 중 제가 수정한 부분입니다.
password_secret = zKvJsVuRBqbrze2SzgHtQISKh5UryYE5kt8naFGp8hZnNMtueC8jwcScz77mCzJh0sJy90W71Lev0cNC

root_password_sha2 = 6ed3afe25f53c78a536f8620d49e7d1c69beea930a4e1f6ec404c

root_timezone = Asia/Seoul

http_bind_address = http://192.168.99.104:9000

http_publish_uri = http://192.168.99.104:9000/
http_external_uri = http://192.168.99.104:9000/
# 파일 끝


```

<br>

<br>

`Start Service: `

```bash
$ systemctl daemon-reload

$ systemctl enable graylog-server

$ systemctl start graylog-server

$ systemctl status graylog-server

```

<br>
만약 서비스 실행후에도 웹 접근이 되지 않는다면 아래 사항을 점검하세요.
<br>

`Check firewall: `

```bash

# selinux 관리용 패키지 설치
$ yum install -y policycoreutils-python

# 웹 서비스 허용(전체)
$ setsebool -P httpd_can_network_connect 1

# OR이며 위 아래 중 선택해서 허용하시면 됩니다. 

# 각 서비스 허용 (보안 정책이 있다면 위 방법말고 아래대로 각 서비스 허용)
$ semanage port -a -t http_port_t -p tcp 9000
$ semanage port -a -t http_port_t -p tcp 9200
$ semanage port -a -t mongod_port_t -p tcp 27017


# Disable firewalld
# 방화벽 서비스 자체를 내리거나 혹은 일부 서비스만 허용할 수 있습니다.

# 방화벽 서비스 중지
$ systemctl stop firewalld && systemctl disable firewalld

# 일부만 허용
$ firewall-cmd --permanent --zone=public --add-service=http
$ firewall-cmd --reload


```



<br>
문의사항은 댓글이나 이메일로 남겨주세요.
<br>
감사합니다.
