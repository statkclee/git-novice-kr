---
title: 라이선싱 (Licensing)
teaching: 5
exercises: 0
questions:
- "본인 작업에 어떤 라이선싱 정보를 포함해야 하나요?"
objectives:
- "프로젝트 저장소에 라이선싱 정보와 인용정보를 달아두는 것이 왜 중요한지 설명한다."
- "적절한 라이선스를 선정한다."
- "라이선싱과 사회적 기대에 대한 차이점을 설명한다."
keypoints:
- "GPL 소프트웨어를 차용한 사람은 본인 소프트웨어도 GPL 라이선으로 공개하여야 한다; 다른 공개 소프트웨어 대부분은 이러한 요구를 하지는 않는다."
- "크리이에티브 커먼즈 라이선스는 출처표시, 파생 저작물, 공유, 상업화에 대한 요건과 제약사항을 명세하고 있다."
- "변호사가 아닌 분은 맨땅에서 라이선스 저작을 시도하지 말아야 한다."
---

## 소프트웨어 라이선스

소스코드, 원고, 다른 창의적 저작물을 갖는 저장소가 공개될 때,
저장소 기반 디렉토리에 `LICENSE` 혹은 `LICENSE.txt` 파일을 포함해서 콘텐츠가 어떤 라이선스로 이용가능한지를 명확히 기술해야된다.
이유는 소스코드가 창의적 저작물로서, 자동적으로 지적재산(따라서 저작권)보호에 대상에 부합되기 때문이다. 
자유로이 이용가능한 것으로 보여지거나, 명시적으로 광고되는 코드는 그런 보호를 유예하지 *않는다*. 
따라서, 라이선스 문장이 없는 코드를 (재)사용하는 누구나 스스로 위험에 처하게 된다. 
왜냐하면 소프트웨어 코드 저자가 항상 일방향으로 재사용을 불법화할 수 있기 때문이다.
즉, 저작권 소유자가 당신을 저작권법 위반으로 고소할 수 있다.


라이선스는 그렇지 않다면 보유하지 못할 권리를 다른 사람(라이선스 허여자, licensee)에게 부여함으로써 이 문제를 해결한다.
어떤 조건아래서 무슨 권리가 부여될지는 라이선스마다 다소 차이가 난다.
독점적 라이선스와 대조하여, [Open Source Initiative](http://opensource.org/)에서 공인된 
[오픈 라이선스(open licences)](http://opensource.org/licenses/alphabetical)는 최소한 다음에 나온 권리를 모두 부여한다. 
이런 권리를 [오픈 소스 정의(Open Source Definition)](http://opensource.org/osd)로 부른다.:

1. 소스코드는 제약없이 이용가능하고, 사용되고, 재배포될 수 있다.
   종합 배포의 일부로서도 포함된다.
2. 변형 혹은 다른 파생 저작물도 허락되고, 또한 재배포될 수 있다.
3. 이런 권리를 누가 받느냐의 질문이 차별의 조건이 되지 않는다.
   예를 들어 상업적 혹은 학술적처럼 노력 분야에 의해서가 아님도 포함된다.

특히, 지금까지 라이선스 몇개가 인기를 얻고 있는데, 
[choosealicense.com](https://choosealicense.com/) 웹사이트에서 본인 상황에 적합한 
일반적인 라이선스를 선택하는데 도움이 된다. 주요한 고려사항에는 다음이 포함된다:

* 특허권을 주장하고자 하는가?
* 파생 저작물을 배포하는데 다른 사람도 소스코드를 배포하도록 강제할 것인가?
* 라이선싱하는 콘텐트가 소스코드인가?
* 이왕이면 소스코드도 라이선스할 것인가?

적절한 라이선스를 가장 잘 선택하는 것이 상당히 많은 가능한 조합이 있어 주눅이 들 수도 있다. 
실무에서, 일부 라이선스만 지금까지 가장 인기가 있고, 다음이 그 범주에 포함된다:

* [GNU 일반공중 라이선스](http://opensource.org/licenses/GPL-3.0)
  (GPL),
* [MIT 라이선스](http://opensource.org/licenses/MIT),
* [BSD 라이선스](http://opensource.org/licenses/BSD-2-Clause),
* [아파치 라이선스, 버젼 2.0](http://opensource.org/licenses/Apache-2.0).

GPL은 다른 대부분의 공개소스 라이선스와 다른데, 
[전염성이 있는(infective)](http://swcarpentry.github.io/git-novice/reference.html#infective) 특징이 있다: 코드의 수정된 버젼을 배포하는 누구나 혹은 GPL 코드를 포함한 어느 것이든지, *자신의* 코드도 동일하게 자유로이 공개가능하게 만들어야 한다.

흔히 사용되는 라이선서를 선택하는 것이 기여자나 사용자의 삶을 편하게 한다.
왜냐하면, 기여자나 사용자 모두 해당 라이선스에 친숙해서 사용할 때 상당한 양의 전문용어를 
꼼꼼히 살펴볼 필요가 없기 때문이다.

[Open Source Initiative](https://opensource.org/licenses)와 [Free Software Foundation](https://www.gnu.org/licenses/license-list.html) 모두 
좋은 선택이 될 수 있는 라이선스 목록을 유지관리하고 있다.

코드를 작성하는 과학자 관점에서 라이선싱과 라이선싱 선택지에 대한 전반적인 정보를 
[이 기사][software-licensing]를 통해서 살펴볼 수 있다.

결국 가장 중요한 것은 라이선스가 무엇인지에 대해 분명한 문장이 있는지와 라이선스가 [OSI](http://opensource.org)와 [FSF](https://www.gnu.org/licenses/license-list.html)에서 승인되고 이미 검증된 것인지 여부다.
또한, 저장소에 공개된 것이 아닐지라도, 
처음부터 최선으로 라이선스를 선택해야 된다.
결정을 미루는 것은 나중에 더 복잡하게 된다.
왜냐하면, 매번 새로운 협력자가 기여하기 시작하면, 협력자도 저작권을 갖게된다.
따라서, 라이선스를 고르자 마자, 승인을 득해야할 필요가 있기 때문이다.


> ## 본인이 오픈 라이선스를 사용할 수 있나요?
>
> 여러분이 작성하고 있는 소프트웨어에 오픈소스 소프트웨어 라이선스를 적용할 수 있는지 알아본다.
> 여러분이 라이선스 적용을 일방적으로 할 수 있는가? 
> 혹은 여러분의 기관이나 조직의 다른 사람에게서 허락이 필요한가? 
> 만약 그렇다면 누굴까?
{: .challenge}

> ## 본인은 어떤 라이선스를 이미 승인했나요?
>
> (금번 워크샵을 포함해서) 매일 사용하는 대다수 소프트웨어는 오픈-소스 소프트웨어로 
> 출시되었다. 
> 아래 목록 혹은 본인이 직접 고른 GitHub 사이트에서 프로젝트를 하나 고른다.
> 라이선스를 찾아(보통 `LICENSE` 혹은 `COPYING` 이름이 붙은 파일)보고, 
> 소프트웨어 사용을 어떻게 제약하는지 살펴보자.
> 이번 세션에서 논의된 라이선스 중 하나인가? 
> 차이점은 어떻게 나는가?
> - [Git](https://github.com/git/git), 소스코드 관리 도구
> - [CPython](https://github.com/python/cpython), 파이썬 언어 구현 
> - [Jupyter](https://github.com/jupyter), 웹기반 파이썬 노트북 프로젝트
> - [EtherPad](https://github.com/ether/etherpad-lite), 실시간 협업 편집기
{: .challenge}

[software-licensing]: https://doi.org/10.1371/journal.pcbi.1002598

## 콘텐츠 라이선스

만약 저장소 콘텐츠가 소프트웨어가 아닌 데이터, 창의적 저작물(매뉴얼, 기술 보고서, 원고) 같은 연구제품이 포함되면,
소프트웨어를 위해 설계된 라이선스 대부분은 적합하지는 *못하다*.

* **데이터:** 대부분 국가사법권에서 데이터 유형 대부분은 자연에 대한 사실로 간주된다. 
  그럼으로, 저작권 보호를 받을 자격이 없다.(단, 아마도 사진과 의료영상정보 등은 예외)
  따라서, 저작자 표시로 사회적 혹은 학자적 기대치를 알리려고, 저작권을 정의로 주장하는 방식으로 라이선스를 사용하는 것은 단지 법적으로 혼탁한 상황만 조장할 뿐이다. 
  [크리에이티브 커먼즈 제로(Creative Commons Zero, CC0)](https://creativecommons.org/publicdomain/zero/1.0/) 처럼 공중도메인 권리포기를 지지하는 법적 표시를 분명히 하는 것이 더 낫다. [Dryad](http://datadryad.org) 데이터 저장소는 사실 이를 요구하고 있다.

* **창의적 저작물(Creative works):** 매뉴얼, 보고서, 원고, 기타 창의적 저작물은 지적재산 보호 대상이 된다. 따라서 소프트웨어와 마찬가지로 자동으로 저작권으로 보호된다. [크리에이티브 커먼즈(Creative Commons)](http://creativecommons.org/) 조직이 기본 제약사항 4개를 조합해서 [라이선스 집합](http://creativecommons.org/licenses/)을 마련했다:

    *   저작자 표시(Attribution): 파생 저작물에 대해서 최초 저작자의 이름, 출처 등의 정보를 반드시 표시해야 한다.
    *   변경 금지(No Derivative): 저작물을 복사할 수도 있으나 저작물을 변경 혹은 저작물을 이용하여 2차적 저작물로 제작을 금한다.
    *   동일조건변경허락(Share Alike): 2차적 저작물을 제작할 수 있으나, 2차적 저작물은 원래 저작물과 동일한 라이선스를 적용한다.
    *   비영리(Noncommercial): 저작물을 영리 목적으로 사용할 수 없음. 영리 목적을 위해서는 별도의 계약이 필요하다.  
  

출처표시 ([CC-BY](http://creativecommons.org/licenses/by/4.0/))와 동일조건변경허락([CC-BY-SA](http://creativecommons.org/licenses/by-sa/4.0/)) 라이선스만이 "[오픈](http://opendefinition.org/) 라이선스"로 간주된다.

[소프트웨어 카펜트리](http://software-carpentry.org/license.html)는 가능하면 폭넓게 재사용될 수 있도록 수업자료에 대해서는 CC-BY, 코드에는 MIT 라이선스를 사용한다. 
다시 한번, 가장 중요한 것은 프로젝트 루트 디렉토리에 있는 `LICENSE` 파일에 라이선스가 무엇인지 분명하게 언급한다.
본인 프로젝트를 참조하는 방법을 기술하는데 `CITATION` 혹은 `CITATION.txt` 파일을 포함할 수도 있다. 소프트웨어 카펜트리 사례는 다음과 같다:


~~~
To reference Software Carpentry in publications, please cite both of the following:

Greg Wilson: "Software Carpentry: Lessons Learned". arXiv:1307.5448, July 2013.

@online{wilson-software-carpentry-2013,
  author      = {Greg Wilson},
  title       = {Software Carpentry: Lessons Learned},
  version     = {1},
  date        = {2013-07-20},
  eprinttype  = {arxiv},
  eprint      = {1307.5448}
}
~~~
  