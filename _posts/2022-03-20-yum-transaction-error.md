---
layout: post
title:  "Yum 트랜잭션 오류 처리 방법 (Yum Transaction Error)"
author: Sejin
tags: yum transactionerror error centos
categories: Linux
sitemap:
 changefreq: daily
 priority: 1.0
---

## Yum Transaction Error
---
안녕하세요, <br>
이번 포스팅은 Yum이라는 RPM 기반의 패키지 관리 도구에 대한 트랜잭션 오류를 경험하고 처리했던 내용에 대한 기술입니다.
<br>

<figure>
  <img data-action="zoom" src='{{ "/assets/img/yum_transaction_error_1.JPG" | relative_url }}' alt='absolute'>
  <figcaption style="text-align: center;">Yum Install</figcaption>
</figure>



레드햇 계열의 리눅스를 다루다보면 아주 익숙하실거라 예상되는 Yum을 이용한 패키지 설치 명령어입니다.
<br>
이렇게 단순한 명령어로  설치하려다가도 알 수 없는 오류가 참 많이 발생하게 되는데 그 중 제가 경험한 로그는 아래와 같습니다.
<br>
<br>
`Yum Transaction Error Log :`

```bash
오류 예시)
Transaction check error:
file /usr/lib/rpm/redhat/find-provides.ksyms from install of system-rpm-config-9.1.0-76.amzn2.0.10.noarch conflicts with file from package redhat-rpm-config.9.1.0-80.el7.centos.noarch
```
<br>
위 로그의 경우에 2가지 방식으로 처리가 가능한데 저는 redhat-rpm-config를 제거하고 system-rpm-config를 설치하는 방법으로 해결했습니다.<br>

## Yum cleanup duplicate packages
---
<br>
```bash

1. Install the yum-utils package:
# yum install yum-utils

2. The package-cleanup --dupes lists all duplicate packages:
# package-cleanup --dupes

3. The package-cleanup --cleandupes removes the duplicates 
(it asks for a confirmation to remove all duplicates unless the -y switch is given):
# package-cleanup --cleandupes  

4. Edit /etc/yum.conf, set the following line:
exactarch=1

5. Run yum command:
# yum clean all
# yum update
```
<br>
<figure>
  <img data-action="zoom" src='{{ "/assets/img/yum_transaction_error_2.JPG" | relative_url }}' alt='absolute'>
  <figcaption style="text-align: center;">Yum Cleanup Command</figcaption>
</figure>

<br>
<br>
이렇게 중복 패키지에 대한 첫 번째 처리 방법이었는데, 개인적으로는 근복적으로 오류가 나는 패키지를 지우고 재설치하는 방법을 선호합니다. 이 방법은 Yum을 사용중이신 많은 분들에게 익숙하실텐데 혹시 궁금하신 분들을 위해 남깁니다.
<br>
<br>
## Reinstall Packages
---
<br>
명령어는 단순합니다. Yum의 Shell 명령을 활용하여 지우고 설치.
<br>
<br>
```bash
# yum shell 
> remove [지워야 하는 패키지명] 
> install [다시 설치해야 하는 패키지명] 
> run
```
<br>
<br>
## 마치며
---
<br>
일을 하며 느꼈던 부분은 동일한 오류가 발생하더라도 항상 같은 방법으로 해결되는 것은 아니더라구요.
<br>
그래도 스스로 참고할 수 있도록, 혹은 문제 해결이 필요하신 분들께 미약하게나마 도움이 되기를 바랍니다.
<br>
<br>
문의는 댓글이나 메일로 남겨주세요. <br>
감사합니다.

