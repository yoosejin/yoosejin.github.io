---
layout: post
title:  "Ansible, AWX 설치 방법(CentOS 7.8)"
author: Sejin
tags: ansible ansible-tower awx install centos7
categories: Ansible
sitemap:
 changefreq: daily
 priority: 1.0
---

## 개요
---
자동화를 위한 도구 중 Ansible과 Ansible Tower의 무료버전인 AWX 설치 방법에 대한 포스팅입니다.

이해가 어려운 부분에 대해서는 **sejin_yoo@naver.com**으로 메일주시면 답장 드리겠습니다.
<br>
<br> 
## 설치 환경
---
Virtual Machine : `Oracle VirtualBox 6.1`
  
OS : `CentOS 7.8 x86_64`
  
Network : 외부 통신 가능
  
  <br>
<br>
## 설치 과정
---
  
<br>
먼저 필요 패키지를 설치합니다.
<br>
이번 포스팅에서는 AWX를 Docker Container 위에서 구동할 예정이라 Docker 패키지도 설치합니다.
<br>

**yum install -y epel-release**는 fedora의 최신 패키지를 받을 수 있도록 repository에 추가하는 명령어입니다.
<br>

`Install packages:` 

```bash
$ yum install -y epel-release

$ yum install -y yum-utils device-mapper-persistent-data lvm2 ansible git python-devel python-pip python-docker-py vim-enhanced

# Install docker
$ yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo

$ yum install docker-ce -y && systemctl start docker && systemctl enable docker
```

<br>
이제 필요한 패키지를 모두 설치했으니 AWX의 소스를 서버로 가져와야 합니다.
<br>

`Clone AWX source:`

```bash
$ mkdir -p /app

$ git clone https://github.com/ansible/awx.git

$ cd /app/awx

# 아래는 awx에서 사용하는 로고들을 다운로드 받는 명령어입니다.
$ git clone https://github.com/ansible/awx-logos.git
```

<br>
이제 AWX의 최신 소스를 가져왔으니 설치를 진행합니다.
<br>

`Install AWX:`

```bash
$ cd /app/awx/installer

$ vi inventory
==================
# 123 line
awx_official false -> true
==================

$ ansible-playbook -i inventory install.yml -vvv

위 명령어를 실행하면 에러가 발생할 수 있는데 에러 종류에 맞게 유동적으로 처리가 필요합니다. (환경마다 설치된 패키지가 다르기 때문에)
```

<br>

<br>

## 설치 과정 오류 해결 방법
---

<br>
설치 과정에서 오류가 발생할 수 있는데 제가 겪었던 오류들을 공유합니다.

<br>

`module_stderr: usr/bin/env: python3: No such file or directory`
```bash
이 오류의 경우 에러가 명확한데 python3 파일을 찾을 수 없다는 메시지입니다.

이 때, 진행 내역은 아래와 같습니다.

$ yum install python3 -y 
```

<br>

`Aborting, target uses selinux but python bindings (libselinux-python) aren't installed!`

```bash
이 경우 2가지를 점검해야 합니다.
1. selinux 정책에서 허용했거나 혹은 selinux를 비활성화 했는지
2. libselinux-python 라이브러리가 설치되어 있는지

저의 경우 selinux를 비활성화 한 후 설치를 진행하고 있어 2의 경우를 의심했습니다.

python3을 추가로 설치했기 때문에 python3 관련 라이브러리를 설치합니다.
$ yum install libselinux-python3 -y

```

<br>

`The error was: No module named requests`

```bash
모듈을 찾을 수 없다는 오류입니다. 최신으로 설치된 기타 패키지에 맞게 업그레이드를 진행합니다.

$ pip install docker-py

$ pip install --upgrade requests
```

<br>

### 설치 완료 후 방화벽 추가, 서비스 실행 확인
---
<br>

`도커 서비스 확인:`

```bash
$ docker ps
CONTAINER ID        IMAGE                COMMAND                  CREATED             STATUS              PORTS                  NAMES
7dd337c884b6        ansible/awx:13.0.0   "tini -- /usr/bin/la…"   9 hours ago         Up 9 hours          8052/tcp               awx_task
9eeed62e84c9        ansible/awx:13.0.0   "tini -- /bin/sh -c …"   9 hours ago         Up 9 hours          0.0.0.0:80->8052/tcp   awx_web
dbde0248a608        redis                "docker-entrypoint.s…"   10 hours ago        Up 9 hours          6379/tcp               awx_redis
3b630bc0ed92        postgres:10          "docker-entrypoint.s…"   10 hours ago        Up 9 hours          5432/tcp               awx_postgres

```
<br>

`방화벽 추가:`

```bash
$ firewall-cmd --zone=public --add-masquerade --permanent
$ firewall-cmd --permanent --add-service=http
$ firewall-cmd --permanent --add-service=https
$ firewall-cmd --reload
```

<br>

방화벽 설정까지 모두 끝났다면 `http://server-ip`로 웹 페이지를 확인할 수 있습니다.
<br>
기본적으로 ID/PW는 admin/password일텐데 변경하고 싶으시다면 `<awx 설치경로>/installer/inventory` 파일에서 아래 부분을 수정하시면 됩니다.
<br>

```bash
# 110-111 line
admin_user=admin
admin_password=변경하고자 하는 비밀번호

```
<br>

`웹 페이지 호출 결과`

<figure>
  <img data-action="zoom" src='{{ "/assets/img/ansible1.PNG" | relative_url }}' alt='absolute'>
  <figcaption>http://server-ip 접속 로그인 화면</figcaption>
</figure>

<br>

`로그인 후 대시보드 화면`

<figure>
  <img data-action="zoom" src='{{ "/assets/img/ansible2.PNG" | relative_url }}' alt='absolute'>
  <figcaption>AWX 대시보드</figcaption>
</figure>

<br>

다음 포스팅은 AWX 웹 페이지에서 설정하는 방법으로 찾아뵙겠습니다.


<br>
문의사항은 댓글이나 이메일로 남겨주세요.
<br>
감사합니다.
