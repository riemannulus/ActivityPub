[목차로 돌아가기](ActivityPubContents.md)

## C. 감사의 말 (Acknowledgements)

*이 항목은 비표준입니다.*

~~*This section is non-normative.*~~

이 사양은 웹에서 연합 공간을 탐색하는 여러 커뮤니티의 수년간의 노력과 경험에서 비롯되었습니다. 특히, 이 사양의 대부분은 StatusNet (현재의 GNU Social) 및 Pump.io에 의해 개척된 [OStatus](https://www.w3.org/community/ostatus/wiki/Main_Page) 및 [Pump API](https://github.com/pump-io/pump.io/blob/master/API.md)에 의해 제공되었습니다. 이 두 가지 시작점은 많은 개발자들의 노력의 결과물이였지만, 다른 누구보다도 이 분야에서 장기간 리더였던 Evan Prodromou의 노력이 없었더라면 액티비티펍이 지금과 같은 지위를 가졌을 가능성은 낮다고 봅니다.

~~This specification comes from years of hard work and experience by a number of communities exploring the space of federation on the web. In particular, much of this specification is informed by [OStatus](https://www.w3.org/community/ostatus/wiki/Main_Page) and the [Pump API](https://github.com/pump-io/pump.io/blob/master/API.md), as pioneered by StatusNet (now GNU Social) and Pump.io. Both of those initiatives were the product of many developers' hard work, but more than anyone, Evan Prodromou has been a constant leader in this space, and it is unlikely that ActivityPub would exist in something resembling its current state without his hard work.~~

Erin Shepherd는 이 문서의 초기 버전을 작성했으며, [Pump API](https://github.com/pump-io/pump.io/blob/master/API.md) 문서의 아이디어를 가져와, 비록 거의 처음부터 재작성하였지만, 이들 중 핵심 아이디어들을 ActivityStreams 1에서 ActivityStreams 2로 전환할 때 적용하였습니다.

~~Erin Shepherd built the initial version of this specification, borrowed from the ideas in the [Pump API](https://github.com/pump-io/pump.io/blob/master/API.md) document, mostly as a complete rewrite of text, but sharing most of the primary ideas while switching from ActivityStreams 1 to ActivityStreams 2.~~

[//Comment]: # "번역이 매끄럽지 않은 것 같습니다."

Jessica Tallon과 Christopher Lemmer Webber는 이 표준이 <abbr title="World Wide Web Consortium">W3C</abbr> 소셜 워킹 그룹으로 이관되었을 때 편집자로 활동하였으며, 이들은 Erin Shepherd의 문서에서 현재 상태의 액티비티펍으로 전환하는 수많은 작업들을 하였습니다. 소셜 워킹 그룹의 기나긴 피드백 작업 하에서 문서의 많은 부분이 재작성되고 재구성되었습니다.

~~Jessica Tallon and Christopher Lemmer Webber took over as editors when the standard moved to the <abbr title="World Wide Web Consortium">W3C</abbr> Social Working Group and did the majority of transition from Erin Shepherd's document to its current state as ActivityPub. Much of the document was rewritten and reorganized under the long feedback process of the Social Working Group.~~

액티비티펍은 <abbr title="World Wide Web Consortium">W3C</abbr> 소셜 워킹그룹의 많은 회원들의 신중한 의견들을 통하여 형성되었습니다. 액티비티펍은 특히 [[소셜 웹 프로토콜](https://www.w3.org/TR/activitypub/#bib-Social-Web-Protocols)]를 작성하여 서로 다른 소셜 워킹 그룹 문서들에서 아이디어를 매핑하기 위해 누구보다도 많은 작업을 한 Amy Guy에게 큰 빚을 지고 있습니다. Amy는 또한 Christopher Allan Webber와 함께 4일 동안 밤낮을 새며 액티비티펍 사양의 중요한 리팩토링을 위한 토대를 마련했습니다. 이러한 수정 사항으로 인해 액티비티펍과 [[LDN](https://www.w3.org/TR/activitypub/#bib-LDN)]의 관계에 대한 명확성과 함께 클라이언트-서버와 서버 구성 요소가 명확하게 분리되었습니다. 구현 보고서 템플릿을 구성해준 Benjamin Goering에게도 특별히 감사드립니다. 또한 멋진 튜토리얼 일러스트(이 문서의 나머지 부분과 동일한 라이센스에 따라 라이센스가 부여됨)를 제작 해주신 mray에게도 감사드립니다.

~~ActivityPub has been shaped by the careful input of many members in the <abbr title="World Wide Web Consortium">W3C</abbr> Social Working Group. ActivityPub especially owes a great debt to Amy Guy, who has done more than anyone to map the ideas across the different Social Working Group documents through her work on [[Social-Web-Protocols](https://www.w3.org/TR/activitypub/#bib-Social-Web-Protocols)]. Amy also laid out the foundations for a significant refactoring of the ActivityPub spec while sprinting for four days with Christopher Allan Webber. These revisions lead to cleaner separation between the client to server and server components, along with clarity about ActivityPub's relationship to [[LDN](https://www.w3.org/TR/activitypub/#bib-LDN)], among many other improvements. Special thanks also goes to Benjamin Goering for putting together the implementation report template. We also thank mray for producing the spectacular tutorial illustrations (which are licensed under the same license as the rest of this document).~~

[//Comment]: # "의역"

많은 사람들이 신중한 검토를 통해 액티비티펍에 도움을 주었습니다. 특히 다음과 같은 분들께 감사의 말씀을 드립니다: Aaron Parecki, AJ Jordan, Benjamin Goering, Caleb Langeslag, Elsa Balderrama, elf Pavlik, Eugen Rochko, Erik Wilde, Jason Robinson, Manu Sporny, Michael Vogel, Mike Macgirvin, nightpool, Puck Meerburg, Sandro Hawke, Sarven Capadisli, Tantek Çelik 및 Yuri Volkov.

~~Many people also helped ActivityPub along through careful review. In particular, thanks to: Aaron Parecki, AJ Jordan, Benjamin Goering, Caleb Langeslag, Elsa Balderrama, elf Pavlik, Eugen Rochko, Erik Wilde, Jason Robinson, Manu Sporny, Michael Vogel, Mike Macgirvin, nightpool, Puck Meerburg, Sandro Hawke, Sarven Capadisli, Tantek Çelik, and Yuri Volkov.~~

이 문서를 지구의 모든 시민들에게 바칩니다. 당신은 의사 소통의 자유를 받을 권리가 있습니다; 설령 작은 부분이었을지라도, 저희는 그 목표를 향해, 올바른 방향으로 기여하였기를 바랍니다.

~~This document is dedicated to all citizens of planet Earth. You deserve freedom of communication; we hope we have contributed in some part, however small, towards that goal and right.~~