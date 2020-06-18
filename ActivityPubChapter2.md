[목차로 돌아가기](ActivityPubContents.md)

## 2. 규칙 ~~Conformance~~

비표준으로 표시된 섹션들과 마찬가지로, 본 규격의 모든 작성된 가이드라인, 다이어그램, 예시, 노트들은 비표준입니다. 그 외의 모든 것들은 표준입니다.

~~As well as sections marked as non-normative, all authoring guidelines, diagrams, examples, and notes in this specification are non-normative. Everything else in this specification is normative.~~

[//]: # "참조: [(W3C))HTML Media Capture](https://techhtml.github.io/html-media-capture/#conformance)"

*~수 있다 (MAY)*,*~해야 한다 (MUST)*, *~해서는 아니 된다 (MUST NOT)*, *~권장된다 (SHOULD)*, *~권장되지 않는다 (SHOULD NOT)* 의 키워드들의 해석은  [[RFC2119](https://www.w3.org/TR/activitypub/#bib-RFC2119)] 를 따릅니다.  

~~The key words  *MAY*, *MUST*, *MUST NOT*, *SHOULD*, and *SHOULD NOT* are    to be interpreted as described in [[RFC2119](https://www.w3.org/TR/activitypub/#bib-RFC2119)].~~

### 2.1 규격 개요 ~~Specification Profiles~~

본 규격은 두 가지 밀접하게 관련되어 있으며 상호작용하는 프로토콜들을 정의합니다:

~~This specification defines two closely related and interacting protocols:~~

**클라이언트 - 서버 간 프로토콜, 혹은 "소셜 API"**
- 이 프로토콜은 클라이언트가 사용자를 *대신하여* 활동하도록 허가합니다. 예를 들어, 이 프로토콜은 휴대폰의 어플리케이션이 사용자의 액터에 있는 소셜 스트림과 상호작용할 수 있도록 사용됩니다. 
  
**서버 - 서버 간 프로토콜, 혹은 "연합 프로토콜"** 
- 이 프로토콜은 액티비티를 다른 서버의 액터들 사이에 퍼져나가게 하는 동시에 동일한 소셜 그래프 내에 묶어두는데 쓰입니다.

~~**A client to server protocol, or "Social API"**~~

~~This protocol permits a client to act *on behalf* of a user.  For example, this protocol is used by a mobile phone application to interact with a social stream of the user's actor.~~

~~**A  server to server protocol, or "Federation Protocol"**~~

~~This protocol is used to distribute activities between actors on different servers, tying them into the same social graph.~~

액티비티펍 규격은 두 프로토콜 중 하나가 구현되면, 아주 적은 노력만 사용하여도 나머지 하나를 지원할수 있게 설계되었습니다. 그러나 서버는 여전히 나머지 하나 없이 단 한 가지의 프로토콜만 도입 할 수도 있습니다. 이는 세 가지의 준수 경우로 구분 할 수 있습니다:

~~The ActivityPub specification is designed so that once either of these protocols are implemented, supporting the other is of very little additional effort. However, servers may still implement one without the other. This gives three conformance classes:~~

**액티비티펍 준수 클라이언트**
- 이 분류는 클라이언트 - 서버 간 프로토콜에서 클라이언트 부분이 완전히 구현되었을 경우 적용됩니다.

**액티비티펍 준수 서버**
- 이 분류는 클라이언트 - 서버 간 프로토콜에서 서버 부분이 완전히 구현되었을 경우 적용됩니다.

**액티비티펍 준수 연합 서버**
- 이 분류는 완전한 연합 프로토콜이 구현되었을 경우 적용됩니다.

~~**ActivityPub conformant Client**~~

~~This designation applies to any implementation of the entirety of the client portion of the client to server protocol.~~

~~**ActivityPub conformant Server**~~

~~This designation applies to any implementation of the entirety of the server portion of the client to server protocol.~~

~~**ActivityPub conformant Federated Server**~~

 ~~This designation applies to any implementation of the entirety of the federation protocols.~~

이는 구현된 연합 프로토콜이 규격의 일부만 적용되었을 때 마다 호출됩니다. 또한 요구사항이 특정될 때마다 클라이언트, (클라이언트-서버 간 프로토콜을 위한) 서버, 또는 서버-서버 간 프로토콜에서 전송/전달을 위하여 적용할 것인지 알기 위하여 위의 규격이 다시 호출되게 됩니다.

~~It is called out whenever a portion of the specification only applies to implementation of the federation protocol. In addition, whenever requirements are specified, it is called out whether they apply to the client or server (for the client-to-server protocol) or whether referring to a sending or receiving server in the server-to-server protocol.~~
