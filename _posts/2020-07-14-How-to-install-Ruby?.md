---
layout: post
title:  "CentOS7에서 Ruby 설치하기"
author: Sejin
tags: ruby install centos7 opensource
categories: Etc
sitemap:
 changefreq: daily
 priority: 1.0
---


## 개요
---
Github 블로그 생성을 위해 필요한 환경 설정에 대한 포스팅입니다.

`<b>Youtube Link</b> :` [CentOS7에서 Ruby 설치하기](https://youtu.be/ucBdQU3pIr4)


이해가 어려운 부분에 대해서는 **sejin_yoo@naver.com**으로 메일주시면 답장 드리겠습니다.
<br>
<br> 
## 설치 환경
---
OS : `CentOS 7.8 x86_64`

Network : 외부 통신 가능
  
  <br>
<br>
## 설치 과정
---
저는 인터넷 환경에서 설치를 진행중이라 무리없이 진행됐지만 인터넷이 불가한 환경에 계시다면 [CentOS 패키지 설치하기](https://blog.naver.com/sejin_yoo/221609835305) 를  참고해서 진행하시면 될 것 같습니다.
  
<br>
먼저 ruby 설치에 필요한 의존성 패키지를 먼저 설치합니다. 
<br>
  
`Install dependency packages:` 

```bash
$ yum install gcc-c++ patch readline readline-devel zlib zlib-devel libffi-devel \
 openssl-devel make bzip2 autoconf automake libtool bison sqlite-devel -y
```
 
<br>
rvm 공식 홈페이지에서 패키지를 다운받기 위해 키를 설치합니다.
<br>

`Install GPG Keys:`

```bash
$ gpg2 --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
```
 
<br>
명령어 2개 중에 선택해서 설치하시면 되는데 차이는 설치되는 패키지입니다.
<br>

`Install RVM or Install RVM with Ruby and Rails (Ruby Version 2.0.0):`

```bash
$ curl -sSL https://get.rvm.io | bash -s stable

OR

$ curl -sSL https://get.rvm.io | bash -s stable --rails
```
<br>
설치 후 적용 시키는 과정입니다.
<br>

`Reload RVM:`

```bash
$ source /etc/profile.d/rvm.sh
$ rvm reload
```
 <br>


`Check all Dependencies and install any missing dependencies:`

```bash
rvm requirements 명령을 사용하면 의존성 패키지를 확인하고 부족한 패키지를 자동으로 설치합니다.

$ rvm requirements run
Checking requirements for centos.
Requirements installation successful.
```
<br> 
설치 가능한 Ruby 버전을 확인합니다.
<br>

`Find available ruby version:`

```bash
일부만 발췌한 내용으로 실제 설치 가능한 종류는 더 많습니다.

$ rvm list known
[ruby-]1.8.6[-p420]
[ruby-]1.8.7[-head] # security released on head
[ruby-]1.9.1[-p431]
[ruby-]1.9.2[-p330]
[ruby-]1.9.3[-p551]
[ruby-]2.0.0[-p648]
[ruby-]2.1[.10]
[ruby-]2.2[.10]
[ruby-]2.3[.8]
[ruby-]2.4[.9]
[ruby-]2.5[.7]
[ruby-]2.6[.5]
[ruby-]2.7[.0]
ruby-head
```
 <br>
저는 안정성을 중시하기 때문에 최신 버전은 가능하면 지양하는 편입니다.
따라서, 2.7이 아닌 2.6 버전의 Ruby를 설치하도록 하겠습니다.
아래 명령어처럼 마이너 버전을 생략하면 해당 메이저 버전에서 가장 최신을 설치하게 됩니다.
<br>

`Install ruby 2.6:`

```bash
$ rvm install 2.6
Searching for binary rubies, this might take some time.
Found remote file https://rvm_io.global.ssl.fastly.net/binaries/centos/7/x86_64/ruby-2.6.5.tar.bz2
Checking requirements for centos.
Requirements installation successful.
ruby-2.6.5 - #configure
ruby-2.6.5 - #download
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 17.9M  100 17.9M    0     0  1143k      0  0:00:16  0:00:16 --:--:-- 1796k
No checksum for downloaded archive, recording checksum in user configuration.
ruby-2.6.5 - #validate archive
ruby-2.6.5 - #extract
ruby-2.6.5 - #validate binary
ruby-2.6.5 - #setup
ruby-2.6.5 - #gemset created /usr/local/rvm/gems/ruby-2.6.5@global
ruby-2.6.5 - #importing gemset /usr/local/rvm/gemsets/global.gems..................................
ruby-2.6.5 - #generating global wrappers.......
ruby-2.6.5 - #gemset created /usr/local/rvm/gems/ruby-2.6.5
ruby-2.6.5 - #importing gemsetfile /usr/local/rvm/gemsets/default.gems evaluated to empty gem list
ruby-2.6.5 - #generating default wrappers.......
```
 <br>
설치된 리스트를 확인합니다.
<br>

`Installed list:`
```bash
리스트에서 확인 가능한 가장 좌측의 표시는 아래와 같은 의미를 가지고 있습니다.
=> 현재 버전
=* 현재 버전 및 기본 버전
*  기본 버전


$ rvm list
=> ruby-2.6.5 [ x86_64 ]
 * ruby-2.7.0 [ x86_64 ]
```

<br> 


`Set up the default ruby:`
```bash
위 리스트를 확인하면 2.7의 ruby가 기본 버전으로 설정되어 있는 것을 확인할 수 있습니다.
아래는 우리가 설치한 버전을 기본 버전으로 적용하는 명령입니다.


$ rvm use 2.6 --default
Using /usr/local/rvm/gems/ruby-2.6.5
```

<br> 


`Check current version:`

```bash
현재 버전을 확인합니다.

$ ruby --version
ruby 2.6.5p114 (2019-10-01 revision 67812) [x86_64-linux]
```
 
<br>
<br>

참고 링크 : [RVM 공식 사이트](https://rvm.io/)
