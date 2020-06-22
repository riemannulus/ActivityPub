[목차로 돌아가기](ActivityPubContents.md)

## 4. 액터 (Actors)

액티비티펍의 액터는 일반적으로는 [액티비티스트림 액터 타입](https://www.w3.org/TR/activitystreams-vocabulary/#actor-types)에 해당되지만, 반드시 그럴 필요는 없습니다. 예를 들어, [프로필(Profile)](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-profile) 객체는 액터로 사용할 수도 있으나, 액티비티스트림 확장의 타입으로 사용될 수도 있습니다. 액터들은 액티비티펍의 다른 객체들과 마찬가지로 [검색](https://www.w3.org/TR/activitypub/#retrieving-objects)할 수 있습니다. 다른 액티비티스트림 객체처럼, 액터들은 URI인 `id`가 있습니다. 사용자 인터페이스에 직접 입력하는 경우(예: 로그인 양식)에는 단순화된 이름을 지원하는것이 바람직합니다. 이를 위하여 ID 정규화는 다음과 같이 수행되기를 *권장합니다* :

~~ActivityPub actors are generally one of the [ActivityStreams Actor Types](https://www.w3.org/TR/activitystreams-vocabulary/#actor-types), but they don't have to be. For example, a [Profile](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-profile) object might be used as an actor, or a type from an ActivityStreams extension. Actors are [retrieved](https://www.w3.org/TR/activitypub/#retrieving-objects) like any other Object in ActivityPub. Like other ActivityStreams objects, actors have an `id`, which is a URI. When entered directly into a user interface (for example on a login form), it is desirable to support simplified naming. For this purpose, ID normalization *SHOULD* be performed as follows:~~

1. 입력된 ID가 유효한 URI일 경우, 그대로 사용합니다.
2. 사용자가 `example.org/alice/`처럼 유효한 것으로 보이지만, 해당 URI에 대한 체계를 추가하지 않은 것으로 보이는 경우, 클라이언트는 `https`같은 기본 체계를 제공하려고 시도 *할 수도* 있습니다.
3. 위 사항들에 해당하지 않는 경우, 입력값은 유효하지 않은 것으로 간주됩니다.


~~1. If the entered ID is a valid URI, then it is to be used directly.~~

~~2. If it appears that the user neglected to add a scheme for a URI that would otherwise be considered valid, such as `example.org/alice/`, clients *MAY* attempt to provide a default scheme, preferably `https`.~~

~~3. Otherwise, the entered value should be considered invalid.~~

액터의 URI가 식별되면, 이것의 역 참조가 권장됩니다.

~~Once the actor's URI has been identified, it should be dereferenced.~~

>참고
>
>액티비티펍은 "사용자"와 액터들간의 관계를 강요하지 않으므로 수많은 구성들이 가능합니다. 다수의 인간 사용자들이나 조직들이 액터를 제어할 수 있으며, 한명의 인간 또는 조직이 다수의 액터를 제어하는 경우가 있을 수 도 있습니다. 이와 유사하게 액터는 봇과 같은 소프트웨어나 자동화 된 프로세스를 나타낼 수 있습니다. 더 자세한 "사용자" 모델링은 동일한 개체에게 제어되는 액터들을 모아보거나 또는 하나의 액터가 다수의 대체 프로필들이나 상태들을 통하여 제공할 수 있는 것 처럼 구현의 재량에 달려 있습니다.

~~Note~~

~~ActivityPub does not dictate a specific relationship between "users" and Actors; many configurations are possible. There may be multiple human users or organizations controlling an Actor, or likewise one human or organization may control multiple Actors. Similarly, an Actor may represent a piece of software, like a bot, or an automated process. More detailed "user" modelling, for example linking together of Actors which are controlled by the same entity, or allowing one Actor to be presented through multiple alternate profiles or aspects, are at the discretion of the implementation.~~

[//]: # "'Note'를 '참고' 로 변역한것은 [(W3C)웹 콘텐츠 접근성 지침 2.1](http://www.kwacc.or.kr/WAI/wcag21/)를 참조하였습니다."
[//]: # "마지막 문장의 번역을 좀 과도하게 한 감이 있습니다. 차후에 재검토하겠습니다.(//TODO)"

### 4.1 액터 객체 (Actor objects)

액터 객체들은 *반드시* [3.1 객체 지정자](https://www.w3.org/TR/activitypub/#obj-id)에 정의된 속성들 외에도 다음과 같은 속성들을 지니고 있습니다:

~~Actor objects *MUST* have, in addition to the properties mandated by [3.1 Object Identifiers](https://www.w3.org/TR/activitypub/#obj-id), the following properties:~~

**인박스(inbox)**
* 액터가 수신한 모든 메세지로 구성되어 있는 [액티비티스트림](https://www.w3.org/TR/activitypub/#bib-ActivityStreams) [`OrderedCollection`](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-orderedcollection)에 대한 참조; [5.2 인박스](https://www.w3.org/TR/activitypub/#inbox) 참고바람.

**아웃박스(outbox)**
* 액터가 생성한 모든 메세지로 구성되어 있는 [액티비티스트림](https://www.w3.org/TR/activitypub/#bib-ActivityStreams) [`OrderedCollection`](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-orderedcollection)에 대한 참조; [5.1 아웃박스](https://www.w3.org/TR/activitypub/#outbox) 참고바람.

~~**inbox**~~
~~* A reference to an [[ActivityStreams](https://www.w3.org/TR/activitypub/#bib-ActivityStreams)] [`OrderedCollection`](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-orderedcollection) comprised of all the messages received by the actor; see [5.2 Inbox](https://www.w3.org/TR/activitypub/#inbox).~~

~~**outbox**~~
~~* An [[ActivityStreams](https://www.w3.org/TR/activitypub/#bib-ActivityStreams)] [`OrderedCollection`](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-orderedcollection) comprised of all the messages produced by the actor; see [5.1 Outbox](https://www.w3.org/TR/activitypub/#outbox).~~

구현체는 다음과 같은 속성들을 제공하는것이 *권장됩니다* :

~~Implementations *SHOULD*, in addition, provide the following properties:~~

**팔로잉(following)**

* 주어진 액터가 팔로우 하고 있는 액터들의 [[액티비티스트림](https://www.w3.org/TR/activitypub/#bib-ActivityStreams)] 에 대한 링크; [5.4 팔로잉 ](https://www.w3.org/TR/activitypub/#following) 참고바람.

**팔로워(followers)**

* 주어진 액터를 팔로우 하고 있는 액터들의 [[액티비티스트림](https://www.w3.org/TR/activitypub/#bib-ActivityStreams)] 에 대한 링크; [5.3 팔로워 ](https://www.w3.org/TR/activitypub/#followers) 참고바람.

~~**following**~~
~~* A link to an [[ActivityStreams](https://www.w3.org/TR/activitypub/#bib-ActivityStreams)] collection of the actors that this actor is following; see [5.4 Following Collection](https://www.w3.org/TR/activitypub/#following).~~

~~**followers**~~
~~* A link to an [[ActivityStreams](https://www.w3.org/TR/activitypub/#bib-ActivityStreams)] collection of the actors that follow this actor; see [5.3 Followers Collection](https://www.w3.org/TR/activitypub/#followers).~~

구현체는 다음과 같은 속성들을 제공 *할 수도* 있습니다:

~~Implementations *MAY* provide the following properties:~~

**좋아하는 (liked)**

* 주어진 액터가 좋아하는 [[액티비티스트림](https://www.w3.org/TR/activitypub/#bib-ActivityStreams)] 에 대한 링크; [5.5 좋아하는 ](https://www.w3.org/TR/activitypub/#liked) 참고바람.


~~**liked**~~
~~* A link to an [[ActivityStreams](https://www.w3.org/TR/activitypub/#bib-ActivityStreams)] collection of objects this actor has liked; see [5.5 Liked Collection](https://www.w3.org/TR/activitypub/#liked).~~

> 예시 9
>
> ```json
> {
>   "@context": ["https://www.w3.org/ns/activitystreams",
>                {"@language": "ja"}],
>   "type": "Person",
>   "id": "https://kenzoishii.example.com/",
>   "following": "https://kenzoishii.example.com/following.json",
>   "followers": "https://kenzoishii.example.com/followers.json",
>   "liked": "https://kenzoishii.example.com/liked.json",
>   "inbox": "https://kenzoishii.example.com/inbox.json",
>   "outbox": "https://kenzoishii.example.com/feed.json",
>   "preferredUsername": "kenzoishii",
>   "name": "石井健蔵",
>   "summary": "この方はただの例です",
>   "icon": [
>     "https://kenzoishii.example.com/image/165987aklre4"
>   ]
> }
> ```

[//]: # "@language가 ja로 설정되어 있으므로 따로 번역을 하지 않겠습니다."

구현체는 다음과 같은 속성들을 추가적으로 제공 *할 수도* 있습니다:

~~Implementations *MAY*, in addition, provide the following properties:~~

**스트림(streams)**

* 관심이 있을법한 부가 컬렉션들의 리스트.

**선호하는 사용자 이름(preferredUsername)**

* 유일성이 보장되지 않고, 액터를 지칭하는데 사용될 수 있는 간략한 사용자이름.

**엔드포인트(endpoints)**

* 주어진 액터 또는 주어진 액터를 참조하는 누군가에게 유용할 수 있는 추가 (일반적으로는 서버/도메인 전체) 엔드포인트를 매핑하는 json 객체입니다. 이 매핑은 액터 문서 속에 값이나 이러한 속성을 지닌 JSON-LD 문서에 대한 링크일 수 도 있습니다.

~~**streams**~~
~~* A list of supplementary Collections which may be of interest.~~

~~**preferredUsername**~~
~~* A short username which may be used to refer to the actor, with no uniqueness guarantees.~~

~~**endpoints**~~
~~* A json object which maps additional (typically server/domain-wide) endpoints which may be useful either for this actor or someone referencing this actor. This mapping may be nested inside the actor document as the value or may be a link to a JSON-LD document with these properties.~~

`endpoints` 매핑은 다음과 같은 속성들을 포함 *할 수도* 있습니다:

~~The `endpoints` mapping *MAY* include the following properties:~~

**프록시 Url(proxyUrl)**

* 주어진 액터의 클라이언트들이 인증이 필요한 원격 액티비티스트림 객체에 접근을 가능하게 하는 엔드포인트 URI. 이 엔드포인트를 사용하려면 클라이언트는 요청된 액티비티스트림 객체의 `id`값을 `x-www-form-urlencoded`를 통하여 전송합니다.

~~**proxyUrl**~~

~~* Endpoint URI so this actor's clients may access remote ActivityStreams objects which require authentication to access. To use this endpoint, the client posts an `x-www-form-urlencoded` id parameter with the value being the `id` of the requested ActivityStreams object.~~

**oauth 인증 엔드포인트(oauthAuthorizationEndpoint)**

* 만약 OAuth 2.0 bearer 토큰 [[RFC6749](https://www.w3.org/TR/activitypub/#bib-RFC6749)] [[RFC6750](https://www.w3.org/TR/activitypub/#bib-RFC6750)] 을 사용하여 [클라이언트-서버 간 상호작용](https://www.w3.org/TR/activitypub/#client-to-server-interactions)을 인증하는데 사용할 경우, 이 엔드포인트는 브라우저-인증된 사용자가 새 권한을 부여 받을 수 있는 URI를 제공합니다.

~~**oauthAuthorizationEndpoint**~~

~~* If OAuth 2.0 bearer tokens [[RFC6749](https://www.w3.org/TR/activitypub/#bib-RFC6749)] [[RFC6750](https://www.w3.org/TR/activitypub/#bib-RFC6750)] are being used for authenticating [client to server interactions](https://www.w3.org/TR/activitypub/#client-to-server-interactions), this endpoint specifies a URI at which a browser-authenticated user may obtain a new authorization grant.~~

**oauth 토큰 엔드포인트(oauthTokenEndpoint)**

* 만약 OAuth 2.0 bearer 토큰 [[RFC6749](https://www.w3.org/TR/activitypub/#bib-RFC6749)] [[RFC6750](https://www.w3.org/TR/activitypub/#bib-RFC6750)] 을 사용하여 [클라이언트-서버 간 상호작용](https://www.w3.org/TR/activitypub/#client-to-server-interactions)을 인증하는데 사용할 경우, 이 엔드포인트는 클라이언트가 새 권한을 부여 받을 수 있는 URI를 제공합니다.

~~**oauthTokenEndpoint**~~

~~* If OAuth 2.0 bearer tokens [[RFC6749](https://www.w3.org/TR/activitypub/#bib-RFC6749)] [[RFC6750](https://www.w3.org/TR/activitypub/#bib-RFC6750)] are being used for authenticating [client to server interactions](https://www.w3.org/TR/activitypub/#client-to-server-interactions), this endpoint specifies a URI at which a client may acquire an access token.~~

**사용자 키 제공(provideClientKey)**

* 만약 링크된 데이터 서명과 HTTP 서명이 인증 및 권한부여에 사용되는 경우, 이 엔드포인트는 브라우저-인증된 사용자가 [클라이언트-서버 간 상호작용](https://www.w3.org/TR/activitypub/#client-to-server-interactions)에 대한 클라이언트의 공개 키를 승인 할 수 있는 URI를 제공합니다.

~~**provideClientKey**~~

~~* If Linked Data Signatures and HTTP Signatures are being used for authentication and authorization, this endpoint specifies a URI at which browser-authenticated users may authorize a client's public key for [client to server interactions](https://www.w3.org/TR/activitypub/#client-to-server-interactions).~~

**클라이언트 키 인증(signClientKey)**

* 만약 링크된 데이터 서명과 HTTP 서명이 인증 및 권한부여에 사용되는 경우, 이 엔드포인트는 클라이언트가 외부 서버와 상호 작용할 때 액터를 대신하여 잠시동안 액터의 키를 사용하여 클라이언트의 키에 서명 할 수 있는 URI를 제공합니다.

~~**signClientKey**~~

~~* If Linked Data Signatures and HTTP Signatures are being used for authentication and authorization, this endpoint specifies a URI at which a client key may be signed by the actor's key for a time window to act on behalf of the actor in interacting with foreign servers.~~

**공유 인박스(sharedInbox)**

* 선택적 엔드포인트는 [공개적으로 알려진 액티비티들과 팔로워들에게 전송 된 액티비티들의 광범위한 전달에 사용됩니다](https://www.w3.org/TR/activitypub/#shared-inbox-delivery). `sharedInbox` 엔드포인트들은 [공개(Public)](https://www.w3.org/TR/activitypub/#public-addressing) 특수 컬렉션으로 지정된 객체들을 포함하는, 공개적으로 읽을 수 있는 `OrderedCollection` 객체들이기를 *권장됩니다*. `sharedInbox` 엔드포인트에서 읽기(Reading)를 수행할 경우, `Public` 엔드포인트로 지정되지 않은 객체를 표시해서는 *절대 안됩니다*.

~~**sharedInbox**~~

~~* An optional endpoint [used for wide delivery of publicly addressed activities and activities sent to followers](https://www.w3.org/TR/activitypub/#shared-inbox-delivery). `sharedInbox` endpoints *SHOULD* also be publicly readable `OrderedCollection` objects containing objects addressed to the [Public](https://www.w3.org/TR/activitypub/#public-addressing) special collection. Reading from the `sharedInbox` endpoint *MUST NOT* present objects which are not addressed to the `Public` endpoint.~~

[//]: # "'Browser-authenticated'를 브라우저-인증으로 번역해 두었습니다."
[//]: # "'Specify'를 '지정합니다'가 아닌 '제공합니다'로 번역해 두었습니다."
[//]: # "'provideClientKey'에서 'Linked Data Signatures'를 '링크된 데이터 서명'으로 번역해 두었습니다."
[//]: # "'signClientKey'에서 'time window'를 '잠시동안'으로 번역해 두었습니다. '일정 구간', '일정 시간'이나 다른 번역이 올바를수도 있으므로 차후에 검토해보겠습니다.(//TODO)"
[//]: # "'sharedInbox'에서 'SHOULD'를 Chapter2의 RFC2119 해석 (MAY/MUST/...) 표준에 맞추기 위하여 '권장됩니다' 로 해석하였으나 부자연스러운 감이 있는 것 같습니다. '...객체들 이여야 합니다' 같이 자연스러운 해석이 옳은 것인지는 차후에 검토해 보겠습니다.(//TODO)"

>참고
>
>액티비티펍의 상위(upstream) 어휘로써, 적용 가능한 모든 [[액티비티스트림](https://www.w3.org/TR/activitypub/#bib-ActivityStreams)] 속성은 액티비티펍의 액터에 사용할 수 있습니다. 일부 액티비티스트림 속성들은 액티비티펍을 구현하는데 있어서는 특히 강조하여 설명할 가치가 있습니다:
>
>**url**
>- `id`값과 일치하지 않은 경우, 주어진 액터의 "프로필 웹 페이지".
>
>**이름(name)**
>- 주어진 액터의 선호하는 "닉네임" 또는 "표기 이름".
>
>**요약(summary)**
>- 주어진 사용자에 대한 간략한 요약 또는 자기소개.
>
>**아이콘(icon)**
>- 주어진 사용자의 (썸네일 일 수도 있는) 프로필 사진을 나타내는 이미지 객체 또는 이미지에 대한 링크.

[//Comment]: # "'upstream vocabulary'에 대한 적절한 번역이 없는 바, Chapter1의 내용을 고려하여 '상위 어휘'로 번역하였습니다"

~~Note~~

~~As the upstream vocabulary for ActivityPub, any applicable [[ActivityStreams](https://www.w3.org/TR/activitypub/#bib-ActivityStreams)] property may be used on ActivityPub Actors. Some ActivityStreams properties are particularly worth highlighting to demonstrate how they are used in ActivityPub implementations.~~

~~**url**~~
~~A link to the actor's "profile web page", if not equal to the value of `id`.~~

~~**name**~`
~~The preferred "nickname" or "display name" of the actor.~~

~~**summary**~~
~~A quick summary or bio by the user about themselves.~~

~~**icon**~~
~~A link to an image or an Image object which represents the user's profile picture (this may be a thumbnail).~~

>참고
>
>`name`, `preferredUserName`, 또는 `summary`와 같은 자연어 값을 포함하는 속성에 대해서는 [액티비티스트림에 정의 되어있는 자연어 지원](https://www.w3.org/TR/activitystreams-core/#naturalLanguageValues)을 사용하여 주시길 바랍니다.

~~Note~~

~~Properties containing natural language values, such as `name`, `preferredUsername`, or `summary`, make use of [natural language support defined in ActivityStreams](https://www.w3.org/TR/activitystreams-core/#naturalLanguageValues).~~
