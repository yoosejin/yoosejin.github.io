---
layout: post
title:  "CentOS8 지원 종료와 CentOS8 Stream (CentOS8 Early EOL)"
author: Sejin
tags: centos8 centos8stream centos8eol
categories: Etc
sitemap:
 changefreq: daily
 priority: 1.0
---

## CentOS8 지원 종료 (CentOS8 Early EOL)
---
안녕하세요, <br>
이번 포스팅은 출시된지 불과 1년밖에 되지 않은 비운(?)의 OS인 CentOS8의 지원 종료 소식과 앞으로 CentOS8의 방향인 "CentOS8 Stream"에 대해 작성하려 합니다.
<br>

<figure>
  <img data-action="zoom" src='{{ "/assets/img/centos8_logo.png" | relative_url }}' alt='absolute'>
  <figcaption style="text-align: center;">CentOS8 Logo</figcaption>
</figure>



모두가 아시겠지만 CentOS는 "Community Enterprise Operating System"의 약어로 RHEL(RedHat Enterprise Linux)를 완벽히 호환하며 Redhat의 상표권만 제거하여 제작되는 OS입니다.
<br>
독립적인 오픈소스 프로젝트로 운영되다가 2014년 Redhat에 인수되고 CentOS가 사라지는게 아닐까 걱정이 많았었는데 아니나 다를까.. 그날이 오고야 말았습니다.
<br>
CentOS8이 <b>2019년 9월 25일에 출시</b>되었는데 2년 3개월만인 <b>2021년 12월 31일부로 지원 종료</b>가 된다고 합니다.
<br>
<br>
<br>

## CentOS8 지원 종료 후 우리의 대응?
---
Redhat은 CentOS8 자체가 사라지는 것이 아니라 Upstream 버전(=Stream)으로 유지될 것이라고 말하기는 하는데,
<br>
Fedora를 아시는 분들은 짐작하시겠지만 Stream 버전으로 전환되고 나면 RHEL의 테스팅 버전이 되는것이고 안정성이 떨어질거라는 건 분명한 사실입니다.
<br>

<figure>
  <img data-action="zoom" src='{{ "/assets/img/3os.png" | relative_url }}' alt='absolute'>
  <figcaption style="text-align: center;">구조</figcaption>
</figure>

<br>
그럼 우리가 선택할 수 있는 방법은 무엇일지 생각해보면, <br>
1. CentOS8에서 CentOS7로 마이그레이션(CentOS7의 경우 2024년 6월 30일 EOL) <br>
2. CentOS8에서 CentOS8 Stream으로 마이그레이션(이 경우 안정성이 떨어짐) <br>
3. 신규 RHEL Forking 프로젝트인 "Rocky Linux"(=CentOS의 창립자 중 한명이 주도하여 만드려고 하는 새로운 RHEL 복제품)을 기다리며 현상 유지<br>
<br>
위 3가지 정도가 있을 것 같습니다. (기타 OS로 전환하거나 유료 버전의 RHEL을 구매하는 경우는 제외)
<br>
다들 많이 고민하시겠지만 제 생각에 CentOS 유지가 필요한 상황이라면 CentOS7로 다시 전환하는 것이 가장 좋은 선택인 것 같습니다.


<br>

## CentOS8 Stream이란?
---
쉽게 생각하면, 각 Linux에는 테스트용 배포판이 있습니다 <br>
1. Debian에는 Debian Testing <br>
2. SUSE Enterprise Linux에는 OpenSUSE <br>
3. RHEL(RedHat Enterprise Linux)에는 Fedora<br>
<br>
이 중에 (3)의 과정에 한 가지가 더 추가되어 Fedora > CentOS Stream > RHEL이 되는 것입니다. <br>
<br>

<figure>
  <img data-action="zoom" src='{{ "/assets/img/old_and_new.png" | relative_url }}' alt='absolute'>
  <figcaption style="text-align: center;">비교</figcaption>
</figure>


<br>

<br>
<br>

## Fedora가 있는데 왜 CentOS8 Stream이 필요할까?
---
저를 포함해서 많은분들이 궁금하셨을거라 생각합니다. <br>

Redhat은 이미 테스트 목적의 Fedora가 있는데 왜 추가적인 테스트 OS(CentOS Stream)를 만들었을까? <br>
답은 간단합니다. <br>
Fedora에 설치된 모든 소프트웨어는 항상 최신버전이며 이 버전을 개량하여 RHEL을 만들게 되는데 안정성을 추구하는 Enterprise OS에서 이 속도를 따라가기 힘들기 때문에 중간 지점의 필요를 느끼게 된 것 같습니다.
<br>
참고로 RHEL7(=CentOS7)의 경우 Fedora19와 Fedora20을 기반으로 제작되었고, RHEL8(=CentOS8)의 경우 Fedora28을 기반으로 제작되었는데,
<br>
실제로 Fedora는 33버전까지 나올정도로 속도가 빠르기(Release 주기가 6개월)때문에 RHEL이 더 이상 따라가는건 힘들다고 판단했고, 그래서 출시한 것이 CentOS8 Stream입니다.
<br>

<br>
<br>

## 마치며
---
CentOS8 Stream의 경우 Fedora처럼 모든 최신 소프트웨어를 제공하지는 않을 것이라고 합니다. 
<br>
Debian Testing, OpenSUSE와 같이 버전을 최대한 안정적으로 유지할 것처럼 보이며 이미지에서 보셨듯이 최신 RHEL과 최신 Fedora의 중간지점의 OS로 생각보다는 안정적일수도 있겠다는 생각이 듭니다.
<br>
새로운 버전의 CentOS가 출시되며 저도 재밌게 다뤄봤었는데 빠르게 단종 후 새로운 OS로 전환이라니  Redhat에 대한 반감이 무럭무럭 자라네요.
<br>
부디 CentOS8로 전환하신 모든 분들이 무사히 CentOS7 혹은 다른 대안으로 잘 해결하시기를 바라겠습니다.


<br>
문의는 댓글이나 메일로 남겨주세요. <br>
감사합니다.
