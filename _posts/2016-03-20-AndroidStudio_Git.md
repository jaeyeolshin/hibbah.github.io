---
layout: post
title: 안드로이드 스튜디오 & Git 연동
comments: true
---

<br><br>

----

## 서론

개발에 필요한 직접적인 코드는 안드로이드 스튜디오에서 작성하지만 버전 관리를 위해 Git을 사용하기 위해 SourceTree 또는 커맨드 명령을 써야만한다. 매번 왔다갔다 하면서 사용하는것도 매우 번거로우므로, 안드로이드 스튜디오에서 Git을 연동하여 쓰는 방법에 대해서 설명한다.

<br><br>

----

## 1. Local Repository 생성

![importVCS]({{ site.url }}/img/importVCS.jpg)

안드로이드 스튜디오를 실행하여 버전관리를 하고자하는 프로젝트를 오픈하고 상단 메뉴에서 `VCS - Import into Version Control`을 선택한다.

![selectProject]({{ site.url }}/img/select_project.jpg)

현재 프로젝트를 Local Repository로 등록한다. `OK`를 누르면 등록한 폴더에 자동으로 .git 폴더가 생성된다. (git에게 해당 폴더의 변경사항을 추적하도록 한다.)

Local Repository에 등록을 완료하면 스튜디오상에 표시되는 각종 파일들의 색깔이 다르게 나타나는데 각 색상별로 의미하는 바는 아래와 같다.

##### 파일상태

- 붉은색 : git에서 추적하지 않는 파일. `add`필요

- 파란색 : 변동사항이 있는 파일. `commit` or `push`

- 초록색 : 변동 사항이 없는 파일. 최신상태.

<br><br>

----

## 2. Commit

로컬저장소만 생성하고 관리할 코드는 git에게 알려주지 않았다. 새롭게 추가된 파일은 git에게 추적할 것을 알려줘야하므로 `Add`를 먼저 수행하여 이후부터는 해당 파일들에 대해 변화를 추적해줄 것을 git에게 부탁(?)한다.

프로젝트 익스플로러에서 프로젝트를 우클릭하여  `Git - Add`과 `Git - Commit Directory`를 차례로 진행하여 첫 commit을 진행한다.

![commit]({{ site.url }}/img/commit.jpg)

위 그림처럼 commit창에서 commit메시지와 함께 commit만 진행할지 push까지 함께 진행할지도 선택할 수 있다. 또, 그림에는 생략되었지만 상단부에서 어떤 브랜치에 commit을 수행할지도 선택할 수 있다.


<br><br>

----

## 3. remote 연동

앞의 두 과정은 **Local** 저장소와 연동한 것이지 실제로 Git에 있는 원격저장소와 연동된 것이 아니다. 원격 저장소와 연동하기 위해 github웹에 들어가서 새로운 저장소를 생성한다.

![repository]({{ site.url }}/img/repository.jpg)

처음 저장소를 생성하면 당연히 아무것도 없다. 1에서 생성한 Local Repository를 위의 Remote Repository와 연동시키기 위해 위 그림의 빨간영역을 클릭하여 Remote Repository의 주소를 복사한다. 그리고 1에서 등록한 프로젝트 폴더를 열어 우클릭 후에 `Git Bash here`를 선택하여 현재 경로를 대상으로 git 터미널을 띄운다.

![gitbash]({{ site.url }}/img/bash.jpg)

**git remote add origin 복사한주소**를 타이핑하여 현재경로(로컬)를 Hibbah의 AndroidProject(원격)저장소에 등록한다. 이후부터는 Git을 이용하기 위해 별도의 버전관리 툴을 사용할 필요없이, 안드로이드 스튜디오 내에서 아래의 단축키로 Git의 Remote Repository에 commit, push, .. 의 작업을 할 수 있다.

commit : `Ctrl + K`

push : `Ctrl + Shift + K`

<br><br>

----

## 참고문서
- http://wii.logdown.com/posts/2013/11/15/android-studio-git-tutorial
- https://www.davidlab.net/ko/tech/android-studio-tips-git-integration-part1/
- http://blog.dudmy.net/android-studio-github-sync/
