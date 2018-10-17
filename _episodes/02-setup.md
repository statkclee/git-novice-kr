---
title: Git 구축 및 설정
teaching: 5
exercises: 0
questions:
- "Git을 사용하려면 어떻게 환경을 구축해야 할까?"
objectives:
- "`git` 환경설정하고 본인 컴퓨터에 사용한다."
- "`--global` 환경설정 플래그 의미를 이해한다."
keypoints:
- "`--global` 선택옵션을 `git config`에 적용하여 사용자명(user name), 전자우편, 편집기 등 선호사항을 컴퓨터에 적용한다."
---


처음 Git를 새로운 컴퓨터에 사용할 때, 몇가지 설정이 필요하다. 
다음에 Git을 시작할 때, 설정해야 되는 몇가지 사례가 나와있다:


*   이름과 전자우편 주소
*   선호하는 텍스트 편집기 선정
*   전역(즉, 모든 프로젝트)으로 이런 설정을 할지 여부

명령라인에서 Git 명령어는 다음과 같이 작성된다; `git verb options`, 즉, `git 동사 선택옵션`.
`verb` 가 실제로 수행하고자 하는 명령어가 되고, `options`는 `verb`에 필요할지도 모르는 추가 선택옵션 정보가 된다.
다음에 Dracula가 새로 구입한 노트북에 환경설정하는 방법이 나와있다:

~~~
$ git config --global user.name "Vlad Dracula"
$ git config --global user.email "vlad@tran.sylvan.ia"
~~~
{: .language-bash}

Dracula 대신에 본인 이름과 본인 전자우편 주소를 사용합니다. 사용자명과 전자우편 주소는 후속 Git 활동과 연관된다.
이것이 의미하는 바는 
[GitHub](https://github.com/),
[BitBucket](https://bitbucket.org/),
[GitLab](https://gitlab.com/), 혹은 Git 호스트 서버에 푸쉬하는 어떤 변경사항도 사용자명과 전자우편 주소를 담게되는 것을 의미한다.


> ## 줄마침(Line Endings)
>
> 다른 키보트 타이핑과 마찬가지로, 키보드로 <kbd>Return</kbd>를 치게 되면,
> 컴퓨터는 엔터값을 문자로 인코딩한다.
> 줄마침을 표현하기 위해서 운영체제마다 별도 문자를 사용한다.
> (개행 혹은 줄중단, 영어로 newline 혹은 line breaks를 들어봤을 수도 있다.)
> Git이 파일을 비교하는데 이러한 문자를 사용하기 때문에,
> 운영체제가 다른 컴퓨텅에서 파일을 편집할 때 예기치 않은 이슈가 발생될 수 있다.
> 이 문제는 금번 학습 범위를 넘어서는 것이지만, [on this GitHub page](https://help.github.com/articles/dealing-with-line-endings/)
> 웹페이지에서 좀더 자세한 정보를 얻을 수 있다.
> 
{: .callout}
>
> Git에서 줄마침을 인식하고 인코딩하는 방식을 변경하려면,
> `git config`에 `core.autocrlf` 명령을 사용한다.
> 권장되는 설정은 다음과 같다:
>
> 맥OS와 리눅스:
>
> ~~~
> $ git config --global core.autocrlf input
> ~~~
> {: .language-bash}
>
> 윈도우:
>
> ~~~
> $ git config --global core.autocrlf true
> ~~~
> {: .language-bash}
> 

이번 학습에서, [GitHub](https://github.com/)을 사용하게 되는데, 사용되는 전자우편주소는 GitHub 계정을 설정할 때 사용하는 것과 같은 것이 되어야 한다.
만약, 개인정보에 대해 걱정이 된다면, [GitHub's instructions for keeping your email address private][git-privacy]을 참조한다.
GitHub에서 사적인 개인 전자우편주소를 선택하기로 했다면,
`user.email`에 동일한 전자우편주소를 사용한다. 즉, `username`을 GitHub의 설정된 것으로 바꿔놓아 `username@users.noreply.github.com`게 된다.
나중에 `git config` 명령어를 사용해서 전자우편 주소를 변경할 수 있다.

Dracula도 자신이 선호하는 텍스트 편집기를 설정해야 하는데, 다음 표를 참조한다:

| 편집기             | 환경설정 명령어                             |
|:-------------------|:-------------------------------------------------|
| Atom               | `$ git config --global core.editor "atom --wait"`|
| nano               | `$ git config --global core.editor "nano -w"`    |
| BBEdit (Mac, with command line tools) | `$ git config --global core.editor "bbedit -w"`    |
| Sublime Text (Mac) | `$ git config --global core.editor "/Applications/Sublime\ Text.app/Contents/SharedSupport/bin/subl -n -w"` |
| Sublime Text (Win, 32-bit install) | `$ git config --global core.editor "'c:/program files (x86)/sublime text 3/sublime_text.exe' -w"` |
| Sublime Text (Win, 64-bit install) | `$ git config --global core.editor "'c:/program files/sublime text 3/sublime_text.exe' -w"` |
| Notepad++ (Win, 32-bit install)    | `$ git config --global core.editor "'c:/program files (x86)/Notepad++/notepad++.exe' -multiInst -notabbar -nosession -noPlugin"`|
| Notepad++ (Win, 64-bit install)    | `$ git config --global core.editor "'c:/program files/Notepad++/notepad++.exe' -multiInst -notabbar -nosession -noPlugin"`|
| Kate (Linux)       | `$ git config --global core.editor "kate"`       |
| Gedit (Linux)      | `$ git config --global core.editor "gedit --wait --new-window"`   |
| Scratch (Linux)    | `$ git config --global core.editor "scratch-text-editor"`  |
| Emacs              | `$ git config --global core.editor "emacs"`   |
| Vim                | `$ git config --global core.editor "vim"`   |

원할 때마다 Git에 사용할 텍스트 편집기 환경설정을 다시 할 수 있다.

> ## Vim 나가기
>
> 다수 프로그램에서 Vim이 기본설정된 편집기다.
> Vim을 예전에 사용한 적이 없고, 변경사항을 저장하지 않고 세션을 빠져나가고자 한다면,
> <kbd>Esc</kbd> 다음에, `:q!`를 타이핑하고 나서 <kbd>Return</kbd>를 친다.
> 변경사항을 저장하고 나가려면, <kbd>Esc</kbd> 다음에, `:wq`를 타이핑하고 <kbd>Return</kbd>을 친다.
> 
{: .callout}

앞서 실행한 상기 명령어는 한번만 실행하면 된다: `--global` 플래그는 Git으로 하여금 해당 컴퓨터에 본인 계정의
모든 프로젝트에 환경설정한 것을 사용하도록 한다.

본인이 설정한 환경설정 내용은 언제라도 다음 명령어를 입력하여 확인할 수 있다:

~~~
$ git config --list
~~~
{: .language-bash}

원하는 만큼 환경설정을 바꿀 수도 있다: 편집기를 바꾸거나 전자우편주소를 갱신할 때 
동일한 명령어를 사용하면 된다.

> ## 프록시(Proxy)
>
> 일부 네트워크에서 [proxy](https://en.wikipedia.org/wiki/Proxy_server)를 사용할 필요가 있다.
> 이런 경우, Git에게 프록시에 대해 일러줘야 한다:
>
> ~~~
> $ git config --global http.proxy proxy-url
> $ git config --global https.proxy proxy-url
> ~~~
> {: .language-bash}
>
> 프록시를 비활성화 하는 경우, 다음 명령어를 사용한다.
>
> ~~~
> $ git config --global --unset http.proxy
> $ git config --global --unset https.proxy
> ~~~
> {: .language-bash}
{: .callout}

> ## Git 도움말과 매뉴얼
>
> 항상 기억할 것은 `git` 명령어를 잊은 경우, `-h` 선택옵션을 주어 명령어 목록을 볼 수 있고, `--help`를 사용해서 Git 매뉴얼도 이용할 수 있다:
>
> ~~~
> $ git config -h
> $ git config --help
> ~~~
> {: .language-bash}
{: .callout}

[git-privacy]: https://help.github.com/articles/keeping-your-email-address-private/
