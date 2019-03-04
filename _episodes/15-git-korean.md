---
title: "Git 추가설정"
teaching: 10
exercises: 0
questions:
- "Git 관련 환경설정하면서 자주 사용하는 설정은 무엇인가?"
objectives:
- "Git/GitHub 설정관련 내용을 정리한다."
keypoints:
- "아주 가끔 사용하는 Git/GitHub 설정사항을 이해한다."
---


# 로컬 PC와 SSH 키(Key) 연결 {#local-PC-ssh-keys}

[GitHub](https://github.com/)에 저장소(repository)를 만들고 여러 PC에서 작업을 진행할 경우 GitHub에 인증작업을 거쳐 진행하는 것이 여러모로 편리하다. 그중 하나의 방식이 공개키(public key)를 GitHub에 등록시켜 작업을 하는 것이 여기에 포함된다.

1. 먼저 윈도우를 사용할 경우 [Git for windows](https://gitforwindows.org/)를 다운로드 받아 설치한다. 
1. `ssh-keygen` 명령어로 공개키/비밀키를 생성한다.
1. 생성된 공개키를 [GitHub](https://github.com/) 계정에 등록시킨다.

## SSH 공개키/비밀키 생성 [^github-ssh] {#local-PC-ssh-keys-generate}

[^github-ssh]: [nickjoIT (2017), "GitHub SSH 키 생성 및 등록하여 사용하기"](https://nickjoit.tistory.com/94)

SSH 공개키/비밀키를 생성시키고 이를 GitHub 홈페이지에 등록한다. 
먼저 `ssh-keygen` 명령어에 매개변수 인자를 넣고 GitHub 전자우편주소도 함께 지정한다.


~~~
$ ssh-keygen -t rsa -C “your_email@example.com”
~~~
{: .language-bash}

`ssh-keygen` 명령어로 생성된 키를 GitHub에 등록한다.

- 우측상단 [Settings] &rarr; [SSH and GPG keys] &rarr; [New SSH key]

[New SSH key]를 클릭하게 되면  Title, Key를 넣는 입력부분이 보인다.
Title에 식별가능한 이름을 지정하고 앞서 생성한 `id_rsa.pub` 내용을 Key에 복사해서 붙여넣는다.

~~~
$ cat ~/.ssh/id_rsa.pub
~~~
{: .language-bash}

~~~
ssh-rsa AAAxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxYxY9 email_address@mail.com
~~~
{: .output}


## 첫 커밋(commit) {#local-PC-ssh-keys-commit}

인증작업을 완료한 후에 저장소에서 작업할 파일을 처음 커밋(commit)하는 경우 `git add`, `git commit -m`명령어를 이어서 전달시키게 되면 커밋을 때리는 사람이 누구인지를 등록하는 절차가 발생된다. `git config`를 통해 전자우편과 사용자명을 등록하게 되면 커밋을 정상적으로 진행시킬 수 있게 된다.

~~~
$ git config --global user.email "you@example.com"
$ git config --global user.name "Your Name"
~~~
{: .language-bash}


## 비밀번호 입력없이 푸쉬(push) [^github-without-password] {#local-PC-ssh-keys-push}

[^github-without-password]: [Stackoverflow, "How do I avoid the specification of the username and password at every git push?"](https://stackoverflow.com/questions/8588768/how-do-i-avoid-the-specification-of-the-username-and-password-at-every-git-push)

다음 단계로 비밀번호 없이 커밋된 내용을 GitHub에 전달하는 방법은 자격인증(credential) 캐싱을 통한 간단한 방법이 있다. 물론 처음에는 사용자명과 비번을 입력하는 과정을 필수적으로 거치게 된다.

~~~
$ git config credential.helper store
$ git push https://github.com/repo.git
~~~
{: .language-bash}

~~~
Username for 'https://github.com': <USERNAME>
Password for 'https://USERNAME@github.com': <PASSWORD>
~~~
{: .output}

캐쉬에 시간제한을 두어서 7200초 즉 2시간 보안을 강화시킨다.

~~~
$ git config --global credential.helper 'cache --timeout 7200'
~~~
{: .language-bash}



