[목차로 돌아가기](ActivityPubContents.md)

## 7. 서버-서버 간 상호작용 (Server to Server Interactions)

서버는 액터의 [인박스](https://www.w3.org/TR/activitypub/#inbox) 엔드포인트에 액티비티를 게시하여 다른 서버들과 통신하고 소셜 그래프에서 정보를 전파합니다. 네트워크를 통하여 전송된 액티비티는 일시적인 경우(이 경우 `id`를 생략 *할 수도* 있습니다)를 제외하면 `id`를 보유하고 있는 것이 *권장됩니다*.

~~Servers communicate with other servers and propagate information across the social graph by posting activities to actors' [inbox](https://www.w3.org/TR/activitypub/#inbox) endpoints. An Activity sent over the network *SHOULD* have an `id`, unless it is intended to be transient (in which case it *MAY* omit the `id`).~~

`POST` 요청 (예: 인박스로 가는 요청)은 *반드시* `application/ld+json; profile="https://www.w3.org/ns/activitystreams"` 컨텐츠 타입과 Accept 헤더가 `application/ld+json; profile="https://www.w3.org/ns/activitystreams"`인 `GET` 요청([3.2 객체 회수](https://www.w3.org/TR/activitypub/#retrieving-objects) 참고바람) 으로 이루어져 있어야 합니다. 서버는 서버-서버간 상호작용을 위하여 `application/activity+json as equivalent to application/ld+json; profile="https://www.w3.org/ns/activitystreams"` 컨텐츠 타입 또는 Accept 헤더를 해석하여야 합니다.

~~`POST` requests (eg. to the inbox) *MUST* be made with a Content-Type of `application/ld+json; profile="https://www.w3.org/ns/activitystreams"` and `GET` requests (see also [3.2 Retrieving objects](https://www.w3.org/TR/activitypub/#retrieving-objects)) with an Accept header of `application/ld+json; profile="https://www.w3.org/ns/activitystreams"`. Servers *SHOULD* interpret a Content-Type or Accept header of `application/activity+json as equivalent to application/ld+json; profile="https://www.w3.org/ns/activitystreams"` for server-to-server interactions.~~

소셜 그래프 전체에 업데이트를 전파하기 위해서는 액티비티들이 적절한 수신자들에게 전송됩니다. 우선, 이러한 수신자들은 액터에 도달 할 때까지 객체 간의 적절한 링크를 따라 결정된 후, 액티비티가 액터의 *인박스* ([전달](https://www.w3.org/TR/activitypub/#delivery))에 추가됩니다. 이를 통하여 수신자 서버들은 다음을 수행 할 수 있습니다:

~~In order to propagate updates throughout the social graph, Activities are sent to the appropriate recipients. First, these recipients are determined through following the appropriate links between objects until you reach an actor, and then the Activity is inserted into the actor's *inbox* ([delivery](https://www.w3.org/TR/activitypub/#delivery)). This allows recipient servers to:~~

* 액티비티와 관련된 부작용을 수행합니다 (예를 들어 액터가 객체를 좋아했다는(liked) 알림이 객체의 갯수를 업데이트 하는데 사용될 수 있습니다);
* 업데이트가 소셜 그래프 전체로 전파되도록 하기 위하여 원본 객체의 수신자에게 액티비티를 전달합니다 ([인박스 전달](https://www.w3.org/TR/activitypub/#inbox-forwarding) 참고바람).

~~conduct any side effects related to the Activity (for example, notification that an actor has liked an object is used to update the object's like count);~~

~~deliver the Activity to recipients of the original object, to ensure updates are propagated to the whole social graph (see [inbox delivery](https://www.w3.org/TR/activitypub/#inbox-forwarding)).~~

전달은 일반적으로 다음과 같은 경우에 발생합니다:

~~Delivery is usually triggered by, for example:~~

* 액터의 [아웃박스](https://www.w3.org/TR/activitypub/#outbox)에서 [팔로워 컬렉션](https://www.w3.org/TR/activitypub/#followers)을 수신자로 하여 생성된 액티비티.
* 직접 지정된 수신자들을 수신자와 함께 액터의 [아웃박스](https://www.w3.org/TR/activitypub/#outbox)에 생성되는 액티비티.
* 사용자가 직접 관리하는 컬렉션을 수신자로 하여 액터의 [아웃박스](https://www.w3.org/TR/activitypub/#outbox)에 생성되는 액티비티.
* 액터의 [아웃박스](https://www.w3.org/TR/activitypub/#outbox) 또는 다른 개체를 참조하는 [인박스](https://www.w3.org/TR/activitypub/#inbox)에 생성되는 액티비티.

~~an Activity being created in an actor's [outbox](https://www.w3.org/TR/activitypub/#outbox) with their [Followers Collection](https://www.w3.org/TR/activitypub/#followers) as the recipient.~~

~~an Activity being created in an actor's [outbox](https://www.w3.org/TR/activitypub/#outbox) with directly addressed recipients.~~

~~an Activity being created in an actors's [outbox](https://www.w3.org/TR/activitypub/#outbox) with user-curated collections as recipients.~~

~~an Activity being created in an actor's [outbox](https://www.w3.org/TR/activitypub/#outbox) or [inbox](https://www.w3.org/TR/activitypub/#inbox) which references another object.~~

다른 서버에 있는 액터의 `inbox` 또는 `sharedInbox` 속성으로 전달을 수행하는 서버는 *반드시* 다음과 같은 액티비티에게 `object` 속성을 제공하여야 합니다: `Create`, `Update`, `Delete`, `Follow`, `Add`, `Remove`, `Like`, `Block`, `Undo`. 또한, 다음 액티비티들의 서버-서버 간 전달을 수행하는 서버는 *반드시* 다음과 같은 대상 속성을 제공하여야 합니다: `Add`, `Remove`.

~~Servers performing delivery to the `inbox` or `sharedInbox` properties of actors on other servers *MUST* provide the `object` property in the activity: `Create`, `Update`, `Delete`, `Follow`, `Add`, `Remove`, `Like`, `Block`, `Undo`. Additionally, servers performing server to server delivery of the following activities *MUST* also provide the target property: `Add`, `Remove`.~~

HTTP 캐싱 메커니즘 [[RFC7234](https://www.w3.org/TR/activitypub/#bib-RFC7234)]은 다른 서버로부터 응답을 전달받을 뿐만 아니라 다른 서버로 응답을 보낼 때에도 준수되기를 *권장합니다*.

~~HTTP caching mechanisms [[RFC7234](https://www.w3.org/TR/activitypub/#bib-RFC7234)] *SHOULD* be respected when appropriate, both when receiving responses from other servers as well as sending responses to other servers.~~

### 7.1 전송(Delivery)

*이 항목은 다른 연합 서버와 통신하는 연합 서버에서만 요구됩니다*

~~*The following is required by federated servers communicating with other federated servers only.*~~

액티비티는 먼저 대상의 인박스들을 찾은 다음, 해당 인박스들에 액티비티를 게시하여 대상들([액터들](https://www.w3.org/TR/activitypub/#actors))에게 전달됩니다. 게재할 대상들은 [액티비티스트림 대상 타게팅](https://www.w3.org/TR/activitystreams-vocabulary/#audienceTargeting)을 확인하여 결정됩니다; 이는 액티비티의 `to`, `bto`, `cc`, `bcc`, 및 `audience` 필드에 해당합니다.

~~An activity is delivered to its targets (which are [actors](https://www.w3.org/TR/activitypub/#actors)) by first looking up the targets' inboxes and then posting the activity to those inboxes. Targets for delivery are determined by checking the [ActivityStreams audience targeting](https://www.w3.org/TR/activitystreams-vocabulary/#audienceTargeting); namely, the `to`, `bto`, `cc`, `bcc`, and `audience` fields of the activity.~~

[인박스](https://www.w3.org/TR/activitypub/#inbox)는 먼저 [대상 액터의 JSON-LD 표현](https://www.w3.org/TR/activitypub/#retrieving-objects)을 검색 한 후 `inbox` 속성을 조회하여 결정됩니다. 수신자가 `Collection` 또는 `OrderedCollection`일 경우, 서버는 *반드시* (사용자의 자격증명을 사용하여) 콜렉션을 역 참조하고 콜렉션의 각 항목에 대한 인박스를 검색하여야 합니다. 서버는 *반드시* 수행될 컬렉션들을 통하여 단 한개 *일수도 있는* 우회 접근의 레이어 갯수를 제한하여야 합니다.

~~The [inbox](https://www.w3.org/TR/activitypub/#inbox) is determined by first [retrieving the target actor's JSON-LD representation](https://www.w3.org/TR/activitypub/#retrieving-objects) and then looking up the `inbox` property. If a recipient is a `Collection` or `OrderedCollection`, then the server *MUST* dereference the collection (with the user's credentials) and discover inboxes for each item in the collection. Servers *MUST* limit the number of layers of indirections through collections which will be performed, which *MAY* be one.~~

서버는 *반드시* 최종 수신자 목록을 중복제거 하여야 합니다. 또한 서버는 *반드시* 통보되는 액티비티의 `actor`와 동일한 액터들을 목록에서 제외하여야 합니다. 즉, 액터는 자신의 활동을 자기 자신에게 전달하여서는 안됩니다.

~~Servers *MUST* de-duplicate the final recipient list. Servers *MUST* also exclude actors from the list which are the same as the `actor` of the Activity being notified about. That is, actors shouldn't have their own activities delivered to themselves.~~

>참고: 음소거 및 비공개 액티비티
>
>수신자가 지정되지 않은 경우 수행해야 할 작업은 정의되어 있지 않지만, 이 경우에 객체는 완전히 비공개로 유지시키고 접근 제어는 객체에 대한 접근을 제한하는 것이 권장됩니다. 만약 객체가 "공개(public)" 컬렉션으로 전송되었을 경우, 객체는 어떠한 액터에게도 전달되지는 않지만 액터의 보낼 편지함에서 공개적으로 볼 수가 있습니다.

~~Note: Silent and private activities~~

~~What to do when there are no recipients specified is not defined, however it's recommended that if no recipients are specified the object remains completely private and access controls restrict the access to object. If the object is just sent to the "public" collection the object is not delivered to any actors but is publicly viewable in the actor's outbox.~~

액티비티가 요청의 body로 사용되어 [인박스](https://www.w3.org/TR/activitypub/#inbox)에 (제출할 사용자의 인증과 함께) HTTP POST 요청이 전달됩니다. 이 액티비티는 수신자가 [인박스](https://www.w3.org/TR/activitypub/#inbox) OrderedCollection에서 `item`으로 추가가 됩니다. 비-연합 서버의 인박스로의 전송 시도는 `405 Method Not Allowed`로 응답하기를 *권장합니다*.

~~An HTTP POST request (with authorization of the submitting user) is then made to the [inbox](https://www.w3.org/TR/activitypub/#inbox), with the Activity as the body of the request. This Activity is added by the receiver as an `item` in the [inbox](https://www.w3.org/TR/activitypub/#inbox) OrderedCollection. Attempts to deliver to an inbox on a non-federated server *SHOULD* result in a `405 Method Not Allowed` response.~~

타사 서버로 전송을 하는 연합 서버의 경우, 전달을 비동기적으로 수행하기를 *권장하며*, 네트워크 오류로 수신자에게 전달이 실패할 경우 수신자에게 추가적으로 다시 전달을 시도하는것이 *권장됩니다*.

~~For federated servers performing delivery to a third party server, delivery *SHOULD* be performed asynchronously, and *SHOULD* additionally retry delivery to recipients if it fails due to network error.~~

**참고**: 동일한 발신지룰 가지고 있는 액터들 간에 분배되는 활동은 내부 메커니즘을 사용할 수 있으며 HTTP를 사용할 필요는 없습니다.

~~**Note**: Activities being distributed between actors on the same origin may use any internal mechanism, and are not required to use HTTP.~~

>참고: 연결된 데이터 알림들과의 관계
>
>액티비티펍 규칙을 이해하기 위하여 이 참고문서를 읽을 필요는 없지만, 액티비티펍의 타게팅 및 전달 메커니즘이 [연결된 데이터 알림들(Linked Data Notifications)](https://www.w3.org/TR/ldn/) 사양 및 두가지 사양이 상호 운용적으로 결합 될 수가 있는 점을 알아두시길 바랍니다. 특히 `inbox` 속성은 액티비티펍과 '연결된 데이터 알림' 모두 동일하며, 이 문서에 설명된 타게팅 및 전달 시스템은 '연결된 데이터 알림'에 의하여 지원됩니다. '연결된 데이터 알림'은 JSON-LD 암축된 액티비티스트림 문서 외에도 액티비티펍 구현에 필요하지 않은 많은 RDF 직렬화를 지원합니다. 그러나 '연결된 데이터 알림'의 구현과 더욱 더 광범위하게 호환되려고 하는 액티비티펍을 구현할 경우, 다른 RDF 표현 역시 지원하는 것이 좋을 수도 있습니다.

~~Note: Relationship to Linked Data Notifications~~

~~While it is not required reading to understand this specification, it is worth noting that ActivityPub's targeting and delivery mechanism overlaps with the [Linked Data Notifications](https://www.w3.org/TR/ldn/) specification, and the two specifications may interoperably combined. In particular, the `inbox` property is the same between ActivityPub and Linked Data Notifications, and the targeting and delivery systems described in this document are supported by Linked Data Notifications. In addition to JSON-LD compacted ActivityStreams documents, Linked Data Notifications also supports a number of RDF serializations which are not required for ActivityPub implementations. However, ActivityPub implementations which wish to be more broadly compatible with Linked Data Notifications implementations may wish to support other RDF representations.~~

#### 7.1.1 서버-서버 간 아웃박스 전송 요구사항 (Outbox Delivery Requirements for Server to Server)

([클라이언트-서버 간 상호작용](https://www.w3.org/TR/activitypub/#client-to-server-interactions) 및 [서버-서버 간 상호작용](https://www.w3.org/TR/activitypub/#server-to-server-interactions)을 둘 다 지원하는 서버의)[아웃박스](https://www.w3.org/TR/activitypub/#outbox)에서 객체를 수신 한 경우, 서버는 *반드시* 다음과 같은 대상을 지정하고 전송하여야 합니다:

~~When objects are received in the [outbox](https://www.w3.org/TR/activitypub/#outbox) (for servers which support both [Client to Server interactions](https://www.w3.org/TR/activitypub/#client-to-server-interactions) and [Server to Server Interactions](https://www.w3.org/TR/activitypub/#server-to-server-interactions)), the server *MUST* target and deliver to:~~

* 다음과 같은 필드의 값이 액터가 소유한 사람들 목록 또는 컬렉션들 일 경우: `to`, `bto`, `cc`, `bcc` 또는 `audience` 필드.

~~The `to`, `bto`, `cc`, `bcc` or `audience` fields if their values are individuals or Collections owned by the actor.~~

이 필드들은 액티비티를 아웃박스에 게시 한 [클라이언트가 적절하게 채운](https://www.w3.org/TR/activitypub/#client-addressing) 필드입니다.

~~These fields will have been [populated appropriately by the client](https://www.w3.org/TR/activitypub/#client-addressing) which posted the Activity to the outbox.~~

#### 7.1.2 인박스에서의 포워딩 (Forwarding from Inbox)

>참고 : 비어있는 응답 문제를 피하기 위한 포워딩
>
>다음 항목은 때때로 연합 네트워크에서 문제를 일으키는 "비어있는 응답"문제를 완화하기 위한 항목입니다. 이 문제는 예제를 통해 가장 잘 설명됩니다.
>
>Alyssa는 컨퍼런스에서 논문을 성공적으로 발표했다는 소식을 작성하고 친구 Ben을 포함한 팔로워 컬렉션으로 보냅니다. Ben은 Alyssa의 메시지에 답장을 보내며 그녀의 팔로워 컬렉션을 수신자로 추가합니다. 그러나 Ben은 Alyssa의 팔로워 컬렉션 구성원을 볼 수 없으므로 그의 서버는 해당 메시지를 그들의 인박스로 전달하지 않습니다. 다음 메커니즘이 없다면, Alyssa가 Ben에게 답장을 할 경우, 그녀의 팔로워들은 Ben이 상호작용하는 것을 본적이 없으나 Alyssa가 Ben에게 응답하는 것을 보게 될 것입니다. 이는 매우 혼란스러울 것입니다!

~~Note: Forwarding to avoid the ghost replies problem~~

~~The following section is to mitigate the "ghost replies" problem which occasionally causes problems on federated networks. This problem is best demonstrated with an example.~~

~~Alyssa makes a post about her having successfully presented a paper at a conference and sends it to her followers collection, which includes her friend Ben. Ben replies to Alyssa's message congratulating her and includes her followers collection on the recipients. However, Ben has no access to see the members of Alyssa's followers collection, so his server does not forward his messages to their inbox. Without the following mechanism, if Alyssa were then to reply to Ben, her followers would see Alyssa replying to Ben without having ever seen Ben interacting. This would be very confusing!~~

액티비티들이 [인박스](https://www.w3.org/TR/activitypub/#inbox)에 수신되었을 경우, 서버는 발신지가 전송을 하지 못하였던 수신자들에게 이를 다시 전달하여야 합니다. 이를 위해서는 서버는 다음과 같은 항목이 모두 참일 경우에 한하여 *반드시* `to`, `cc`, 및/또는 `audience` 값으로 수신자를 지정 후 전송하여야 합니다:

~~When Activities are received in the [inbox](https://www.w3.org/TR/activitypub/#inbox), the server needs to forward these to recipients that the origin was unable to deliver them to. To do this, the server *MUST* target and [deliver](https://www.w3.org/TR/activitypub/#delivery) to the values of `to`, `cc`, and/or `audience` if and only if all of the following are true:~~

* 서버에서 이 액티비티를 처음으로 보았을 경우.
* `to`, `cc`, 및/또는 `audience`값이 해당 서버가 보유중인 컬렉션의 값일 경우.
* `inReplyTo`, `object`, `target` 및/또는 `tag`의 값이 해당 서버가 보유중인 객체들의 값일 경우. 서버는 이 값들을 통하여 서버가 소유한 연결된 객체를 찾아야 하며, 재귀에 대한 최대 한계를 두는 것이 *권장됩니다* (예를 들어, 타래가 너무 깊어 수신자와 직접적인 관련이 없을 경우 수신자인 팔로워들이 더 이상 업데이트를 하지 않아도 상관없는 지점). 서버는 *반드시* 포워딩하는 원본 객체의 `to`, `cc`, 및/또는 `audience` 값들만 대상으로 하여야 하며, (주소들이 클라이언트에 의하여/통하여 의도적으로 수정된 경우) 연결된 객체들을 통하여 재귀적으로 처리하는 동안 새로운 주소를 가져오지 말아야 합니다.

~~This is the first time the server has seen this Activity.~~

~~The values of `to`, `cc`, and/or `audience` contain a Collection owned by the server.~~

~~The values of `inReplyTo`, `object`, `target` and/or `tag` are objects owned by the server. The server *SHOULD* recurse through these values to look for linked objects owned by the server, and *SHOULD* set a maximum limit for recursion (ie. the point at which the thread is so deep the recipients followers may not mind if they are no longer getting updates that don't directly involve the recipient). The server *MUST* only target the values of `to`, `cc`, and/or `audience` on the original object being forwarded, and not pick up any new addressees whilst recursing through the linked objects (in case these addressees were purposefully amended by or via the client).~~

서버는 (스팸 필터링 같은) 구현 별 규칙에 따라 전송할 대상을 필터링 할 수도 있습니다.

~~The server *MAY* filter its delivery targets according to implementation-specific rules (for example, spam filtering).~~

#### 7.1.3 공유 인박스 전송 (Shared Inbox Delivery)

많은 액터들을 호스팅하는 서버의 경우, 모든 팔로워들에게 전달하는 상황에서는 엄청난 수의 메세지들이 전송 될 수가 있습니다. 일부 서버는 "알려진 네트워크"에 공개적으로 게시된 메세지 목록을 표시하는것을 선호할 수 있습니다. 그러므로 액티비티펍은 이 두가지 사례를 해결하기 위한 선택적 메커니즘을 제공합니다.

~~For servers hosting many actors, delivery to all followers can result in an overwhelming number of messages sent. Some servers would also like to display a list of all messages posted publicly to the "known network". Thus ActivityPub provides an optional mechanism for serving these two use cases.~~

객체가 발신원인 액터의 팔로워들에게 전달될 때, 서버는 동일한 [공유인박스(sharedInbox)](https://www.w3.org/TR/activitypub/#sharedInbox)를 공유하는 모든 팔로워들을 식별하여, 이에 해당하지 않는 개별 수신자들을 제외한 사람들에게는 개별적으로 전달하는 대신 해당하는 공유인박스로 전달 *할 수도* 있습니다. 따라서 이 경우에는 원격/수신 서버는 대상을 결정하고 특정받은 인박스들로 배달하는데 참여합니다.

~~When an object is being delivered to the originating actor's followers, a server *MAY* reduce the number of receiving actors delivered to by identifying all followers which share the same [sharedInbox](https://www.w3.org/TR/activitypub/#sharedInbox) who would otherwise be individual recipients and instead deliver objects to said `sharedInbox`. Thus in this scenario, the remote/receiving server participates in determining targeting and performing delivery to specific inboxes.~~

또한 객체가 [공개(Public)](https://www.w3.org/TR/activitypub/#public-addressing) 컬렉션에 포함되었을 경우, 서버는 해당 객체를 네트워크의 알려진 모든 `sharedInbox` 엔드포인트에 배달합니다.

~~Additionally, if an object is addressed to the [Public](https://www.w3.org/TR/activitypub/#public-addressing) special collection, a server *MAY* deliver that object to all known `sharedInbox` endpoints on the network.~~

공개적으로 발표된 액티비티들을 `sharedInbox` 엔드포인트들로 보내는 발신지 서버는 `sharedInbox`가 없는 경우, `sharedInbox` 메커니즘을 통하여 액티비티를 수신받지 못하므로 *반드시* (`to`, `bto`, `cc`, `bcc`, 및 `audience`를 통하여) 액터 및 컬렉션에 전달하여야 합니다.

~~Origin servers sending publicly addressed activities to `sharedInbox` endpoints *MUST* still deliver to actors and collections otherwise addressed (through `to`, `bto`, `cc`, `bcc`, and `audience`) which do not have a `sharedInbox` and would not otherwise receive the activity through the `sharedInbox` mechanism.~~

### 7.2 생성 액티비티 (Create Activity)

`Create` 액티비티를 `inbox`에서 전달받는것은 놀랍게도 부작용이 매우 적습니다; 액티비티는 액터의 `inbox`에 나타나야 하며, 서버는 이 액티비티와 이의 딸려오는 객체의 표현을 로컬에 저장하려고 할 것입니다. 그러나 이는 `inbox`로 전달되는 액티비티들을 처리하는 과정에서 발생합니다.

~~Receiving a `Create` activity in an `inbox` has surprisingly few side effects; the activity should appear in the actor's `inbox` and it is likely that the server will want to locally store a representation of this activity and its accompanying object. However, this mostly happens in general with processing activities delivered to an `inbox` anyway.~~

### 7.3 갱신 액티비티 (Update Activity)

서버-서버 간 상호작용의 경우, `Update` 액티비티는 수신하는 서버가 동일한 `id`의 `object` 복사본을 `Update` 액티비티에 제공된 복사본으로 갱신하는 것을 *권장되는 것* 을 의미합니다. [클라이언트-서버간 업데이트 액티비티 처리](https://www.w3.org/TR/activitypub/#update-activity-outbox)와 다르게 이는 부분 업데이트가 아닌 객체를 완전히 대체합니다.

~~For server to server interactions, an `Update` activity means that the receiving server *SHOULD* update its copy of the `object` of the same `id` to the copy supplied in the `Update` activity. Unlike the [client to server handling of the Update activity](https://www.w3.org/TR/activitypub/#update-activity-outbox), this is not a partial update but a complete replacement of the object.~~

수신 서버는 *반드시* `Update`가 `object`를 수정할 권한이 있는지 확인하여야 합니다. 최소한 `Update`와 `object`의 출저는 동일해야 합니다.

~~The receiving server *MUST* take care to be sure that the `Update` is authorized to modify its `object`. At minimum, this may be done by ensuring that the `Update` and its `object` are of same origin.~~

### 7.4 삭제 액티비티 (Delete Activity)

이것을 수신할 경우의 부작용은 (`object`를 전달하는 액터 / 서버가 소유하고 있다고 가정할 시) 삭제 액티비티를 수신하는 서버는 동일한 `id`를 보유하고 있는 `object`의 표현을 제거하는 것을 *권장하며*, 해당 표현을 `Tombstone` 객체로 교체 *할 수도* 있습니다.

~~The side effect of receiving this is that (assuming the `object` is owned by the sending actor / server) the server receiving the delete activity *SHOULD* remove its representation of the `object` with the same `id`, and *MAY* replace that representation with a `Tombstone` object.~~

(액티비티가 발신지 서버에서 원격 서버로 전송 된 이후, 액티비티펍 프로토콜로는 개체 표시의 원격 삭제를 강요할 수 있는 방법이 없습니다).

~~(Note that after an activity has been transmitted from an origin server to a remote server, there is nothing in the ActivityPub protocol that can enforce remote deletion of an object's representation).~~

### 7.5 팔로우 액티비티 (Follow Activity)

**인박스** 에서 이것을 수신할 경우의 부작용은 서버가 팔로우를 `object`로 간주하여 `Accept` 또는 `Reject` 활동을 생성하여 팔로우의 `actor`에게 전달하는 것이 *권장되는 것* 입니다. `Accept`또는 `Reject`는 자동으로 생성 *될 수도* 있고 (사용자가 검토 한 후 약간의 지연이 있을 수가 있으므로) 사용자 입력의 결과 *일 수도* 있습니다. 서버는 `Follow`에 대한 응답으로 `Reject`를 보내지 *않을 수도* 있지만, 구현하시는 분들께서는 요청을 보내는 서버가 중도 상태에 있음을 알고 있어야 합니다. 예를 들어 서버는 사용자의 개인정보를 보호하기 위하여 `Reject`를 보내지 않을 수도 있습니다.

~~The side effect of receiving this in an **inbox** is that the server *SHOULD* generate either an `Accept` or `Reject` activity with the Follow as the `object` and deliver it to the `actor` of the Follow. The `Accept` or `Reject` *MAY* be generated automatically, or *MAY* be the result of user input (possibly after some delay in which the user reviews). Servers *MAY* choose to not explicitly send a `Reject` in response to a `Follow`, though implementors ought to be aware that the server sending the request could be left in an intermediate state. For example, a server might not send a `Reject` to protect a user's privacy.~~

이 `Follow`를 객체로 참조하는 `Accept`를 수신 한 경우, 서버는 객체를 객체 액터의 [팔로워 컬렉션](https://www.w3.org/TR/activitypub/#followers) 에 추가하는 것을 *권장합니다*. `Reject`의 경우, 서버는 액터를 객체 액터의 [팔로워 컬렉션](https://www.w3.org/TR/activitypub/#followers)에 추가하는 것을 *절대 하지 말아야* 합니다.

~~In the case of receiving an `Accept` referencing this `Follow` as the object, the server *SHOULD* add the `actor` to the object actor's [Followers Collection](https://www.w3.org/TR/activitypub/#followers). In the case of a `Reject`, the server *MUST NOT* add the actor to the object actor's [Followers Collection](https://www.w3.org/TR/activitypub/#followers).~~

>참고
>
>경우에 따라 성공적인 `Follow` 구독이 발생할 수 있지만 언젠가는 상당한 시간동안 팔로워에게 전송을 실패 할 수도 있습니다. 구현시 네트워크의 액터가 언제나 접근 가능한 상태를 유지할 수 있다는 보장이 없기 때문에 이에 따라 구현하여야 합니다. 예를 들어, 팔로워가 접근 할 수 없는 상태에서 6개월 동안 액터에게 전달하려고 하면 전달 서버가 구독자를 `followers` 목록에서 제거하는 것이 합리적입니다. 도달 할 수 없는 액터를 처리하기 위한 타임프레임 및 동작은 전달 서버의 재량에 달려 있습니다.

~~Note~~

~~Sometimes a successful `Follow` subscription may occur but at some future point delivery to the follower fails for an extended period of time. Implementations should be aware that there is no guarantee that actors on the network will remain reachable and should implement accordingly. For instance, if attempting to deliver to an actor for perhaps six months while the follower remains unreachable, it is reasonable that the delivering server remove the subscriber from the `followers` list. Timeframes and behavior for dealing with unreachable actors are left to the discretion of the delivering server.~~

### 7.6 수락 액티비티 (Accept Activity)

**인박스**에서 이것을 수신받는 경우의 부작용은 수신된 `object`의 유형에 따라 결정되며, 이 문서에서 설명하지 않은 유형(예: `Offer`)을 받아 들일 수도 있습니다.

~~The side effect of receiving this in an **inbox** is determined by the type of the `object` received, and it is possible to accept types not described in this document (for example, an `Offer`).~~

**인박스**에서 수신된 `Accept`의 `object`가 수신자가 이전에 보낸 `Follow` 액티비티인 경우, 서버는 주어진 `actor`를 수신자의 [팔로잉 컬렉션](https://www.w3.org/TR/activitypub/#following)에 추가하는 것을 *권장합니다*.

~~If the `object` of an `Accept` received to an **inbox** is a `Follow` activity previously sent by the receiver, the server *SHOULD* add the `actor` to the receiver's [Following Collection](https://www.w3.org/TR/activitypub/#following).~~

### 7.7 거절 액티비티 (Reject Activity)

**인박스**에서 이것을 수신받는 경우의 부작용은 수신된 `object`의 유형에 따라 결정되며, 이 문서에서 설명하지 설명하지 않은 유형(예: `Offer`)을 받아 들일 수도 있습니다.

~~The side effect of receiving this in an **inbox** is determined by the type of the `object` received, and it is possible to reject types not described in this document (for example, an `Offer`).~~

**인박스**에서 수신된 `Reject`의 `object`가 수신자가 이전에 보낸 `Follow` 액티비티인 경우, 이는 수신자가 `Follow` 요청을 승인하지 않았음을 의미합니다. 서버는 수신자의 [팔로잉 컬렉션](https://www.w3.org/TR/activitypub/#following)에 `actor`를 추가하는 것을 *절대 하지 말아야* 합니다.

~~If the `object` of a `Reject` received to an **inbox** is a `Follow` activity previously sent by the receiver, this means the recipient did not approve the `Follow` request. The server *MUST NOT* add the `actor` to the receiver's [Following Collection](https://www.w3.org/TR/activitypub/#following).~~

### 7.8 추가 엑티비티 (Add Activity)

**인박스**에 `Add` 액티비티를 수신하면 서버는 다음과 같은 경우를 제외하고 `target` 속성에 지정된 컬렉션에 `object`를 추가하는 것을 *권장합니다* :

~~Upon receipt of an `Add` activity into the **inbox**, the server *SHOULD* add the `object` to the collection specified in the `target` property, unless:~~

* `target`을 수신받는 서버가 소유하지 않을 경우 이를 업데이트 할 수 없습니다.
* 수신자의 판단에 따라 `object`를 `target` 컬렉션에 추가하지 않게 할 수도 있습니다.

~~the `target` is not owned by the receiving server, and thus they can't update it.~~

~~the `object` is not allowed to be added to the `target` collection for some other reason, at the receiver's discretion.~~

### 7.9 제거 액티비티 (Remove Activity)

**인박스**에 `Remove` 액티비티를 수신하면 서버는 다음과 같은 경우를 제외하고 `object`를 `target` 속성에 명시되어 있는 컬렉션에서 제거하는 것을 *권장합니다* :

~~Upon receipt of a `Remove` activity into the **inbox**, the server *SHOULD* remove the `object` from the collection specified in the `target` property, unless:~~

* `target`을 수신받는 서버가 소유하지 않을 경우 이를 업데이트 할 수 없습니다.
* 수신자의 판단에 따라 `object`를 `target` 컬렉션에서 제거하지 않게 할 수도 있습니다.

~~the `target` is not owned by the receiving server, and thus they can't update it.~~

~~the `object` is not allowed to be removed to the `target` collection for some other reason, at the receiver's discretion.~~

### 7.10 좋아요 액티비티 (Like Activity)

**인박스**에서 이것을 수신받는 부작용으로는 이 서버에 [`likes`](https://www.w3.org/TR/activitypub/#likes) 컬렉션이 존재한다면, 이 컬렉션에 수신된 액티비티를 추가하여 주어진 객체의 '좋아요' 수를 증가시키는 것을 *권장합니다*.

~~The side effect of receiving this in an **inbox** is that the server *SHOULD* increment the object's count of likes by adding the received activity to the [`likes`](https://www.w3.org/TR/activitypub/#likes) collection if this collection is present.~~

### 7.11 전파 액티비티 (공유) (Announce Activity (sharing))

**인박스**에서 `Announce` 액티비티를 수신하면, 이 서버에 [`shares`](https://www.w3.org/TR/activitypub/#shares) 컬렉션이 존재한다면, 이 컬렉션에 수신된 액티비티를 추가하여 객체의 '공유됨' 수를 증가시키는 것을 *권장합니다*.  

~~Upon receipt of an `Announce` activity in an **inbox**, a server *SHOULD* increment the object's count of shares by adding the received activity to the [`shares`](https://www.w3.org/TR/activitypub/#shares) collection if this collection is present.~~

>참고
>
>`Announce` 액티비티는 효과적으로 다른 소셜 네트워크에서 "공유", "리포스팅" 또는 "부스팅"으로 알려져 있습니다. 

~~Note~~

~~The `Announce` activity is effectively what is known as "sharing", "reposting", or "boosting" in other social networks.~~

### 7.12 실행취소 액티비티 (Undo Activity)

`Undo` 액티비티는 이전 액티비티들의 부작용을 실행취소 하는데 사용됩니다. [역 액티비티들과 "실행취소(Undo)"](https://www.w3.org/TR/activitystreams-vocabulary/#inverse)에 대한 액티비티스트림 문서를 참고하시길 바랍니다. `Undo` 액티비티의 범위와 제한은 [클라이언트-서버 간 상호작용의 컨텍스트에서의 실행취소 액티비티](https://www.w3.org/TR/activitypub/#undo-activity-outbox)와 동일하지만 이 경우에는 연합 컨텍스트에 적용됩니다.

~~The `Undo` activity is used to undo the side effects of previous activities. See the ActivityStreams documentation on [Inverse Activities and "Undo"](https://www.w3.org/TR/activitystreams-vocabulary/#inverse). The scope and restrictions of the `Undo` activity are the same as for [the Undo activity in the context of client to server interactions](https://www.w3.org/TR/activitypub/#undo-activity-outbox), but applied to a federated context.~~
