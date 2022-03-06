---
layout: post
title:  "Apache Log4j 취약점 대응 (Apache Log4j Security Issue)"
author: Sejin
tags: apache log4 security
categories: Security
sitemap:
 changefreq: daily
 priority: 1.0
---

## Apache Log4j
---
안녕하세요, <br>
이번 포스팅은 Apache Log4j 취약점에 대한 내용과 실제 업무에서 어떻게 대응했었는지에 대한 내용을 작성하려 합니다.
<br>

<figure>
  <img data-action="zoom" src='{{ "/assets/img/Apache_HTTP_server_logo.png" | relative_url }}' alt='absolute'>
  <figcaption style="text-align: center;">Apache Server Logo</figcaption>
</figure>



먼저 <b>Log4j는 아파치(Apache)재단에서 지원 및 관리하고 있는 Java 기반의 로깅 라이브러리</b>입니다. <br>
프로그램의 동작에 대한 기록을 남기고 싶은 경우에 사용하는 라이브러리인데 이번에 발견된 보안 취약점을 통해 데이터를 탈취하거나 악성코드를 심는 등의 행위를 할 수 있어 더 심각하게 받아 들여지고 있습니다.
<br>
외부에 게시되는 서비스들의 특성상 다양하게 취약점이 노출되는데 이번 Log4j의 취약점은 위험성 평가 점수(CVSS)에서 무려 <b>최고점인 10점</b>을 받았습니다.
<br>
해커들이 비밀번호 없이 내부에 침투할 수 있다는 점에서 굉장히 위험하고 오픈소스라는 특성상 많은 기업들에서 Log4j를 채택했기 때문에 역대급 보안 이슈로 남지 않을까 하는 생각이 있습니다.
<br>
<br>
<br>

## Log4j 이슈, 어떻게 대응해야 할까?
---
가장 좋은 방법은 사용하는 소프트웨어 보안 혹은 버그 탭을 크롤링하여 주기적으로 확인하는 것인데요.
<br>
예를 들어 이번의 경우는 [Apache Security Vulnerabilities]에서 취약점 내용과 수정된 사항에 대해 확인할 수 있습니다.
[Apache Security Vulnerabilities]: https://logging.apache.org/log4j/2.x/security.html
<br>
하지만 이조차도 버거운 경우가 많을 것 같아요. 그럼 차선으로 선택할 수 있는건 [KISA 인터넷 보호나라]를 크롤링하거나 주기적으로 방문하여 취약점에 대한 정보를 얻는 방법이 있습니다.
[KISA 인터넷 보호나라]: https://www.krcert.or.kr/main.do

<figure>
  <img data-action="zoom" src='{{ "/assets/img/kisa.png" | relative_url }}' alt='absolute'>
  <figcaption style="text-align: center;">인터넷 보호나라</figcaption>
</figure>

<br>
그럼 이번 Apache Log4j 취약점을 조치했던 순서를 따라해보면 <br>
먼저, 인터넷 보호나라에 들어가서 취약점을 확인하고, [Log4j 취약점 링크]
[Log4j 취약점 링크]: https://www.krcert.or.kr/data/secNoticeView.do?bulletin_writing_sequence=36397
<br>
<figure>
  <img data-action="zoom" src='{{ "/assets/img/log4j_issue.JPG" | relative_url }}' alt='absolute'>
  <figcaption style="text-align: center;">취약점 및 영향버전 확인</figcaption>
</figure>
<br>
실제 서버에 접속하여 사용중인 Log4j 버전을 확인합니다. <br>
<figure>
  <img data-action="zoom" src='{{ "/assets/img/check_rpm.JPG" | relative_url }}' alt='absolute'>
  <figcaption style="text-align: center;">Log4j 버전 확인</figcaption>
</figure>
<br>
지금은 rpm으로 설치된 패키지를 대상으로 확인하는 과정이지만 tar나 zip 형태로 직접 설치하여 사용하는 경우는 별도로 확인해야 합니다. (저는 주로 Java 프로세스를 확인하여 해당 경로에 있는 설정들을 살펴보는 편입니다)
<br>
이렇게 확인하는 과정을 거쳐 팀원이나 서비스 담당자에게 공유하기 위해 기록하는 과정을 거칩니다.
<figure>
  <img data-action="zoom" src='{{ "/assets/img/check_log4j_version.JPG" | relative_url }}' alt='absolute'>
  <figcaption style="text-align: center;">Log4j 리스트</figcaption>
</figure>
<br>
내부 문서를 공개할 수 없어 임의로 작성한 표이고, 실제로는 내부에서 사용중인 Jira, Confluence를 활용하여 기록 및 공유하였습니다.
<br>
<br>
이렇게 담당자에게 공유된 후 협의를 통해 취약 대상인 설정을 제거하거나 패키지를 업데이트, 미사용중인 라이브러리 제거 등의 조치를 진행하게 됩니다.

## 마치며
---
실제 취약점이 발생했던 때는 작년 11월이었고, 주기적으로 업데이트하며 취약점을 해소하는 과정이 있었습니다.
<br>
회사의 인프라 담당자로서 이런 보안 위협은 항상 긴장되고 어려운 일인 것 같습니다. <br>
그럼에도 불구하고 자체 개발이 아닌 상용 및 오픈소스를 사용중이라면 항상 이런 위험을 감지하고 대응할 수 있도록 노력해야겠다는 생각이 많이 드네요.
<br>

<br>
문의는 댓글이나 메일로 남겨주세요. <br>
감사합니다.

