---
title: 충돌 (Conflicts)
teaching: 15
exercises: 0
questions:
- "본인 변경사항이 다른 누군가의 변경사항과 충돌나는 경우 어떻게 해야 하나요?"
objectives:
- "충돌이 무엇이고, 언제 생기는지를 설명한다."
- "병합(merge)로부터 생기는 충돌을 해결한다."
keypoints:
- "충돌은 2명 혹은 그 이상의 사람들이 동시에 동일한 파일에 변경사항을 가할 때 발생된다."
- "버전제어 시스템은 다른 사람의 변경사항을 그냥 덮어쓰게 하는 것을 허락하지 않고, 충돌나는 곳을 강조해서 해결될 수 있도록 한다."
---

사람들이 병렬로 작업을 할 수 있게 됨에 따라, 
누군가 다른 사람 작업영역에 발을 들여 넣을 가능성이 생겼다.
혼자서 작업할 경우에도 이런 현상이 발생한다: 
소프트웨어 개발을 개인 노트북과 연구실 서버에서 작업한다면, 
각 작업본에 다른 변경사항을 만들 수 있다. 
버젼 제어(version control)가 겹치는 변경사항을 
[해결(resolve)]({{ page.root }}/reference.html#resolve)하는 툴을 제공함으로서, 
이러한 [충돌(conflicts)]({{ page.root }}/reference.html#conflicts)을 관리할 수 있게 돕는다.

충돌을 어떻게 해소할 수 있는지 확인하기 위해서, 
먼저 파일을 하나 생성하자. 
`mars.txt` 파일은 현재 두 협업하는 사람의 `planets` 저장소 사본에서는 다음과 같이 보인다:


~~~
$ cat mars.txt
~~~
{: .language-bash}

~~~
Cold and dry, but everything is my favorite color
The two moons may be a problem for Wolfman
But the Mummy will appreciate the lack of humidity
~~~
{: .output}

파트너 사본에만 한 줄을 추가하자:

~~~
$ nano mars.txt
$ cat mars.txt
~~~
{: .language-bash}

~~~
Cold and dry, but everything is my favorite color
The two moons may be a problem for Wolfman
But the Mummy will appreciate the lack of humidity
This line added to Wolfman's copy
~~~
{: .output}

그리고 나서, 변경사항을 GitHub에 푸쉬하자:

~~~
$ git add mars.txt
$ git commit -m "Add a line in our home copy"
~~~
{: .language-bash}

~~~
[master 5ae9631] Add a line in our home copy
 1 file changed, 1 insertion(+)
~~~
{: .output}

~~~
$ git push origin master
~~~
{: .language-bash}

~~~
Counting objects: 5, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 352 bytes, done.
Total 3 (delta 1), reused 0 (delta 0)
To https://github.com/vlad/planets
   29aba7c..dabb4c8  master -> master
~~~
{: .output}

이제 또다른 파트너가 GitHub에서 갱신(update)하지 *않고*,
본인 사본에 다른 변경사항을 작업한다:

~~~
$ nano mars.txt
$ cat mars.txt
~~~
{: .language-bash}

~~~
Cold and dry, but everything is my favorite color
The two moons may be a problem for Wolfman
But the Mummy will appreciate the lack of humidity
We added a different line in the other copy
~~~
{: .output}

로컬 저장소에 변경사항을 커밋할 수 있다:

~~~
$ git add mars.txt
$ git commit -m "Add a line in my copy"
~~~
{: .language-bash}

~~~
[master 07ebc69] Add a line in my copy
 1 file changed, 1 insertion(+)
~~~
{: .output}

하지만, Git이 GitHub에는 푸쉬할 수 없게 한다:

~~~
$ git push origin master
~~~
{: .language-bash}

~~~
To https://github.com/vlad/planets.git
 ! [rejected]        master -> master (non-fast-forward)
error: failed to push some refs to 'https://github.com/vlad/planets.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Merge the remote changes (e.g. 'git pull')
hint: before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
~~~
{: .output}

![충돌하는 변경사항](../fig/conflict.svg)

Git이 푸쉬를 거절한다.
이유는 로컬 브랜로 반영되지 않는 신규 업데이터트가 원격 저장소에 있음을 Git이 탐지했기 때문이다.
즉, 본인이 작업한 변경사항이 다른 사람이 작업한 변경사항과 중첩되는 것을 Git이 탐지해서, 
앞에서 작업한 것을 뭉개지 않도록 정지시킨다.
이제 해야될 작업은 GitHub에서 변경사항을 풀(Pull)해서 가져오고, 
현재 작업중인 작업본과 [병합(merge)]({{ page.root }}/reference.html#merge)해서 푸쉬한다. 
풀(Pull)부터 시작하자:

~~~
$ git pull origin master
~~~
{: .language-bash}

~~~
remote: Counting objects: 5, done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 1), reused 3 (delta 1)
Unpacking objects: 100% (3/3), done.
From https://github.com/vlad/planets
 * branch            master     -> FETCH_HEAD
Auto-merging mars.txt
CONFLICT (content): Merge conflict in mars.txt
Automatic merge failed; fix conflicts and then commit the result.
~~~
{: .output}

`git pull` 명령어는 로컬 저장소를 갱신할 때 원격 저장소에 이미 반영된 변경사항을 포함시키도록 한다.
원격 저장소 브랜치에서 변경사항을 가져온(fetch) 후에, 
로컬 저장소 사본의 변경사항이 원격 저장소 사본과 겹치는 것을 탐지해냈다.
따라서, 앞서 작업한 것이 뭉개지지 않도록 서로 다른 두 버젼의 병합(merge)을 승인하지 않고 거절한 것이다.
해당 파일에 충돌나는 부분을 다음과 같이 표식해 놓는다:

~~~
$ cat mars.txt
~~~
{: .language-bash}

~~~
Cold and dry, but everything is my favorite color
The two moons may be a problem for Wolfman
But the Mummy will appreciate the lack of humidity
<<<<<<< HEAD
We added a different line in the other copy
=======
This line added to Wolfman's copy
>>>>>>> dabb4c8c450e8475aee9b14b4383acc99f42af1d
~~~
{: .output}

`<<<<<<< HEAD`으로 시작되는 부분에 본인 변경사항이 나와있다. 
Git이 자동으로 `=======`을 넣어서 충돌나는 변경사항 사이에 구분자로 넣고,
`>>>>>>>`기호는 GitHub에서 다운로드된 파일 내용의 마지막을 표시한다. 
(`>>>>>>>` 표시자 다음에 문자와 숫자로 구성된 문자열로 방금 다운로드한 커밋번호도 식별자로 제시한다.)

파일을 편집해서 표시자/구분자를 제거하고 변경사항을 일치하는 것은 전적으로 여러분에게 달려있다. 
원하는 무엇이든지 할 수 있다: 
예를 들어, 로컬 저장소의 변경사항을 반영하든, 
원격 저장소의 변경사항을 반영하든, 
로컬과 원격 저장소의 내용을 대체하는 새로운 것을 작성하든, 
혹은 변경사항을 완전히 제거하는 것도 가능하다. 
로컬과 원격 모두 교체해서 다음과 같이 파일이 보이도록 하자:

~~~
$ cat mars.txt
~~~
{: .language-bash}

~~~
Cold and dry, but everything is my favorite color
The two moons may be a problem for Wolfman
But the Mummy will appreciate the lack of humidity
We removed the conflict on this line
~~~
{: .output}

병합을 마무리하기 위해서, 
병합으로 생성된 변경사항을 `mars.txt` 파일에 추가하고 커밋한다:


~~~
$ git add mars.txt
$ git status
~~~
{: .language-bash}

~~~
On branch master
All conflicts fixed but you are still merging.
  (use "git commit" to conclude merge)

Changes to be committed:

	modified:   mars.txt

~~~
{: .output}

~~~
$ git commit -m "Merge changes from GitHub"
~~~
{: .language-bash}

~~~
[master 2abf2b1] Merge changes from GitHub
~~~
{: .output}

이제 변경사항을 GitHub에 푸쉬할 수 있다:

~~~
$ git push origin master
~~~
{: .language-bash}

~~~
Counting objects: 10, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (6/6), done.
Writing objects: 100% (6/6), 697 bytes, done.
Total 6 (delta 2), reused 0 (delta 0)
To https://github.com/vlad/planets.git
   dabb4c8..2abf2b1  master -> master
~~~
{: .output}

Git이 병합하면서 수행한 것을 모두 추적하고 있어서, 
수작업으로 다시 고칠 필요는 없다. 
처음 변경사항을 만든 협력자 프로그래머가 다시 풀하게 되면:


~~~
$ git pull origin master
~~~
{: .language-bash}

~~~
remote: Counting objects: 10, done.
remote: Compressing objects: 100% (4/4), done.
remote: Total 6 (delta 2), reused 6 (delta 2)
Unpacking objects: 100% (6/6), done.
From https://github.com/vlad/planets
 * branch            master     -> FETCH_HEAD
Updating dabb4c8..2abf2b1
Fast-forward
 mars.txt | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)
~~~
{: .output}

병합된 파일을 얻게 된다:

~~~
$ cat mars.txt
~~~
{: .language-bash}

~~~
Cold and dry, but everything is my favorite color
The two moons may be a problem for Wolfman
But the Mummy will appreciate the lack of humidity
We removed the conflict on this line
~~~
{: .output}


다시 병합할 필요는 없는데, 
다른 누군가 작업을 했다는 것을 Git가 알기 때문이다.

충돌을 해소하는 Git 기능은 매우 유용하지만,
충돌해소에는 시간과 노력이 수반되고, 충돌이 올바르게 해소되지 않게 되면
오류가 스며들게 된다.
프로젝트 와중에 상당량의 충돌을 해소하는데 시간을 쓰고 있다고 생각되면,
충돌을 줄일 수 있는 기술적인 접근법도 고려해보는 것이 좋겠다.

- 좀더 자주 upstream을 풀(Pull)하기, 특히 신규 작업을 시작하기 전이라면 더욱 그렇다.
- 작업을 구별하기 위해서 토픽 브랜치를 사용해서, 작업을 완료하면 마스터(master) 브랜치에 병합시킨다.
- 좀더 작게 원자수준 커밋을 한다.
- 논리적으로 적절하다면, 큰 파일을 좀더 작은 것으로 쪼갠다. 그렇게 함으로써 
  두 저작자가 동시에 동일한 파일을 변경하는 것을 줄일 수 있을 듯 싶다.

프로젝트 관리 전략으로 충돌(conflicts)을 최소화할 수도 있다:

- 동료 협력자와 누가 어떤 분야에 책임이 있는지 명확히 한다.
- 동료 협력자와 작업순서를 협의해서, 동일한 라인에 변경사항이 있을 수 있는 작업이 동시에 작업되지 않게 시간차를 둔다.
- 충돌이 문체변동(탭 vs 2 공백) 때문이라면, 프로젝트 관례를 수립하고, 
  코딩 스타일 도구(`htmltidy`, `perltidy`, `rubocop` 등)를 사용해서 필요한 경우 강제한다.


> ## 본인이 생성한 충돌 해소하기
>
> 강사가 생성한 저장소를 복제하세요. 
> 저장소에 새 파일을 추가하고, 
> 기존 파일을 변경하세요. (강사가 변경할 기존 파일이 어느 것인지 알려줄 것이다.) 
> 강사의 말에 따라 충돌을 생성하는 연습을 위해서,
> 저장소에서 변경사항을 가져오도록 풀(Pull)하세요. 
> 그리고 충돌을 해소하고 해결해 보세요.
{: .challenge}

> ## 텍스트 파일이 아닌 충돌
>
> 버젼 제어 저장소의 이미지 파일이나 혹은 다른 텍스트가 아닌 파일에서 충돌이 발생할 때, 
> Git는 무엇을 하나요?
>
> > ## 해답
> >
> > 먼저 시도해 보자. 
> > 드라큘라가 화성 표면에서 사진을 찍어 `mars.jpg`로 저장했다고 가정한다.
> >
> > 화성 이미지 파일이 없다면 다음과 같이 더미 바이너리 파일을 생성할 수도 있다.
> >
> > ~~~
> > $ head --bytes 1024 /dev/urandom > mars.jpg
> > $ ls -lh mars.jpg
> > ~~~
> > {: .language-bash}
> >
> > ~~~
> > -rw-r--r-- 1 vlad 57095 1.0K Mar  8 20:24 mars.jpg
> > ~~~
> > {: .output}
> >
> > `ls` 명령어를 사용해서 파일 크기가 1 킬로바이트임이 확인된다.
> > `/dev/urandom` 특수 파일에서 불러온 임의 바이트로 꽉 차있다.
> >
> > 이제, 드라큘라가 `mars.jpg` 파일을 본인 저장소에 저장한다고 상정한다:
> >
> > ~~~
> > $ git add mars.jpg
> > $ git commit -m "Add picture of Martian surface"
> > ~~~
> > {: .language-bash}
> >
> > ~~~
> > [master 8e4115c] Add picture of Martian surface
> >  1 file changed, 0 insertions(+), 0 deletions(-)
> >  create mode 100644 mars.jpg
> > ~~~
> > {: .output}
> >
> > 늑대인간도 비슷한 시점에 유사한 사진을 추가했다고 가정한다.
> > 늑대인간의 사진은 화성하늘 사진인데, 이름도 `mars.jpg`로 동일하다.
> > 드라큘라가 푸쉬하게 되면 유사한 메시지를 받게 된다: 
> > 
> > ~~~
> > $ git push origin master
> > ~~~
> > {: .language-bash}
> >
> > ~~~
> > To https://github.com/vlad/planets.git
> >  ! [rejected]        master -> master (fetch first)
> > error: failed to push some refs to 'https://github.com/vlad/planets.git'
> > hint: Updates were rejected because the remote contains work that you do
> > hint: not have locally. This is usually caused by another repository pushing
> > hint: to the same ref. You may want to first integrate the remote changes
> > hint: (e.g., 'git pull ...') before pushing again.
> > hint: See the 'Note about fast-forwards' in 'git push --help' for details.
> > ~~~
> > {: .output}
> >
> > 풀을 먼저한 뒤에 충돌나는 것을 해소한다는 것을 학습했다:
> >
> > ~~~
> > $ git pull origin master
> > ~~~
> > {: .language-bash}
> >
> > 이미지나 기타 바이너리 파일에 충돌이 생길 때,
> > Git은 다음과 같은 메시지를 출력한다:
> >
> > ~~~
> > $ git pull origin master
> > remote: Counting objects: 3, done.
> > remote: Compressing objects: 100% (3/3), done.
> > remote: Total 3 (delta 0), reused 0 (delta 0)
> > Unpacking objects: 100% (3/3), done.
> > From https://github.com/vlad/planets.git
> >  * branch            master     -> FETCH_HEAD
> >    6a67967..439dc8c  master     -> origin/master
> > warning: Cannot merge binary files: mars.jpg (HEAD vs. 439dc8c08869c342438f6dc4a2b615b05b93c76e)
> > Auto-merging mars.jpg
> > CONFLICT (add/add): Merge conflict in mars.jpg
> > Automatic merge failed; fix conflicts and then commit the result.
> > ~~~
> > {: .output}
> >
> > 이번에도 충돌 메시지가 `mars.txt`에 나온 것과 거의 동일하다.
> > 하지만, 중요한 추가 라인 한줄이 있다:
> >
> > ~~~
> > warning: Cannot merge binary files: mars.jpg (HEAD vs. 439dc8c08869c342438f6dc4a2b615b05b93c76e)
> > ~~~
> >
> > Git은 자동으로 텍스트 파일에 했던 것처럼 이미지 파일에 충돌지점 표식을 
> > 끼워넣을 수 없다.
> > 그래서 이미지 파일을 편집하는 대신에, 
> > 간직하고자 하는 버전을 쳇아웃(checkout)하고 나서 해당 버전을 추가(add)하고 커밋한다.
> > 
> > 중요한 라인에, `mars.jpg` 두가지 버전에 대해서 
> > 커밋 식별자(commit identifier)를 Git이 제시하고 있다.
> > 현재 작업 버젼은 `HEAD`이고, 늑대인간 작업버전은 `439dc8c0...`이다.
> > 본인 작업버젼을 사용하고자 하면, `git checkout` 명령어를 사용한다:
> > 
> >
> > ~~~
> > $ git checkout HEAD mars.jpg
> > $ git add mars.jpg
> > $ git commit -m "Use image of surface instead of sky"
> > ~~~
> > {: .language-bash}
> >
> > ~~~
> > [master 21032c3] Use image of surface instead of sky
> > ~~~
> > {: .output}
> >
> > 대신에 늑대인간 버젼을 사용하려고 하면, 
> > `git checkout` 명령어를 늑대인간 `439dc8c0` 커밋 식별자와 함께 사용하면 된다:
> >
> > ~~~
> > $ git checkout 439dc8c0 mars.jpg
> > $ git add mars.jpg
> > $ git commit -m "Use image of sky instead of surface"
> > ~~~
> > {: .language-bash}
> >
> > ~~~
> > [master da21b34] Use image of sky instead of surface
> > ~~~
> > {: .output}
> >
> > 이미지 모두 보관할 수도 있다.
> > 동일한 이미지명으로 보관할 수는 없다는 것이 중요하다.
> > 순차적으로 각 버젼을 쳇아웃(checkout)하고 나서 이미지명을 변경한다.
> > 그리고 나서 이름을 변경한 버젼을 추가한다.
> > 먼저, 각 이미지를 쳇아웃하고 이름을 변경하자:
> >
> > ~~~
> > $ git checkout HEAD mars.jpg
> > $ git mv mars.jpg mars-surface.jpg
> > $ git checkout 439dc8c0 mars.jpg
> > $ mv mars.jpg mars-sky.jpg
> > ~~~
> > {: .language-bash}
> >
> > 그리고 나서, `mars.jpg` 이전 파일을 삭제하고 
> > 신규 파일 두개를 추가한다:
> >
> > ~~~
> > $ git rm mars.jpg
> > $ git add mars-surface.jpg
> > $ git add mars-sky.jpg
> > $ git commit -m "Use two images: surface and sky"
> > ~~~
> > {: .language-bash}
> >
> > ~~~
> > [master 94ae08c] Use two images: surface and sky
> >  2 files changed, 0 insertions(+), 0 deletions(-)
> >  create mode 100644 mars-sky.jpg
> >  rename mars.jpg => mars-surface.jpg (100%)
> > ~~~
> > {: .output}
> >
> > 이제 화성 이미지 파일 두개가 저장소에서 확인되지만 `mars.jpg` 파일은 더이상 존재하지 않는다.
> {: .solution}
{: .challenge}

> ## 일반적인 작업 시간 A Typical Work Session
>
> You sit down at your computer to work on a shared project that is tracked in a
> remote Git repository. During your work session, you take the following
> actions, but not in this order:
> 원격 Git 저장소를 활용하여 공동 프로젝트로 작업하는 컴퓨터 앞에 않아있다.
> 작업시간동안에 다음 동작을 취하지만, 작업순서는 다르다:
>
> - *변경한다(make change)*: `numbers.txt` 텍스트 파일에 숫자 `100`을 추가.
> - *원격 저장소 갱신시키기(Update remote)*: 로컬 저장소와 매칭되어 동기화시킴.
> - *축하하기(Celebrate)*: 맥주로 성공을 자축함.
> - *로컬 저장소 갱신시키기(Update local)*: 원격 저장소와 매칭되어 동기화시킴.
> - *변경사항 준비영역으로 보내기(Stage change)*: 커밋대상으로 추가하기.
> - *변경사항(Commit change)*: 로컬 저장소에 커밋하기 
> 
> 어떤 순서로 작업을 수행해야 충돌이 날 가능성을 최소화할 수 있을까?
> 아래표 *action* 칼럼에 순서대로 상기 명령어를 적어 본다.
> 작업 순서를 정했으면, *command* 칼럼에 대응되는 명령어를 적어본다.
> 일부 단계를 시작하는데 도움이 되도록 채워져 있다.
> 
>
> |order|action . . . . . . . . . . |command . . . . . . . . . . |
> |-----|---------------------------|----------------------------|
> |1    |                           |                            |
> |2    |                           | `echo 100 >> numbers.txt`  |
> |3    |                           |                            |
> |4    |                           |                            |
> |5    |                           |                            |
> |6    | Celebrate!                | `AFK`                      |
>
> > ## 해답
> >
> > |order|action . . . . . . |command . . . . . . . . . . . . . . . . . . . |
> > |-----|-------------------|----------------------------------------------|
> > |1    | Update local      | `git pull origin master`                     |
> > |2    | Make changes      | `echo 100 >> numbers.txt`                    |
> > |3    | Stage changes     | `git add numbers.txt`                        |
> > |4    | Commit changes    | `git commit -m "Add 100 to numbers.txt"`     |
> > |5    | Update remote     | `git push origin master`                     |
> > |6    | Celebrate!        | `AFK`                                        |
> >
> {: .solution}
{: .challenge}
