## ActivityPub

액티비티펍(ActivityPub) 프로토콜은 [ActivityStreams 2.0](<https://www.w3.org/TR/activitypub/#bib-ActivityStreams>)의 데이터 포맷을 이용한 탈 중앙화된 소셜 네트워킹 프로토콜 입니다. 컨텐츠에 대한 CRUD를 지원하는 클라이언트-서버 API를 제공하며, ‘연합’으로 이루어진 서버-서버 간의 알림과 컨텐츠 전달을 위한 API도 제공합니다.

### 1. 개론 ~~Overview~~

액티비티펍은 2개의 레이어로 구성되어 있습니다:

- 서버와 서버 간의 연합 프로토콜 (이것을 이용하여 웹사이트간의 탈 중앙화된 정보 공유가 가능해집니다.)
- 클라이언트와 서버간의 프로토콜(이것을 이용하여 실제 세상의 유저, 봇, 그리고 다른 자동화된 프로세스들을 포함한 유저들이 각각의 계정과 서버를 가지고 기계의 제한 없이 액티비티펍을 이용하여 대화할 수 있습니다.)

액티비티펍 구현체는 이것들 중 하나 혹은 전부를 구현해야 합니다. 그러나 이 레이어 중 하나를 구현했다면, 나머지 하나를 구현하는 데에는 많은 노력이 들지 않습니다. 그리고 둘 다 구현하는 것으로 얻어지는 혜택은 들이는 노력에 비해 매우 큽니다. (당신의 웹사이트 중 일부가 탈 중앙화된  소셜 웹으로 동작하고, 다른 소셜 웹사이트에서 사용하는 클라이언트들을 당신의 웹사이트에서도 사용할 수 있습니다!)

액티비티펍에서 유저는 서버에 있는 사용자의 계정인 "액터(Actor)"로 대표됩니다. 다른 서버에 있는 사용자 계정은 또 다른 액터에 해당합니다. 각각의 액터는 다음과 같은 것들을 가지고 있습니다:

- 인박스(inbox): 액터가 세상으로부터 메세지를 받는 방법.
- 아웃박스(outbox): 액터가 세상으로 메세지를 보내는 방법.

![인박스 아웃박스를 가지고 있는 액터](./img/tutorial-1.png)

이것들은 종단점이거나, 단지 액터 안에 액티비티스트림(ActivityStreams)으로 서술 된 리스트들에 불과합니다. (나중에 액티비티스트림에 대해 좀 더 이야기합니다.)

여기 우리의 친구 알리사의 기록을 통해 예제를 살펴보도록 합시다.

```json
{
    "@context": "https://www.w3.org/ns/activitystream",
    "type": "Person",
    "id": "https://social.example/alyssa/",
    "name": "Alyssa P. Hacker",
    "preferredUserName": "alyssa",
    "summary": "Lisp enthusiast hailing from MIT",
 	"inbox": "https://social.example/alyssa/inbox/",
 	"outbox": "https://social.example/alyssa/outbox/",
 	"followers": "https://social.example/alyssa/followers/",
 	"following": "https://social.example/alyssa/following/",
 	"liked": "https://social.example/alyssa/liked/"
}
```

액티비티펍은 [[액티비티스트림]](https://www.w3.org/TR/activitystreams-core/)을 어휘로 사용합니다. 이 액티비티스트림은 일반적으로 소셜 네트워크를 구성하는 데 통용되는 컨텐츠의 흐름을 나타내기에 적합합니다. 이렇게 액티비티스트림은 당신이 원하는 대부분의 것들을 지원하지만, 만약 원하는 게 없더라도 액티비티스트림은 [[JSON-LD]](https://www.w3.org/TR/json-ld/) 를 통해 내용을 확장할 수 있습니다. 이미 JSON-LD를 사용할 줄 안다면 JSON-LD에서 사용하는 연결된 데이터 접근 방식을 사용할 수 있습니다. 딱히 그렇지 않더라도 JSON-LD는 JSON을 알고 있다면 이해하기 어렵지 않습니다.

자, 이제 알리사는 그녀의 친구와 대화하고 싶어합니다. 그리고 그녀의 친구도 당연히 그렇겠죠! 다행이도 인박스와 아웃박스가 어떻게 해 줄 수 있을 것 같습니다. 그리고 둘 다 GET과 POST에 대해 다르게 동작합니다. 이것들이 어떻게 작동하는지 알아봅시다.

![Actor with messages flowing from rest of
world to inbox and from outbox to rest of world](./img/tutorial-2.png)

흠, 그러니까 설명하자면...

- 나는 메세지를 보내기 위해 수신자의 인박스에 POST 할 수 있습니다. (서버-서버 간)
- 나는 내 인박스로부터 최근 메세지를 가져오기 위해 GET 할 수 있습니다 (클라이언트-서버 간)
- 나는 누군가에게 메세지를 보내기 위해 내 아웃박스에 POST를 할 수 있습니다. (클라이언트-서버 간)
- 나는 누군가가 쓴 글을 읽기 위해 그 사람의 아웃박스에 GET을 할 수 있습니다. (클라이언트-서버 / 서버-서버 간)

당연하게도, 누군가가 쓴 글을 읽기 위해 일일이 그 사람의 아웃박스에 GET을 한다면, 이건 별로 효과적인 프로토콜이라고 부를 수 없겠죠! 당연하게도, 연합에서는 종종 액터에서 다른 서버의 액터가 가지고 있는 인박스로 메세지를 보내는 일이 자주 있습니다.

예제를 한번 들어보죠! 우리의 친구 알리사가 '벤'에게 연락을 하고 싶어하네요. 알리사는 벤에게 빌려줬던 책을 돌려받고 싶어합니다. 그녀의 액티비티스트림을 이용한 메세지를 구성하고 있는 요소들을 살펴보도록 하죠:

```json
{"@context": "https://www.w3.org/ns/activitystreams",
 "type": "Note",
 "to": ["https://chatty.example/ben/"],
 "attributedTo": "https://social.example/alyssa/",
 "content": "Say, did you finish reading that book I lent you?"}
```

이건 벤에게로 가야 할 쪽지입니다. 알리사는 이것을 그녀의 아웃박스로 POST 요청을 날립니다.

![Actor posting message to outbox](./img/tutorial-3.png)

이 쪽지는 논-액티비티non-activity 객체인 관계로, 서버는 이 쪽지를 새로이 생성된 객체로 판단하고, 이것을 생성 액티비티Create activity로 랩핑합니다. (ActivityPub에서 돌아다니는 활동들은 일반적으로 어떤 액터actor가 특정 객체object에 취한 액티비티의 패턴을 따릅니다. 이 경우에는 이 활동은 어떤 사람에 의한 노트 객체의 생성Create이 액티비티가 됩니다.)

~~Since this is a non-activity object, the server recognizes that this is an object being newly created, and does the courtesy of wrapping it in a Create activity. (Activities sent around in ActivityPub generally follow the pattern of some activity by some actor being taken on some object. In this case the activity is a Create of a Note object, posted by a Person).~~

```json
{"@context": "https://www.w3.org/ns/activitystreams",
 "type": "Create",
 "id": "https://social.example/alyssa/posts/a29a6843-9feb-4c74-a7f7-081b9c9201d3",
 "to": ["https://chatty.example/ben/"],
 "actor": "https://social.example/alyssa/",
 "object": {"type": "Note",
            "id": "https://social.example/alyssa/posts/49e2d03d-b53a-4c4c-a95c-94a6abf45a19",
            "attributedTo": "https://social.example/alyssa/",
            "to": ["https://chatty.example/ben/"],
            "content": "Say, did you finish reading that book I lent you?"}}
```

알리사의 서버는 벤의 액티비티스트림 액터 객체를 검색하고, 벤의 인박스 엔드포인트를 찾아서, 객체를 벤의 인박스로 POST합니다.

~~Alyssa's server looks up Ben's ActivityStreams actor object, finds his inbox endpoint, and POSTs her object to his inbox.~~

​         ![Server posting to remote actor's inbox](./img/tutorial-4.png)       

기술적으로 이러한 활동은 두 개의 단계로 나뉘는데... 하나는 클라이언트에서 (알리사의) 서버로 가는 통신과, 하나는 서버 간 통신(페더레이션)입니다. 그러나 이번 예제에서는 두 가지 모두를 사용하게 되므로, 여기서는 아웃박스에서 인박스로 나가는 능률적인streamlined 제출이라고 추상적으로 생각하도록 하겠습니다:

~~Technically these are two separate steps... one is client to server communication, and one is server to server communication (federation). But, since we're using them both in this example, we can abstractly think of this as being a streamlined submission from outbox to inbox:~~

​         ![Note flowing from one actor's outbox to other actor's inbox](./img/tutorial-5.png)       

좋습니다! 잠시 뒤에 알리사는 그녀에게 무슨 메세지가 도착했는지를 확인합니다. 그녀의 폰은 그녀의 인박스를 GET해서 얻어오고, 그녀의 친구들이 올린 무수한 고양이 동영상과 그녀의 자매가 올린 조카 사진들 가운데서 다음과 같은 내용을 발견합니다.

~~Cool! A while later, Alyssa checks what new messages she's gotten. Her phone polls her inbox via GET, and amongst a bunch of cat videos posted by friends and photos of her nephew posted by her sister, she sees the following:~~

```json
{"@context": "https://www.w3.org/ns/activitystreams",
 "type": "Create",
 "id": "https://chatty.example/ben/p/51086",
 "to": ["https://social.example/alyssa/"],
 "actor": "https://chatty.example/ben/",
 "object": {"type": "Note",
            "id": "https://chatty.example/ben/p/51085",
            "attributedTo": "https://chatty.example/ben/",
            "to": ["https://social.example/alyssa/"],
            "inReplyTo": "https://social.example/alyssa/posts/49e2d03d-b53a-4c4c-a95c-94a6abf45a19",
            "content": "<p>Argh, yeah, sorry, I'll get it back to you tomorrow.</p>
                        <p>I was reviewing the section on register machines,
                           since it's been a while since I wrote one.</p>"}}
```

알리사는 안심하고 벤의 포스트에 별을 박습니다:

~~Alyssa is relieved, and likes Ben's post:~~

```json
{"@context": "https://www.w3.org/ns/activitystreams",
 "type": "Like",
 "id": "https://social.example/alyssa/posts/5312e10e-5110-42e5-a09b-934882b3ecec",
 "to": ["https://chatty.example/ben/"],
 "actor": "https://social.example/alyssa/",
 "object": "https://chatty.example/ben/p/51086"}
```

그녀는 이 메세지를 그녀의 아웃박스에 POST합니다. (이것은 액티비티이므로, 그녀의 서버는 이걸 생성Create 객체로 랩핑할 필요가 없다는 걸 알고 있습니다) 만족스러워진 알리사는 그녀의 팔로워들에게 퍼블릭 메세지를 날리기로 결정합니다. 곧 아래와 같은 메세지가 그녀의 팔로워 컬렉션 멤버 전원에게 날아가고, 별도로 퍼블릭 그룹이라고 적혀있으므로, 일반적으로 누구에게나 읽혀질 수 있습니다.

~~She POSTs this message to her outbox. (Since it's an activity, her server knows it doesn't need to wrap it in         a Create object). Feeling happy about things, she decides to post a public message to her followers. Soon the following message is blasted to all the members of her followers collection, and since it has the special Public group addressed, is generally readable by anyone.~~

```json
{"@context": "https://www.w3.org/ns/activitystreams",
 "type": "Create",
 "id": "https://social.example/alyssa/posts/9282e9cc-14d0-42b3-a758-d6aeca6c876b",
 "to": ["https://social.example/alyssa/followers/",
        "https://www.w3.org/ns/activitystreams#Public"],
 "actor": "https://social.example/alyssa/",
 "object": {"type": "Note",
            "id": "https://social.example/alyssa/posts/d18c55d4-8a63-4181-9745-4e6cf7938fa1",
            "attributedTo": "https://social.example/alyssa/",
            "to": ["https://social.example/alyssa/followers/",
                   "https://www.w3.org/ns/activitystreams#Public"],
            "content": "Lending books to friends is nice.  Getting them back is even nicer! :)"}}
```

#### 1.1 소셜 웹 워킹 그룹 ~~Social Web Working Group~~

[액티비티펍](https://www.w3.org/TR/activitypub/#Overview)은 소셜 웹 워킹 그룹에서 만든 여러 규격 중 하나입니다. 다른 접근 방식이나 보충 프로토콜에 관심이 있는 도입자들은 [[마이크로펍](https://www.w3.org/TR/activitypub/#bib-Micropub)이나 [[소셜-웹-프로토콜](https://www.w3.org/TR/activitypub/#bib-Social-Web-Protocols) 문서를 검토하십시오.

~~[ActivityPub](https://www.w3.org/TR/activitypub/#Overview) is one of several related specifcations being produced by the Social Web Working Group. Implementers interested in alternative approaches and complementary protocols should review [[Micropub](https://www.w3.org/TR/activitypub/#bib-Micropub)] and the overview document [[Social-Web-Protocols](https://www.w3.org/TR/activitypub/#bib-Social-Web-Protocols)].~~



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

### 3. Objects

Objects are the core concept around which both [[ActivityStreams](https://www.w3.org/TR/activitypub/#bib-ActivityStreams)] and ActivityPub are built. Objects are often wrapped in Activities and are contained in streams of Collections, which are themselves subclasses of Objects. See the [[Activity-Vocabulary](https://www.w3.org/TR/activitypub/#bib-Activity-Vocabulary)] document, particularly the [Core Classes](https://www.w3.org/TR/activitystreams-vocabulary/#types); ActivityPub follows the mapping of this vocabulary very closely. 

ActivityPub defines some terms in addition to those provided by ActivityStreams. These terms are provided in the ActivityPub [JSON-LD context](http://www.w3.org/TR/json-ld/#the-context) at `https://www.w3.org/ns/activitystreams`.         Implementers *SHOULD* include the ActivityPub context in their object definitions. Implementers *MAY* include additional context as appropriate. 

ActivityPub shares the same [URI / IRI conventions as in ActivityStreams](https://www.w3.org/TR/activitystreams-core/#urls).

Servers *SHOULD* validate the content they receive to avoid content spoofing attacks. (A server should do something at least as robust as checking that the object appears as received at its origin, but mechanisms         such as checking signatures would be better if available). No particular mechanism for verification is authoritatively specified by this document, but please see [Security Considerations](https://www.w3.org/TR/activitypub/#security-considerations) for some suggestions and good practices.

As an example, if example.com receives the activity

```json
{
  "@context": "https://www.w3.org/ns/activitystreams",
  "type": "Like",
  "actor": "https://example.net/~mallory",
  "to": ["https://hatchat.example/sarah/",
         "https://example.com/peeps/john/"],
  "object": {
    "@context": {"@language": "en"},
    "id": "https://example.org/~alice/note/23",
    "type": "Note",
    "attributedTo": "https://example.org/~alice",
    "content": "I'm a goat"
  }
}
```

it should dereference the `id` both to ensure that it exists and is a valid object, and that it is not misrepresenting the object. (In this example, Mallory could be spoofing an object allegedly posted
by Alice).



#### 3.1 Object Identifiers

 All Objects in [ActivityStreams] should have unique global identifiers. ActivityPub extends this requirement; all objects distributed by the ActivityPub protocol MUST have unique global identifiers, unless they are intentionally transient (short lived activities that are not intended to be able to be looked up, such as some kinds of chat messages or game notifications). These identifiers must fall into one of the following groups:

1. Publicly dereferencable URIs, such as HTTPS URIs, with their authority belonging to that of their originating server. (Publicly facing content SHOULD use HTTPS URIs).
2. An ID explicitly specified as the JSON null object, which implies an anonymous object (a part of its parent context)

Identifiers MUST be provided for activities posted in server to server communication, unless the activity is intentionally transient. However, for client to server communication, a server receiving an object posted to the outbox with no specified id SHOULD allocate an object ID in the actor's namespace and attach it to the posted object.

All objects have the following properties:

**id**
    The object's unique global identifier (unless the object is transient, in which case the id MAY be omitted). 
**type**
    The type of the object. 

#### 3.2 Retrieving objects

The HTTP GET method may be dereferenced against an object's id property to retrieve the activity. Servers MAY use HTTP content negotiation as defined in [RFC7231](https://www.w3.org/TR/activitypub/#bib-RFC7231) to select the type of data to return in response to a request, but MUST present the ActivityStreams object representation in response to `application/ld+json; profile="https://www.w3.org/ns/activitystreams"`, and SHOULD also present the ActivityStreams representation in response to `application/activity+json` as well. The client MUST specify an Accept header with the `application/ld+json; profile="https://www.w3.org/ns/activitystreams"` media type in order to retrieve the activity.

Servers *MAY* implement other behavior for requests which do not comply with the above requirement. (For example, servers may implement additional legacy protocols, or may use the same URI for both HTML and ActivityStreams representations of a resource).

Servers MAY require authorization as specified in B.1 [Authentication and Authorization](https://www.w3.org/TR/activitypub/#authorization), and may additionally implement their own authorization rules. Servers SHOULD fail requests which do not pass their authorization checks with the appropriate HTTP error code, or the 403 Forbidden error code where the existence of the object is considered private. An origin server which does not wish to disclose the existence of a private target MAY instead respond with a status code of 404 Not Found. 

#### 3.3 The source property

 In addition to all the properties defined by the [Activity-Vocabulary](https://www.w3.org/TR/activitypub/#bib-Activity-Vocabulary), ActivityPub extends the `Object` by supplying the `source` property. The `source` property is intended to convey some sort of source from which the `content` markup was derived, as a form of provenance, or to support future editing by clients. In general, clients do the conversion from `source` to `content`, not the other way around.

The value of `source` is itself an object which uses its own `content` and `mediaType` fields to supply source information. 

```json
{
  "@context": ["https://www.w3.org/ns/activitystreams",
               {"@language": "en"}],
  "type": "Note",
  "id": "http://postparty.example/p/2415",
  "content": "<p>I <em>really</em> like strawberries!</p>",
  "source": {
    "content": "I *really* like strawberries!",
    "mediaType": "text/markdown"}
}
```

	Note: What to do when clients can't meaningfully handle a mediaType?
	
	In general, it's best to let a user edit their original post in the same source format they originally composed it in. But not all clients can reliably provide a nice interface for all source types, and since clients are expected to do the conversion from source to content, some clients may work with a media type that another client does not know how to work with. While a client could feasibly provide the content markup to be edited and ignore the source, this means that the user will lose the more desirable form of the original source in any future revisions. A client doing so should thus provide a minimally obtrusive warning cautioning that the original source format is not understood and is thus being ignored.
	
	For example, Alyssa P. Hacker likes to post to her ActivityPub powered blog via an Emacs client she has written, leveraging Org mode. Later she switches to editing on her phone's client, which has no idea what text/x-org is or how to render it to HTML, so it provides a text box to edit the original content instead. A helpful warning displays above the edit area saying, "This was originally written in another markup language we don't know how to handle. If you edit, you'll lose your original source!" Alyssa decides the small typo fix isn't worth losing her nice org-mode markup and decides to make the update when she gets home.
