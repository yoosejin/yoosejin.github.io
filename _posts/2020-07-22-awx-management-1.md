---
layout: post
title:  "AWX 활용하기-1 (Ansible Playbook)"
author: Sejin
tags: AWX Ansible configuration install centos7 opensource
categories: Ansible
sitemap:
 changefreq: daily
 priority: 1.0
---

## 개요
---
지난번 예고드렸던대로 Ansible Playbook을 AWX에서 활용하는 포스팅입니다.

이해가 어려운 부분에 대해서는 **sejin_yoo@naver.com**으로 메일주시면 답장 드리겠습니다.
<br>
<br> 

## AWX에 대한 설명
---

<br>
AWX는 자동화를 위해 작성한 Ansible 코드(=Playbook)를 실행하는 환경입니다. (단일 명령으로 실행하는 것은 [Ansible-Adhoc]이라고 하고 링크를 확인하시면 공식 문서에서 다양한 예제를 확인하실 수 있습니다.)
<br>
Ansible 코드를 실행하는 환경에 걸맞게 AWX는 다양한 환경 설정을 제공하고 모든 설정이 완료되면 작업을 수행할 수 있는 템플릿 기능도 제공하고 있습니다.
<br>
자세한 사항은 [Ansible-Tower-Manual]에서 확인하시면 될 것 같습니다.

`AWX 기능 소개:` 

```
# Credentials
 : 각 서비스(혹은 서버)에 접근하기 위한 인증 정보를 등록합니다.
 : 인증 정보가 등록되면 주요 정보는 암호화 됩니다. (Password, Private Key 등)
   따라서, 등록한 사람도 정보를 기억하고 있지 않다면 저장된 정보의 내용이 무엇인지 알 수 없습니다.

# Projects
 : 하나의 서비스를 뜻합니다. (Git과 같은)
 : 생성된 Credentials를 Projects와 연결하여 서비스에 접속할 수 있습니다.

# Inventories
 : 작업할 대상입니다.
 : 일반적으로 hosts 파일을 생성해서 목록을 관리하지만 단일 IP나 Domain으로 등록하여 사용하기도 합니다.

# Templates
 : Playbook을 실행하는 곳입니다.
 : Template 작성을 위해서는 Credentials, Projects, Inventories가 기본적으로 생성되어 있어야 합니다. (실제 설정 가능한 항목은 더 많습니다.)


```

<br>
## 간단한 테스트 과정
<br>
`과정: `

```
Credentials 등록 > Projects 등록 > Inventories 등록 > Templates 등록 
```
<br>

`Credentials 등록: `

아래 이미지에 붉게 표시된 부분을 선택하여 등록 과정을 진행합니다.

<figure>
  <img data-action="zoom" src='{{ "/assets/img/credentials1.PNG" | relative_url }}' alt='absolute'>
  <figcaption>Credentials Menu</figcaption>
</figure>

<br>
+로 표시된 버튼을 클릭하면 아래 이미지처럼 화면이 나올텐데 이 중 *"CREDENTIAL TYPE"*을 통해 어떤 형식의 자격 증명을 등록할지 선택하게 됩니다.

<figure>
  <img data-action="zoom" src='{{ "/assets/img/credentials2.PNG" | relative_url }}' alt='absolute'>
  <figcaption>New Credential</figcaption>
</figure>

<br>
저는 타입으로 *"Machine"*을 선택하도록 하겠습니다.

<figure>
  <img data-action="zoom" src='{{ "/assets/img/credentials3.PNG" | relative_url }}' alt='absolute'>
  <figcaption>Select Credential Type</figcaption>
</figure>

<br>

타입을 선택하고 나면 많은 필드가 보이실텐데 성공적으로 등록하기 위한 최소 조건은 Name, Credential Type(방금 선택했던 설정) 그리고 인증 정보(Username과 Password 혹은 Username과 SSH Key)입니다.

<br>
이번 포스팅에서는 Private Key를 통한 인증으로 진행하겠습니다.
<br>

`CentOS에서 Private Key 생성하기:`

```bash
# 먼저 인증에 사용하고자 하는 사용자 계정으로 로그인 하고 순서대로 진행하시면 됩니다.

$ ssh-keygen -t rsa

# 위 명령어를 사용하면 몇 가지 질문이 나오는데 Enter를 입력하여 기본 설정으로 진행합니다.
# 이 과정을 거치고 나면 /home/<사용자 계정명>/.ssh/ 경로에 id_rsa과 id_rsa.pub 파일이 생성됩니다. 
# 이중에 id_rsa가 Private Key고 id_rsa.pub가 Public Key입니다.

# 아래 명령어를 사용하여 확인한 키를 AWX Credential "SSH PRIVATE KEY"에 넣고 "SAVE" 버튼으로 저장합니다.
$ cat id_rsa

```
<br>

<figure>
  <img data-action="zoom" src='{{ "/assets/img/keygen.PNG" | relative_url }}' alt='absolute'>
  <figcaption>Create Private Key</figcaption>
</figure>

<br>
키를 입력하고 저장한 후에 다시 해당 키를 선택해서 확인해보면 *"ENCRYPTED"*라는 문구와 함께 기존에 입력했던 값을 확인할 수 없는 것을 알 수 있습니다.
<br>
따라서 등록한 정보는 서버 혹은 별도의 공간에 잘 보관하셔야 합니다. (아래 이미지를 참고해주세요)
<br>
<figure>
  <img data-action="zoom" src='{{ "/assets/img/privatekey-encrypt.PNG" | relative_url }}' alt='absolute'>
  <figcaption>Encrypt key</figcaption>
</figure>

<br>
<br>

`Project 생성하기: `

진입은 좌측 메뉴에서 Projects > Credential 생성과 동일하게 + 버튼을 클릭하시면 됩니다.
<br>
Subversion(=svn) 프로젝트를 추가할텐데 Name, Organization, SCM Url, SCM Update Options만 선택하면 동기화하고 기본적인 사용에 전혀 문제가 없습니다.
<br>
<figure>
  <img data-action="zoom" src='{{ "/assets/img/newproject.PNG" | relative_url }}' alt='absolute'>
  <figcaption>New Project</figcaption>
</figure>

<br>

프로젝트 추가 후, 하단에 *"PROJECTS"*에서 등록된 프로젝트의 우측 화살표 아이콘을 클릭하여 동기화를 진행합니다.
<br>
<figure>
  <img data-action="zoom" src='{{ "/assets/img/syncsvn.PNG" | relative_url }}' alt='absolute'>
  <figcaption>SVN 동기화</figcaption>
</figure>
<br>


<br>
여기까지 프로젝트 생성이고 2편에서 인벤토리, 템플릿 등록을 진행하도록 하겠습니다.




<br>	
문의사항은 댓글이나 이메일로 남겨주세요.
<br>
감사합니다.




[Ansible-Adhoc]: https://docs.ansible.com/ansible/latest/user_guide/intro_adhoc.html
[Ansible-Tower-Manual]: https://docs.ansible.com/ansible-tower/latest/html/userguide/credentials.html#credential-types
