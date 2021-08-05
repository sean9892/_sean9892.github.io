---
layout: post
comments: true
date: 2021-08-05 19:30
title: "Pitchicle Jekyll 블로그 제작기"
author: yubin.choi
tags: [computer-science,web]
---

Pitchicle 웹사이트는 기존에 imweb을 사용한 유료 호스팅으로 이루어지고 있었습니다. 하지만 유료 호스팅은 오래 유지하기엔 부담스러운 면이 많았기에, 사이트 이전 방안에 대한 투표를 통해 Jekyll을 이용해 제작한 블로그로 사이트를 이전하게 되었습니다. 그 과정에서 필요했던 지식과 겪은 시행착오를 기록으로 남겨두려 합니다.

### Start

Github에서는 GithubPage라는 서비스를 통해 쉽게 블로그를 만들 수 있습니다. Vercel, HEXO 등 다양한 GithubPage 제작 프로그램이 존재하지만 그 중 제게 익숙한 **Jekyll**을 선택했습니다.

물론, 처음부터 블로그를 디자인하기에는 어려움이 있으므로 기존에 이미 있는 블로그 혹은 테마를 수정하면 제작이 매우 수월해집니다. Pitchicle 블로그의 경우에는 [욱제님 블로그](http://wookje.dance/)에 사용된 [kakao 기술 블로그](http://kakao.github.io)의 테마를 사용하였습니다.

![Github Fork 버튼](https://user-images.githubusercontent.com/46587635/128337677-6385cfcb-d535-44f5-aaa1-fa35dc8a8b02.png)

원본 블로그의 github repository를 fork하여 pitchicle의 github organization에 repository를 만들어주고, 가장 먼저 _config.yml에 존재하는 title, email, url, repository 등을 변경해줍니다. 기본적인 세팅이기도 하고, 변경하지 않은 채 사이트를 배포하려 하면 자칫 원본 블로그에 영향을 미칠 수 있으니 개인적으로는 최우선순위로 두는 편입니다.

![_config.yml의 일부분](https://user-images.githubusercontent.com/46587635/128338070-2179e051-523d-4ef7-90f0-c529cc2ff2b5.png)

### Delete privacies

이전 블로그를 fork하였기 때문에 post, author 등의 정보가 남아있었습니다. 더불어 cover image 역시 pitchicle 동아리에 맞게 수정해 줘야하지만, 이미지를 선정하느라 조금 시간이 지난 후에 수정하였습니다.

![현재의 _posts 폴더](https://user-images.githubusercontent.com/46587635/128338573-095eeafe-38fb-4e81-8eee-eaf60d3ec0c8.png)

### 저자 이름 표시 방식 변경

![현재 authors 페이지의 저자 표시 방식](https://user-images.githubusercontent.com/46587635/128338885-fe7e94a6-03ec-49d9-b2ad-9d8a8e8e73cc.png)

현재의 수정된 저자 이름 표시 방식은 위와 같이 `저자_이름(라틴어표기)` 꼴로 나열되어있습니다. 하지만 fork한 블로그는 1인 블로그로 이러한 저자 페이지를 이용할 필요가 없었기 때문에, 저자 페이지가 데이터 상으로 남아있지만 하이퍼링크를 통한 접속이 불가능하고 저자 혼자의 라틴어 표기만이 덩그라니 쓰여 있는 상태였습니다.

![수정 전 authors.md](https://user-images.githubusercontent.com/46587635/128339376-7745c1c7-aed6-4b7f-a08f-8a6e5a0bd3d9.png)

Jekyll의 모든 페이지는 html로 작성되는 기본 골격인 layout과 그것에 덧붙여 씌워지는 Markdown 파일로 구성됩니다. 이 때 layout인 html 파일과 Markdown 파일 모두 html, Markdown의 문법으로 작성되지만, [Liquid](https://shopify.github.io/liquid/) 코드를 삽입할 수 있습니다. **Liquid**는 웹 어플리케이션에서 이용되는 template language로, 웹사이트를 더 유연하게 구성할 수 있도록 해줍니다.

위 이미지의 liquid 코드는 repository에 기록되어있는 모든 저자를 순회하면서, Markdown 문법으로 그 저자의 이름(라틴어표기)에 그 저자의 페이지로 연결되는 하이퍼링크를 걸도록 설계되어있습니다. 이왕이면 한글 이름이 보이는게 좋으므로, 아래 이미지와 같이 코드를 수정하여 현재와 같이 표시될 수 있도록 수정하였습니다.

![수정 후 authors.md](https://user-images.githubusercontent.com/46587635/128340212-5b07799e-5989-49ae-98cb-20f4bbee1369.png)

**modiauthors**라는 변수에 모든 저자의 리스트를 **title**이라는 요소(pitchicle의 경우에는 여기에 한글 이름을 지정해놓았습니다.)를 기준으로 정렬하고, 이를 순회하면서 `이름(라틴어표기)` 꼴로 보여질 수 있게끔 수정한 것이지요.

### disqus 연동

보통의 블로그 서비스라면 대부분 댓글, 조회수 기능을 지원하지만, 순수한 Jekyll에는 지원되지 않는 기능입니다. 댓글의 경우는 **disqus**라는 서비스를 이용해서 추가하는게 가능하기 때문에 쉽게 추가할 수 있었습니다.

### 이미지 변경

이후 한 작업은 그리 어렵지 않은 작업이었습니다. 비록 이미지 크기 관련 문제에서 많이 애를 먹었지만, 적절한 이미지를 찾아 규격에 맞게 자르고, 사진을 바꿔넣기만 하면 되는 작업이었습니다. 이번 작업에서는 그냥 동일한 파일명으로 이미지를 넣었지만, `screen.css` 파일을 수정하면 다른 파일명의 파일을 지정하는 것도 가능합니다.

### Google Analytics 연동

**Google Analytics**는 웹사이트에서 유저의 웹사이트 사용 방식과, 주요 페이지 이동 흐름, 활성 사용자 수 등의 통계를 쉽게 관리할 수 있는 서비스입니다. Jekyll로 구동되는 블로그는 추적ID를 `_config.yml`에 추가하는 것만으로도 연동할 수 있는 경우가 많기 때문에, 간단하게 연동하였습니다.

### 상단 메뉴 Active 버그 수정

급한 불은 대부분 꺼졌으니, 조금 사소한 영역으로 들어섰습니다. 

![상단 메뉴의 Active 상태](https://user-images.githubusercontent.com/46587635/128341738-19c07d79-5320-4b41-b31a-8312f6ca8c1f.png)

'블로그'라는 글자 밑의 얇은 막대기 보이시나요? 상단 메뉴가 active 상태일 때 이런 막대가 생기는데, 이 막대가 생기는 처리 조건이 제대로 설정되지 않으면 active 상태인 메뉴가 없거나, 여러 메뉴가 동시에 active되는 버그가 발생합니다. 이를 수정하기 위해서는, `_includes/header.html`를 수정해주면 됩니다.

![수정 후 상단 메뉴 표시 코드](https://user-images.githubusercontent.com/46587635/128342099-9a9de9d4-f521-4b57-b179-0ae0864f2257.png)

이미지에서 볼 수 있듯, **page.url**을 기준으로 상단 메뉴를 active 상태로 만듭니다. 이 때, 개별 저자의 페이지에 접속했을 때도 '**저자 소개**'가 active되려면 page.url의 앞 9글자가 `/authors/`면 됩니다. 간단하게 Liquid 문법 중 하나인 **slice**를 사용하면 되지만, 부적절한 index가 삽입될 경우 메뉴가 active되는 현상이 있었기 때문에, **at_most**를 사용하여 슬라이싱할 인덱스를 조절해주는 것으로 해결하였습니다.

### TODO

대부분의 기능을 구현해놓은 상태이지만, 조회수 기능이 필요하다고 생각됩니다. 앞으로 HITS! 서비스를 활용해 조회수를 표시하고, 더욱 개선할 점을 모색할 예정입니다.