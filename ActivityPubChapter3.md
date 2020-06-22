## 3. 객체 (Objects)

객체는 [[액티비티스트림](https://www.w3.org/TR/activitypub/#bib-ActivityStreams)]과 액티비티펍이 기반하는 핵심 개념입니다. 객체는 종종 액티비티들로 감싸져 있고 객체의 하위 클래스인 컬렉션들의 스트림에 포함되어 있습니다. [[액티비티-어휘](https://www.w3.org/TR/activitypub/#bib-Activity-Vocabulary)] 문서를 참고하시고, 특히 액티비티펍에서 중요하게 다뤄지는 [핵심 클래스](https://www.w3.org/TR/activitystreams-vocabulary/#types) 부분을 참고하시길 바랍니다.

~~Objects are the core concept around which both [[ActivityStreams](https://www.w3.org/TR/activitypub/#bib-ActivityStreams)] and ActivityPub are built. Objects are often wrapped in Activities and are contained in streams of Collections, which are themselves subclasses of Objects. See the [[Activity-Vocabulary](https://www.w3.org/TR/activitypub/#bib-Activity-Vocabulary)] document, particularly the [Core Classes](https://www.w3.org/TR/activitystreams-vocabulary/#types); ActivityPub follows the mapping of this vocabulary very closely.~~

[//Comment]: # "Streams->스트림으로 번역하였습니다."
[//Comment]: # "마지막 문장 의역에 주의."

액티비티펍은 액티비티스트림이 제공한 용어들 외에도 추가적으로 용어를 정의합니다. 이러한 용어들은 `https://www.w3.org/ns/activitystreams` 의 액티비티펍 [JSON-LD 컨텍스트](http://www.w3.org/TR/json-ld/#the-context)에서 제공되고 있습니다. 구현하시는 분들께서는 액티비티펍 컨텍스트를 객체의 정의에 포함시키는 것을 *권장합니다*. 또한 구현하시는 분들께서는 추가적인 컨텍스트를 적절하게 포함시킬 *수도* 있습니다.

~~ActivityPub defines some terms in addition to those provided by ActivityStreams. These terms are provided in the ActivityPub [JSON-LD context](http://www.w3.org/TR/json-ld/#the-context) at `https://www.w3.org/ns/activitystreams`. Implementers *SHOULD* include the ActivityPub context in their object definitions. Implementers *MAY* include additional context as appropriate.~~

액티비티펍은 [액티비티스트림과 동일한 URI / IRI 관습](https://www.w3.org/TR/activitystreams-core/#urls)을 공유합니다.

~~ActivityPub shares the same [URI / IRI conventions as in ActivityStreams](https://www.w3.org/TR/activitystreams-core/#urls).~~

서버는 컨텐츠 스푸핑 공격을 방지하기 위하여 수신한 콘텐츠의 유효성을 검증하기를 *권장합니다.* (서버는 적어도 오브젝트가 원 발신지로부터 수신되었는지 검증하는 등의 단순한 절차를 거쳐야 하지만, 가능하다면 서명 등을 검증하는 등의 메커니즘이 더 좋습니다). 이 문서에서는 검증 메커니즘에 대해서는 특별히 규정하고 있지 않지만, [보안 고려사항](https://www.w3.org/TR/activitypub/#security-considerations) 를 참고하여 제안과 선례 등을 참고해 주시길 바랍니다.

~~Servers *SHOULD* validate the content they receive to avoid content spoofing attacks. (A server should do something at least as robust as checking that the object appears as received at its origin, but mechanisms such as checking signatures would be better if available). No particular mechanism for verification is authoritatively specified by this document, but please see [Security Considerations](https://www.w3.org/TR/activitypub/#security-considerations) for some suggestions and good practices.~~

[//Comment]: # "\* 원문은 consideration 입니다. 사전적 의미는 참작 등으로 번역될 수 있으나 문맥상 提高로 번역하였습니다."
[//Comment]: # " ㄴ 위 사항에 대해서는 목차 목록 일치화를 위하여 '보안 고려사항'으로 변경하였습니다. "

예를 들어, example.com 이 액티비티를 전달받았을 경우

~~As an example, if example.com receives the activity~~

>예시 7
>
>```json
>{
>  "@context": "https://www.w3.org/ns/activitystreams",
>  "type": "Like",
>  "actor": "https://example.net/~mallory",
>  "to": ["https://hatchat.example/sarah/",
>         "https://example.com/peeps/john/"],
>  "object": {
>    "@context": {"@language": "en"},
>    "id": "https://example.org/~alice/note/23",
>    "type": "Note",
>    "attributedTo": "https://example.org/~alice",
>    "content": "I'm a goat"
>  }
>}
>```

[//Comment]: # "@language가 en으로 설정되어 있으므로 따로 번역을 하지 않겠습니다."

전달받은 측은 `id` 를 역 참조하여 이것이 유효한 객체로 존재하는지의 여부와 객체를 잘못 반영하고 있지 않은지를 검증하여야 합니다. (예시의 경우, Mallory는 Alice 에 의해 포스트된 것으로 보이는 오브젝트를 변조하였을 수 있습니다.)

~~it should dereference the `id` both to ensure that it exists and is a valid object, and that it is not misrepresenting the object. (In this example, Mallory could be spoofing an object allegedly posted by Alice).~~

### 3.1 객체 구분자 (Object Identifiers)

[액티비티스트림]의 모든 객체는 고유의 전역 구분자(global identifiers)를 가져야 합니다. 액티비티펍은 이 요구사항의 연장선에 있습니다; 액티비티펍 프로토콜에 의해 배포되는 모든 객체들은 의도적으로 일시적인 객체로 만든 경우(접근되도록 의도되지 않은 단기 활동들로 챗 메세지나 게임 알림 등이 이에 해당합니다)에 해당하지 않는 한, 고유의 전역 구분자를 *반드시* 가져야 합니다. 이러한 구분자들은 아래의 그룹 중 하나에 속합니다:

~~All Objects in [ActivityStreams] should have unique global identifiers. ActivityPub extends this requirement; all objects distributed by the ActivityPub protocol *MUST* have unique global identifiers, unless they are intentionally transient (short lived activities that are not intended to be able to be looked up, such as some kinds of chat messages or game notifications). These identifiers must fall into one of the following groups:~~

1. 원 발신지 서버에 권한이 귀속되는, HTTPS URI 등 공개적으로 역 참조가 가능한 URI. (외부에 닿아있는 컨텐츠는 HTTPS URI를 사용하기를 *권장합니다*.)
2. JSON `null` 객체임이 명시적으로 특정되어 (부모 컨텍스트의 일부인) 익명의 객체임을 암시하는 ID.

~~1. Publicly dereferencable URIs, such as HTTPS URIs, with their authority belonging to that of their originating server. (Publicly facing content *SHOULD* use HTTPS URIs).~~

~~2. An ID explicitly specified as the JSON `null` object, which implies an anonymous object (a part of its parent context)~~

[//Comment]: # "\* 원문은 authority"
[//Comment]: # " ㄴ 위 사항에 대해서는 번역 일관성을 위하여 '권한'으로 변경하였습니다. "

서버-서버 간 통신에서 액티비티가 의도적으로 일시적인 객체로 만들은 경우가 아닌 한 구분자는 *반드시* 제공되어야 합니다. 그러나, 클라이언트-서버 간 통신에서는 특정된 `id` 없이 아웃박스에 게시된 객체를 수신한 서버는 객체 ID를 액터의 namespace에 할당한 후 이를 게시된 객체에 첨부하기를 *권장합니다*.

~~Identifiers *MUST* be provided for activities posted in server to server communication, unless the activity is intentionally transient. However, for client to server communication, a server receiving an object posted to the outbox with no specified `id` *SHOULD* allocate an object ID in the actor's namespace and attach it to the posted object.~~

모든 객체들은 다음과 같은 속성들을 가집니다:

~~All objects have the following properties:~~

**id**

객체의 고유한 전역 구분자 (객체가 일시적인 경우에는 id가 생략되어 *있을 수도* 있습니다)

**type**

객체의 타입

~~**id**~~

~~The object's unique global identifier (unless the object is transient, in which case the id *MAY* be omitted).~~

~~**type**~~

~~The type of the object.~~

### 3.2 객체 회수 (Retrieving objects)

액티비티를 검색하기 위하여 HTTP GET 방식은 객체의 `id` 속성에 대하여 역 참조를 할 수도 있습니다. 서버는 [RFC7231](https://www.w3.org/TR/activitypub/#bib-RFC7231)에 정의되어 있는 HTTP 컨텐츠 협상을 사용하여 요청에 대한 응답으로 반환할 데이터 유형을 선택 *할 수도* 있지만, `application/ld+json; profile="https://www.w3.org/ns/activitystreams"`에 대한 응답으로 액티비티스트림 객체 표현을 *반드시* 제시하여야 하며, 또한 `application/activity+json`에 대한 응답으로는 액티비티스트림 표현을 제시하는 것을 *권장합니다*. 클라이언트는 액티비티를 검색하기 위하여 *반드시* `application/ld+json; profile="https://www.w3.org/ns/activitystreams"` 미디어 타입이 포함되어 있는 수락 헤더를 지정하여야 합니다.

~~The HTTP GET method may be dereferenced against an object's `id` property to retrieve the activity. Servers *MAY* use HTTP content negotiation as defined in [RFC7231](https://www.w3.org/TR/activitypub/#bib-RFC7231) to select the type of data to return in response to a request, but *MUST* present the ActivityStreams object representation in response to `application/ld+json; profile="https://www.w3.org/ns/activitystreams"`, and *SHOULD* also present the ActivityStreams representation in response to `application/activity+json` as well. The client *MUST* specify an Accept header with the `application/ld+json; profile="https://www.w3.org/ns/activitystreams"` media type in order to retrieve the activity.~~

서버는 위의 요구 사항을 준수하지 않는 요청에 대해 다른 동작을 도입 *할 수도* 있습니다. (예를 들어 서버는 추가 레거시 프로토콜을 구현하거나 HTML의 HTML 및 액티비티스트림 표현에 동일한 URI를 사용할 수 있습니다).

~~Servers *MAY* implement other behavior for requests which do not comply with the above requirement. (For example, servers may implement additional legacy protocols, or may use the same URI for both HTML and ActivityStreams representations of a resource).~~

서버는 B.1 [인증 및 권한](https://www.w3.org/TR/activitypub/#authorization)에 지정된 권한을 요구 *할 수도* 있으며, 추가로 자체 권한 부여 규칙을 구현 할 수도 있습니다. 서버는 적절한 HTTP 오류 코드 또는 객체가 비공개로 간주되는 403 Forbidden error 에러코드와 함꼐 권한 부여 검사를 통과하지 못한 요청에 대하여 실패하는 것을 *권장합니다*. 비공개 대상의 존재를 공개하지 않으려는 발신원 서버는 404 Not Found로 응답 *할 수도* 있습니다.

~~Servers *MAY* require authorization as specified in B.1 [Authentication and Authorization](https://www.w3.org/TR/activitypub/#authorization), and may additionally implement their own authorization rules. Servers *SHOULD* fail requests which do not pass their authorization checks with the appropriate HTTP error code, or the 403 Forbidden error code where the existence of the object is considered private. An origin server which does not wish to disclose the existence of a private target *MAY* instead respond with a status code of 404 Not Found.~~

### 3.3 출저 속성 (The source property)

액티비티펍은 [액티비티-어휘](https://www.w3.org/TR/activitypub/#bib-Activity-Vocabulary)에서 정의한 모든 속성 외에도 `source` 속성을 제공하여 `Object`를 확장합니다. `source`속성은 `content` 마크업의 출저(source)를 출발지(provenance)의 형태로 전달하거나 클라이언트들의 항후 편집을 지원하기 위하여 존재합니다. 일반적으로 클라이언트는 `source`에서 `content`으로만의 변환을 수행합니다.

~~In addition to all the properties defined by the [Activity-Vocabulary](https://www.w3.org/TR/activitypub/#bib-Activity-Vocabulary), ActivityPub extends the `Object` by supplying the `source` property. The `source` property is intended to convey some sort of source from which the `content` markup was derived, as a form of provenance, or to support future editing by clients. In general, clients do the conversion from `source` to `content`, not the other way around.~~

`source`의 값은 객체이며, 객체 자신의 `content` 및 `mediaType` 필드를 사용하여 출저 정보를 제공합니다.

~~The value of `source` is itself an object which uses its own `content` and `mediaType` fields to supply source information.~~

>예시 8
>
>```json
>{
>  "@context": ["https://www.w3.org/ns/activitystreams",
>               {"@language": "en"}],
>  "type": "Note",
>  "id": "http://postparty.example/p/2415",
>  "content": "<p>I <em>really</em> like strawberries!</p>",
>  "source": {
>    "content": "I *really* like strawberries!",
>    "mediaType": "text/markdown"}
>}
>```

[//]: # "@language가 en으로 설정되어 있으므로 따로 번역을 하지 않겠습니다."

> 참고 : 클라이언트가 mediaType을 적절히 처리 할 수 없는 경우에는 어떻게 해야합니까?
>
> 일반적으로 사용자가 원본 게시물을 원본과 동일한 출저 형식으로 편집하도록하는 것이 가장 좋습니다. 그러나 모든 클라이언트가 모든 출저 유형에 대해 훌륭한 인터페이스를 안정적으로 제공 할 수있는 것은 아니며 클라이언트가 `source`에서`content`로 변환하는 작업을 하는 것을 전제로 둠으로, 일부 클라이언트는 다른 클라이언트가 처리하는 방법을 모르는 미디어 타입으로 작업을 할 수도 있습니다. 클라이언트가 편집할수 있는 `content` 마크업을 제공하고 출저를 무시할 수는 있지만, 하지만 이는 향후 개정판에서 사용자는 원본 `source`보다 바람직한 형태를 잃게 될 수도 있다는 것을 의미합니다. 따라서 클라이언트는 원본 출저 형식을 이해하지 못하여 무시되고 있음을 경고하는 최소한의 방해 경고를 제공해야합니다.
>
> 예를 들어 Alyssa P. Hacker는 자신이 작성한 Emacs 클라이언트를 통해 [Org mode](http://orgmode.org/)를 활용하여 액티비티펍에 기반헌 자신의 블로그에 게시하는 것을 좋아합니다. 나중에 그녀는 휴대폰의 클라이언트에서 편집모드로 전환하는데, `text / x-org`가 무엇인지, 또는 HTML로 렌더링하는 방법을 모릅니다. 대신 원본 `content`를 편집 할 수있는 텍스트 상자를 제공합니다. 편집 영역 위에 "이것은 원래 처리 방법을 모르는 다른 마크업 언어로 작성되었습니다. 편집하면 원본 출저를 잃게됩니다!" 라는 유용한 경고가 표시됩니다. Alyssa는 작은 오타 수정을 위해 멋진 org-mode 마크업을 잃을 가치가 없다고 결정하고 집에 도착하면 업데이트하기로 결정합니다.

~~Note: What to do when clients can't meaningfully handle a mediaType?~~

~~In general, it's best to let a user edit their original post in the same source format they originally composed it in. But not all clients can reliably provide a nice interface for all source types, and since clients are expected to do the conversion from `source` to `content`, some clients may work with a media type that another client does not know how to work with. While a client could feasibly provide the `content` markup to be edited and ignore the source, this means that the user will lose the more desirable form of the original `source` in any future revisions. A client doing so should thus provide a minimally obtrusive warning cautioning that the original source format is not understood and is thus being ignored.~~

~~For example, Alyssa P. Hacker likes to post to her ActivityPub powered blog via an Emacs client she has written, leveraging [Org mode](http://orgmode.org/). Later she switches to editing on her phone's client, which has no idea what `text/x-org` is or how to render it to HTML, so it provides a text box to edit the original `content` instead. A helpful warning displays above the edit area saying, "This was originally written in another markup language we don't know how to handle. If you edit, you'll lose your original source!" Alyssa decides the small typo fix isn't worth losing her nice org-mode markup and decides to make the update when she gets home.~~
