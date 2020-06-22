[목차로 돌아가기](ActivityPubContents.md)

# [액티비티펍(ActivityPub)](https://www.w3.org/TR/activitypub) 

2018년 1월 23일 <abbr title="World Wide Web Consortium">W3C</abbr>권고안

~~<abbr title="World Wide Web Consortium">W3C</abbr>Recommendation 23 January 2018~~

**현재 버전:** ~~This Version~~
* https://www.w3.org/TR/2018/REC-activitypub-20180123/

**최신 게시 버전:** ~~Latest published version~~
* https://www.w3.org/TR/activitypub/

**최신 편집자 초안:** ~~Latest editor's draft~~
* https://w3c.github.io/activitypub/

**테스트 모음:** ~~Test suite~~
* https://test.activitypub.rocks/

**구현 보고서:** ~~Implementation report~~
* https://activitypub.rocks/implementation-report

**이전 버전:** ~~Previous version~~
* https://www.w3.org/TR/2017/PR-activitypub-20171205/

**편집자:** ~~Editors~~
* [Christopher Lemmer Webber](https://dustycloud.org/)
* [Jessica Tallon](https://tsyesika.se/)

**저자:** ~~Authors~~
* [Christopher Lemmer Webber](https://dustycloud.org/)
* [Jessica Tallon](https://tsyesika.se/)
* [Erin Shepherd](http://erinshepherd.net/)
* [Amy Guy](https://rhiaro.co.uk/)
* [Evan Prodromou](https://en.wikipedia.org/wiki/Evan_Prodromou)

**저장소:** ~~Repository~~
* [Git 저장소](https://github.com/w3c/activitypub) ~~Git repository~~
* [이슈들](https://github.com/w3c/activitypub/issues) ~~Issues~~
* [커밋들](https://github.com/w3c/activitypub/commits/gh-pages) ~~Commits~~

[//]: # "참조: [(W3C)웹 콘텐츠 접근성 지침 (WCAG) 2.1](http://www.kwacc.or.kr/WAI/wcag21/)"

이 게시물 이후 보고된 오류나 이슈에 대해서는 [정오표](https://www.w3.org/wiki/ActivityPub_errata) 를 확인해 주시길 바랍니다.

~~Please check the [errata](https://www.w3.org/wiki/ActivityPub_errata) for any errors or issues reported since publication.~~

[//]: # "참조: [(W3C)Role 속성 1.0](https://techhtml.github.io/role-attribute/)"

[번역본](http://www.w3.org/2003/03/Translations/byTechnology?technology=activitypub)도 제공합니다.

~~see also [translations](http://www.w3.org/2003/03/Translations/byTechnology?technology=activitypub).~~ 

[//]: # "참조: [(W3C)Role 속성 1.0](https://techhtml.github.io/role-attribute/)"
[//]: # "참조: [(W3C)성능 타임라인](https://techhtml.github.io/performance-timeline/)"

[Copyright](https://www.w3.org/Consortium/Legal/ipr-notice#Copyright) © 2018 [<abbr title="World Wide Web Consortium">W3C</abbr>](https://www.w3.org/)® (<abbr title="매사추세츠 공과대학교">[MIT](https://www.csail.mit.edu/)</abbr>, <abbr title="European Research Consortium for Informatics and Mathematics">[ERCIM](https://www.ercim.eu/)</abbr>, [Keio](https://www.keio.ac.jp/), [Beihang](http://ev.buaa.edu.cn/)). <abbr title="World Wide Web Consortium">W3C</abbr> [법적 책임](https://www.w3.org/Consortium/Legal/ipr-notice#Legal_Disclaimer), [상표](https://www.w3.org/Consortium/Legal/ipr-notice#W3C_Trademarks)와 [문서 허용 라이센스](https://www.w3.org/Consortium/Legal/2015/copyright-software-and-document) 규칙이 적용됩니다.

~~[Copyright](https://www.w3.org/Consortium/Legal/ipr-notice#Copyright) © 2018 [<abbr title="World Wide Web Consortium">W3C</abbr>](https://www.w3.org/)® (<abbr title="Massachusetts Institute of Technology">[MIT](https://www.csail.mit.edu/)</abbr>, <abbr title="European Research Consortium for Informatics and Mathematics">[ERCIM](https://www.ercim.eu/)</abbr>, [Keio](https://www.keio.ac.jp/), [Beihang](http://ev.buaa.edu.cn/)). <abbr title="World Wide Web Consortium">W3C</abbr> [liability](https://www.w3.org/Consortium/Legal/ipr-notice#Legal_Disclaimer), [trademark](https://www.w3.org/Consortium/Legal/ipr-notice#W3C_Trademarks) and [permissive document license](https://www.w3.org/Consortium/Legal/2015/copyright-software-and-document) rules apply.~~

[//]: # "참조: [(W3C)웹 콘텐츠 접근성 지침 (WCAG) 2.1](http://www.kwacc.or.kr/WAI/wcag21/)"

### 개요 (Abstract)

액티비티펍(ActivityPub) 프로토콜은 [액티비티스트림(ActivityStreams)](https://www.w3.org/TR/activitypub/#bib-ActivityStreams) 2.0 데이터 형식을 기반으로 하는 분산 소셜 네트워킹 프로토콜입니다. 컨텐츠 생성, 갱신, 삭제를 위한 클라이언트-서버 간 API를 제공하며, ‘연합’으로 이루어진 서버-서버 간의 알림과 컨텐츠 전달을 위한 API도 제공합니다.

~~The ActivityPub protocol is a decentralized social networking protocol based upon the [[ActivityStreams](https://www.w3.org/TR/activitypub/#bib-ActivityStreams)] 2.0 data format. It provides a client to server API for creating, updating and deleting content, as well as a federated server to server API for delivering notifications and content.~~

[//]: # "'Client to Server'를 '클라이언트와 서버간' 으로 번역하였듯 단방향이 아닌 양방향으로 해석될 여지가 있으나 이를 의도하는게 맞다고 판단되어 A와B간(사이)로 번역하였습니다. 차후 챕터 번역시 해당 내용이 오류였다고 생각이 들 경우 수정하겠습니다(//TODO)"

[//]: # "해당 내용은 사전에 작성되어있던 ActivityPub Chapter1의 번역되어 있던 Intro부분을 참조하였습니다. 일단 해당 번역과 차이나는 부분은: {탈중앙화->분산, CRUD->생성/갱신/삭제} 정도인 것 같습니다. (//TODO)"

[//]: # "원문중 'federated server to server' 부분을 '[연합]으로 이루어진 서버-서버간' 으로 번역하거나 '연합서버-서버 간' 으로 번역할 수 있는 선택지에서 일단은 전자를 택하겠습니다. 차후에 의미 전달이 명확하지 않았다 판단될 경우에는 수정하겠습니다. (//TODO)"

### 이 문서의 상태 (Status of This Document)

이 문단에서는 현 문서의 발행 당시의 상태에 대하여 기술합니다. 다른 문서가 이 문서를 대체 할 수 있습니다. 현재 <abbr title="World Wide Web Consortium">W3C</abbr> 출판물 목록 및 이 기술보고서의 최신 개정판은 https://www.w3.org/TR/ 에 있는 [<abbr title="World Wide Web Consortium">W3C</abbr> 기술보고서 색인](https://www.w3.org/TR/)에서 찾아볼 수 있습니다.

~~This section describes the status of this document at the time of its publication. Other documents may supersede this document. A list of current <abbr title="World Wide Web Consortium">W3C</abbr> publications and the latest revision of this technical report can be found in the [<abbr title="World Wide Web Consortium">W3C</abbr> technical reports index](https://www.w3.org/TR/) at https://www.w3.org/TR/.~~

[//]: # "참조: [(W3C)검증가능한 크리덴셜 데이터 모델 1.0](https://ssimeetupkorea.github.io/vc-data-model/)"

이 문서는 [소셜 웹 워킹 그룹(Social Web Working Group)](https://www.w3.org/Social/WG)에 의하여 작성된 권고안입니다.

~~This document was published by the [Social Web Working Group](https://www.w3.org/Social/WG) as a Recommendation.~~

모든 이해 관계자들은 작업 그룹의 [[이슈 트래커]](https://github.com/w3c/activitypub/issues)에 구현체 및 기타 의견을 제공하도록 초대되었습니다. 제공된 의견들은 [소셜 웹 워킹 그룹](https://www.w3.org/Social/WG)에서 논의될 것이며, 이후 버전에서 고려될 것입니다.

~~All interested parties are invited to provide implementation and bug reports and other comments through the Working Group's [Issue tracker](https://github.com/w3c/activitypub/issues). These will be discussed by the [Social Web Community Group](http://www.w3.org/wiki/SocialCG) and considered in any future versions of this specification.~~

[//]: # "참조: [(W3C)Role 속성 1.0](https://techhtml.github.io/role-attribute/)"
[//]: # "참조: [(W3C)웹 콘텐츠 접근성 지침 2.1](http://www.kwacc.or.kr/WAI/wcag21/)"
[//]: # "참조: [(W3C)Web Content Accessibility Guidelines 2.1](https://www.w3.org/TR/WCAG21/)"
[//]: # "참조: [(W3C)Web内容无障碍指南2.1](https://www.w3.org/Translations/WCAG21-zh/)"
[//]: # "의견: 조금더 다듬어야 할 것 같습니다. 관심있는 분들께서는~ 같은 형식도 괜찮을것 같습니다. (//TODO)"

작업 그룹의 [구현 레포트](https://activitypub.rocks/implementation-report)를 확인해 주시길 바랍니다.

~~Please see the Working Group's [implementation report](https://activitypub.rocks/implementation-report).~~

[//]: # "참조: [(W3C)미디어 쿼리](https://techhtml.github.io/css3-mediaqueries/)"

이 문서는 <abbr title="World Wide Web Consortium">W3C</abbr> 회원, 소프트웨어 개발자 및 기타 W3C 그룹과 이해 관계자들에게 검토되었으며, 위원장이 <abbr title="World Wide Web Consortium">W3C</abbr> 권고안으로 발표하였습니다. 이 문서는 안정적이며, 참고자료로 사용되거나 다른 문서에서 인용될수 있습니다. <abbr title="World Wide Web Consortium">W3C</abbr>가 권고안을 만드는 것은 표준사양에 대하여 주의를 환기시키고, 보다 넓은 범위에서 그 사용을 촉진시키기 위함입니다. 이는 웹의 기능성과 보편성을 향상시킬 수 있습니다.

~~This document has been reviewed by <abbr title="World Wide Web Consortium">W3C</abbr> Members, by software developers, and by other <abbr title="World Wide Web Consortium">W3C</abbr> groups and interested parties, and is endorsed by the Director as a <abbr title="World Wide Web Consortium">W3C</abbr> Recommendation. It is a stable document and may be used as reference material or cited from another document. <abbr title="World Wide Web Consortium">W3C</abbr>'s role in making the Recommendation is to draw attention to the specification and to promote its widespread deployment. This enhances the functionality and interoperability of the Web.~~

[//]: # "참조: [(W3C)웹 콘텐츠 접근성 지침 2.1](http://www.kwacc.or.kr/WAI/wcag21/)"
[//]: # "참조: [(W3C)성능 타임라인](https://techhtml.github.io/performance-timeline/)"
[//]: # "참조: [(W3C)웹 콘텐츠(의) 접근성(을 높이기 위한 제작) 지침 1.0](http://gregshin.pe.kr/wcag/wai-pageauth.html)"

이 문서는 [<abbr title="World Wide Web Consortium">W3C</abbr> 특허 정책](https://www.w3.org/Consortium/Patent-Policy/)에 따라 운영되는 그룹이 작성하였습니다. <abbr title="World Wide Web Consortium">W3C</abbr>는 그룹의 성과물에 관한 [모든 공개 특허 공개 목록](https://www.w3.org/2004/01/pp-impl/72531/status)을 관리합니다. 이곳에서는 특허 공개에 대한 지시사항도 포함됩니다. [필수 주장(들)](https://www.w3.org/Consortium/Patent-Policy/#def-essential)을 포함한다고 생각되는 특허에 대하여 실제 지식을 보유한 개인은 [<abbr title="World Wide Web Consortium">W3C</abbr> 특허 정책](https://www.w3.org/Consortium/Patent-Policy/#sec-Disclosure)에 의하여 정보를 공개하여야 합니다.

~~This document was produced by a group operating under the [<abbr title="World Wide Web Consortium">W3C</abbr> Patent Policy](https://www.w3.org/Consortium/Patent-Policy/). <abbr title="World Wide Web Consortium">W3C</abbr> maintains a [public list of any patent disclosures](https://www.w3.org/2004/01/pp-impl/72531/status) made in connection with the deliverables of the group; that page also includes instructions for disclosing a patent. An individual who has actual knowledge of a patent which the individual believes contains [Essential Claim(s)](https://www.w3.org/Consortium/Patent-Policy/#def-essential) must disclose the information in accordance with [section 6 of the <abbr title="World Wide Web Consortium">W3C</abbr> Patent Policy](https://www.w3.org/Consortium/Patent-Policy/#sec-Disclosure).~~

[//]: # "참조: [(W3C)웹 콘텐츠 접근성 지침 2.1](http://www.kwacc.or.kr/WAI/wcag21/)"
[//]: # "참조: [(W3C)성능 타임라인](https://techhtml.github.io/performance-timeline/)"
[//]: # "의견: 조금 더 다듬어야 할 것 같습니다. '모든 공개 특허 공개 목록'같이 불필요한 단어가 들어가 있는 듯한 느낌도 있습니다. (//TODO)"

이 문서는 [2018년 3월 1일 <abbr title="World Wide Web Consortium">W3C</abbr> 프로세스 문서](https://www.w3.org/2017/Process-20170301/)를 적용받습니다.

~~This document is governed by the [1 March 2017 <abbr title="World Wide Web Consortium">W3C</abbr> Process Document](https://www.w3.org/2017/Process-20170301/).~~

[//]: # "참조: [(W3C)웹 콘텐츠 접근성 지침 2.1](http://www.kwacc.or.kr/WAI/wcag21/)"
[//]: # "참조: [(W3C)Web内容无障碍指南2.1](https://www.w3.org/Translations/WCAG21-zh/)"