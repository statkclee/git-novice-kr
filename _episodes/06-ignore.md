---
title: 추적대상에서 제외
teaching: 5
exercises: 0
questions:
- "어떻게 하면 Git에게 파일 추적을 하지 못하게 할 수 있을까?"
objectives:
- "특정 파일을 무시하여 관리에서 제외하도록 Git 환경설정."
- "파일을 무시하는 것이 왜 유용한지 설명한다."
keypoints:
- "`.gitignore` 파일에 Git 추적대상에서 제외할 목록을 기록한다."
---

만약 Git가 추적하기 않았으면 하는 파일이 있다면 어떨까요? 
편집기에서 자동 생성되는 백업파일 혹은 자료 분석 중에 생성되는 중간 임시 파일이 좋은 예가 된다. 몇개 마루타 더미(dummy) 파일을 생성하자:

~~~
$ mkdir results
$ touch a.dat b.dat c.dat results/a.out results/b.out
~~~
{: .language-bash}

그려면 Git은 다음을 보여준다:

~~~
$ git status
~~~
{: .language-bash}

~~~
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)

	a.dat
	b.dat
	c.dat
	results/
nothing added to commit but untracked files present (use "git add" to track)
~~~
{: .output}

벼젼 제어 아래 이런 파일을 놓는 것은 디스크 공간 낭비다.
더 좋은 않는 것은, 이런 파일을 모두 관리목록에 넣는 것이 실제적으로 중요한 변경사항을 관리하는데 집중하지 못하게 한다는 것이다.
그래서 Git에게 중요하지 않는 이런 파일을 무시하게 일러준다.

`.gitignore`라는 프로젝트 루트 디렉토리에 파일을 생성해서 무시할 것을 명기함으로써 해당작업을 수행한다:


~~~
$ nano .gitignore
$ cat .gitignore
~~~
{: .language-bash}

~~~
*.dat
results/
~~~
{: .output}

상기 패턴은 `.dat` 확장자를 갖는 임의 파일과 `results` 디렉토리에 있는 모든 것을 무시한다. 
(하지만, 이들 파일 중 일부가 이미 추적되고 있다면, Git은 계속 추적한다.)

`.gitignore` 파일을 생성하자마자, `git status` 출력결과는 훨씬 깨끗해졌다:


~~~
$ git status
~~~
{: .language-bash}

~~~
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)

	.gitignore
nothing added to commit but untracked files present (use "git add" to track)
~~~
{: .output}

이제 Git가 알아차리는 유일한 것은 새로 생성된 `.gitignore` 파일이 된다. 
우리는 이들 파일을 추적하여 관리하지 않는다고 생각할 수도 있지만, 
우리와 저장소를 공유하고 있는 다른 모든 사람도 우리가 추적관리하지 않는 동일한 것을 무시하고 싶을 것이다. 
`.gitignore` 를 추가하고 커밋하자:


~~~
$ git add .gitignore
$ git commit -m "Ignore data files and the results folder."
$ git status
~~~
{: .language-bash}

~~~
# On branch master
nothing to commit, working directory clean
~~~
{: .output}

보너스로, `.gitignore`는 실수로 추적하고 싶지 않는 파일이 저장소에 추가되는 것을 피할 수 있게 돕는다:

~~~
$ git add a.dat
~~~
{: .language-bash}

~~~
The following paths are ignored by one of your .gitignore files:
a.dat
Use -f if you really want to add them.
~~~
{: .output}

만약 `.gitignore` 설정에 우선해서 파일을 추가하려면, 
`git add -f`를 사용해서 강제로 Git에 파일을 추가할 수 있다. 
예를 들어, `git add -f a.dat`.
추적관리되지 않는 파일의 상태를 항상 보려면 다음을 사용한다:


~~~
$ git status --ignored
~~~
{: .language-bash}

~~~
On branch master
Ignored files:
 (use "git add -f <file>..." to include in what will be committed)

        a.dat
        b.dat
        c.dat
        results/

nothing to commit, working directory clean
~~~
{: .output}

> ## 중첩된 파일 추적하지 않기
>
> 디렉토리 구조가 다음과 같다:
>
> ~~~
> results/data
> results/plots
> ~~~
> {: .language-bash}
>
> 어떻게 하면 `results/plots` 만 추적하지 않을 수 있을까? `results/data` 디렉토리는 추적한다.
>
> > ## 해답
> >
> > 대부분의 프로그래밍 이슈와 마찬가지로,
> > 이 문제를 해결하는 몇가지 방식이 있다.
> > `results/plots` 디렉토리 콘텐츠만 추적하지 않기로 한다면,
> > `.gitignore` 파일에 `/plots/` 폴더만 추적하지 않도록 다음과 같이 
> > `.gitignore` 파일을 변경하면 된다:
> >
> > `results/plots/`
> >
> > 대신에 `/results/` 디렉토리에 모든 것을 추적하지 않지만, `results/data`만 추적하고자 한다면,
> > `results/` 폴더를 `.gitignore` 파일에 추가하고 `results/data/` 폴더에 대해서 예외를 생성한다.
> > 다음 도전과제가 이런 유형의 해법을 다루게 된다.
> >
> > 종종 `**` 패턴이 사용하기 수월한데, 다수 디렉토리와 매칭을 지원한다.
> > 예를 들어, `**/results/plots/*`은 루트 디렉토리에 `results/plots` 디렉토리를 추정하기 않게 도니다.
> {: .solution}
{: .challenge}

> ## 특정 파일만 포함시키기
>
> `final.data` 파일만 제외하고 모든 `.data` 파일은 추적하지 않고자 하면 어떻게 하면 될까?
> 힌트: `!` (느낌표 연산자) 부호가 수행하는 작업을 알아본다.
>
> > ## 해법
> >
> > `.gitignore` 파일에 다음 두 줄을 추가한다:
> >
> > ~~~
> > *.data           # 모든 data 파일을 추적하지 않는다.
> > !final.data      # final.data 파일만 예외로 한다.
> > ~~~
> > {: .output}
> >
> > 느낌표 연산자가 앞서 제외된 항목을 포함시키게 한다.
> {: .solution}
{: .challenge}

> ## 디렉토리에 모든 파일 추적하지 않기
>
> 디렉토리 구조가 다음과 같다:
>
> ~~~
> results/data/position/gps/a.data
> results/data/position/gps/b.data
> results/data/position/gps/c.data
> results/data/position/gps/info.txt
> results/plots
> ~~~
> {: .language-bash}
>
> `result/data/position/gps` 디렉토리에 모든 `.data` 파일을 추적하지 않도록 
> `.gitignore` 파일에 규칙을 작성하는데 가장 짧은 규칙은 무엇일까?
> `info.txt` 파일은 추적하자.
>
> > ## 해답
> >
> > `results/data/position/gps` 디렉토리에 `.data`로 끝나는 모든 파일은  `results/data/position/gps/*.data` 규칙으로 매칭된다.
> > `results/data/position/gps/info.txt` 파일은 확장자가 달라서 계속 추적된다.
> {: .solution}
{: .challenge}

> ## 적용 규칙 순서
>
> `.gitignore` 파일에 다음 내용이 담겨있다:
>
> ~~~
> *.data
> !*.data
> ~~~
> {: .language-bash}
>
> 적용 결과는 어떻게 될까?
>
> > ## 해답
> >
> > `!` 연산자는 이전에 정의된 추적제외 패턴을 부정한다.
> > `.gitignore`파일에서 `!*.data` 규칙은 앞서 추적에서 제외한 `.data` 모든 파일 추적제외를 부정한다.
> > 따라서, 어떤 것도 추적제외되지 않고, `.data` 파일 모두가 추적된다.
> >
> {: .solution}
{: .challenge}

> ## 로그 파일
>
> 스크립트를 작성해서 `log_01`, `log_02`, `log_03` 형태의 중간 로그 파일이 많이 생성되었다.
> 로그 파일을 보관하고자 하지만, `git`으로 추적하고 싶지는 않다.
>
> 1. `log_01`, `log_02` ... 형태 모든 파일을 추적 제외하는 `.gitignore` 규칙을 **하나** 작성한다.
>
> 2. `log_01` 형태 마루타 파일을 생성해서 "추적제외 패턴"을 테스트한다.
>
> 3. 종국에 `log_01` 파일이 매우 중요하는 것을 알게 되어서 `.gitignore` 파일을 변경하지 않고 추적되게 추가한다.
>
> 4. 추적하기를 원하지 않지만, `.gitignore`를 통해서 추적제외할 수 있는 파일이 어떤 유형이 있는지 주변 동료와 상의하자.
>
> > ## 해답
> >
> > 1. `log_*`  혹은  `log*` 규칙을 `.gitignore` 파일에 추가한다.
> > 3. `git add -f log_01` 명령어를 사용해서 `log_01` 파일에 대한 추적을 수행한다.
> {: .solution}
{: .challenge}
