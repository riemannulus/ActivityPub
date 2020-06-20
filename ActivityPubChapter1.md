[목차로 돌아가기](ActivityPubContents.md)

## 1. 개론 (Overview)

액티비티펍은 2개의 레이어로 구성되어 있습니다:

~~ActivityPub provides two layers:~~

- **서버와 서버 간의 연합 프로토콜** (이것을 이용하여 웹사이트간의 탈 중앙화된 정보 공유가 가능해집니다.)
- **클라이언트와 서버 간의 프로토콜** (이것을 이용하여 실제 세상의 사용자, 봇, 그리고 다른 자동화된 프로세스들을 포함한 사용자들이 각각의 계정과 서버를 가지고 기계의 제한 없이 액티비티펍을 이용하여 대화할 수 있습니다.)

~~**A server to server federation protocol** (so decentralized websites can share information)~~

~~**A client to server protocol** (so users, including real-world users, bots, and other automated processes, can communicate with ActivityPub using their accounts on servers, from a phone or desktop or web application or whatever)~~

액티비티펍 구현체는 이것들 중 하나 혹은 전부를 구현해야 합니다. 그러나 이들 중 하나를 구현했다면, 나머지 하나를 구현하는 데에는 많은 노력이 들지 않습니다. 그리고 둘 다 구현하는데 들이는 노력에 비하면 매우 많은 혜택을 얻을수 있습니다! (당신의 웹사이트 중 일부가 탈 중앙화된 소셜 웹으로 동작하고, 다른 소셜 웹사이트에서 사용하는 클라이언트들을 당신의 웹사이트에서도 사용할 수 있습니다).

~~ActivityPub implementations can implement just one of these things or both of them. However, once you've implemented one, it isn't too many steps to implement the other, and there are a lot of benefits to both (making your website part of the decentralized social web, and being able to use clients and client libraries that work across a wide variety of social websites).~~

액티비티펍에서의 사용자는 서버에 있는 사용자의 계정을 통해 "[액터(Actor)](https://www.w3.org/TR/activitypub/#actors)"로 표시됩니다. 서로 다른 서버에 있는 사용자 계정은 각기 다른 액터에 해당합니다. 각각의 액터는 다음과 같은 것들을 가지고 있습니다:

~~In ActivityPub, a user is represented by "[actors](https://www.w3.org/TR/activitypub/#actors)" via the user's accounts on servers. User's accounts on different servers correspond to different actors. Every Actor has:~~

[//TODO]: # "1번문장 매끄럽게"

- `인박스(inbox)`: 액터가 세상으로부터 메세지를 받는 방법.
- `아웃박스(outbox)`: 액터가 세상으로 메세지를 보내는 방법.

~~**An** `inbox`: How they get messages from the world~~

~~**An** `outbox`: How they send messages to others~~

![인박스와 아웃박스를 가지고 있는 액터](./img/tutorial-1.png)

~~[Actor with inbox and outbox]()~~

[//Link]: # "https://www.w3.org/TR/activitypub/illustration/tutorial-1.png"

이들은 엔드포인트 이거나, 단지 액터 안의 액티비티스트림(ActivityStreams)으로 서술 된 리스트들에 불과합니다. (액티비티스트림에 대해서는 나중에 더 이야기하겠습니다).

~~These are endpoints, or really, just URLs which are listed in the ActivityPub actor's ActivityStreams description. (More on ActivityStreams later).~~

여기 우리의 친구 알리사(Alyssa)의 기록을 통해 예제를 살펴보도록 합시다:

~~Here's an example of the record of our friend Alyssa P. Hacker:~~

>예시 1
>
>```json
>{"@context": "https://www.w3.org/ns/activitystream",
> "type": "Person",
> "id": "https://social.example/alyssa/",
> "name": "알리사 P. 해커",
> "preferredUserName": "alyssa",
> "summary": "MIT 출신의 Lisp 애호가",
> "inbox": "https://social.example/alyssa/inbox/",
> "outbox": "https://social.example/alyssa/outbox/",
> "followers": "https://social.example/alyssa/followers/",
> "following": "https://social.example/alyssa/following/",
> "liked": "https://social.example/alyssa/liked/"
>}
>```

액티비티펍은 [[액티비티스트림](https://www.w3.org/TR/activitystreams-core/)]을 어휘로 사용합니다. 이것이 의미하는 바는 소셜 네트워크의 통용되는 컨텐츠의 흐름이나 행동들을 표현하는데 필요한 모든 일반적인 용어들을 액티비티스트림에서 제공받을수 있다는 뜻입니다. 이렇듯 액티비티스트림은 당신이 원하는 대부분의 것들을 지원하지만, 만약 원하는 것이 없더라도 액티비티스트림은 [[JSON-LD](https://www.w3.org/TR/json-ld/)]를 통해 내용을 확장할 수 있습니다. 이미 JSON-LD를 사용할줄 안다면 JSON-LD에서 사용하는 연결된 데이터 접근 방식을 사용할 수 있습니다. 딱히 그렇지 않더라도 JSON-LD는 JSON을 알고 있다면 이해하기 어렵지 않습니다.

~~ActivityPub uses [[ActivityStreams](https://www.w3.org/TR/activitystreams-core/)] for its vocabulary. This is pretty great because ActivityStreams includes all the common terms you need to represent all the activities and content flowing around a social network. It's likely that ActivityStreams already includes all the vocabulary you need, but even if it doesn't, ActivityStreams can be extended via [[JSON-LD](https://www.w3.org/TR/json-ld/)]. If you know what JSON-LD is, you can take advantage of the cool linked data approaches provided by JSON-LD. If you don't, don't worry, JSON-LD documents and ActivityStreams can be understood as plain old simple JSON. (If you're going to add extensions, that's the point at which JSON-LD really helps you out).~~

[//Comment]: # "의역!"

자, 이제 알리사는 그녀의 친구와 대화하고 싶어합니다. 그리고 그녀의 친구도 당연히 그렇겠죠! 다행이도 "인박스"와 "아웃박스"가 어떻게든 해줄수 있을것 같습니다. 그리고 둘 다 GET과 POST에 대해 다르게 동작합니다. 이것들이 어떻게 작동하는지 알아봅시다:

~~So, okay. Alyssa wants to talk to her friends, and her friends want to talk to her! Luckily these "inbox" and "outbox" things can help us out. They both behave differently for GET and POST. Let's see how that works:~~

![온 세상에서 인박스로 흘러들어가는 메세지들, 아웃박스에서 온 세상으로 흘러나가는 메세지들과 함께 있는 액터](./img/tutorial-2.png)

~~[Actor with messages flowing from rest of world to inbox and from outbox to rest of world]()~~

[//Link]: # "https://www.w3.org/TR/activitypub/illustration/tutorial-2.png"

흠, 그러니까 설명하자면...

~~Hey nice, so just as a recap:~~

- 당신은 메세지를 보내기 위해 수신자의 인박스에 POST를 할 수 있습니다. (서버에서 서버 / 서버에서 연합서버 한정)
- 당신은 당신의 인박스로부터 최근 메세지를 가져오기 위해 GET을 할 수 있습니다 (클라이언트에서 서버; 당신의 소셜 네트워크 스트림을 보는것 같이 말이죠)
- 당신은 누군가에게 메세지를 보내기 위해 당신의 아웃박스에 POST를 할 수 있습니다. (클라이언트에서 서버)
- 당신은 누군가가 게시한 글을 읽기 위해 (권한이 있다면) 그 사람의 아웃박스에 GET을 할 수 있습니다. (클라이언트에서 서버 및/또는 서버에서 서버)

~~You can POST to someone's inbox to send them a message (server-to-server / federation only... this is federation!)~~

~~You can GET from your inbox to read your latest messages (client-to-server; this is like reading your social network stream)~~

~~You can POST to your outbox to send messages to the world (client-to-server)~~

~~You can GET from someone's outbox to see what messages they've posted (or at least the ones you're authorized to see). (client-to-server and/or server-to-server)~~

[//Comment]: # "'social network stream' 번역에 관해서는 KIPRIS 특허정보넷에 '소셜 네트워크 서비스 스트림...'같은 사전 특허번역 사례가 있으므로 일단은 그대로 사용하였습니다."
[//Comment]: # "개론파트이므로 'server to server' 같은 문장은 '서버에서 서버(로)' 로 번역해두었습니다. 후반 챕터에서는 서버-서버 간 이라고 번역하였으므로 괴리가 있긴 합니다."


당연하지만, 누군가가 쓴 글을 읽기 위해서 그 사람의 아웃박스에 GET을 하는 방법이 유일한 방법이라면 이건 별로 효과적인 프로토콜이라고 부를 수 없겠죠! 실제로 연합에서는 종종 액터에서 다른 서버의 액터가 가지고 있는 인박스로 메세지를 보내는 일이 자주 있습니다.

~~Of course, if that last one (GET'ing from someone's outbox) was the only way to see what people have sent, this wouldn't be a very efficient federation protocol! Indeed, federation happens usually by servers posting messages sent by actors to actors on other servers' inboxes.~~

예제를 한번 들어보죠! 우리의 친구 알리사가 벤(Ben)에게 연락을 하고 싶어하네요. 알리사는 벤에게 빌려줬던 책을 돌려받고 싶어합니다. 그녀가 액티비티스트림 객체를 이용해 만든 메세지를 구성하고 있는 요소들을 살펴보도록 하죠:

~~Let's see an example! Let's say Alyssa wants to catch up with her friend, Ben Bitdiddle. She lent him a book recently and she wants to make sure he returns it to her. Here's the message she composes, as an ActivityStreams object:~~

[//Comment]: # "2번째 문장이 뭔가 말을 되풀이하는 느낌입니다"

>예시 2
>
>```json
>{"@context": "https://www.w3.org/ns/activitystreams",
> "type": "Note",
> "to": ["https://chatty.example/ben/"],
> "attributedTo": "https://social.example/alyssa/",
> "content": "혹시 내가 빌려 준 책 다 읽었니?"}
>```

이건 벤에게로 가야 할 노트입니다. 알리사는 이것을 그녀의 아웃박스로 POST 요청을 보냅니다.

~~This is a note addressed to Ben. She POSTs it to her outbox.~~

![아웃박스로 메세지를 전송하는 액터](./img/tutorial-3.png)

[//Link]: # "https://www.w3.org/TR/activitypub/illustration/tutorial-3.png"

~~[Actor posting message to outbox]()~~

이 노트는 비-액티비티(non-activity) 객체인 관계로, 서버는 이 노트를 새로이 생성된 객체로 판단하고, 이것을 생성 액티비티(Create activity)로 랩핑합니다. (액티비티펍에서 돌아다니는 액티비티들은 일반적으로 어떤 액터가 특정 객체에 취한 행동의 패턴을 따릅니다. 위 예시같은 경우, 액티비티는 어떤 사람에 의한 '노트 객체 생성' 입니다.)

~~Since this is a non-activity object, the server recognizes that this is an object being newly created, and does the courtesy of wrapping it in a Create activity. (Activities sent around in ActivityPub generally follow the pattern of some activity by some actor being taken on some object. In this case the activity is a Create of a Note object, posted by a Person).~~

>예시 3
>
>```json
>{"@context": "https://www.w3.org/ns/activitystreams",
> "type": "Create",
> "id": "https://social.example/alyssa/posts/a29a6843-9feb-4c74-a7f7-081b9c9201d3",
> "to": ["https://chatty.example/ben/"],
> "actor": "https://social.example/alyssa/",
> "object": {"type": "Note",
>            "id": "https://social.example/alyssa/posts/>49e2d03d-b53a-4c4c-a95c-94a6abf45a19",
>            "attributedTo": "https://social.example/alyssa/",
>            "to": ["https://chatty.example/ben/"],
>            "content": "혹시 내가 빌려 준 책 다 읽었니?"}}
>```

알리사의 서버는 벤의 액티비티스트림 액터 객체를 검색하고, 벤의 인박스 엔드포인트를 찾아서, 그녀의 객체를 벤의 인박스로 POST합니다.

~~Alyssa's server looks up Ben's ActivityStreams actor object, finds his inbox endpoint, and POSTs her object to his inbox.~~

![원격 액터의 인박스로 전송하는 서버](./img/tutorial-4.png)

[//Link]: # "https://www.w3.org/TR/activitypub/illustration/tutorial-4.png"

~~[Server posting to remote actor's inbox]~~

기술적으로 이러한 활동은 두 개의 단계로 나뉘는데... 하나는 클라이언트에서 서버로 가는 통신과, 하나는 서버 간 통신(연합)입니다. 그러나 이번 예제에서는 두 가지 모두를 사용하게 되므로, 여기서는 아웃박스에서 인박스로 나가는 간단한 통신이라고 간소화하여 생각하도록 하겠습니다:

~~Technically these are two separate steps... one is client to server communication, and one is server to server communication (federation). But, since we're using them both in this example, we can abstractly think of this as being a streamlined submission from outbox to inbox:~~

​![하나의 액터의 인박스에서 다른 액터의 인박스로 흘러 들어가는 노트](./img/tutorial-5.png)

[//Link]: # "https://www.w3.org/TR/activitypub/illustration/tutorial-5.png"

~~[Note flowing from one actor's outbox to other actor's inbox]~~

좋습니다! 잠시 뒤에 알리사는 그녀에게 무슨 메세지가 도착했는지를 확인합니다. 그녀의 폰은 그녀의 인박스를 GET해서 얻어오고, 그녀의 친구들이 올린 무수한 고양이 동영상들과 그녀의 자매가 올린 조카 사진들 가운데서 다음과 같은 내용을 발견합니다.

~~Cool! A while later, Alyssa checks what new messages she's gotten. Her phone polls her inbox via GET, and amongst a bunch of cat videos posted by friends and photos of her nephew posted by her sister, she sees the following:~~

>예시 4
>
>```json
>{"@context": "https://www.w3.org/ns/activitystreams",
> "type": "Create",
> "id": "https://chatty.example/ben/p/51086",
> "to": ["https://social.example/alyssa/"],
> "actor": "https://chatty.example/ben/",
> "object": {"type": "Note",
>            "id": "https://chatty.example/ben/p/51085",
>            "attributedTo": "https://chatty.example/ben/",
>            "to": ["https://social.example/alyssa/"],
>            "inReplyTo": "https://social.example/alyssa/posts/49e2d03d-b53a-4c4c-a95c-94a6abf45a19",
>            "content": "<p>아 이런, 미안, 내일 가져다 줄께.</p>
>                        <p>예전에 적은 레지스터 기기 부분을 검토중이였어.
>                           작성한지 한참 됐거든.</p>"}}
>```

알리사는 안심하고 벤의 포스트에 좋아요(likes) 를 누릅니다.

~~Alyssa is relieved, and likes Ben's post:~~

>예시 5
>
>```json
>{"@context": "https://www.w3.org/ns/activitystreams",
> "type": "Like",
> "id": "https://social.example/alyssa/posts/5312e10e-5110-42e5-a09b-934882b3ecec",
> "to": ["https://chatty.example/ben/"],
> "actor": "https://social.example/alyssa/",
> "object": "https://chatty.example/ben/p/51086"}
>```

그녀는 이 메세지를 그녀의 아웃박스에 POST합니다(이것은 액티비티이므로, 그녀의 서버는 이걸 생성 객체(Create object)로 랩핑할 필요가 없다는 걸 알고 있습니다).

~~She POSTs this message to her outbox. (Since it's an activity, her server knows it doesn't need to wrap it in a Create object).~~

만족스러워진 알리사는 그녀의 팔로워들에게 퍼블릭 메세지를 날리기로 결정합니다. 곧 아래와 같은 메세지가 그녀의 팔로워 멤버 전원에게 보내지는데, 해당 메세지에는 퍼블릭 그룹이라고 명시되어 있었으므로 일반적으로는 누구에게나 읽혀질 수 있습니다.

~~Feeling happy about things, she decides to post a public message to her followers. Soon the following message is blasted to all the members of her followers collection, and since it has the special Public group addressed, is generally readable by anyone.~~

[//Comment]: # "'Public'이 대문자 P로 시작하였기 때문에 따로 번역은 하지 않았습니다."

>예시 6
>
>```json
>{"@context": "https://www.w3.org/ns/activitystreams",
> "type": "Create",
> "id": "https://social.example/alyssa/posts/9282e9cc-14d0-42b3-a758-d6aeca6c876b",
> "to": ["https://social.example/alyssa/followers/",
>        "https://www.w3.org/ns/activitystreams#Public"],
> "actor": "https://social.example/alyssa/",
> "object": {"type": "Note",
>            "id": "https://social.example/alyssa/posts/d18c55d4-8a63-4181-9745-4e6cf7938fa1",
>            "attributedTo": "https://social.example/alyssa/",
>            "to": ["https://social.example/alyssa/followers/",
>                   "https://www.w3.org/ns/activitystreams#Public"],
>            "content": "친구에게 책을 빌려 주는것은 참 좋은것 같아. 되돌려 받는건 더더욱 좋고! :)"}}
>```

### 1.1 소셜 웹 워킹 그룹 (Social Web Working Group)

[액티비티펍](https://www.w3.org/TR/activitypub/#Overview)은 소셜 웹 워킹 그룹(Social Web Working Group)에서 만든 여러 규격 중 하나입니다. 다른 접근 방식이나 보충 프로토콜에 관심이 있는 도입하실 분들께서는 [[마이크로펍(Micropub)](https://www.w3.org/TR/activitypub/#bib-Micropub)]이나 [[소셜-웹-프로토콜(Social-Web-Protocols)](https://www.w3.org/TR/activitypub/#bib-Social-Web-Protocols)] 문서를 검토해 주시길 바랍니다.

~~[ActivityPub](https://www.w3.org/TR/activitypub/#Overview) is one of several related specifications being produced by the Social Web Working Group. Implementers interested in alternative approaches and complementary protocols should review [[Micropub](https://www.w3.org/TR/activitypub/#bib-Micropub)] and the overview document [[Social-Web-Protocols](https://www.w3.org/TR/activitypub/#bib-Social-Web-Protocols)].~~