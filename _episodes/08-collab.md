---
title: 협업 (Collaborating)
teaching: 25
exercises: 0
questions:
- "버전 제어를 사용해서 어떻게 다른 분들과 협업할 수 있을까요?"
objectives:
- "원격 저장소를 클론한다."
- "공동 저장소에 푸시해서 협업한다."
- "기본적인 협업 작업흐름을 기술한다."
keypoints:
- "`git clone` 명령어는 원격 저장소를 복제해서 로컬 저장소를 생성하는데 `origin` 이라는 원격저장소도 자동으로 설정해준다."
---

For the next step, get into pairs.  One person will be the "Owner" and the other
will be the "Collaborator". The goal is that the Collaborator add changes into
the Owner's repository. We will switch roles at the end, so both persons will
play Owner and Collaborator.

다음 단계로, 짝을 이룬다. 
한 사람이 "소유자"(연습을 시작하는데 사용될 GitHub 저장소 주인)가 되고, 
다른 사람이 "협력자"(소유자 저장소를 복제해서 변경을 하는 사람)가 된다.
목표는 협력자가 변경사항을 소유자 저장소에 추가하는 것이다.
말미에는 역할을 바꿔서 두 사람 모두 소유자와 협력자의 역할을 수행한다.

> ## 혼자 훈련하기
>
> 혼자 힘으로 이번 학습을 쭉 진행해 왔다면, 두번째 터미널을 열어서 계속 진행할 수 있다.> 
> 두번째 윈도우가 여러분의 협력자를 나타내고, 다른 컴퓨터에서 작업하고 있는 것으로 볼 수 있다.
> GitHub 접근권한을 다른 사람에게 줄 필요가 없어졌다.
> 왜냐하면 두 '파트너' 모두 여러분이기 때문이다.
{: .callout}

소유자가 협력자에게 접근권한을 부여할 필요가 있다. 
GitHub에서 오른쪽에 'setting' 버튼을 클릭해서 협력자(`Collaborators`)를 선택하고, 
파트너 이름을 입력한다.

![Adding Collaborators on GitHub](../fig/github-add-collaborators.png)

소유자 저장소에 접근 권한이 부여되면, 
협력자(Collaborator)는 [https://github.com/notifications](https://github.com/notifications)으로 이동한다.
그곳에서 소유자 저장소의 접근을 받아들이면 된다.

다음으로 협력자(Collaborator)는 소유자 저장소 사본을 본인 컴퓨터로 내려받는다.
이런 작업을 "저장소 복제(cloning a repo)"라고 부른다.
소유자의 저장소를 본인 바탕화면(`Desktop`) 폴더에 클론하려면, 협력자는 다음 명령어를 입력한다:

~~~
$ git clone https://github.com/vlad/planets.git ~/Desktop/vlad-planets
~~~
{: .language-bash}

'vlad'를 소유자 사용자이름(저장소를 소유하고 있는 사람)으로 바꾼다.

![After Creating Clone of Repository](../fig/github-collaboration.svg)

앞서 작업했던 것과 정확하게 동일한 방식으로, 
협력자는 이제 소유자의 저장소 클론에서 변경을 마음대로 할 수 있다:

~~~
$ cd ~/Desktop/vlad-planets
$ nano pluto.txt
$ cat pluto.txt
~~~
{: .language-bash}

~~~
It is so a planet!
~~~
{: .output}

~~~
$ git add pluto.txt
$ git commit -m "Add notes about Pluto"
~~~
{: .language-bash}

~~~
 1 file changed, 1 insertion(+)
 create mode 100644 pluto.txt
~~~
{: .output}

그리고 나서, 변경사항을 GitHub의 *소유자 저장소*로 푸쉬한다:

~~~
$ git push origin master
~~~
{: .language-bash}

~~~
Counting objects: 4, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 306 bytes, done.
Total 3 (delta 0), reused 0 (delta 0)
To https://github.com/vlad/planets.git
   9272da5..29aba7c  master -> master
~~~
{: .output}

주목할 점은 `origin` 이라는 원격 저장소를 생성할 필요는 없다:
저장소를 복제(clone)할때 Git이 자동으로  `origin` 이름을 붙여준다. 
(수작업으로 원격 설정을 할 때, 앞에서 왜 `origin` 이름을 사용한 것이 현명한 선택인 이유다.)


이제 GitHub 웹사이트에서 소유자 저장소를 살펴본다(아마도 웹브라우져 다시 고치기를 수행할 필요가 있을 수 있다.)
협력자가 신규 커밋을 한 것을 확인할 수 있다.

소유자 로컬 컴퓨터로 GitHub 원본 저장소의 변경사항을 다운로드하려면, 
소유자는 다음과 같이 입력한다:

~~~
$ git pull origin master
~~~
{: .language-bash}

~~~
remote: Counting objects: 4, done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 3 (delta 0)
Unpacking objects: 100% (3/3), done.
From https://github.com/vlad/planets
 * branch            master     -> FETCH_HEAD
Updating 9272da5..29aba7c
Fast-forward
 pluto.txt | 1 +
 1 file changed, 1 insertion(+)
 create mode 100644 pluto.txt
~~~
{: .output}

이제 저장소 3개 (소유자 로컬 저장소, 협력자 로컬 저장소, GitHub의 소유자 저장소) 모두 동기화 되었다.

> ## 기본적인 협업 작업흐름
>
> 실무에서 협업하는 저장소의 가장 최신 버전을 갖도록 확인하고 확인하는 것이 좋다.
> 어떤 변경을 가하기 전에 `git pull` 명령어를 먼저 실행해야 한다.
> 기본적인 협업 작업흐름은 다음과 같다.
>
> * `git pull origin master` 명령어로 본인 로컬 저장소를 최신상태로 갱신한다.
> * 변경 작업을 수행하고 `git add` 명령어로 준비단계(staging area)로 보낸다.
> * `git commit -m` 명령어로 변경사항을 커밋한다.
> * GitHub에 `git push origin master` 명령어로 변경사항을 업로드한다.
>
> 상당한 변경사항을 포함한 단 한번의 커밋보다 작은 변화를 준 커밋을 많이 하는 것이 좋다:
> 작은 커밋이 가독성도 좋고 리뷰하기도 더 편하다.
{: .callout}

> ## 역할을 바꾸고 반복한다.
> 
> 역할을 바꿔서 전체 과정을 반복한다.
{: .challenge}

> ## 변경사항 리뷰
>
> The Owner pushed commits to the repository without giving any information
> to the Collaborator. How can the Collaborator find out what has changed with
> command line? And on GitHub?
> 협력자에게 어떤 정보도 주지않고 소유자가 저장소에 커밋을 푸쉬했다.
> 협력자는 명령라인으로 무엇이 변경되었는지 어떻게 알 수 있을까요?
>
> > ## 해답
> > 
> > 명령라인에서 협력자는 로컬 저장소에 원격 저장소 변경사항을 
> > ```git fetch origin master``` 명려어를 사용해서 가져올 수 있다.
> > 하지만, 그 자체로 병합(merge)되는 것은 아니다.
> > ```git diff master origin/master``` 명령어를 실행해서,
> > 협력자는 터미널에 변경사항을 확인할 수 있다.
> >
> > GitHub에서도 협력자는 포크된 저장소로 가서 "This branch is 1 commit behind Our-Repository:master."
> > 메시지를 볼 수 있다.
> > `Compare` 아이콘과 링크가 걸려있다.  `Compare` 페이지에서 
> > 협력자는 base fork를 본인 저장소로 변경하고 나서, "compare across forks" 위에 
> > 링크를 클릭한다. 마지막으로 head fork를 주 저장소로 변경한다.
> > 이 작업을 하게 되면 차이가 나는 모든 커밋을 볼 수 있게 된다.
> {: .solution}
{: .challenge}

> ## GitHub에서 변경사항 주석(comment)달기
>
> 협력자는 소유자가 변경한 한 줄에 대해 질문을 가질 수 있고,
> 일부 제안사항도 있다.
>
> GitHub으로 커밋 차이에 대해 주석을 다는 것도 가능하다.
> 파란색 주석 아이콘(comment icon)을 클릭하면 주석 윈도우(comment window)을 열 수 있다.
>
> 협력자는 GitHub 인터페이스를 사용해서 코멘트와 제안을 남길 수 있다.
{: .challenge}

> ## 버전 이력, 백업, 그리고 버전 제어
>
> 일부 백업 소프트웨어는 파일 버전에 대한 이력을 기록하고 있다.
> 도한, 특정 버전을 복구하는 기능도 제공하고 있다.
> 이러한 기능이 버전 제어와 어떻게 다른가?
> 버전제어, Git, GitHub을 사용하는 좋은 점은 무엇인가?
{: .challenge}
