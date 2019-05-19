## ActivityPub

액티비티펍(ActivityPub) 프로토콜은 [ActivityStreams 2.0](<https://www.w3.org/TR/activitypub/#bib-ActivityStreams>)의 데이터 포맷을 이용한 탈 중앙화된 소셜 네트워킹 프로토콜 입니다. 컨텐츠에 대한 CRUD를 지원하는 클라이언트-서버 API를 제공하며, ‘연합’으로 이루어진 서버-서버 간의 알림과 컨텐츠 전달을 위한 API도 제공합니다.

### 1. Overview

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
world to inbox and from outbox to rest of world](C:\Users\riema\Google Drive\ActivityPub\img\tutorial-2.png)

흠, 그러니까 설명하자면...

- 당신은 상대의 인박스에 메세지를 보내기 위해 POST 할 수 있습니다. (서버-서버 간)
- 당신은 자기 인박스로부터 최근 메세지를 가져오기 위해 GET 할 수 있습니다 (클라이언트-서버 간)
- 당신은 세상에게 메세지를 보내기 위해 아웃박스에 POST를 할 수 있습니다. (클라이언트-서버 간)
- 당신은 누군가가 쓴 글을 읽기 위해 아웃박스에 GET을 할 수 있습니다. (클라이언트-서버 / 서버-서버 간)

