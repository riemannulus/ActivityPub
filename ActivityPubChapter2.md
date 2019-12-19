### 2. 준수  ~~Conformance~~

비규범적인 것으로 표시된 섹션들과 마찬가지로, 본 규격의 모든 작성된 가이드라인, 다이어그램, 예시, 노트들은 비규범적입니다. 규격 안의 모든 나머지 것들은 규범적입니다. (준수되어야 합니다)

~~As well as sections marked as non-normative, all authoring guidelines, diagrams, examples, and notes in this specification are non-normative. Everything else in this specification is   normative.~~

`~수 있다`, `~해야 한다`, `~해서는 아니 된다`, `~ 권장된다`, `~권장되지 않는다` 의 키워드들의 해석은  [[RFC2119](https://www.w3.org/TR/activitypub/#bib-RFC2119)] 를 따릅니다.  

~~The key words  *MAY*, *MUST*, *MUST NOT*, *SHOULD*, and *SHOULD NOT* are    to be interpreted as described in [[RFC2119](https://www.w3.org/TR/activitypub/#bib-RFC2119)].~~

#### 2.1 규격 프로파일 ~~Specification Profiles~~

본 규격은 두 가지 근접한 상호작용 프로토콜을 정의합니다.

~~This specification defines two closely related and interacting protocols:~~

- 클라이언트 - 서버 프로토콜, 혹은 "소설 API" ~~A client to server protocol, or "Social API"~~
- 이 프로토콜은 클라이언트가 사용자 _를 대신하여_ 활동하도록 수락합니다. 예를 들어, 이 프로토콜은 모바일 폰 어플리케이션이 사용자의 액터의 소셜 스트림과 상호작용할 수 있도록 사용됩니다. ~~This protocol permits a client to act *on behalf* of a user.  For example, this protocol is used by a mobile phone application to interact with a social stream of the user's actor.~~
  
- 서버 - 서버 간 프로토콜, 혹은 "연합 프로토콜" ~~A  server to server protocol, or "Federation Protocol"~~

  - 이 프로토콜은 액티비티를 다른 서버의 액터들 사이에 퍼져나가게 하는 동시에 동일한 소셜 그래프 내에 묶어두는데 쓰입니다. ~~This protocol is used to distribute activities between actors on different servers, tying them into the same social graph.~~

이 액티비티펍 규격은 두 규격 중 하나가 도입되면, 나머지 하나를 지원하는 것에 아주 적은 추가적인 노력만이 들도록 설계되었습니다. 그러나 서버는 여전히 나머지 하나 없이 한 가지를 도입해야 합니다. 이는 세 가지 준수 클래스를 제시합니다

~~The ActivityPub specification is designed so that once either of these protocols are implemented, supporting the other is of very little additional effort. However, servers may still implement one without the other. This gives three conformance classes:~~

- 액티비티펍 준수 클라이언트 ~~ActivityPub conformant Client~~
  - 이 분류는 클라이언트 - 서버 프로토콜의 모든 클라이언트 도입 부분에 적용됩니다. ~~This designation applies to any implementation of the entirety of the client portion of the client to server protocol.~~
- 액티비티펍 준수 서버 ~~ActivityPub conformant Server~~
  - 이 분류는 클라이언트 - 서버 프로토콜의 모든 서버 도입 부분에 적용됩니다. ~~This designation applies to any implementation of the entirety of the server portion of the client to server protocol.~~
- 액티비티펍 준수 연합 서버 ~~ActivityPub conformant Federated Server~~
  - 이 분류는 모든 연합 서버 도입 부분에 적용됩니다. ~~This designation applies to any implementation of the entirety of the federation protocols.~~

~~연합 프로토콜 도입에만 규격 일부가 적용될 때 콜아웃이 됩니다(?)~~ 또한 필수항목이 특정될 때마다, 필수항목들이 (클라이언트 - 서버 통신에서)클라이언트에 적용되는지 서버에 적용되는지 혹은 서버 간 프로토콜에서 발신 측에 적용이 되는지 혹은 수신 측에 적용이 되는지 여부가 콜아웃 됩니다.

~~It is called out whenever a portion of the specification only applies to implementation of the federation protocol. In addition, whenever requirements are specified, it is called out whether they apply to the client or server (for the client-to-server protocol) or whether referring to a sending or receiving server in the server-to-server protocol.~~
