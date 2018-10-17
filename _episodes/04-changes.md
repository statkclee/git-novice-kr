---
title: 변경사항 추적
teaching: 20
exercises: 0
questions:
- "Git으로 변경사항을 어떻게 기록할 수 있을까?"
- "버전 제어 저장소의 상태를 어떻게 확인할 수 있을까?"
- "어떻게 내가 만든 변경사항에 노트로 남겨 기록할 수 있을까? 그리고 노트를 남기는 이유는 무엇을까?"
objectives:
- "파일 한개 혹은 다수 파일에 대해서 변경-추가-커밋(modify-add-commit) 주기를 수행한다."
- "각 Git 커밋 작업흐름 단계별로 정보가 어디에 저장되는지 설명한다."
-  "기술이 잘된 커밋 메시지와 그렇지 않는 커밋 메시지를 구별한다."
keypoints:
- "`git status` 명령어는 저장소 상태를 보여준다."
- "파일은 (사용자가 볼수 있는) 프로젝트 작업 디렉토리에 저장될 수 있고, (다음 커밋이 생성되는) 준비영역(staging area)에 있을 수 있고, (커밋이 영구적으로 기록되는) 로컬 저장소에 저장될 수 있다."
- "`git add` 명령어른 파일을 준비영역(staging area)에 위치시킨다."
- "`git commit` 명령어는 준비영역에 있는 파일을 새로운 커밋으로 로컬 저장소에 저장시킨다."
- "정확하게 변경사항을 기술하는 커밋 메시지를 작성한다."
---

먼저 디렉토리 위치가 맞는 확인하자.
`planets` 디렉토리에 위치해야 한다.

~~~
$ pwd
~~~
{: .language-bash}
~~~
/home/vlad/Desktop/planets
~~~
{: .output}

`moons` 디렉토리에 여전히 있다면, `planets` 디렉토로리 되돌아간다.

~~~
$ pwd
~~~
{: .language-bash}
~~~
/home/vlad/Desktop/planets/moons
~~~
{: .output}
~~~
$ cd ..
~~~
{: .language-bash}


전진기지로서 화성의 적합성에 관한 기록을 담고 있는 `mars.txt` 파일을 생성한다. 
(파일 편집을 위해서 `nano` 편집기를 사용한다; 원하는 어떤 편집기를 사용해도 된다. 
특히, 앞에서 전역으로 설정한 `core.editor`일 필요는 없다.
하지만, 파일을 새로 생성하거나 편집할 때 배쉬 명령어는 사용자가 선택한 편집기에 의존하게 된다.(`nano`일 필요는 없다.)
텍스트 편집기에 대한 환기로, [The Unix Shell](https://swcarpentry.github.io/shell-novice/)의 ["Which Editor?"](https://swcarpentry.github.io/shell-novice/03-create/) 부분을 참고한다.

~~~
$ nano mars.txt
~~~
{: .language-bash}


`mars.txt` 파일에 다음 텍스트를 타이핑한다:

~~~
Cold and dry, but everything is my favorite color
~~~

`mars.txt` 파일은 이제 한 줄을 포함하게 되어서, 다음 명령어로 내용을 확인할 수 있다:

~~~
$ ls
~~~
{: .language-bash}

~~~
mars.txt
~~~
{: .output}

~~~
$ cat mars.txt
~~~
{: .language-bash}

~~~
Cold and dry, but everything is my favorite color
~~~
{: .output}

다시 한번 프로젝트의 상태를 확인하고자 하면, 
새로운 파일이 인지되었다고 Git이 일러준다:


~~~
$ git status
~~~
{: .language-bash}

~~~
On branch master

Initial commit

Untracked files:
   (use "git add <file>..." to include in what will be committed)

	mars.txt
nothing added to commit but untracked files present (use "git add" to track)
~~~
{: .output}

"untracked files" 메시지가 의미하는 것은 Git가 추적하고 있지 않는 파일 하나가 디렉토리에 있다는 것이다. 
`git add`를 사용해서 Git에게 추적관리하라고 일러준다:

~~~
$ git add mars.txt
~~~
{: .language-bash}

그리고 나서, 올바르게 처리되었는지 확인한다:

~~~
$ git status
~~~
{: .language-bash}

~~~
On branch master

Initial commit

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

	new file:   mars.txt

~~~
{: .output}

이제 Git은 `mars.txt` 파일을 추적할 것이라는 것을 알고 있지만, 
커밋으로 아직 저장소에는 어떤 변경사항도 기록되지 않았다. 
이를 위해서 명령어 하나 더 실행할 필요가 있다:

~~~
$ git commit -m "Start notes on Mars as a base"
~~~
{: .language-bash}

~~~
[master (root-commit) f22b25e] Start notes on Mars as a base
 1 file changed, 1 insertion(+)
 create mode 100644 mars.txt
~~~
{: .output}

`git commit`을 실행할 때, 
Git은 `git add`를 사용해서 저장하려고 하는 모든 대상을 받아서 
`.git` 디렉토리 내부에 영구적으로 사본을 저장한다. 
이 영구 사본을 [커밋(commit)]({{ page.root }}/reference#commit)
(혹은 [수정(revision)]({{ page.root }}/reference#revision))이라고 하고, 
짧은 식별자는 `f22b25e`이다. (여러분의 커밋번호의 짧은 식별자는 다를 수 있다.)

`-m` ("message"를 위미) 플래그를 사용해서 나중에 무엇을 왜 했는지 기억에 도움이 될 수 있는 주석을 기록한다. 
`-m`옵션 없이 `git commit`을 실행하면, 
Git는 `nano`(혹은 처음에 `core.editor`에서 설정한 다른 편집기)를 실행해서 좀더 긴 메시지를 작성할 수 있다.

[좋은 커밋 메시지(Good commit messages)][commit-messages] 작성은 
커밋으로 만들어진 간략한 (영문자 기준 50문자 이하) 변경사항 요약으로 시작된다.
일반적으로 메시지는 완전한 문장이 되어야 한다. 예를 들어, "If applied, this commit will" <commit 메시지>.
만약 좀더 상세한 사항을 남기려면,
요약줄 사이에 빈줄을 추가하고 추가적인 내역을 적는다.
추가되는 공간에 왜 변경을 하는지 사유를 남기고, 어떤 영향을 미치는지도 기록한다.

이제 `git status`를 시작하면:

~~~
$ git status
~~~
{: .language-bash}

~~~
On branch master
nothing to commit, working directory clean
~~~
{: .output}

모든 것이 최신 상태라고 보여준다. 
최근에 작업한 것을 알고자 한다면, 
`git log`를 사용해서 프로젝트 이력을 보여주도록 Git에게 명령어를 보낸다:

~~~
$ git log
~~~
{: .language-bash}

~~~
commit f22b25e3233b4645dabd0d81e651fe074bd8e73b
Author: Vlad Dracula <vlad@tran.sylvan.ia>
Date:   Thu Aug 22 09:51:46 2013 -0400

    Start notes on Mars as a base
~~~
{: .output}

`git log`는 시간 역순으로 저장소의 모든 변경사항을 나열한다.
각 수정사항 목록은 전체 커밋 식별자(앞서 `git commit` 명령어로 출력한 짧은 문자와 동일하게 시작), 
수정한 사람, 
언제 생성되었는지, 
커밋을 생성할 때 Git에 남긴 로그 메시지가 포함된다. 



> ## 내가 작성한 변경사항은 어디있나?
>
> 이 시점에서 `ls` 명령어를 다시 실행하면, 
> `mars.txt` 파일만 덩그러니 보게 된다. 
>  왜냐하면, Git이 앞에서 언급한 `.git` 특수 디렉토리에 파일 변경 이력 정보를 저장했기 때문이다.
> 그래서 파일 시스템이 뒤죽박죽되지 않게 된다. 
> (따라서, 옛 버젼을 실수로 편집하거나 삭제할 수 없다.)
{: .callout}

이제 드라큘라가 이 파일에 정보를 더 추가했다고 가정하자. 
(다시 한번 `nano`편집기로 편집하고 나서 `cat`으로 파일 내용을 살펴본다. 
다른 편집기를 사용할 수도 있고, `cat`으로 파일 내용을 꼭 볼 필요도 없다.)

~~~
$ nano mars.txt
$ cat mars.txt
~~~
{: .language-bash}

~~~
Cold and dry, but everything is my favorite color
The two moons may be a problem for Wolfman
~~~
{: .output}

`git status`를 실행하면, 
Git이 이미 알고 있는 파일이 변경되었다고 일러준다:

~~~
$ git status
~~~
{: .language-bash}

~~~
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   mars.txt

no changes added to commit (use "git add" and/or "git commit -a")
~~~
{: .output}

마지막 줄이 중요한 문구다: 
"no changes added to commit". 
`mars.txt` 파일을 변경했지만, 아직 Git에게는 변경을 사항을 저장하려고 하거나 (`git add`로 수행),
저장소에 저장하라고  (`git commit`로 수행) 일러주지도 않았다. 
이제 행동에 나서보자.
저장하기 전에 변경사항을 항상 검토하는 것은 좋은 습관이다.
`git diff`를 사용해서 작업 내용을 두번 검증한다. 
`git diff`는 현재 파일의 상태와 가장 최근에 저장된 버젼의 차이를 보여준다:


~~~
$ git diff
~~~
{: .language-bash}

~~~
diff --git a/mars.txt b/mars.txt
index df0654a..315bf3a 100644
--- a/mars.txt
+++ b/mars.txt
@@ -1 +1,2 @@
 Cold and dry, but everything is my favorite color
+The two moons may be a problem for Wolfman
~~~
{: .output}

출력 결과가 암호같은데 이유는 한 파일이 주어졌을 때 다른 파일 하나를 어떻게 재구성하는지를 일러주는 
`patch`와 편집기 같은 도구를 위한 일련의 명령어라서 그렇다.
만약 해당 내역을 조각내서 쪼개다면:


1.  첫번째 행은 Git이 신규 파일과 옛 버젼 파일을 비교하는 유닉스 `diff` 명령어와 유사한 출력결과를 생성하고 있다.
2.  두번째 행은 정확하게 Git이 파일 어느 버젼을 비교하는지 일러준다; 
      `df0654a`와 `315bf3a`은 해당 버젼에 대해서 중복되지 않게 컴퓨터가 생성한 표식이다.
3.  세번째와 네번째 행은 변경되는 파일 명칭을 다시한번 보여주고 있다.      
4.  나머지 행이 가장 흥미롭다. 실제 차이가 나는 것과 어느 행에서 발생했는지 보여준다. 
     특히 첫번째 열의 `+` 기호는 어디서 행이 추가 되었는지 보여준다.

변경사항 검토후에, 변경사항을 커밋(commit)하자.

~~~
$ git commit -m "Add concerns about effects of Mars' moons on Wolfman"
$ git status
~~~
{: .language-bash}

~~~
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   mars.txt

no changes added to commit (use "git add" and/or "git commit -a")
~~~
{: .output}

이럴 수가, `git add`을 먼저 하지 않아서 Git이 커밋을 할 수 없다. 
고쳐봅시다:

~~~
$ git add mars.txt
$ git commit -m "Add concerns about effects of Mars' moons on Wolfman"
~~~
{: .language-bash}

~~~
[master 34961b1] Add concerns about effects of Mars' moons on Wolfman
 1 file changed, 1 insertion(+)
~~~
{: .output}

실제로 무엇을 커밋하기 전에 커밋하고자하는 파일을 먼저 추가하라고 Git이 주문하는데, 
이유는  한번에 모든것을 커밋하지 싶지 않을수도 있기 때문이다. 
예를 들어, 작성하고 있는 논문에 지도교수 논문을 일부 인용하여 추가한다고 가정하자. 
논문 중간에 인용되는 추가부분과 상응되는 참고문헌을 커밋하고는 싶지만,
결론 부분을 커밋하고는 싶지 *않다.* (아직 결론이 완성되지 않았다.)

이런 점을 고려해서, 
Git은 특별한 *준비 영역(staging)*이 있어서 현재 [변경부분(change set)]({{ page.root }}/reference#changeset)을 추가는 했으나 아직 커밋하지 않는 것을 준비 영역에서 추적하고 있다. 

> ## 준비 영역(Staging area)
>
> 프로젝트 기간 동안에 걸쳐 발생된 변경사항에 대해 스냅사진을 찍는 것으로 Git을 바라보면,
> `git add` 명령어는 *무엇*이 스냅사진(준비영역에 놓는 것)에 들어갈지 명세하고,
> `git commit` 명령어는 *실제로* 스탭사진을 찍는 것이다.
> 만약 `git commit`을 타이핑할 때 준비된 어떤 것도 없다면,
> Git이 `git commit -a` 혹은 `git commit --all` 명령어 사용을 재촉한다.
> 사진을 찍으려고 *모두* 모이세요 하는 것과 같다.
> 하지만, 준비영역에 추가할 것을 명시적으로 하는 것이 항상 좋다.
> 왜냐하면 커밋을 했는데 잊은 것이 있을 수도 있기 때문이다.
> (스냅사진으로 돌아가서, `-a` 옵션을 사용했기 때문에 스냅사진에 들어갈 항목을 불완전하게
작성했을 수도 있다!)
> 수작업으로 준비영역에 올리거나,
> 원하는 것보다 많은 것을 올렸다면 "git undo commit"을 찾아보라.
> 
{: .callout}

![The Git Staging Area](../fig/git-staging-area.svg)

파일 변경사항을 편집기에서 준비 영역으로, 그리고 장기 저장소로 옮기는 것을 살펴보자. 
먼저, 파일에 행 하나를 더 추가한다:

~~~
$ nano mars.txt
$ cat mars.txt
~~~
{: .language-bash}

~~~
Cold and dry, but everything is my favorite color
The two moons may be a problem for Wolfman
But the Mummy will appreciate the lack of humidity
~~~
{: .output}

~~~
$ git diff
~~~
{: .language-bash}

~~~
diff --git a/mars.txt b/mars.txt
index 315bf3a..b36abfd 100644
--- a/mars.txt
+++ b/mars.txt
@@ -1,2 +1,3 @@
 Cold and dry, but everything is my favorite color
 The two moons may be a problem for Wolfman
+But the Mummy will appreciate the lack of humidity
~~~
{: .output}

지금까지 좋다. 
파일의 끝에 행을 하나 추가했다(첫 열에 `+`이 보인다). 
이제, 준비영역에 변경 사항을 놓고, `git diff` 명령어가 보고하는 것을 살펴보자:

~~~
$ git add mars.txt
$ git diff
~~~
{: .language-bash}

출력결과가 없다. 
Git이 일러줄 수 있는 것은 영구히 저장되는 것과 현재 디렉토리에 작업하고 있는 것에 차이가 없다는 것이다. 
하지만, 다음과 같이 명령어를 친다면:


~~~
$ git diff --staged
~~~
{: .language-bash}

~~~
diff --git a/mars.txt b/mars.txt
index 315bf3a..b36abfd 100644
--- a/mars.txt
+++ b/mars.txt
@@ -1,2 +1,3 @@
 Cold and dry, but everything is my favorite color
 The two moons may be a problem for Wolfman
+But the Mummy will appreciate the lack of humidity
~~~
{: .output}

마지막으로 커밋된 변경사항과 준비 영역(Staging)에 있는 것과 차이를 보여준다. 
변경사항을 저장하자:

~~~
$ git commit -m "Discuss concerns about Mars' climate for Mummy"
~~~
{: .language-bash}

~~~
[master 005937f] Discuss concerns about Mars' climate for Mummy
 1 file changed, 1 insertion(+)
~~~
{: .output}

현재 상태를 확인하자:

~~~
$ git status
~~~
{: .language-bash}

~~~
On branch master
nothing to commit, working directory clean
~~~
{: .output}

그리고 지금까지 작업한 이력을 살펴보자:

~~~
$ git log
~~~
{: .language-bash}

~~~
commit 005937fbe2a98fb83f0ade869025dc2636b4dad5
Author: Vlad Dracula <vlad@tran.sylvan.ia>
Date:   Thu Aug 22 10:14:07 2013 -0400

    Discuss concerns about Mars' climate for Mummy

commit 34961b159c27df3b475cfe4415d94a6d1fcd064d
Author: Vlad Dracula <vlad@tran.sylvan.ia>
Date:   Thu Aug 22 10:07:21 2013 -0400

    Add concerns about effects of Mars' moons on Wolfman

commit f22b25e3233b4645dabd0d81e651fe074bd8e73b
Author: Vlad Dracula <vlad@tran.sylvan.ia>
Date:   Thu Aug 22 09:51:46 2013 -0400

    Start notes on Mars as a base
~~~
{: .output}

> ## 단어 단위 차이분석(Word-based diffing)
>
> 경우에 따라서는 줄단위로 텍스트 차이 분석이 너무 자세하지 않을 수도 있다.
> `git diff` 명령어에 `--color-words` 선택옵션이 유용할 수 있는데 
> 이유는 색상을 사용해서 변경된 단어를 강조해서 표시해 주기 때문이다. 
{: .callout}

> ## 로그 페이지별 보기
>
> 화면에 `git log` 출력결과가 너무 긴 경우,
> `git`에 화면 크기에 맞춰 페이지 단위로 쪼개주는 프로그램이 제공된다.
> 페이지별 쪼개보기("pager")가 호출되면, 화면 마지막 줄에 프롬프트 대신에 `:`이 나타난다.
>
> *   페이저(pager)에서  나오려면, <kbd>Q</kbd>를 타이핑한다.
> *   다음 페이지로 이동하려면, <kbd>Spacebar</kbd>를 타이핑한다.
> *   전체 페이지에서 특정 단어를 검색하려면, 
>     <kbd>/</kbd> 타이핑하고,
>     and 특정단어를 검색하는 `검색어`를 타이핑한다.
>     검색에 매칭되는 단어를 따라가려면 <kbd>N</kbd>을 타이핑한다.
{: .callout}

> ## 로그 크기 제한걸기
>
> `git log`가 전체 터미널 화면을 접수하는 것을 피하려면,
> `-N` 선택옵션을 적용해서 Git이 화면에 출력하는 커밋 숫자에 제한을 건다.
> 여기서 `-N`은 보고자 하는 커밋 갯수가 된다. 
> 예를 들어 가장 마지막 커밋만 보려고 한다면 다음과 같이 타이핑한다:
>
> ~~~
> $ git log -1
> ~~~
> {: .language-bash}
>
> ~~~
> commit 005937fbe2a98fb83f0ade869025dc2636b4dad5
> Author: Vlad Dracula <vlad@tran.sylvan.ia>
> Date:   Thu Aug 22 10:14:07 2013 -0400
>
>    Discuss concerns about Mars' climate for Mummy
> ~~~
> {: .output}
>
> `--oneline` 선택옵션을 사용해서 출력되는 로그 메시지 크기를 줄일 수도 있다:
>
> ~~~
> $ git log --oneline
> ~~~
> {: .language-bash}
> ~~~
> * 005937f Discuss concerns about Mars' climate for Mummy
> * 34961b1 Add concerns about effects of Mars' moons on Wolfman
> * f22b25e Start notes on Mars as a base
> ~~~
> {: .output}
>
> `--oneline` 선택옵션과 다른 선택옵션을 조합할 수도 있다.
> 유용한 조합 사례로 다음이 있다:
>
> ~~~
> $ git log --oneline --graph --all --decorate
> ~~~
> {: .language-bash}
> ~~~
> * 005937f Discuss concerns about Mars' climate for Mummy (HEAD, master)
> * 34961b1 Add concerns about effects of Mars' moons on Wolfman
> * f22b25e Start notes on Mars as a base
> ~~~
> {: .output}
{: .callout}

> ## 디렉토리
>
> Git에서 디렉토리에 관해서 알아두면 좋을 두가지 사실.
>
> 1. Git은 그 자체로 디렉토리를 추적하지 않고, 디렉토리에 담긴 파일만 추적한다.
> 믿지 못하겠다면, 직접 다음과 같이 시도해 본다:
>
>    ~~~
>    $ mkdir directory
>    $ git status
>    $ git add directory
>    $ git status
>    ~~~
>    {: .language-bash}
>
>    새로 생성된 `directory` 이름을 갖는 디렉토리가 `git add` 명령어로 명시적으로 
>    추가했음에도 불구하고 untracked files 목록에 나오지 않고 있다.
>    이런 이유로 인해서 가끔 `.gitkeep` 파일을 보게 된다. 
>    `.gitignore`와 달리, 특별하지는 않고 유일한 목적은 디렉토리를 만들어 내어 
>    Git이 저장소에 추가하도록 하는 역할만 수행한다.
>    사실 원하는 이름으로 파일명을 붙일 수 있다.
>
> 2. Git 저장소에 디렉토리를 생성하고 파일로 채워넣으면, 
>    다음과 같이 디렉토리의 모든 파일을 추가할 수 있다:
>
>    ~~~
>    git add <directory-with-files>
>    ~~~
>    {: .language-bash}
>
{: .callout}

요약하면, 
변경사항을 저장소에 추가하고자 할 때, 
먼저 변경된 파일을 준비 영역(Staging)에 `git add` 명령어로 추가하고 나서, 
준비 영역의 변경사항을 저장소에 `git commit` 명령어로 최종 커밋한다:

![The Git Commit Workflow](../fig/git-committing.svg)

> ## 커밋 메시지 고르기
>
> 다음 중 어떤 커밋 메시지가 `mars.txt` 파일의 마지막 커밋으로 가장 적절할까요?
>
> 1. "Changes"
> 2. "Added line 'But the Mummy will appreciate the lack of humidity' to mars.txt"
> 3. "Discuss effects of Mars' climate on the Mummy"
>
> > ## 해답
> > 1번은 충분히 기술되어 있지 못하고 커밋 목적이 불확실하다;
> > 2번은 "git diff" 명령어를 사용한 것과 불필요하게 중복된다;
> > 3번이 좋다: 짧고, 기술이 잘되어 있고, 피할 수 없게 명백하다(imperative).
> {: .solution}
{: .challenge}

> ## Git에 변경사항 커밋하기
>
> 다음 중 어떤 명령어가 로컬 Git 저장소에 
> `myfile.txt` 파일 변경사항을 저장시키는걸까?
>
> 1. ~~~
>    $ git commit -m "my recent changes"
>    ~~~
>    {: .language-bash}
> 2. ~~~
>    $ git init myfile.txt
>    $ git commit -m "my recent changes"
>    ~~~
>    {: .language-bash}
> 3. ~~~
>    $ git add myfile.txt
>    $ git commit -m "my recent changes"
>    ~~~
>    {: .language-bash}
> 4. ~~~
>    $ git commit -m myfile.txt "my recent changes"
>    ~~~
>    {: .language-bash}
>
> > ## Solution
> >
> > 1. 파일이 이미 준비영역(staging)에 올라온 경우만 커밋이 생성된다.
> > 2. 신규 저장소를 생성하게 된다.
> > 3. 정답: 파일을 준비영역에 추가하고 나서, 커밋하게 된다.
> > 4. myfile.txt 파일에 "my recent changes" 메시지를 갖는 커밋을 생성한다.
> {: .solution}
{: .challenge}

> ## 파일 다수를 커밋
>
> 준비영역(staging area)은 스냅샷 한번에 원하는 만큼 파일을 변경사항을 담아 낼 수 있다.
>
> 1. `mars.txt` 파일에 전진기지로 생각하는 금성(Venus)를 고려하고 있다는 결정을 담은 텍스트를 추가한다.
> 2. `venus.txt` 파일을 새로 생성해서 본인과 친구들에게 금성에 관한 첫생각을 담아낸다.
> 3. 파일 두개에 변경사항을 준비영역에 추가하고 커밋한다.
>
> > ## 해답
> >
> > 먼저, `mars.txt`, `venus.txt` 파일에 변경사항을 기록한다:
> > ~~~
> > $ nano mars.txt
> > $ cat mars.txt
> > ~~~
> > {: .language-bash}
> > ~~~
> > Maybe I should start with a base on Venus.
> > ~~~
> > {: .output}
> > ~~~
> > $ nano venus.txt
> > $ cat venus.txt
> > ~~~
> > {: .language-bash}
> > ~~~
> > Venus is a nice planet and I definitely should consider it as a base.
> > ~~~
> > {: .output}
> > 준비영역에 파일 두개를 추가한다.
> > 한줄로 추가작업을 수행할 수 있다:
> >
> > ~~~
> > $ git add mars.txt venus.txt
> > ~~~
> > {: .language-bash}
> > 혹은 명령어를 다수 타이핑하면 된다:
> > ~~~
> > $ git add mars.txt
> > $ git add venus.txt
> > ~~~
> > {: .language-bash}
> > 이제 파일을 커밋할 준비가 되었다. 
> > `git status`를 사용해서 확인하면, 커밋을 할 준비가 되었다:
> > ~~~
> > $ git commit -m "Write plans to start a base on Venus"
> > ~~~
> > {: .language-bash}
> > ~~~
> > [master cc127c2]
> >  Write plans to start a base on Venus
> >  2 files changed, 2 insertions(+)
> >  create mode 100644 venus.txt
> > ~~~
> > {: .output}
> {: .solution}
{: .challenge}

> ## `bio` 저장소
>
> * `bio`라는 새로운 Git 저장소를 본인 로컬 컴퓨터에 생성한다.
> * `me.txt`라는 파일로 본인에 대한 3줄 이력서를 작성한다.
> 변경사항을 커밋한다.
> * 그리고 나서 한줄을 바꾸고, 네번째 줄을 추가하고 나서,
> * 원래 상태와 갱신된 상태의 차이를 화면에 출력한다.
> 
>
> > ## 해답
> >
> > 필요하다면, `planets` 폴더에서 빠져나온다:
> >
> > ~~~
> > $ cd ..
> > ~~~
> > {: .language-bash}
> >
> > `bio` 폴더를 새로 생성하고 `bio` 폴더로 이동한다:
> >
> > ~~~
> > $ mkdir bio
> > $ cd bio
> > ~~~
> > {: .language-bash}
> >
> > git 명령어로 초기화한다:
> >
> > ~~~
> > $ git init
> > ~~~
> > {: .language-bash}
> >
> > `nano` 혹은 선호하는 편집기를 사용해서 `me.txt` 파일에 본인 일대기를 작성한다.
> > 파일을 추가하고 나서, 저장소에 커밋한다:
> >
> > ~~~
> > $ git add me.txt
> > $ git commit -m'Adding biography file'
> > ~~~
> > {: .language-bash}
> >
> > 기술된 것(한줄 변경하고, 4번째 줄을 추가한다)처럼 파일을 변경한다.
> > 원본 상태와 수정된 상태를 `git diff` 명령어를 사용해서 화면에 출력한다:
> >
> > ~~~
> > $ git diff me.txt
> > ~~~
> > {: .language-bash}
> >
> {: .solution}
{: .challenge}

> ## 저자(Author)와 커미터(Committer)
>
> 
> 매번 커밋을 할 때마다, Git은 이름을 두번 저장한다.
> 본인 이름이 저자(Author)와 커미터(Committer)로 기록된다.
> 마지막 커밋에 추가 정보를 Git에게 요구하면 확인이 가능하다:
>
> ~~~
> $ git log --format=full
> ~~~
> {: .language-bash}
>
> 커밋할 때, 저자를 다른 누군가로 바꿀 수 있다:
>
> ~~~
> $ git commit --author="Vlad Dracula <vlad@tran.sylvan.ia>"
> ~~~
> {: .language-bash}
>
> 커밋을 두개 생성한다: 하나는 `--author` 옵션을 갖는 것으로 
> 저자로 동료이름을 반영한다.
> `git log`와 `git log --format=full` 명령어를 실행한다.
> 이런 방식이 동료와 협업하는 방식이 될 수도 있겠다고는 생각이 될 수 있다.
>
> > ## 해법
> >
> > ~~~
> > $ git add me.txt
> > $ git commit -m "Update Vlad's bio." --author="Frank N. Stein <franky@monster.com>"
> > ~~~
> > {: .language-bash}
> > ~~~
> > [master 4162a51] Update Vlad's bio.
> > Author: Frank N. Stein <franky@monster.com>
> > 1 file changed, 2 insertions(+), 2 deletions(-)
> >
> > $ git log --format=full
> > commit 4162a51b273ba799a9d395dd70c45d96dba4e2ff
> > Author: Frank N. Stein <franky@monster.com>
> > Commit: Vlad Dracula <vlad@tran.sylvan.ia>
> >
> > Update Vlad's bio.
> >
> > commit aaa3271e5e26f75f11892718e83a3e2743fab8ea
> > Author: Vlad Dracula <vlad@tran.sylvan.ia>
> > Commit: Vlad Dracula <vlad@tran.sylvan.ia>
> >
> > Vlad's initial bio.
> > ~~~
> > {: .output}
> {: .solution}
{: .challenge}

[commit-messages]: https://chris.beams.io/posts/git-commit/
