[목차로 돌아가기](ActivityPubContents.md)

## 5. 컬렉션 (Collections)

[[액티비티스트림](https://www.w3.org/TR/activitypub/#bib-ActivityStreams)]은 컬렉션의 개념을 정의합니다; 액티비티펍은 특수한 동작을 하는 여러 컬렉션들을 정의합니다. 액티비티펍은 [액티비티스트림 페이징(ActivityStreams paging)](https://www.w3.org/TR/activitystreams-core/#paging)을 사용하여 큰 규모의 개체 집합들을 탐색합니다.

~~[[ActivityStreams](https://www.w3.org/TR/activitypub/#bib-ActivityStreams)] defines the collection concept; ActivityPub defines several collections with special behavior. Note that ActivityPub makes use of [ActivityStreams paging](https://www.w3.org/TR/activitystreams-core/#paging) to traverse large sets of objects.~~

이러한 컬렉션들의 일부는 [`OrderedCollection`](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-orderedcollection) 타입으로만 지정되어 있으며, 나머지는 [`Collection`](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-collection)이나 [`OrderedCollection`](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-orderedcollection) 타입으로 지정 될 수가 있습니다. [`OrderedCollection`](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-orderedcollection)은 *반드시* 역 시간순으로 일관되게 제공되어야 합니다.

~~Note that some of these collections are specified to be of type [`OrderedCollection`](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-orderedcollection) specifically, while others are permitted to be either a [`Collection`](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-collection) or an [`OrderedCollection`](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-orderedcollection). An [`OrderedCollection`](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-orderedcollection) *MUST* be presented consistently in reverse chronological order.~~


>참고
>
>역 시간순을 결정하는데 사용되는 속성은 표준에서 의도적으로 정의하지 않았고 구현에 달려있게 하였습니다. 예를 들어 여러 SQL같은 데이터베이스들은 식별자(identifier)로 자동으로 증가하는 정수(incrementing integer)를 사용하는데, 이는 대부분의 경우 삽입 순서를 처리하는데에는 합리적으로 사용될 수 있습니다. 다른 종류의 데이터베이스들의 경우, 삽입 시간 타임스탬프가 선호될 수도 있습니다. 무엇이 사용되는지는 중요하지 않지만, 개체들의 순서는 반드시 일관적이여야 하며, 새로운 개체는 언제나 가장 앞의 항목이 되어야 합니다. "마지막으로 업데이트 된" 같이 주기적으로 변경되는 속성은 역 시간순을 결정하는데 사용하기를 권장하지 않습니다.

~~Note~~

~~What property is used to determine the reverse chronological order is intentionally left as an implementation detail. For example, many SQL-style databases use an incrementing integer as an identifier, which can be reasonably used for handling insertion order in most cases. In other databases, an insertion time timestamp may be preferred. What is used isn't important, but the ordering of elements must remain intact, with newer items first. A property which changes regularly, such a "last updated" timestamp, should not be used.~~

### 5.1 아웃박스 (Outbox)

아웃박스는 [액터](https://www.w3.org/TR/activitypub/#actors) 의 프로필(profile) 안의 `outbox`속성을 통하여 볼 수 있습니다. `outbox`는 *반드시* [`OrderedCollection`](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-orderedcollection) 이여야 합니다.

~~The outbox is discovered through the `outbox` property of an [actor's](https://www.w3.org/TR/activitypub/#actors) profile. The `outbox` *MUST* be an [`OrderedCollection`](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-orderedcollection).~~

주어진 사용자의 액티비티를 검색 할 수 있는 요청자의 권한에 따라 사용자의 아웃박스 스트림에는 주어진 사용자가 게시한 액티비티가 들어가 있습니다. (이는 아웃박스의 내용물은 읽는 사람의 권한에 따라 필터링 됩니다). 만약 사용자가 [인증](https://www.w3.org/TR/activitypub/#authorization) 없이 요청을 제출할 경우에 서버는 모든 [공개](https://www.w3.org/TR/activitypub/#public-addressing) 게시물들로 응답하여야 합니다. 이 응답에 해당하는 게시물의 범위는 서버의 구현 및 배포하는 사람의 재량에 달려 있지만, 사용자가 게시한 것들과 연관된 모든 객체들 일수도 있습니다.

~~The outbox stream contains activities the user has published, subject to the ability of the requestor to retrieve the activity (that is, the contents of the outbox are filtered by the permissions of the person reading it). If a user submits a request without [Authorization](https://www.w3.org/TR/activitypub/#authorization) the server should respond with all of the [Public](https://www.w3.org/TR/activitypub/#public-addressing) posts. This could potentially be all relevant objects published by the user, though the number of available items is left to the discretion of those implementing and deploying the server.~~

아웃박스는 [클라이언트-서버 간 상호작용](https://www.w3.org/TR/activitypub/#client-to-server-interactions)에 기재되어 있는 동작방식을 따라 HTTP POST 요청을 수락합니다.

~~The outbox accepts HTTP POST requests, with behaviour described in [Client to Server Interactions](https://www.w3.org/TR/activitypub/#client-to-server-interactions).~~

### 5.2 인박스 (Inbox)

인박스는 [액터](https://www.w3.org/TR/activitypub/#actors) 의 프로필(profile) 안의 `inbox` 속성을 통하여 볼 수 있습니다. `inbox`는 *반드시* [`OrderedCollection`](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-orderedcollection) 이여야 합니다.

~~The inbox is discovered through the `inbox` property of an [actor's](https://www.w3.org/TR/activitypub/#actors) profile. The `inbox` *MUST* be an [`OrderedCollection`](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-orderedcollection).~~

인박스 스트림에는 액터가 받은 모든 액티비티들이 포함됩니다. 서버는 요청자의 권한에 따라 컨텐츠를 필터링 하는 것을 *권장합니다*. 일반적으로 인박스의 소유자는 인박스 안의 모든 내용에 대하여 접근 할 수 있습니다. 접근 제어 여부에 따라 다르지만 인박스에 접근이 가능할 경우, 일부 컨텐츠는 공개(public) 일 수도 있고, 소유자가 아닌 사용자들에게 인증을 요구 할 수도 있습니다.

~~The inbox stream contains all activities received by the actor. The server *SHOULD* filter content according to the requester's permission. In general, the owner of an inbox is likely to be able to access all of their inbox contents. Depending on access control, some other content may be public, whereas other content may require authentication for non-owner users, if they can access the inbox at all.~~

서버는 *반드시* 인박스에 의해 반환된 액티비티들의 중복 제거를 수행하여야 합니다. 액티비티의 중복은 액티비티가 액터의 팔로워들과 수신자 액터를 팔로잉하는 특정한 액터 모두에게 전파되어 있고, 서버가 수신자들의 목록을 중복 제거하지 못하였을 경우에 생깁니다. 이러한 중복 제거는 *반드시* 액티비티들의 `id`들을 비교하여 이미 본 액티비티들을 삭제하는 방식으로 수행하여야 합니다.

~~The server *MUST* perform de-duplication of activities returned by the inbox. Duplication can occur if an activity is addressed both to an actor's followers, and a specific actor who also follows the recipient actor, and the server has failed to de-duplicate the recipients list. Such deduplication *MUST* be performed by comparing the `id` of the activities and dropping any activities already seen.~~

[//Comment]: # "'addressed'를 '전파되었다'로 번역해두었습니다."

연합 서버에서 액터의 인박스들은 [전송(Delivery)](https://www.w3.org/TR/activitypub/#delivery)에 기재되어 있는 동작방식을 따라 HTTP POST 요청을 수락합니다. 비-연합 서버는 POST 요청을 수신하면 405 Method Not Allowed를 반환하여야 합니다.

~~The inboxes of actors on federated servers accepts HTTP POST requests, with behaviour described in [Delivery](https://www.w3.org/TR/activitypub/#delivery). Non-federated servers *SHOULD* return a 405 Method Not Allowed upon receipt of a POST request.~~

### 5.3 팔로워 컬렉션 (Followers Collection)

모든 액터들은 `followers` 컬렉션을 보유하고 있는 것을 *권장합니다*. 이 컬렉션은 액터에 대한 [팔로우(Follow)](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-follow) 액티비티를 보냈으며 [부작용](https://www.w3.org/TR/activitypub/#follow-activity-outbox)을 통하여 리스트에 추가된 사람들의 목록입니다. 이 컬렉션에서는 액터를 팔로잉 하고있는 모든 액터들의 목록을 찾을 수 있습니다. `followers` 컬렉션은 *반드시* [`OrderedCollection`](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-orderedcollection) 이거나 [`Collection`](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-collection)이여야 하고, 인증된 사용자의 권한에 따라 또는 인증이 제공되지 않은 경우 적절하게 필터링을 *할 수도* 있습니다.

~~Every [actor](https://www.w3.org/TR/activitypub/#actors) *SHOULD* have a `followers` collection. This is a list of everyone who has sent a [Follow](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-follow) activity for the actor, added as a [side effect](https://www.w3.org/TR/activitypub/#follow-activity-outbox). This is where one would find a list of all the actors that are following the actor. The `followers` collection *MUST* be either an [`OrderedCollection`](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-orderedcollection) or a [`Collection`](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-collection) and *MAY* be filtered on privileges of an authenticated user or as appropriate when no authentication is given.~~

>참고: 알림 타게팅의 기본값
>
>팔로우 액티비티는 기본적으로는 액터가 생성하는 객체들을 보려는 요청입니다. 이를 통하여 팔로워 컬렉션이 알림 [전송](https://www.w3.org/TR/activitypub/#delivery)에 적합한 기본적인 대상이 되게 합니다.

~~Note: Default for notification targeting~~

~~The follow activity generally is a request to see the objects an actor creates. This makes the Followers collection an appropriate default target for [delivery](https://www.w3.org/TR/activitypub/#delivery) of notifications.~~

### 5.4 팔로잉 (Following Collection)

모든 액터는 `following` 컬렉션을 보유하고 있는 것을 *권장합니다*. 이 컬렉션은 주어진 액터가 [부작용](https://www.w3.org/TR/activitypub/#follow-activity-outbox)을 통하여 추가한, 팔로잉 하는 모든 사람들의 리스트입니다. `following` 컬렉션은 *반드시* [`OrderedCollection`](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-orderedcollection) 이거나 [`Collection`](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-collection)이여야 하고, 인증된 사용자의 권한에 따라 또는 인증이 제공되지 않은 경우 적절하게 필터링을 *할 수도* 있습니다.

~~Every actor *SHOULD* have a `following` collection. This is a list of everybody that the actor has followed, added as a [side effect](https://www.w3.org/TR/activitypub/#follow-activity-outbox). The `following` collection *MUST* be either an [`OrderedCollection`](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-orderedcollection) or a [`Collection`](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-collection) and *MAY* be filtered on privileges of an authenticated user or as appropriate when no authentication is given.~~

### 5.5 좋아하는 컬렉션 (Liked Collection)

모든 액터는 `liked` 컬렉션을 가지고 *있을 수* 있습니다. 이 컬렉션은 주어진 액터가 [부작용](https://www.w3.org/TR/activitypub/#like-activity-outbox)을 통하여 추가한, 모든 `Like` 액티비티의 전체 객체들의 목록입니다. 이 `liked` 컬렉션은 *반드시* [`OrderedCollection`](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-orderedcollection) 이거나 [`Collection`](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-collection)이여야 하고, 인증된 사용자의 권한에 따라 또는 인증이 제공되지 않은 경우 적절하게 필터링을 *할 수도* 있습니다.

~~Every actor *MAY* have a `liked` collection. This is a list of every object from all of the actor's `Like` activities, added as a [side effect](https://www.w3.org/TR/activitypub/#like-activity-outbox). The `liked` collection *MUST* be either an [`OrderedCollection`](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-orderedcollection) or a [`Collection`](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-collection) and *MAY* be filtered on privileges of an authenticated user or as appropriate when no authentication is given.~~

[//Comment]: # "'모든 `Like` 액티비티 객체들의 전체 목록'? '모든 `Like` 액티비티의 전체 객체들의 목록'? 목록 전체?"

### 5.6 공개 전파 (Public Addressing)

[액티비티스트림](https://www.w3.org/TR/activitypub/#bib-ActivityStreams) 컬렉션과 객체 외에도 액티비티는 `https://www.w3.org/ns/activitystreams#Public` 식별자를 사용하면 특수한 "공개(public)" 컬렉션으로 추가 될 수 있습니다. 예를 들어:

~~In addition to [[ActivityStreams](https://www.w3.org/TR/activitypub/#bib-ActivityStreams)] collections and objects, Activities may additionally be addressed to the special "public" collection, with the identifier `https://www.w3.org/ns/activitystreams#Public`. For example:~~

>예시 10
>
>```json
>{
>  "@context": "https://www.w3.org/ns/activitystreams",
>  "id": "https://www.w3.org/ns/activitystreams#Public",
>  "type": "Collection"
>}
>```

이 특별한 URI로 전송 된 액티비티들은 모든 사용자가 인증없이 접근이 가능하여야 합니다. "공개(public)" 특수 컬렉션은 실제 액티비티들을 수용할 수 없기 때문에 구현체들은 이 컬렉션에게 전달을 *절대 해서는 안됩니다*. 하지만 액터들에게는 (팔로워-전용 게시물들을 포함한) 공개 게시물들을 효율적으로 공유할 수 있는 [`sharedInbox`](https://www.w3.org/TR/activitypub/#sharedInbox) 엔드포인트가 *있을 수도 있습니다*; [7.1.3 공유 인박스 전송](https://www.w3.org/TR/activitypub/#shared-inbox-delivery) 참고바람.

~~Activities addressed to this special URI shall be accessible to all users, without authentication. Implementations *MUST NOT* deliver to the "public" special collection; it is not capable of receiving actual activities. However, actors MAY have a [`sharedInbox`](https://www.w3.org/TR/activitypub/#sharedInbox) endpoint which is available for efficient shared delivery of public posts (as well as posts to followers-only); see [7.1.3 Shared Inbox Delivery](https://www.w3.org/TR/activitypub/#shared-inbox-delivery).~~

>참고
>
> 액티비티스트림 JSON-LD 컨텍스트를 사용하여 액티비티스트림 객체를 압축하면 `https://www.w3.org/ns/activitystreams#Public`이 단순히 `Public` 또는 `as:Public`으로 표시가 될 수도 있고, 이는 공개 컬렉션(Public collection)의 유효한 표기입니다. JSON-LD 툴링(tooling)을 사용하여 들어오는 액티비티들을 로컬 컨텍스트로 변환하는 방법 대신 액티비티스트림 객체를 단순한 JSON으로 처리하는 구현 방식은 위와 같은 표기들을 인지하였고, 위의 세 가지 표현을 모두 수용 할 수 있도록 준비되어야 합니다.

~~Note~~

~~Compacting an ActivityStreams object using the ActivityStreams JSON-LD context might result in `https://www.w3.org/ns/activitystreams#Public` being represented as simply `Public` or `as:Public` which are valid representations of the Public collection. Implementations which treat ActivityStreams objects as simply JSON rather than converting an incoming activity over to a local context using JSON-LD tooling should be aware of this and should be prepared to accept all three representations.~~

### 5.7 좋아요 컬렉션 (Likes Collection)

모든 객체들은 `likes` 컬렉션을 가지고 *있을 수도* 있습니다. 이 컬렉션은 이 객체를 `object` 속성으로 포함하는 모든 `Like` 액티비티들을 [부작용](https://www.w3.org/TR/activitypub/#like-activity-inbox)을 통하여 추가한 목록입니다. `likes` 컬렉션은 *반드시* [`OrderedCollection`](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-orderedcollection) 이거나 [`Collection`](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-collection)이여야 하고, 인증된 사용자의 권한에 따라 또는 인증이 제공되지 않은 경우 적절하게 필터링을 *할 수도* 있습니다. 

~~Every object *MAY* have a `likes` collection. This is a list of all `Like` activities with this object as the `object` property, added as a [side effect](https://www.w3.org/TR/activitypub/#like-activity-inbox). The `likes` collection *MUST* be either an [`OrderedCollection`](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-orderedcollection) or a [`Collection`](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-collection) and *MAY* be filtered on privileges of an authenticated user or as appropriate when no authentication is given.~~

>참고
>
>[`likes`](https://www.w3.org/TR/activitypub/#likes) 컬렉션과 [`liked`](https://www.w3.org/TR/activitypub/#liked)는 서로 비슷한 이름이지만 혼동되어서는 안됩니다. 요약해서 말하자면:
>
>* **좋아하는(liked)**
>액터의 속성만을 지칭합니다. 이 컬렉션은 주어진 액터가 실행한 *좋아요* 액티비티들을 [아웃박스로 전송할시의 부작용](https://www.w3.org/TR/activitypub/#like-activity-outbox)을 통하여 추가한 컬렉션입니다.
>* **좋아요(likes)**
>아무런 객체의 속성 일 수도 있습니다. 이 컬렉션은 이 객체를 참조하는 *좋아요* 액티비티들을 [인박스로 전송할시의 부작용](https://www.w3.org/TR/activitypub/#like-activity-inbox)을 통하여 추가한 컬렉션입니다.

~~Note~~

~~Care should be taken to not confuse the the [`likes`](https://www.w3.org/TR/activitypub/#likes) collection with the similarly named but different [`liked`](https://www.w3.org/TR/activitypub/#liked) collection. In sum:~~

~~* **liked**: Specifically a property of actors. This is a collection of *Like* activities performed by the actor, added to the collection as a [side effect of delivery to the outbox](https://www.w3.org/TR/activitypub/#like-activity-outbox).~~

~~* **likes**: May be a property of any object. This is a collection of *Like* activities referencing this object, added to the collection as a [side effect of delivery to the inbox](https://www.w3.org/TR/activitypub/#like-activity-inbox).~~

### 5.8 공유 컬렉션 (Shares Collection)

모든 객체는 `shares` 컬렉션을 보유하고 *있을 수도* 있습니다. 이 컬렉션은 이 `object`를 `object` 속성으로 포함하는 모든 `Announce` 액티비티들을  [부작용](https://www.w3.org/TR/activitypub/#announce-activity-inbox)을 통하여 추가한 목록입니다. 이 `shares` 컬렉션은 *반드시* [`OrderedCollection`](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-orderedcollection) 이거나 [`Collection`](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-collection)이여야 하고, 인증된 사용자의 권한에 따라 또는 인증이 제공되지 않은 경우 적절하게 필터링을 *할 수도* 있습니다.

~~Every object *MAY* have a `shares` collection. This is a list of all `Announce` activities with this `object` as the `object` property, added as a [side effect](https://www.w3.org/TR/activitypub/#announce-activity-inbox). The `shares` collection *MUST* be either an [`OrderedCollection`](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-orderedcollection) or a [`Collection`](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-collection) and *MAY* be filtered on privileges of an authenticated user or as appropriate when no authentication is given.~~
