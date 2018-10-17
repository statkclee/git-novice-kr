---
title: 이력 탐색
teaching: 25
exercises: 0
questions:
- "파일 이전 버젼을 어떻게 확인할 수 있을까요?"
- "변경사항을 어떻게 리뷰할 수 있을까요?"
- "이전 파일 버전을 어떻게 복구할 수 있을까요?"
objectives:
- "저장소 헤드(HEAD)가 무엇인지, 사용법도 설명한다."
- Git 커밋 번호를 식별하고 사용한다.
- "다양한 추적 파일버젼을 비교한다."
- "옛 버젼 파일을 복원한다."
keypoints:
- "`git diff` 명령어는 커밋 사이 차이나는 바를 출력한다."
- "`git checkout` 명령어는 파일 이전 버전을 복구해낸다."
---

앞선 학습에서 살펴봤듯이, 식별자로 커밋을 조회할 수 있다.
`HEAD` 식별자를 사용해서 작업 디렉토리의 *가장 최근 커밋*을 조회할 수 있다.

`mars.txt` 파일에 한번에 한줄씩 추가했다. 따라서, 눈으로 봐도 진행사항을 쉽게 추적할 수 있다.
`HEAD`를 사용해서 추적작업을 수행해보자. 시작전에 `mars.txt` 파일에 변경을 가해보자.

~~~
$ nano mars.txt
$ cat mars.txt
~~~
{: .language-bash}

~~~
Cold and dry, but everything is my favorite color
The two moons may be a problem for Wolfman
But the Mummy will appreciate the lack of humidity
An ill-considered change
~~~
{: .output}

이제, 변경된 사항을 살펴보자.

~~~
$ git diff HEAD mars.txt
~~~
{: .language-bash}

~~~
diff --git a/mars.txt b/mars.txt
index b36abfd..0848c8d 100644
--- a/mars.txt
+++ b/mars.txt
@@ -1,3 +1,4 @@
 Cold and dry, but everything is my favorite color
 The two moons may be a problem for Wolfman
 But the Mummy will appreciate the lack of humidity
+An ill-considered change.
~~~
{: .output}

`HEAD`만 빼면, 앞서 살펴본 것과 동일하다.
이러한 접근법의 정말 좋은 점은 이전 커밋을 조회살 수 있다는 점이다.
`~1`("~"은 "틸드(tilde)", 발음기호 [**til**-d*uh*])을 추가해서 `HEAD` 이전 첫번째 커밋을 조회할 수 있다.

~~~
$ git diff HEAD~1 mars.txt
~~~
{: .language-bash}

`git diff` 명령어를 사용해서 이전 커밋과 차이난 점을 보고자 한다면, 
`HEAD~1`, `HEAD~2` 표기법을 사용해서 조회를 쉽게 할 수 있다:


~~~
$ git diff HEAD~2 mars.txt
~~~
{: .language-bash}

~~~
diff --git a/mars.txt b/mars.txt
index df0654a..b36abfd 100644
--- a/mars.txt
+++ b/mars.txt
@@ -1 +1,4 @@
 Cold and dry, but everything is my favorite color
+The two moons may be a problem for Wolfman
+But the Mummy will appreciate the lack of humidity
+An ill-considered change
~~~
{: .output}

`git show`를 사용해서도 커밋 메시지 분만 아니라 이전 커밋과 변경사항을 보여준다. 
`git diff`는 작업 디렉토리와 커밋 사이 *차이나는 부분*을 보여준다. 

~~~
$ git show HEAD~2 mars.txt
~~~
{: .language-bash}

~~~
commit 34961b159c27df3b475cfe4415d94a6d1fcd064d
Author: Vlad Dracula <vlad@tran.sylvan.ia>
Date:   Thu Aug 22 10:07:21 2013 -0400

    Start notes on Mars as a base

diff --git a/mars.txt b/mars.txt
new file mode 100644
index 0000000..df0654a
--- /dev/null
+++ b/mars.txt
@@ -0,0 +1 @@
+Cold and dry, but everything is my favorite color
~~~
{: .output}


이런 방식으로, 
연쇄 커밋 사슬을 구성할 수 있다. 
가장 최근 사슬의 끝값은 `HEAD`로 조회된다; 
`~` 표기법을 사용하여 이전 커밋을 조회할 수 있다. 
그래서 `HEAD~1`("head 마이너스 1"으로 읽는다.)은 "바로 앞선 커밋"을 의미하고, 
`HEAD~123`은 지금 있는 위치에서 123번째 이전 수정으로 간다는 의미가 된다.

커밋된 것을 `git log` 명령어로 화면에 뿌려주는 숫자와 문자로 구성된 긴 문자열을 사용하여 조회할 수도 있다. 
변경사항에 대해서 중복되지 않는 ID로 "중복되지 않는(unique)"의 의미는 정말 유일하다는 의미다: 
특정 컴퓨터에 있는 임의 파일 집합에 대한 모든 변경사항은 중복되지 않는 40-문자 식별자가 붙어있다. 
첫번째 커밋은 ID로 f22b25e3233b4645dabd0d81e651fe074bd8e73b 이 주어졌다. 
그래서 다음과 같이 시도하자:


~~~
$ git diff f22b25e3233b4645dabd0d81e651fe074bd8e73b mars.txt
~~~
{: .language-bash}

~~~
diff --git a/mars.txt b/mars.txt
index df0654a..93a3e13 100644
--- a/mars.txt
+++ b/mars.txt
@@ -1 +1,4 @@
 Cold and dry, but everything is my favorite color
+The two moons may be a problem for Wolfman
+But the Mummy will appreciate the lack of humidity
+An ill-considered change
~~~
{: .output}

올바든 정답이지만, 
난수 40-문자로 된 문자열을 타이핑하는 것은 매우 귀찮은 일이다. 
그래서 Git 앞의 몇개 문자만으로도 사용할 수 있게 했다:

~~~
$ git diff f22b25e mars.txt
~~~
{: .language-bash}

~~~
diff --git a/mars.txt b/mars.txt
index df0654a..93a3e13 100644
--- a/mars.txt
+++ b/mars.txt
@@ -1 +1,4 @@
 Cold and dry, but everything is my favorite color
+The two moons may be a problem for Wolfman
+But the Mummy will appreciate the lack of humidity
+An ill-considered change
~~~
{: .output}

좋았어요! 
파일에 변경사항을 저장할 수 있고 변경된 것을 확인할 수 있다. 
어떻게 옛 버젼 파일을 되살릴 수 있을까? 
우연히 파일을 덮어썼다고 가정하자:


~~~
$ nano mars.txt
$ cat mars.txt
~~~
{: .language-bash}

~~~
We will need to manufacture our own oxygen
~~~
{: .output}

이제 `git status`를 통해서 파일이 변경되었다고 하지만, 
변경사항은 아직 준비영역(Staging area)에 옮겨지지 않은 것으로 확인된다:

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

`git checkout` 명령어를 사용해서 과거에 있던 상태로 파일을 돌려 놓을 수 있다:
~~~
$ git checkout HEAD mars.txt
$ cat mars.txt
~~~
{: .language-bash}

~~~
Cold and dry, but everything is my favorite color
The two moons may be a problem for Wolfman
But the Mummy will appreciate the lack of humidity
~~~
{: .output}

이름에서 유추할 수 있듯이, `git checkout` 명령어는 파일 옛 버젼을 확인하고 갖고 나간다. 즉, 되살린다. 
이 경우 `HEAD`에 기록된 가장 최근에 저장된 파일 버젼을 되살린다. 
좀더 오래된 버젼을 되살리고자 한다면, 대신에 커밋 식별자를 사용한다:


~~~
$ git checkout f22b25e mars.txt
~~~
{: .language-bash}

~~~
$ cat mars.txt
~~~
{: .language-bash}

~~~
Cold and dry, but everything is my favorite color
~~~
{: .output}

~~~
$ git status
~~~
{: .language-bash}

~~~
# On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)
# Changes not staged for commit:
#   (use "git add <file>..." to update what will be committed)
#   (use "git checkout -- <file>..." to discard changes in working directory)
#
#	modified:   mars.txt
#
no changes added to commit (use "git add" and/or "git commit -a")
~~~
{: .output}

변경사항은 준비영역에 머물러 있는 것에 주목한다.
다시, `git checkout` 명령어를 사용해서 이전버젼으로 되돌아 간다:


~~~
$ git checkout HEAD mars.txt
~~~
{: .language-bash}

> ## 헤드(HEAD)를 잃지 말자
>
> Above we used
>
> ~~~
> $ git checkout f22b25e mars.txt
> ~~~
> {: .language-bash}
>
> to revert `mars.txt` to its state after the commit `f22b25e`. But be careful! 
> The command `checkout` has other important functionalities and Git will misunderstand
> your intentions if you are not accurate with the typing. For example, 
> if you forget `mars.txt` in the previous command.
>
> ~~~
> $ git checkout f22b25e
> ~~~
> {: .language-bash}
> ~~~
> Note: checking out 'f22b25e'.
>
> You are in 'detached HEAD' state. You can look around, make experimental
> changes and commit them, and you can discard any commits you make in this
> state without impacting any branches by performing another checkout.
>
> If you want to create a new branch to retain commits you create, you may
> do so (now or later) by using -b with the checkout command again. Example:
>
>  git checkout -b <new-branch-name>
>
> HEAD is now at f22b25e Start notes on Mars as a base
> ~~~
> {: .error}
>
> The "detached HEAD" is like "look, but don't touch" here,
> so you shouldn't make any changes in this state.
> After investigating your repo's past state, reattach your `HEAD` with `git checkout master`.
{: .callout}

실행 취소를 하는 변경을 하기 *전에** 저장소 상태를 확인하는 커밋 번호를 사용해야 한다는 것을 기억하는 것이 중요하다. 
흔한 실수는 커밋 번호를 사용하는 것이다.
아래 예제에서는 커밋 번호가 `f22b25e`인 가장 최신 커밋(`HEAD~1`) 앞의 상태로 다시 되돌리고자 한다:


![Git Checkout](../fig/git-checkout.svg)

그래서, 모두 한군데 놓아보자:
![https://figshare.com/articles/How_Git_works_a_cartoon/1328266](../fig/git_staging.svg)

> ## Simplifying the Common Case
>
> `git status` 출력결과를 주의깊이 읽게 되면,
> 힌트가 포함된 것을 볼 수 있다.
>
> ~~~
> (use "git checkout -- <file>..." to discard changes in working directory)
> ~~~
> {: .language-bash}
>
> 출력결과가 언급하는 바는, 버전 식별자 없이 `git checkout` 명령어를 실행하게 되면 
> `HEAD`에 저장된 상태로 파일을 원복시킨다.
> 더블 대쉬 `--`가 필요한 경우는 명령어 자체로부터 복구회야 되는 파일명을 구별할 때다:
> 없는 경우, 커밋 식별자에 Git은 파일명을 사용한다.
{: .callout}

파일이 하나씩 하나씩 옛 상태로 되돌린다는 사실이 사람들이 작업을 조직하는 방식에 변화를 주는 경향이 있다. 
모든 것이 하나의 큰 문서로 되어있다면, 
나중에 결론부분에 변경사항을 실행취소하지 않고, 소개부분에 변경을 다시 되돌리기가 쉽지 않다(하지만 불가능하지는 않다). 
다른 한편으로 만약 소개부분과 결론부분이 다른 파일에 저장되어 있다면, 
시간 앞뒤로 이동하기가 훨씬 쉽다.


> ## 파일 이전 버젼 복구하기
>
> 정훈이가 몇주동안 작업한 파이썬 스크립트에 변경을 했고,
> 오늘 아침 정훈이가 작업한 변경사항이 스크립트를 "망가 먹어서" 더이상 실행이 되지 않는다.
> 복도 없이, 버그를 고치는데 1시간 이상 소모했다...
>
> 다행스럽게도, Git을 사용한 프로젝트 버젼을 추적하고 있었다!
> 다음 아래 명령어 중 어떤 것이 `data_cruncher.py`로 불리는 파이썬 스크립트 가장 최근 버젼을 
> 복구하게 할까요?
> 
>
> 1. `$ git checkout HEAD`
>
> 2. `$ git checkout HEAD data_cruncher.py`
>
> 3. `$ git checkout HEAD~1 data_cruncher.py`
>
> 4. `$ git checkout <unique ID of last commit> data_cruncher.py`
>
> 5. Both 2 and 4
{: .challenge}

> ## 커밋 되돌리기(Reverting a Commit)
>
> 정훈이는 동료와 함께 파이썬 코드를 협업해서 작성하고 있다.
> 그룹 저장소에 마지막으로 커밋한 것이 잘못된 것을 알게 되서,
> 실행취소하여 원복하고자 한다.
> 정훈이는 실행취소를 올바르게 해서 그룹저장소를 사용하는 
> 모든 구성원이 제대로된 변경사항을 가지고 작업을 계속하길 원한다.
> `git revert [잘못된 커밋 ID]` 명령어는 정훈이가 이전에 잘못 커밋했던
> 작업에 대해 실행취소하는 커밋을 새로 생성시킨다.
> 따라서, `git revert`는 `git checkout [커밋 ID]`와 다른데
> 이유는 `checkout`이 그룹 저장소에 커밋되지 않는 로컬 변경사항에 
> 대해서 적용된다는 점에서 차이가 난다.
> 정훈이가 `git revert`를 사용할 올바른 절차와 설명이 아래에 나와있다.
> 빠진 명령어가 무엇일까?
>
> 1. `________ # 커밋 ID를 찾을 수 있도록 Git 프로젝트 이력을 살펴본다.
>
> 2. ID를 복사한다. (ID의 첫 문자 몇개만 사용한다. 예를 들어, 0b1d055).
>
> 3. `git revert [커밋 ID]`
>
> 4. 새로운 커밋 메시지를 타이핑한다.
>
> 5. 저장하고 종료한다.
{: .challenge}

> ## 작업흐름과 이력 이해하기
>
> 다음 마지막 명령의 출력결과는 무엇일까?
>
> ~~~
> $ cd planets
> $ echo "Venus is beautiful and full of love" > venus.txt
> $ git add venus.txt
> $ echo "Venus is too hot to be suitable as a base" >> venus.txt
> $ git commit -m "Comment on Venus as an unsuitable base"
> $ git checkout HEAD venus.txt
> $ cat venus.txt #this will print the contents of venus.txt to the screen
> ~~~
> {: .language-bash}
>
> 1. ~~~
>    Venus is too hot to be suitable as a base
>    ~~~
>    {: .output}
> 2. ~~~
>    Venus is beautiful and full of love
>    ~~~
>    {: .output}
> 3. ~~~
>    Venus is beautiful and full of love
>    Venus is too hot to be suitable as a base
>    ~~~
>    {: .output}
> 4. ~~~
>    Error because you have changed venus.txt without committing the changes
>    ~~~
>    {: .output}
>
> > ## 해법
> >
> > 정답은 2. 왜냐하면, `git add venus.txt`가 `Venus is too hot to be suitable as a base` 행을 
> > 추가하기 전에만 적용된다. `git checkout`이 실행될 때 반영이 되지 않아서 그렇다.
> > `git commit` 명령어에  `-a` 플래그를 사용하게 되면 이런 손실을 막을 수 있다.
> {: .solution}
{: .challenge}

> ## `git diff` 이해 확인하기
>
>  `git diff HEAD~3 mars.txt` 명령어를 고려해 보자.
>  이 명령어를 실행하게 되면 실행결과로 예상하는 바를 말해보세요.
>  명령어를 실행하게 되면 어떤 일이 발생하는가? 그리고 이유는 무엇인가?
>
> 또 다른 명령어 `git diff [ID] mars.txt`를 시도해 보자. 
> 여기서, [ID]를 가장 최근 커밋 식별자로 치환한다.
> 무슨 일이 생길까? 그리고 실제로 생긴 일은 무엇인가?
{: .challenge}

> ## 준비 단계 변경사항(Staged Changes) 제거하기
>
> `git checkout` 명령어를 통해서 준비영역으로 올라오지 않은 변경사항이 있을 대, 이전 커밋을 복구할 수 있었다.
> 하지만, `git checkout`은 준비영역에 올라왔지만, 커밋되지 않는 변경사항에 대해서도 동작한다.
> `mars.txt` 파일에 변경사항을 만들고, 변경사항을 추가하고 나서, 
> `git checkout` 명령어를 사용하게 되면 변경사항이 사라졌는지 살펴보자.
{: .challenge}

> ## 변경 이력 탐색과 요약
>
> 변경 이력 탐색은 Git에 있어 중요한 부분 중의 하나로, 
> 특히 커밋이 수개월 전에 이뤄졌다면, 올바른 커밋 ID를 찾는 것이 종종 크나큰 도전과제가 된다.
>
> `planets` 프로젝트가 50 파일 이상으로 구성되었다고 상상해 보자.
> 
> `mars.txt` 파일에 특정 텍스트가 변경된 커밋을 찾고자 한다.
> `git log`를 타이핑하게 되면 매우 긴 목록이 출력된다.
> 어떻게 하면 검색범위를 좁힐 수 있을까?
>
> `git diff` 명령어가 특정 파일만 탐색할 수 있단느 점을 상기하자.
> 예를 들어, `git diff mars.txt`. 이 문제에 유사한 아이디어를 적용해 보자.
>
> ~~~
> $ git log mars.txt
> ~~~
> {: .language-bash}
>
> 불행하게도 커밋 메시지 일부는 매우 애매모호하다. 예를 들어, `update files`.
> 어떻게 하면 파일을 잘 검색할 수 있을까?
>
> `git diff`, `git log` 명령어 모두 매우 유용하다. 두 명령어 모두 변경이력의 다른 부분을 요약해준다.
> 둘을 조합하는 것은 가능할까? 다음 명령어를 실행해 보자:
>
> ~~~
> $ git log --patch mars.txt
> ~~~
> {: .language-bash}
>
> 엄청 긴 출력 목록이 나타난다. 각 커밋마다 커밋 메시지와 차이가 쭉 출력된다.
>
> 질문: 다음 명령어는 무슨 작업을 수행할까요?
>
> ~~~
> $ git log --patch HEAD~3 *.txt
> ~~~
> {: .language-bash}
{: .challenge}
