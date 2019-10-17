# OpenSource Contributions
다른 사람들과 협력하거나 오픈소스가 어떻게 작동하는지 이해하는 방법을 배우는 것이 목표라면, 기존 프로젝트에 기여하는 것이 좋은 방법입니다.  

1. 이미 활용하고 있거나 관심있는 프로젝트부터 시작해보세요.  
    - [gitstar-ranking](https://gitstar-ranking.com/)
    - [Spring Framework](https://github.com/spring-projects/spring-framework/issues)
1. 오타를 수정하거나 문서를 업데이트하는 것 같은 간단한것도 가능합니다.  
    - [Fixed Typo PR](https://github.com/spring-projects/spring-framework/pull/23788)
1. good first issue 를 먼저 해보는것도 좋은 방법입니다.
    - [vuejs-good first issue](https://github.com/vuejs/vue/labels/good%20first%20issue)
    - [Reactjs-good first issue](https://github.com/facebook/react/labels/good%20first%20issue)
1. 최근 Major Version up 릴리즈를 한 Projects를 확인해 보세요.
    - [Tensorflow-good first issue](https://github.com/tensorflow/tensorflow/labels/good%20first%20issue)
1. 문서 번역등의 컨트리뷰션도 좋은 방법입니다.
    - [keras-docs-ko](https://github.com/keras-team/keras-docs-ko) 

## 오픈소스 컨트리뷰션 
오픈소스 프로젝트에 참여하는 데는 여러 가지 방법이 있으며, 
몇 가지 팁을 통해 경험을 최대한 활용할 수 있습니다.
![contributions](images/contributions.png)

오픈소스 프로젝트에는 다음과 같은 역할자가 있습니다:
- Author: 이 프로젝트를 만든 사람 혹은 조직
- Owner: 조직 또는 저장소에 대한 관리 권한을 가진 사람 (항상 Original Author와 동일하지는 않음)
- Maintainers: 비전을 주도하고 프로젝트의 조직 측면을 관리하는 책임이 있는 기여자. (그들은 프로젝트의 저자 또는 소유자일 수도 있습니다.)
- Contributors: 프로젝트에 다시 기여한 모든 사람.
- Community Members: 프로젝트를 사용하는 사람들. 대화에서 적극적이거나 프로젝트 방향에 대한 의견을 표명할 수 있습니다.

### 참고
- [How to Contribute to Open Source](https://opensource.guide/ko/how-to-contribute/)

# 어떻게 오픈소스 프로젝트에 컨트리뷰션 할까?
## 1. 오픈소스 프로젝트의 사용 법 이해하기
### [README.md](https://code.sdsdev.co.kr/codecraft/ColorWall/blob/development/README.md)  
README는 프로젝트 사용 방법을 설명하는 것 이상을 수행합니다.  
또한 프로젝트가 중요한 이유와 사용자가 수행 할 수 있는 작업에 대해 설명합니다.

README통해 다음 질문에 답을 얻을 수 있습니다:
- 이 프로젝트는 무엇을 합니까?
- 이 프로젝트는 왜 유용합니까?
- 어떻게 시작합니까?
- 필요할 경우, 어디에서 더 많은 도움을 받을 수 있습니까?

README 파일을 루트 디렉토리에 포함시키면, GitHub가 자동으로 저장소 홈페이지에 표시합니다.

#### 참고
- [About READMEs](https://help.github.com/en/articles/about-readmes)
- [README Template](https://gist.github.com/PurpleBooth/109311bb0361f32d87a2)

## 2. 오픈소스 프로젝트의 기여방법 확인하기
### [CONTRIBUTING.md](http://code.sdsdev.co.kr/codecraft/ColorWall/blob/development/CONTRIBUTING.md)
CONTRIBUTING 파일은 프로젝트 참여 방법을 알려줍니다. 
예를 들어 다음 정보를 포함 할 수 있습니다:
- 버그 보고서를 제출하는 방법 (이슈와 pull request 템플릿을 사용해보십시오)
- 새로운 기능을 제안하는 방법
- 환경 설정 및 테스트 실행 방법

기술적 세부 사항 외에도, 기여에 대한 기대를 전달할 수도 있습니다.
예로 들면 :
- 당신이 찾고있는 기여의 종류
- 프로젝트 로드맵 또는 비전
- 참여자가 귀하와 연락해야 (또는 하지 말아야) 하는 방법

README에 CONTRIBUTING 파일을 링크하면 더 많은 사람들이 볼 수 있습니다.  
CONTRIBUTING 파일을 프로젝트의 레파지포리에 두면, GitHub는 참여자가 이슈를 생성하거나 pull request를 열면 자동으로 파일에 연결됩니다.  

#### 참고
- [Setting guidelines for repository contributors](http://help.github.com/en/articles/setting-guidelines-for-repository-contributors)  
- [Atom Contributing](http://github.com/atom/atom/blob/master/CONTRIBUTING.md)  
- [Tensorflow Contributing](http://github.com/tensorflow/tensorflow/blob/master/CONTRIBUTING.md)  
- [Kubernetes Contributing](http://github.com/kubernetes/community/tree/master/contributors/guide)

### [CODE_OF_CONDUCT.md](http://code.sdsdev.co.kr/codecraft/ColorWall/blob/development/CODE_OF_CONDUCT.md)
행동강령(Code of Conduct)은 프로젝트 참가자의 행동에 대한 기대치를 설정하는 문서입니다.  
행동강령을 채택하고, 시행하면 커뮤니티에 긍정적인 사회적 분위기를 조성하는데 도움이 될 수 있습니다.  
행동강령은 다음을 설명합니다:
- 행동강령이 효력을 발생하는 곳 (이슈 및 pull requests, 이벤트...)
- 행동강령이 누구에게 적용되는지 (커뮤니티 맴버와 메인테이너, 스폰서....)
- 누군가가 행동강령을 위반하면 어떻게되는가
- 누군가가 위반 사례를 신고 할 수 있는 방법

#### 참고
- [Contributor Covenant](http://www.contributor-covenant.org/)      

## 3. 오픈소스 프로젝트의 라이센스 확인하기
### [LICENSE.md](https://code.sdsdev.co.kr/codecraft/ColorWall/blob/development/LICENSE.md)
오픈소스 라이선스는 타인이 영향을 주지 않고 프로젝트에서 사용, 복사, 수정 및 기여할 수 있음을 보증합니다.  
또한 복잡하게 얽혀있는 법적 상황으로부터 당신을 보호합니다.  
오픈소스 프로젝트를 시작할 때 라이선스를 포함해야합니다.  
MIT, Apache 2.0, 그리고 GPLv3는 가장 인기있는 오픈소스 라이선스입니다.

#### 참고
- [Licensing a repository](http://help.github.com/en/articles/licensing-a-repository)    

## 4. 프로젝트에 이슈 리포팅하기
일반적으로 다음과 같은 상황에서 이슈를 열어야합니다:
- 스스로 해결할 수 없는 오류를 보고
- 높은 수준의 주제 또는 아이디어 (예시. 커뮤니티, 비전, 정책) 토론
- 새로운 기능이나 다른 프로젝트 아이디어 제안

이슈에서 의사소통을 위한 팁:
- 해결하려는 이슈가 공개적으로 보이면, 
    - 중복으로 작업할 가능성이 줄이기 위해 대해 새로 이슈를 생성하지 말고 해당되는 이슈에서 의견을 말하십시오. 
- 이슈가 조금 전에 열렸다면, 
    - 다른 곳에서 해결되었거나, 이미 해결되었을 수 있기 때문에 해당이슈에 대해 작업을 시작하기 전에 확인을 요청하십시오.
- 이슈를 열었지만 나중에 대답을 알아 낸 경우, 
    - 사람들에게 알리고 이슈를 해결할 수 있도록 이슈에 대한 의견을 말하십시오. 

그 결과를 문서화하는 것조차도 프로젝트에 대한 기여입니다.

#### 참고
- [Creating a issue template](http://help.github.com/en/articles/manually-creating-a-single-issue-template-for-your-repository)  
- [Collaborating with issues and pull requests](http://help.github.com/en/categories/collaborating-with-issues-and-pull-requests)

## 4. Pull Request 활용하기
### pull request 열기
- 사소한 수정 사항 제출 (예 : 오타, 깨진 링크 또는 분명한 오류)
- 이미 이슈를 열었거나 이미 논의한 내용을 기여로 시작하기  

pull request은 완료된 작업을 나타내지 않아도됩니다. 일반적으로 초기에 pull request을 열면 다른 사람이 진행 상황을 보거나 피드백을 줄 수 있습니다.  
제목 줄에 “WIP”(진행중인 작업)이라고 표시하십시오. 나중에 커밋을 더 추가 할 수 있습니다.

### pull request 방법
1. 저장소를 포크하고 로컬에 클론합니다. 
1. 수정을 위한 브랜치 생성하기.
1. 모든 관련있는 이슈 혹은 PR에서 지원중인 문서 참조하기 (ex. “close #37”)
1. 전후의 스크린 샷 포함합니다.
1. 변경점을 테스트합니다! 
1. 당신의 능력을 최대한 발휘하여 프로젝트 스타일에 기여하십시오. 

### pull request Tip
1. 하나의 PR에 많은 기능, 이슈를 해결하지 마세요. PR은 작게 만들수록 좋습니다.
1. Commit 은 잦게 쪼개고 한번에 리뷰어가 리뷰할 수 있을정도의 Line 수를 고려하세요
1. 비슷한 이슈는 묶어서 한번에 처리하면 좋습니다.
1. PR을 요청할 경우 기본적으로 테스트하여 확인 후 요청합니다.

### Contributor License Agreement (CLA)
- 오픈 소스 프로젝트를 유지 관리하는 경우 프로젝트를 관리하는 오픈 소스 라이센스를 선택할 수 있습니다. 이것은 프로젝트의 기여자에 대한 암시 적 합의를 제공하는 반면, 기여자 라이센스 계약 (CLA) 은 이러한 조건을 명시적이고 이러한 계약에 대한 기록을 제공합니다.  
- CLA의 목적은 프로젝트 산출물의 관리인이 선택한 라이센스에 따라 배포 할 수 있도록 모든 컨트리뷰션에 대해 필요한 소유권 또는 권리 부여를 갖도록 보장하는 것입니다.  
    - [Reactjs CLA](https://github.com/facebook/react/pull/17118)  
    - [guava CLA](https://github.com/google/guava/pull/3529)  

#### 참고
- [Creating a pull request template](http://help.github.com/en/articles/creating-a-pull-request-template-for-your-repository)
- [Fork a repo](http://help.github.com/en/articles/fork-a-repo)  
- [SAP CLA](https://gist.github.com/CLAassistant/bd1ea8ec8aa0357414e8)
- [CLA assistant](https://cla-assistant.io/)
- [CLA-bot](https://colineberhardt.github.io/cla-bot/#installing-cla-bot)

#### 참고
- [Creating a pull request template](http://help.github.com/en/articles/creating-a-pull-request-template-for-your-repository)
- [Fork a repo](http://help.github.com/en/articles/fork-a-repo)  
- [SAP CLA](https://gist.github.com/CLAassistant/bd1ea8ec8aa0357414e8)
- [CLA assistant](https://cla-assistant.io/)
- [CLA-bot](https://colineberhardt.github.io/cla-bot/#installing-cla-bot)

## 5. 결과기다리기
컨트리뷰션 후 다음 중 하나의 결과를 받을 수 있습니다.
- 컨트리뷰션에 대한 응답을 받지 못합니다.
- 누군가가 컨트리뷰션에 대한 변경을 요청합니다.
- 컨트리뷰션이 받아들여지지 않습니다.
- 컨트리뷰션이 받아들여 졌습니다.

#### 참고
- [About pull request reviews](http://help.github.com/en/articles/about-pull-request-reviews)

