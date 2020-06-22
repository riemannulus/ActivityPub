[목차로 돌아가기](ActivityPubContents.md)

## 6. 클라이언트-서버 간 상호작용 (Client to Server Interactions)

[액티비티스트림](https://www.w3.org/TR/activitypub/#bib-ActivityStreams)에 정의되어 있듯이, 액티비티들은 소셜 그래프 내부에서 컨텐츠의 생성, 수정 및 공유하는 작업들의 핵심 메커니즘입니다.

~~Activities as defined by [[ActivityStreams](https://www.w3.org/TR/activitypub/#bib-ActivityStreams)] are the core mechanism for creating, modifying and sharing content within the social graph.~~

클라이언트-서버 간 상호작용은 액티비티를 액터의 [아웃박스](https://www.w3.org/TR/activitypub/#outbox)로 전송할 때 발생합니다. 이를 위하여 클라이언트는 *반드시* [프로필](https://www.w3.org/TR/activitypub/#actor-objects)에서 주어진 액터의 아웃박스의 URL을 찾아야 하고, *반드시* `application/ld+json; profile="https://www.w3.org/ns/activitystreams"` 컨텐츠-타입을 사용하여 이 URL에 HTTP `POST` 요청을 하여야 합니다. 서버의 경우에는 컨텐츠-타입이나 `application/activity+json`의 헤더를 `application/ld+json; profile="https://www.w3.org/ns/activitystreams"`와 동등하게 해석 *할 수도* 있습니다. 요청은 *반드시* 주어진 아웃박스를 가지고 있는 사용자의 자격 정보로 인증되어야 합니다. `POST`요청의 body는 *반드시* (임베딩 된 객체들을 포함 *할 수도* 있는) 하나의 액티비티나 [서버에서 생성 액티비티로 래핑 될](https://www.w3.org/TR/activitypub/#object-without-create) 하나의 비-액티비티(non-Activity) 객체를 포함하고 있어야 합니다.

~~Client to server interaction takes place through clients posting Activities to an actor's [outbox](https://www.w3.org/TR/activitypub/#outbox). To do this, clients *MUST* discover the URL of the actor's outbox from their [profile](https://www.w3.org/TR/activitypub/#actor-objects) and then *MUST* make an HTTP `POST` request to this URL with the Content-Type of `application/ld+json; profile="https://www.w3.org/ns/activitystreams"`. Servers *MAY* interpret a Content-Type or Accept header of `application/activity+json` as equivalent to `application/ld+json; profile="https://www.w3.org/ns/activitystreams"` for client-to-server interactions. The request *MUST* be authenticated with the credentials of the user to whom the outbox belongs. The body of the `POST` request *MUST* contain a single Activity (which *MAY* contain embedded objects), or a single non-Activity object which [will be wrapped in a Create activity by the server](https://www.w3.org/TR/activitypub/#object-without-create).~~

>예시 11: 액티비티를 아웃박스로 전송할 경우
>
>POST /outbox/ HTTP/1.1
>
>Host: dustycloud.org
>
>Authorization: Bearer XXXXXXXXXXX
>
>Content-Type: application/ld+json; profile="https://www.w3.org/ns/activitystreams"
>
>```json
>{
>  "@context": ["https://www.w3.org/ns/activitystreams",
>               {"@language": "en"}],
>  "type": "Like",
>  "actor": "https://dustycloud.org/chris/",
>  "name": "Chris liked 'Minimal ActivityPub update client'",
>  "object": "https://rhiaro.co.uk/2016/05/minimal-activitypub",
>  "to": ["https://rhiaro.co.uk/#amy",
>         "https://dustycloud.org/followers",
>         "https://rhiaro.co.uk/followers/"],
>  "cc": "https://e14n.com/evan"
>}
>```

~~Example 11: Submitting an Activity to the Outbox~~

[//]: # "@language가 en으로 설정되어 있으므로 따로 번역을 하지 않겠습니다."

만약 액티비티가 `id` 속성이 가지고 있는 값으로 전달이 되었다면, 서버는 이를 *반드시* 무시하고 이 액티비티에 대하여 새로운 `id`를 생성하여야 합니다. 서버는 *반드시* `201 Created` HTTP 코드를 반환하여야 하며, 액티비티가 일시적이지 않을 경우 *반드시* `Location` 헤더에 생성된 새로운 `id`를 포함시켜야 합니다.

~~If an Activity is submitted with a value in the `id` property, servers *MUST* ignore this and generate a new `id` for the Activity. Servers *MUST* return a `201 Created` HTTP code, and unless the activity is transient, *MUST* include the new `id` in the `Location` header.~~

>예시 12: 전달된 액티비티에 대한 아웃박스의 응답
>
>HTTP/1.1 201 Created
>
>Location: https://dustycloud.org/likes/345

~~Example 12: Outbox response to submitted Activity~~

주어진 서버는 액티비티스트림 객체가 발송하기 전에 `bto` 및/또는 `bcc` 속성이 존재할 경우 이들을 *반드시* 제거하여야 하지만, 제거 전에 *반드시* `bto` / `bcc` 속성에 원래 저장된 주소를 사용하여 [배송](https://www.w3.org/TR/activitypub/#delivery) 할 수신자들을 결정하여야 합니다.

~~The server *MUST* remove the `bto` and/or `bcc` properties, if they exist, from the ActivityStreams object before delivery, but *MUST* utilize the addressing originally stored on the `bto` / `bcc` properties for determining recipients in [delivery](https://www.w3.org/TR/activitypub/#delivery).~~

[//]: # "'..., but *MUST* ...' 를 '제거 전에 *반드시* ...'로 번역한 것이 올바른지 차후에 확인해보겠습니다.(//TODO)"

위의 작업이 끝난 후, 서버는 *반드시* 이 새 액티비티를 [아웃박스](https://www.w3.org/TR/activitypub/#outbox) 컬렉션에 추가하여야 합니다. 액티비티의 종류에 따라 서버는 추가적인 부작용들을 수행하여야 할 수 있습니다. (하지만 액티비티가 아웃박스에 특정한 시간에 존재 한다는것을 보장 할 수 없습니다. 액티비티는 지연 후 나타날 수도 있고, 언제나 사라 질 수도 있습니다). 이에 대해서는 아래에 개별 액티비티별로 설명되어 있습니다.

~~The server *MUST* then add this new Activity to the [outbox](https://www.w3.org/TR/activitypub/#outbox) collection. Depending on the type of Activity, servers may then be required to carry out further side effects. (However, there is no guarantee that time the Activity may appear in the outbox. The Activity might appear after a delay or disappear at any period). These are described per individual Activity below.~~

클라이언트-서버 간 지원을 지원하지 않는 서버에 객체를 전송 시도하는 경우에 대해서는 `405 Method Not Allowed`로 응답하는 것이 *권장됩니다*.

~~Attempts to submit objects to servers not implementing client to server support *SHOULD* result in a `405 Method Not Allowed` response.~~

서버로부터 응답을 받는 클라이언트와 클라이언트로 응답을 보내는 서버 모두에서 HTTP 캐싱 메커니즘[[RFC7234](https://www.w3.org/TR/activitypub/#bib-RFC7234)]을 적절하게 준수하기를 *권장합니다*.

~~HTTP caching mechanisms [[RFC7234](https://www.w3.org/TR/activitypub/#bib-RFC7234)] *SHOULD* be respected when appropriate, both in clients receiving responses from servers as well as servers sending responses to clients.~~

### 6.1 클라이언트 처리 (Client Addressing)

[//]: # "Addressing을 처리로 번역하였습니다."

**클라이언트**들은 새로운 액티비티들을 적절히 처리 할 책임이 있습니다. 어느 정도까지는 각 클라이언트들의 구현마다 따라 처리하는 방식이 다르겠지만, 클라이언트는 서버가 `to`, `bto`, `cc`, `bcc`, 와 `audience` 에 해당하는 주소들에게만 새로운 액티비티들을 전송한다는 것을 반드시 알고 있어야 합니다.

~~**Clients** are responsible for addressing new Activities appropriately. To some extent, this is dependent upon the particular client implementation, but clients must be aware that the server will only forward new Activities to addressees in the `to`, `bto`, `cc`, `bcc`, and `audience` fields.~~

새로운 액티비티들을 발송하는데 쓰이는 기본 주소로는 [팔로워 컬렉션](https://www.w3.org/TR/activitypub/#followers) 및/또는 [공개 컬렉션](https://www.w3.org/TR/activitypub/#public-addressing)가 좋은 선택입니다.

~~The [Followers Collection](https://www.w3.org/TR/activitypub/#followers) and/or the [Public Collection](https://www.w3.org/TR/activitypub/#public-addressing) are good choices for the default addressing of new Activities.~~

[//Comment]: # "번역이 매끄럽지 않은 것 같습니다."

클라이언트는 `object`, `target`, `inReplyTo` 및/또는 `tag` 필드를 통하여 새로운 액티비티에 첨부되어 있는 객체를 살펴본 후 그들의 `actor` 또는 `attributedTo` 속성을 가져오고, 발송 속성들을 가져와 이들의 `to` 또는 `cc` 필드에 추가하여야 합니다.
클라이언트는 첨부된 객체들을 통하여 재귀적으로 처리 *할 수도* 있지만, 그렇게 하는 경우에는 재귀에 대한 최대 한계를 두는 것이 *권장됩니다*.
(이로 인해 클라이언트가 개별 수신자들로 구성된 액터 컬렉션을 "언팩" 해야 한다는 것은 아닙니다).

~~Clients *SHOULD* look at any objects attached to the new Activity via the `object`, `target`, `inReplyTo` and/or `tag` fields, retrieve their `actor` or `attributedTo` properties, and *MAY* also retrieve their addressing properties, and add these to the `to` or `cc` fields of the new Activity being created. Clients *MAY* recurse through attached objects, but if doing so, *SHOULD* set a limit for this recursion. (Note that this does not suggest that the client should "unpack" collections of actors being addressed as individual recipients).~~

[//Comment]: # "번역이 매끄럽지 않은 것 같습니다."

클라이언트는 사용자가 UI에서 이 주소를 수정할수 있는 기회를 제공 *할 수도* 있습니다.

~~Clients *MAY* give the user the chance to amend this addressing in the UI.~~

예를 들어 크리스가 에이미가 작성한 다음 기고문을 좋아할(likes) 경우:

~~For example, when Chris likes the following article by Amy:~~

>예시 13: 기고문
>
>```json
>{
>  "@context": ["https://www.w3.org/ns/activitystreams",
>               {"@language": "en-GB"}],
>  "id": "https://rhiaro.co.uk/2016/05/minimal-activitypub",
>  "type": "Article",
>  "name": "Minimal ActivityPub update client",
>  "content": "Today I finished morph, a client for posting ActivityStreams2...",
>  "attributedTo": "https://rhiaro.co.uk/#amy",
>  "to": "https://rhiaro.co.uk/followers/",
>  "cc": "https://e14n.com/evan"
>}
>```

~~Example 13: An Article~~

[//]: # "@language가 en-GB로 설정되어 있으므로 따로 번역을 하지 않겠습니다."

그리고 클라이언트에 의하여 다음과 같이 좋아요(like)가 생성됩니다.

~~the like is generated by the client as:~~

>예시 14: 기고문에 대한 좋아요(Like)
>
>```json
>{
>  "@context": ["https://www.w3.org/ns/activitystreams",
>               {"@language": "en"}],
>  "type": "Like",
>  "actor": "https://dustycloud.org/chris/",
>  "summary": "Chris liked 'Minimal ActivityPub update client'",
>  "object": "https://rhiaro.co.uk/2016/05/minimal-activitypub",
>  "to": ["https://rhiaro.co.uk/#amy",
>         "https://dustycloud.org/followers",
>         "https://rhiaro.co.uk/followers/"],
>  "cc": "https://e14n.com/evan"
>}
>```

~~Example 14: A Like of the Article~~

[//]: # "@language가 en으로 설정되어 있으므로 따로 번역을 하지 않겠습니다."

그러면 Chris(좋아요(like)를 누른 사람)의 팔로워들 뿐만 아니라 기고문 원문을 전달받은 Amy와 Amy의 팔로워들 및 Evan에게도 아웃박스는 [전달(delivery)](https://www.w3.org/TR/activitypub/#delivery)을 수행할수 있습니다.

~~The receiving outbox can then perform [delivery](https://www.w3.org/TR/activitypub/#delivery) to not only the followers of Chris (the liker), but also to Amy, and Amy's followers and Evan, both of whom received the original article.~~

`outbox`에 다음과 같은 액티비티들을 제출하는 클라이언트는 *반드시* 액티비티 안의 `object` 속성을 제공하여야 합니다: `Create`, `Update`, `Delete`, `Follow`, `Add`, `Remove`, `Like`, `Block`, `Undo`. 또한 다음과 같은 액티비티들을 아웃박스에 제출하는 클라이언트는 *반드시* `target` 속성을 제공하여야 합니다: `Add`, `Remove`.

~~Clients submitting the following activities to an `outbox` *MUST* provide the `object` property in the activity: `Create`, `Update`, `Delete`, `Follow`, `Add`, `Remove`, `Like`, `Block`, `Undo`. Additionally, clients submitting the following activities to an outbox *MUST* also provide the `target` property: `Add`, `Remove`.~~

### 6.2 생성 액티비티 (Create Activity)

`Create` 액티비티는 새로운 객체를 게시 할 때 사용됩니다. 이는 (`object` 속성 안의) 주어진 액티비티가 포함할 객체가 생성되는 부작용이 있습니다.

~~The `Create` activity is used when posting a new object. This has the side effect that the object embedded within the Activity (in the `object` property) is created.~~

`Create` 액티비티가 게시되었을 때, 액티비티의 `actor` 는 `object`의 `attributedTo` 필드로 복사되는 것이 *권장됩니다*.

~~When a `Create` activity is posted, the `actor` of the activity *SHOULD* be copied onto the `object`'s `attributedTo` field.~~

생성(Create) 액티비티의 주소 지정과 그 액티비티의 `object`을 비교할 경우 혼동의 여지가 있습니다. 따라서 서버는 처음으로 배포할 때 생성 액티비티의 수신자들을 그것의 `object`로 복사하는 것이 *권장되며*, 이는 마찬가지로 객체의 수신자들의 목록을 생성 액티비티를 래핑할 떄에도 해당됩니다. 주의할 점으로는 (`Update` 액티비티의 예시처럼) `Create`의 주소 지정 변경이 없어도 `object`의 주소 지정은 변경될 가능성이 있습니다.

~~A mismatch between addressing of the Create activity and its `object` is likely to lead to confusion. As such, a server *SHOULD* copy any recipients of the Create activity to its `object` upon initial distribution, and likewise with copying recipients from the object to the wrapping Create activity. Note that it is acceptable for the `object`'s addressing to be changed later without changing the `Create`'s addressing (for example via an `Update` activity).~~

[//Comment]: # "문장 번역이 매끄럽지 않은 것 같습니다."

#### 6.2.1 생성 액티비티 없이 객체 생성 (Object creation without a Create Activity)

클라이언트에서 서버로 게시할 경우, 연관된 액티비티가 없는 객체를 생성하기 위하여 서버로 제출하는 것이 가능합니다. 서버는 *반드시* 아웃박스로에 대한 POST 요청에서 `Activity`의 서브타입이 아닌, 유효한 [[액티비티스트림](https://www.w3.org/TR/activitypub/#bib-ActivityStreams)] 객체를 수용하여야 합니다. 서버는 *반드시* 이 `object`를 [생성 액티비티](https://www.w3.org/TR/activitypub/#create-activity-outbox)의 객체로 첨부하여야 합니다. 일시적이지 않은 객체들에 대해서는, 서버는 *반드시* `id`를 래핑중인 `Create`와 래핑된 `Object` 양쪽 모두에게 첨부하여야 합니다.

~~For client to server posting, it is possible to submit an object for creation without a surrounding activity. The server *MUST* accept a valid [[ActivityStreams](https://www.w3.org/TR/activitypub/#bib-ActivityStreams)] object that isn't a subtype of `Activity` in the POST request to the outbox. The server then *MUST* attach this `object` as the object of a [Create Activity](https://www.w3.org/TR/activitypub/#create-activity-outbox). For non-transient objects, the server *MUST* attach an `id` to both the wrapping `Create` and its wrapped `Object`.~~

[//Comment]: # "문장 번역이 매끄럽지 않은 것 같습니다."

>참고
>
>서버가 반환한 `Location` 값은 (객체보다는) 새로운 생성 액티비티의 URL이여야 합니다.

~~Note~~

~~The `Location` value returned by the server should be the URL of the new Create activity (rather than the object).~~

객체에 지정된 `to`, `bto`, `cc`, `bcc`, 와 `audience` 속성은 *반드시* 서버에 의해 새로운 생성 액티비티에게 복사되어야 합니다.

~~Any `to`, `bto`, `cc`, `bcc`, and `audience` properties specified on the object *MUST* be copied over to the new Create activity by the server.~~

>예시 15: 대상 타게팅이 있는 객체
>
>```json
>{
>  "@context": "https://www.w3.org/ns/activitystreams",
>  "type": "Note",
>  "content": "이것은 노트입니다",
>  "published": "2015-02-10T15:04:55Z",
>  "to": ["https://example.org/~john/"],
>  "cc": ["https://example.com/~erik/followers",
>         "https://www.w3.org/ns/activitystreams#Public"]
>}
>```

~~Example 15: Object with audience targeting~~
~~"content": "This is a note",~~

위의 예시는 다음과 같이 변환될 수 있습니다:

~~The above example could be converted to this:~~

>예시 16: 서버가 만들은 생성 액티비티 래퍼
>
>```json
>{
>  "@context": "https://www.w3.org/ns/activitystreams",
>  "type": "Create",
>  "id": "https://example.net/~mallory/87374",
>  "actor": "https://example.net/~mallory",
>  "object": {
>    "id": "https://example.com/~mallory/note/72",
>    "type": "Note",
>    "attributedTo": "https://example.net/~mallory",
>    "content": "이것은 노트입니다",
>    "published": "2015-02-10T15:04:55Z",
>    "to": ["https://example.org/~john/"],
>    "cc": ["https://example.com/~erik/followers",
>           "https://www.w3.org/ns/activitystreams#Public"]
>  },
>  "published": "2015-02-10T15:04:55Z",
>  "to": ["https://example.org/~john/"],
>  "cc": ["https://example.com/~erik/followers",
>         "https://www.w3.org/ns/activitystreams#Public"]
>}
>```

~~Example 16: Create Activity wrapper generated by the server~~

~~"content": "This is a note",~~

### 6.3 갱신 액티비티 (Update Activity)

`Update` 액티비티는 현존하는 객체를 업데이트 할 경우에 사용됩니다. 주어진 액터가 이 객체를 수정할 수 있는 권한이 있다는 전제 하에, 이 액티비티의 부작용은 갱신 액티비티에 정의 된 새로운 구조를 반영하도록 `object`를 *반드시* 수정하여야 한다는 것입니다.

~~The `Update` activity is used when updating an already existing object. The side effect of this is that the `object` *MUST* be modified to reflect the new structure as defined in the update activity, assuming the actor has permission to update this object.~~

#### 6.3.1 부분 갱신 (Partial Updates)

클라이언트-서버 간 상호작용을 위하여 업데이트는 부분적입니다; 모든 문서를 한번에 업데이트 하는 대신, 제공된 키 값 쌍을 이용하여 기존값을 새 값으로 변경합니다. 이는 업데이트 된 객체의 최상위 필드에게만 적용됩니다. 특수한 예외 사항으로는 값이 json `null` 타입일 경우인데, 이 경우에는 해당 필드가 서버의 객체 표현에서 제거되어야 한다는 의미입니다.

~~For client to server interactions, updates are partial; rather than updating the document all at once, any key value pair supplied is used to replace the existing value with the new value. This only applies to the top-level fields of the updated object. A special exception is for when the value is the json `null` type; this means that this field should be removed from the server's representation of the object.~~

이 동작은 클라이언트가 서버로만 게시하는 경우에만 클라이언트-서버 상호작용을 위한 행동입니다. 부분 업데이트 어플리케이션이 적용된 후, 서버-서버간 상호작용과 서버-클라이언트 업데이트 동작에는 객체의 갱신된 모든 정보가 포함되어야 있어야 합니다. 자세한 내용은 [서버 간 상호작용을 위한 업데이트 활동](https://www.w3.org/TR/activitypub/#update-activity-inbox)에 대한 설명을 참조하시길 바랍니다.

~~Note that this behavior is for client to server interaction where the client is posting to the server only. Server to server interaction and updates from the server to the client should contain the entire new representation of the object, after the partial update application has been applied. See the description of the [Update activity for server to server interactions](https://www.w3.org/TR/activitypub/#update-activity-inbox) for more details.~~

[//Comment]: # "의역"

### 6.4 삭제 액티비티 (Delete Activity)

`Delete` 액티비티는 이미 존재하는 객체를 삭제하는데 사용됩니다. 이 액티비티의 부작용은 서버가 주어진 `object`를 삭제된 객체를 참조하는 액티비티에 표기될 객체의 `Tombstone`으로 변경 *할 수도* 있다는 것입니다. 만일 삭제된 객체가 요청될 경우에 `Tombstone` 객체가 응답할 body에 표기가 되어있다면 서버는 HTTP 410 Gone 상태 코드로 응답하여야 하며, 그러지 않을 경우에는 HTTP 404 Not Found 로 응답하여야 합니다.

~~The `Delete` activity is used to delete an already existing object. The side effect of this is that the server *MAY* replace the `object` with a `Tombstone` of the object that will be displayed in activities which reference the deleted object. If the deleted object is requested the server *SHOULD* respond with either the HTTP 410 Gone status code if a `Tombstone` object is presented as the response body, otherwise respond with a HTTP 404 Not Found.~~

삭제된 객체는:

~~A deleted object:~~

>예시 17
>```json
>{
>  "@context": "https://www.w3.org/ns/activitystreams",
>  "id": "https://example.com/~alice/note/72",
>  "type": "Tombstone",
>  "published": "2015-02-10T15:04:55Z",
>  "updated": "2015-02-10T15:04:55Z",
>  "deleted": "2015-02-10T15:04:55Z"
>}
>```

~~Example 17~~

### 6.5 팔로우 액티비티 (Follow Activity)

`Follow` 액티비티는 다른 액터의 액티비티를 구독하는데 사용됩니다.

~~The `Follow` activity is used to subscribe to the activities of another actor.~~

이것을 **아웃박스** 에서 수신받는 부작용으로는 서버가 `Accept` 액티비티를 수신한 이후 이 액티비티의 객체로 `Follow` 액티비티를 수신한 경우에만 한하여, `object`를 `actor`의 [`following` 컬렉션](https://www.w3.org/TR/activitypub/#following)에 이 객체를 추가하는 것을 *권장하는 것* 입니다.

~~The side effect of receiving this in an **outbox** is that the server *SHOULD* add the `object` to the `actor`'s [`following` Collection](https://www.w3.org/TR/activitypub/#following) when and only if an `Accept` activity is subsequently received with this `Follow` activity as its object.~~

### 6.6 추가 액티비티 (Add Activity)

**아웃박스**에 `Add` 액티비티를 수신하면, 서버는 다음과 같은 경우를 제외하고 `object`를 `target` 속성에 명시되어 있는 컬렉션에 추가하는 것을 *권장합니다* :

~~Upon receipt of an `Add` activity into the **outbox**, the server *SHOULD* add the `object` to the collection specified in the `target` property, unless:~~

* `target`을 수신받는 서버가 소유하지 않을 경우 이를 업데이트 할 권한이 없습니다.
* 수신 서버의 판단에 따라 `object`를 `target` 컬렉션에 추가하지 않게 할 수도 있습니다.

~~the `target` is not owned by the receiving server, and thus they are not authorized to update it.~~

~~the `object` is not allowed to be added to the `target` collection for some other reason, at the receiving server's discretion.~~

### 6.7 제거 액티비티 (Remove Activity)

**아웃박스**에 `Remove` 액티비티를 수신하면, 서버는 다음과 같은 경우를 제외하고 `object`를 `target` 속성에 명시되어 있는 컬렉션에서 제거하는 것을 *권장합니다* :

~~Upon receipt of a `Remove` activity into the **outbox**, the server *SHOULD* remove the `object` from the collection specified in the `target` property, unless:~~

* `target`을 수신받는 서버가 소유하지 않을 경우 이를 업데이트 할 권한이 없습니다.
* 수신 서버의 판단에 따라 `object`를 `target` 컬렉션에서 제거하지 않게 할 수도 있습니다.

~~the `target` is not owned by the receiving server, and thus they are not authorized to update it.~~

~~the `object` is not allowed to be removed from the `target` collection for some other reason, at the receiving server's discretion.~~

### 6.8 좋아요 액티비티 (Like Activity)

`Like` 액티비티는 `actor`가 `object`를 좋아한다는 것(likes)을 나타냅니다.

~~The `Like` activity indicates the `actor` likes the `object`.~~

**아웃박스**에서 이것을 수신받는 부작용으로는 서버가 `object`를 `actor`의 [`liked` 컬렉션](https://www.w3.org/TR/activitypub/#liked)에 추가하는 것을 *권장하는 것* 입니다.

~~The side effect of receiving this in an **outbox** is that the server *SHOULD* add the `object` to the `actor`'s [`liked` Collection](https://www.w3.org/TR/activitypub/#liked).~~

### 6.9 차단 액티비티 (Block Activity)

`Block` 액티비티는 액터가 게시한 게시물들 중 `Block` 액티비티를 포함하는 게시물들을 (`object` 속성에 의되어 있듯) 다른 액터가 상호작용 하기를 원지 않다는 것을 표시합니다. 서버는 차단 된 사용자가 액터가 게시한 객체와 상호작용하지 않도록 하는것을 *권장합니다*.

~~The `Block` activity is used to indicate that the posting actor does not want another actor (defined in the `object` property) to be able to interact with objects posted by the actor posting the `Block` activity. The server *SHOULD* prevent the blocked user from interacting with any object posted by the actor.~~

서버는 차단 액티비티들을 그들의 `object`로 전송하는 것을 *권장하지 않습니다*.

~~Servers *SHOULD NOT* deliver Block Activities to their `object`.~~

### 6.10 실행취소 액티비티 (Undo Activity)

`Undo` 액티비티는 이전 액티비티들을 실행취소 하는데 사용됩니다. [역 액티비티와 "실행취소(Undo)"](https://www.w3.org/TR/activitystreams-vocabulary/#inverse)를 액티비티 단어 문서에서 참조하시길 바랍니다. 예를 들어, `Undo`는 이전의 `Like`, `Follow`, 또는 `Block`들을 실행취소 하는데 사용될 수 있습니다. 실행취소 액티비티와 실행취소가 적용될 액티비티는 *반드시* 모두 동일한 액터를 보유하고 있어야 합니다. 적용되었던 부작용들은 가능한 한 실행취소 되어야 합니다. 예를 들어 `Like`를 실행취소할 경우, 이전에 증가한 모든 카운터들을 적절히 감소시켜야 합니다.

~~The `Undo` activity is used to undo a previous activity. See the Activity Vocabulary documentation on [Inverse Activities and "Undo"](https://www.w3.org/TR/activitystreams-vocabulary/#inverse). For example, `Undo` may be used to undo a previous `Like`, `Follow`, or `Block`. The undo activity and the activity being undone *MUST* both have the same actor. Side effects should be undone, to the extent possible. For example, if undoing a `Like`, any counter that had been incremented previously should be decremented appropriately.~~

예외로는 액티비티에게 대신 사용되어야 할 명시적인 "역 액티비티"가 존재할 경우입니다. `Create`를 기반으로 하는 액티비티들은 `Delete`를 대신 사용하여야 하고, `Add` 액티비티들은 `Remove`를 대신하여 사용하여야 합니다.

~~There are some exceptions where there is an existing and explicit "inverse activity" which should be used instead. `Create` based activities should instead use `Delete`, and `Add` activities should use `Remove`.~~

### 6.11 전송 (Delivery)

연합 서버들은 *반드시* [아웃박스 전송](https://www.w3.org/TR/activitypub/#outbox-delivery)에 따라 **아웃박스**에 게시된 모든 액티비티에 대하여 전송을 수행하여야 합니다.

~~Federated servers *MUST* perform delivery on all Activities posted to the **outbox** according to [outbox delivery](https://www.w3.org/TR/activitypub/#outbox-delivery).~~

### 6.12 미디어 업로드 (Uploading Media)

*이 항목은 비표준입니다.*

~~*This section is non-normative.*~~

서버는 이미지, 비디오 또는 기타 이진 데이터와 같은 액티비티들에서 참조 할 문서 유형 업로드를 지원 *할 수도* 있지만, 정확한 메커니즘은 이 버전의 액티비티펍에서 다룰 범위를 벗어납니다. 소셜 웹 커뮤니티 그룹에서는 [액티비티펍 미디어 업로드 보고서](https://www.w3.org/wiki/SocialCG/ActivityPub/MediaUpload)를 통해 프로토콜을 개선하고 있습니다.

~~Servers *MAY* support uploading document types to be referenced in activites, such as images, video or other binary data, but the precise mechanism is out of scope for this version of ActivityPub. The Social Web Community Group is refining the protocol in the [ActivityPub Media Upload report](https://www.w3.org/wiki/SocialCG/ActivityPub/MediaUpload).~~
