---
layout: post
title: BaseCamp week3
comments: true
---

<br><br>

----

## BaseCamp - week3

<br>

지난 주와 마찬가지로 정신 없었던 한 주.

먼저, 기획발표 직전에 스펙축소를 제안해주셨던 멘토님께 정말 감사드려야겠다(사랑합니다.).

개발하는 동안 예상하지 못 했던 상황들에 종종 직면하고.. (그때마다 해결사 역할을 해 준 경원찡에게도 매우 감사를 드린다.)

중간발표때 배포 문제 때문에 미흡했던것에 이어서 오늘도 필수기능인 댓글에 이미지를 업로드하는 기능이 제대로 동작하지 않음을 발표직전에 알게되어 팀원들 모두 몹시 당황스러워했다. (...)

<br>

2주 동안 많지는 않지만 제법 다양하게 시도를 해본 것 같다.
여지껏 cpp소스파일 달랑 하나에 C/C++컴파일러만 곁들여지면 끝나는 코딩만 해왔으니.. xml파일은 무엇이며.. properties는 또 무엇이고.. 프로젝트 익스플로러가 나를 너무 힘들게 만들었는데,

![oh..god..]({{ site.url }}/img/spring_package_explorer.JPG)

> 살려주시옵소서..

첫 주 내내 매달렸던 원본URL/조회수/주목/댓글 데이터를 DB에서 꺼내오는 작업 중.

처음으로 조회수를 출력하는데 성공했을때는 절대 아닌 줄 분명히 알면서도 '아 뭔가 할 수 있을것만같다.'라고 억지 희열을 만끽했고, 역시나 얼마 못가서 또 새로운것들에 둘러쌓여 고통스러워했다.

두 번째 주에는 errorPage, OAuth, TUI-calendar, RSSreader, Mockito를 적용해 보았는데, 겉돌면서 맛만보는 정도로만 했을 뿐, 여전히 팀 내 기여도는 현저히 낮은것 같아서 괴롭다. 작업에 열중하고 있는데 괜히 방해하는것 같아 붙잡기도 쉽지않고, 질문내용을 몇 번이나 되뇌어 정리해서 겨우 붙잡아 물어보면 또 이해하기 힘들고.. 그런데 다시 물어보자니 어서 보내드려야(?)할 것 같아 'ㅇㅋㅇㅋ'하면서 보내드렸다.

학교에서 한창 강의하던 때는 질문하는 방법이 어쩌고 저쩌고 얘기하고 다닌 주제에 정작 본인도 제대로 못 하면서 ㅎ, ㅎ...

알려주는 사람에게도 질문자를 이해시키려 노력하는 과정에서 의사전달능력 향상에 많이 도움이 된다는 것을 누구보다 잘 알면서 팀원들에게 그 기회를 주지 못했던 것 같다. (경원찡 미안해요)

<br>

그리고, 중요한 한 가지를 깨달은 이번 주.

FaceBook OAuth를 구현하면서 [ScribeJava](https://github.com/scribejava)라이브러리를 이용했는데,

문득, ScribeJava가 갑자기 없어졌을때 '과연 나는 OAuth를 **안다** 고 말할 수 있을까..' 라는 생각이 들었다.

개인적인 흥미가 되었던, 누군가로부터 의뢰를 받았던간에 프로젝트 개발을 진행하면서 특정 기능을 구현하는데 있어서 **'된다/안된다'**의 기준잣대로 판단하는건 일용직 노동자나 학생시절일때나 할만한 발상이지, 그 동안 다양하게 비싼 교육 받아놓고 여전히 구글링이나 해가며 이것저것 붙여보고 '된다'하면 넘어가고 '안된다'하면 다른 붙여넣기 대상을 찾고 있었으니..

굳이 라이브러리에 의존하지 않더라도 교육때 들었던 OAuth의 인증 과정에서 오가는 정보에는 어떤것들이 있으며 절차는 어땠는지를 정말 어렴풋이라도 그려볼 수 있다면 그게 오히려 OAuth를 **안다**라고 말할 수 있지 않을까 하는 생각이 들었다.

괜히,

> OAuth 날로 짜보세요<br>
> 책 보세요<br>
> 표준 문서를 보세요

라고 계속 언급해 주시는게 아닌 것 같다.

생각이 정리되고 팀원들에게도 의사를 전달하여 모두 크게 놓치고 있는것 같다는 생각에 공감하였고, 너무 뒤늦게 깨달아 시간이 많이 부족했지만 그제서야 경원찡의 날로짠 OAuth 코드를 보면서 OAuth를 제대로 이해해보려 스터디도 한 번 빠르게 진행하였고, Mockito도 아주 보잘것 없지만 한 번 건드려보는 등. 교육 자료들을 다시 훑어보면서 이해하고 적용해보려는 기회를 가지게 되었다.

<br>

이젠 학생도 아니요. 일용직 노동자는 더욱 아니요.

개발자라면 **'된다/안된다'**의 마인드에서 최대한 멀어지려 노력해야 할 것이고, **'안다/모른다'**의 기준으로 스스로를 계속 검증하려 불쑥불쑥 내밀어야겠다.

<br><br>

근데 여기저기 계속 쫒기다보면 역시 쉽게는 안될 것 같다 ㅎ, ㅎ.......

<br>





