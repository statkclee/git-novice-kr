---
title: GitHub 원격작업
teaching: 30
exercises: 0
questions:
- "웹상에서 다른 분들과 내가 작업(변경)한 것을 공유할 수 있을까?"
objectives:
- "원격 저장소가 무엇인지 그리고 왜 원격 저장소가 유용한지 설명한다."
- "원격 저장소에서 변경사항을 푸쉬(push)와 풀(pull)한다."
keypoints:
- "로컬 Git 저장소를 하나 이상 원격 저장소에 연결시킬 수 있다."
- "SSH 설정법을 배우기 전까지 HTTPS 프로토콜을 사용해서 원격 저장소에 연결한다."
- "`git push` 명령어는 로컬 저장소의 변경사항을 원격 저장소로 복제한다."
- "`git pull` 명령어는 원격 저장소의 변경사항을 로컬 저장소로 복제한다."
---

버젼 제어(version control)는 다른 사람과 협업할 때 진정으로 다가온다. 
우리는 이미 버젼 제어를 위해 필요한 작업 대부분을 수행했다; 
한가지 빠진 것은 한 저장소에서 다른 저장소로 변경사항을 복사하는 것이다.

Git 같은 시스템은 임의 두 저장소 사이에 작업을 옮길 수 있는 기능을 제공한다.
하지만, 실무에서 다른 사람의 노트북이나 PC보다는 중앙 허브에 웹 방식으로 하나의 원본을 두고 사용하는 것이 가장 쉽다. 
대부분의 프로그래머는 프로그램 마스터 원본을 
[GitHub](http://github.com),
[BitBucket](http://bitbucket.org),
[GitLab](http://gitlab.com/) 호스팅 서비스에 두고 사용한다; 
이번 학습 마지막 부분에서 이러한 접근법의 장점과 단점을 살펴본다.

세상 사람들과 현재 프로젝트에서 변경한 사항을 공유하는 것에서부터 시작해보자. 
GitHub에 로그인하고 나서, 우측 상단 아이콘을 클릭해서 `planets` 이름으로 신규 저장소를 생성한다:


![Creating a Repository on GitHub (Step 1)](../fig/github-create-repo-01.png)

저장소 이름을 "planets"으로 만들고 "Create Repostiory"를 클릭한다:

![Creating a Repository on GitHub (Step 2)](../fig/github-create-repo-02.png)

저장소가 생성되자 마자, GitHub는 URL을 가진 페이지와 로컬 저장소 환경설정 방법에 대한 정보를 화면에 출력한다:

![Creating a Repository on GitHub (Step 3)](../fig/github-create-repo-03.png)

다음 명령어가 실제로 GitHub 서버에서 자동으로 수행된 것이다:

~~~
$ mkdir planets
$ cd planets
$ git init
~~~
{: .language-bash}

`mars.txt` 파일을 추가하고 커밋한 이전 [학습](./04-changes.html)을 상기한다면, 
로컬 저장소는 다음과 같이 도식적으로 표현할 수 있다:


![The Local Repository with Git Staging Area](../fig/git-staging-area.svg)

이제 저장소가 두개로 늘어서, 도식적으로 표현하면 다음과 같다:

![Freshly-Made GitHub Repository](../fig/git-freshly-made-github-repo.svg)

현재 로컬 저장소는 여전히 `mars.txt` 파일에 대한 이전 작업정보를 담고 있다. 
하지만, GitHub의 원격 저장소에는 아직 어떠한 파일도 담고 있지는 않다:

다음 단계는 두 저장소를 연결하는 것이다. 
로컬 저장소를 위해서 GitHub 저장소를 [원격(remote)]({{ page.root }}/reference.html#remote)으로 만들어 두 저장소를 연결한다. 
GitHub의 저장소 홈페이지에 식별하는데 필요한 문자열이 포함되어 있다:

![Where to Find Repository URL on GitHub](../fig/github-find-repo-string.png)

SSH에서 HTTPS로 [프로토콜(protocol)]({{ page.root }}/reference.html#protocol)을 변경하려면 'HTTPS' 링크를 클릭한다.


> ## HTTPS vs. SSH
>
> 부가적인 설정이 필요하지 않아서 여기서는 HTTPS를 사용한다. 
> 워크샵 후에 SSH 접근 설정을 원할지도 모른다. 
> SSH 접근이 좀더 안전하다. 
> [GitHub](https://help.github.com/articles/generating-ssh-keys),
> [Atlassian/BitBucket](https://confluence.atlassian.com/display/BITBUCKET/Set+up+SSH+for+Git), 
> [GitLab](https://about.gitlab.com/2014/03/04/add-ssh-key-screencast/)의 훌륭한 지도서 중 하나를 따라하는 것도 좋다.
> GitLab은 온라인 동영상도 제공한다.
{: .callout}

![Changing the Repository URL on GitHub](../fig/github-change-repo-string.png)

웹 브라우져에서 URL을 복사하고 나서, 로컬 컴퓨터 `planets` 저장소로 가서 다음 명령어를 실행한다:

~~~
$ git remote add origin https://github.com/vlad/planets.git
~~~
{: .language-bash}

Make sure to use the URL for your repository rather than Vlad's: the only
difference should be your username instead of `vlad`.

Vlad가 아니고 여러분 저장소의 URL을 사용했는지 확인한다:
유일한 차이점은 `vlad` 대신에 여러분의 사용자이름(username)이다.

`git remote -v` 실행해서 명령어가 제대로 작동했는지 확인한다:

~~~
$ git remote -v
~~~
{: .language-bash}

~~~
origin   https://github.com/vlad/planets.git (push)
origin   https://github.com/vlad/planets.git (fetch)
~~~
{: .output}


`origin` 이름이 원격 저장소에 대한 로컬 별명이다. 
원한다면 다른 명칭을 사용할 수도 있지만, `origin` 이름이 가장 일반적인 선택이다.

별명이 `origin`으로 설정되면, 
다음 명령어가 변경사항을 로컬 저장소에서 GitHub 원격 저장소로 밀어 넣어 푸쉬(push)한다:

~~~
$ git push origin master
~~~
{: .language-bash}

~~~
Counting objects: 9, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (6/6), done.
Writing objects: 100% (9/9), 821 bytes, done.
Total 9 (delta 2), reused 0 (delta 0)
To https://github.com/vlad/planets
 * [new branch]      master -> master
Branch master set up to track remote branch master from origin.
~~~
{: .output}

> ## 프록시(Proxy)
>
> 만약 연결된 네트워크가 프록시를 사용한다면, 
> "Could not resolve hostname" 오류 메시지로 인해서 마지막 명령어가 실패할 가능성이 있다. 
> 이 문제를 해결하기 위해서, 프록시에 대한 정보를 Git에 전달할 필요가 있다:
>
> ~~~
> $ git config --global http.proxy http://user:password@proxy.url
> $ git config --global https.proxy http://user:password@proxy.url
> ~~~
> {: .language-bash}
>
> 프록시를 사용하지 않는 또다른 네트워크에 연결될 때, 
> Git에게 프록시 기능을 사용하지 않도록 다음 명령어를 사용하여 일러준다:
>
> ~~~
> $ git config --global --unset http.proxy
> $ git config --global --unset https.proxy
> ~~~
> {: .language-bash}
{: .callout}

> ## 비밀번호 관리자(password manager)
>
> 운영체제 위에 비밀번호 관리자(password manager)가 설정되어 있다면, 
> 사용자이름(username)과 비밀번호(passord)가 필요로 할 때,
> `git push` 명령어가 이를 사용하려 한다. 
> "Git Bash on Windows"를 사용하면 기본 디폴트 행동이다.
> 관리자 비밀번호를 사용하는 대신에, 
> 터미널에서 사용자이름과 비밀번호를 입력하려면, 
> `git push`를 실행하기 전에 터미널에서 다음과 같이 타이핑한다:
>
> ~~~
> $ unset SSH_ASKPASS
> ~~~
> {: .language-bash}
>
> [git uses `SSH_ASKPASS` for all credential entry](https://git-scm.com/docs/gitcredentials#_requesting_credentials)에도 불구하고,
> SSH나 HTTPS를 경유하여 Git을 사용하든 `SSH_ASKPASS`을 설정하고 싶지 않을 수도 있다.
> 
> `~/.bashrc` 파일 하단에 `unset SSH_ASKPASS`을 추가해서 
> Git으로 하여금 사용자명과 비밀번호를 사용하도록 기본설정으로 둘 수도 있다.
{: .callout}

이제 로컬 저장소와 원격 저장소는 다음과 같은 상태가 된다:

![GitHub Repository After First Push](../fig/github-repo-after-first-push.svg)

> ## '-u' 플래그(flag) 
>
> Git 문서에서 `git push`과 함께 사용되는 `-u` 옵션을 볼 수 있다.
> `git branch` 명령어에 대한 `--set-upstream-to` 옵션과 동의어에 해당되는 옵션이다.
> 원격 브랜치를 현재 브랜치와 연결시키는데 사용된다. 그래서 `git pull` 명령어가
> 아무런 인자없이 사용될 수 있다.
> 원격 저장소가 설정되면, `git push -u origin master` 명령어만 실행시키면 연결작업이 완료된다.
{: .callout}

또한, 원격 저장소에서 로컬 저장소로 변경사항을 풀(pull)해서 가져올 수도 있다:

~~~
$ git pull origin master
~~~
{: .language-bash}

~~~
From https://github.com/vlad/planets
 * branch            master     -> FETCH_HEAD
Already up-to-date.
~~~
{: .output}

이 경우 가져오기 하는 풀(pull)은 아무런 결과가 없는데, 이유는 두 저장소가 이미 동기화가 되어서다. 
하지만, 만약 누군가 GitHub 저장소에 변경사항을 푸쉬했다면, 상기 명령어는 변경된 사항을 로컬 저장소로 다운로드한다.

> ## GitHub GUI
>
> GitHub 웹사이트에서 `planets` 저장소를 찾아간다.
> `Code` 탭아래 "XX commits"("XX"는 숫자) 텍스트를 클릭한다. 
> 각 커밋 우측의 버튼 세개 여기 저기 둘러보고, 클릭해 본다.
> 버튼을 눌러서 어떤 정보를 모을 수 있거나 탐색할 수 있는가?
> 쉘에서 동일한 정보를 어떻게 얻을 수 있을까?
> 
> > ## 해답
> > (클립보드 그림을 갖는) 가장 좌측 버튼은 클립보드에 커밋 식별자 전체를 복사한다.
> > 쉘에서 ```git log``` 명령어가 각 커밋에 대한 전체 커밋 식별자를 보여준다.
> >
> > 중간 버튼을 클릭하게 되면, 특정 커밋으로 변경한 내용 전체를 확인할 수 있다.
> > 녹색 음영선은 추가를 붉은색 음영선은 삭제를 의미한다.
> > 쉘에서 동일한 작업을 ```git diff```로 할 수 있다.
> > 특히, ```git diff ID1..ID2```(ID1와 ID2은 커밋 식별자다) 명령어(즉, ```git diff a3bf1e5..041e637```)는 두 커밋 사이 차이를 보여준다.
> >
> > 가장 우측 버튼은 커밋 당시에 저장소 모든 파일을 보여준다.
> > 쉘로 이런 작업을 수행하려면, 해당 시점의 저장소를 checkout 해야 한다.
> > 쉘에서 ```git checkout ID```(여기서 ID는 살펴보려고 하는 커밋 식별자) 명령어를 실행하면 된다.
> > checkout 하게 되면, 나중에 저장소를 올바른 상태로 되돌려 놓아야 된다는 것을 기억해야 됩니다. 
> {: .solution}
{: .challenge}

> ## GitHub 시간도장(Timestamp)
>
> GitHub에 원격저장소를 생성하라.
> 로컬 저장소의 콘텐츠를 원격 저장소로 푸쉬하라.
> 로컬 저장소에 변경사항을 만들고, 변경사항을 푸쉬하라.
> 방금 생성한 GitHub 저장소로 가서 
> GitHub 변경사항에 대한 [시간도장(timestamps)](reference.html#timestamp)을 살펴본다. 
> GitHub이 시간정보를 어떻게 기록하는가? 왜 그런가?
>
> > ## 해답
> > GitHub은 시간도장을 사람이 읽기 쉬운 형태로 표시한다(즉, "22 hours ago" 혹은 "three weeks ago").
> > 하지만, 시간도장을 이리저리 살펴보면, 파일의 마지막 변경이 발생된 정확한 시간을 볼 수 있다.
> {: .solution}
{: .challenge}

> ## 푸쉬(Push) vs. 커밋(Commit)
>
> 이번 학습에서, "git push" 명령어를 소개했다.
> "git push" 명령어가 "git commit" 명령어와 어떻게 다른가?
>
> > ## 해답
> > 
> > 변경 사항을 푸쉬하면, 로컬에서 변경한 사항을 원격 저장소와 상호협의하여 최신 상태로 갱신한다.
> > (흔히 다른 사람 변경시킨 것을 공유하는 것도 이에 해당된다.)
> > 커밋은 로컬 저장소만 갱신한다는 점에서 차이가 난다.
> {: .solution}
{: .challenge}

> ## 원격 설정 고치기 Fixing Remote Settings
>
> 원격 URL에 오탈자가 발생되는 일이 실무에서 흔히 발생된다.
> 이번 연습문제는 이런 유형의 이슈를 어떻게 고칠 수 있느냐에 대한 것이다.
> 먼저 잘못된 URL을 원격(remote)에 추가하면서 시작해 보자.
>
> ~~~
> git remote add broken https://github.com/this/url/is/invalid
> ~~~
> {: .language-bash}
>
> `git remote`로 추가할 때 오류를 받았나요?
> 원격 URL을 적법한지 확인해 주는 명령어를 생각해 낼 수 있나요?
> URL을 어떻게 수정할 수 있을까요? (팁: `git remote -h`를 사용한다.)
> 이번 연습문제를 수행한 다음에 원격(remote)를 지워버리는 것을 잊지말자.
>
> > ## 해답
> > 원격(remote)를 추가할 때 어떤 오류 메시지도 볼 수 없다. (원격 remote 를 추가하는 것은 Git에게 알려주기만 할 뿐 아직 사용하지는 않았기 때문이다.) 
> > ```git push``` 명령어를 사용하자마자, 오류 메시지를 보게 된다.
> > ```git remote set-url``` 명령어를 통해서 잘못된 원격 URL을 바꿔 문제를 해결하게 된다.
> {: .solution}
{: .challenge}

> ## GitHub 라이선스와 README 파일
>
> In this section we learned about creating a remote repository on GitHub, but when you initialized your
> GitHub repo, you didn't add a README.md or a license file. If you had, what do you think would have happened when
> you tried to link your local and remote repositories?
> 
> 이번 학습에서 GitHub에 원격 저장소를 생성하는 방법을 학습했다.
> 하지만, GitHub 저장소를 초기화할 때 `README.md` 혹은 라이선스 파일을 추가하지 않았다.
> 로컬 저장소와 원격 저장소를 연결시킬 때 두 파일을 갖게 되면 무슨 일이 발생될 것으로 생각하십니까?
>
> > ## 해답
> > 이런 경우, 관련없는 이력때문에 병합 충돌(merge conflict)이 발생된다.
> > GitHub에서 `README.md` 파일을 생성시키고 원격 저장소에서 커밋작업을 수행한다.
> > 로컬 저장소로 원격 저장소를 풀(pull)로 땡겨오면, Git이 `origin`과 공유되지 않는 이력을 탐지하고 병합(merge)를 거부해 버린다.
> > ~~~
> > $ git pull origin master
> > ~~~
> > {: .language-bash}
> > 
> > ~~~
> > From https://github.com/vlad/planets
> >  * branch            master     -> FETCH_HEAD
> >  * [new branch]      master     -> origin/master
> > fatal: refusing to merge unrelated histories
> > ~~~
> > {: .output}
> > 
> > `--allow-unrelated-histories` 옵션으로 두 저장소를 강제로 병합(merge)시킬 수 있다.
> > 이런 옵션을 사용할 때 주의한다. 병합하기 전에 로컬저장소와 원격저장소의 콘텐츠를 면밀히 조사한다.
> > ~~~
> > $ git pull --allow-unrelated-histories origin master
> > ~~~
> > {: .language-bash}
> > 
> > ~~~
> > From https://github.com/vlad/planets
> >  * branch            master     -> FETCH_HEAD
> > Merge made by the 'recursive' strategy.
> >  README.md | 1 +
> >  1 file changed, 1 insertion(+)
> >  create mode 100644 README.md
> > ~~~
> > {: .output}
> > 
> {: .solution}
{: .challenge}
