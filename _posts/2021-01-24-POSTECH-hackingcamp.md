---
title: "POSTECH 온라인 해킹캠프 후기"
layout: post
author: yubin.choi
comments: true
date: 2021-01-24 00:00
use_math: true
tags: [etc]
---

지난 1월 18일부터 22일까지 5일에 걸쳐 POSTECH 해킹동아리 PLUS에서 주최한 온라인 해킹 캠프에 참여하게 되었다.

과학고 및 영재고에 재학 중인 학생을 대상으로 해킹 전반에 관한 기초를 다지는 캠프였다. 그 이전에 팀 단위 ctf에서 아무것도 못하는게 비참해서 이것저것 배운 후 뭐라도 할 수 있는 사람이 되자는 생각에 지원하였고, 6개 조 중 F팀으로 참가하게 되었다.

개인적인 소감을 말하자면, 기초를 다지기에는 정말 좋은 기회였다. 실습용 리눅스 서버를 개인마다 계정 하나씩 제공해주고, 디스코드로 수업 중에 질문 등에 대한 피드백과 같은 것이 활발히 이루어졌기 때문에 온라인으로 진행함에도 원활히 참가할 수 있었다.

(학교 와이파이가 느려서 유튜브 스트리밍으로 진행되는 부분 중 일부를 제대로 듣지 못했지만 말이다.)

단점을 굳이 뽑아보자면, 캠프가 매일 오전 9시부터 최소 오후 9시까지 진행되어 다른 일을 할 수 없다는 점이겠다. 물론 식사시간은 여유롭게 배치되어있고, 마침 학교 일정과도 맞아 큰 무리는 없었다만 장시간 집중을 유지하는 것은 생각보다 힘든 일이라는 것을 다시 한 번 깨달았다.

## Day 1

첫 날은 아무래도 일면식도 없는 사람들끼리 모아놓았기에, 간단히 자기소개와 캠프의 일정과 진행에 관한 안내로 시작했다.

세미나는 유튜브 스트리밍으로 진행되고, 세미나(혹은 그 중 한 파트)가 끝난 후 실습을 전용 사이트에서 진행하는 방식이었다. 매일 오후 7시부터 9시, 혹은 10시까지는 팀 별 경쟁 활동이 예정되어있었다.(이 팀 별 경쟁 활동은 캠프 도중에 액티비티라고 불렀었고, 편의상 이 글에서도 액티비티라고 언급하도록 하겠다.)

아무튼, 첫 날에 진행된 세미나는 앞으로의 캠프 과정에서 도움이 될만한 기반 지식인 리눅스 기본 명령어 사용법 및 파이썬 기초 문법, 그리고 간단한 스테가노그래피에 대한 내용이었다.

리눅스에서는 그 이후의 캠프 과정에서도 자주 사용한 ls, grep, cat 등의 명령어를 사용하는 방법을 익혔고, 스테가노그래피라는 것을 처음 접했기 때문에 흥미를 갖고 참여했던 것 같다.

첫 날의 액티비티는 **Save the Tux!**라는 이름으로 진행되어, 학습한 리눅스 명령어를 활용해 여러 서버를 돌아다니며 플래그를 수집하는 것이었다. 배운 내용에 고민을 거쳐야만 고득점이 가능했으므로 새삼 준비를 엄청 공들여 했다는 생각이 들었다. Day 1의 액티비티는 운 좋게(혹은 나와 팀원들이 구글링을 열심히 해서?) 압도적인 점수로 1등을 할 수 있었다.

## Day 2

Day 2에서는 리버싱과 포너블에 관한 내용을 다루었다. 참가 자격에 해킹에 관한 지식이 있진 않았고 기초를 다지는 캠프인 만큼, 어셈블리를 다루기는 어려운 환경이었다. 때문에, 어셈블리가 아닌 C 코드가 주어진 환경을 가정하고 문제 풀이 실습이 이루어졌다. 나는 기초적인 수준의 포너블만 해도 벅찼고 머리가 어지러웠기 때문에, 포너블은 나랑 정말 안 맞는건가 하는 생각도 하곤 했다.

Day2의 액티비티는 **Pwnkemon GO**라는 이름으로, 애니메이션 포켓몬의 체육관을 컨셉으로 포너블을 통해 플래그를 따내는 액티비티였다. 나나 팀원들이나 포너블과 잘 안 맞았는지 노력했음에도 상위권의 성적은 거두지 못했고, 4~5위에 그쳤던 것 같다. 

## Day 3

Day 3는 내가 그나마 잘한다고 생각하는 내 주력 분야, 암호학에 관한 세미나가 이루어졌다. 기초 캠프인 만큼 고등학생 수준에서 원리까지 자세히 이해할 수 있을 법한 내용만을 다루었다.

Affine Cipher, Rail Fence Cipher 등의 고전 암호와 빈도분석법 등을 활용한 공격과 같은 내용을 다루었고, RSA와 Diffie-Hellman과 같은 현대암호에 대해서도 다루었다.

Diffie-Hellman은 개념만 간단하게 다루었고(심지어 MIM 공격을 언급조차 하지 않았다.) RSA는 원리를 자세히 알아본 후 Fermat factorization과 small public exponent attack에 대해 알아보았으나 hastad's broadcast attack과 같은 내용은 다루지 않았다. 이미 RSA와 Diffie-Hellman에 관해 어느 정도 알고 있던 입장에서, 이러한 내용이 다루어지지 않은 것이 약간의 아쉬움이 남긴 했다. (사실, 캠프를 준비하신 멘토분들이 다루고 싶었어도 세미나 시간이 모자랐을 것이다.)

Day 3의 액티비티는 **암호는 사드세요....제발**이라는 활동이었다. Day 3에 배운 고전 암호와 현대 암호를 스테이지 별로 나누어 제작하고 상대방의 암호를 공격하는 방식이었다. 약간의 운(다른말로 도박)이 필요한 액티비티였는데 1위였던 다른 팀이 안전하게 하겠다고 포인트를 필요 이상으로 소비해준 덕에 1등을 차지하였다.

(여기부턴 TMI다.) 다양한 스테이지 중의 하나로 미리 준비된 7개의 RSA 공개키 중 안전한 공개키를 찾는 활동이 있었다. 이 활동에서 팀원들은 서로 복호화를 시도할 공개키를 분배하여 풀었는데, 나머지 공개키가 모두 취약점이 파악된 후 안전한 공개키를 받았던 팀원이 '이거 잘 안 되는데 도와주세요'라고 하길래 '그거 안전한 키에요'라고 답하니 당황하던게 살짝은 안쓰럽기도 하고 괜스레 미안해졌다.

## Day 4

Day 4에 진행된 웹 세미나를 들으면서 정말 흥미로웠다. 생각치도 못하게 복잡한 수준으로 웹이 굴러가고 있었고, 그런 새로운 사실들을 알아간다는 것은 늘 신선하고 재밌게 느껴지는 것 같다. 하지만 본격적으로 XSS, SQL Injection을 하면서 캠프 도중 '이 분야는 나랑 안 맞는 것 같다'라는 생각이 두번째로 들었다. 기반지식이 많이 필요한 분야여서 더욱 접근이 어려웠던 것 같으나, 구글링의 힘으로 역경을 ~~정해보다 어렵게~~ 해결해나갔다.

액티비티는 XSS 파트와 SQL Injection 파트로 나뉘어 점수를 획득할 수 있었으나 Blind SQL Injection 자체가 어렵기도 했고, 여러 가지 이유로 모든 팀에서 손을 대지 않아, XSS 파트만으로 점수와 순위가 결정되자 이 액티비티는 무효로 하게 되었다.

## Day 5

마지막 Day 5는 오전 9시부터 오후 9시까지 12시간에 걸쳐 CTF가 진행되었다.

지금까지 학습한 모든 내용을 활용하여 플래그를 따냈는데, 암호학만큼은 퍼블을 수두룩하게 따내긴 했다. 하지만 웹 등 다른 분야에서 미스가 많이 났고 운적인 요소가 작용했던 문제도 있었기 때문에, 기대보다는 살짝 낮은, 아쉬운 성적으로 마무리하였다.

하지만 그 이전의 액티비티에서 좋은 활약을 거두었기 때문에 내가 포함된 F조는 종합 2등을 차지하였고, POSTECH 후드티를 상품으로 보내준다고 한다. 아마 택배가 도착하면 한동안 그걸 계속 입고 다닐 것 같다.(대학 입시와는 무관하게...)

## End Card

요약하자면, 다양하게 배울 수 있어서 좋았다. 종종 문제가 있긴 했다만 이에 대한 대응이 빠르게 이루어졌고, 무엇보다 항상 멘토분들이 지켜봐주시면서 도와주고 피드백해주신 덕분에 수월했던 것 같다.

5일이라는 시간을 투자만 할 수 있다면 해킹 기초를 익히기 위해 충분히 가치 있는 활동이므로, 내년에도 이러한 형식으로 캠프가 열린다면 참여를 추천하고 싶다.

이렇게 긴 글은 블로그에 처음 쓰는 듯 한데, 모쪼록 이 글이 해킹에 관심을 갖는 과학고, 영재고 학생들에게 도움이 되었으면 한다.