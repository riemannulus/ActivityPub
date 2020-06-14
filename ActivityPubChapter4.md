[목차로 돌아가기](ActivityPubContents.md)

## 4. 액터 ~~Actors~~

액티비티펍의 액터는 일반적으로는 [액티비티스트림 액터 타입](https://www.w3.org/TR/activitystreams-vocabulary/#actor-types)에 해당되지만, 반드시 그럴 필요는 없습니다. 예를 들어, [Profile](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-profile) 객체는 액터로 사용되거나 액티비티스트림 확장의 타입처럼 사용될 수가 있습니다. 액터들은 액티비티펍의 다른 객체들과 마찬가지로 [검색](https://www.w3.org/TR/activitypub/#retrieving-objects)할 수 있습니다. 다른 액티비티스트림 객체처럼, 액터들은 URI인 `id`가 있습니다. 사용자 인터페이스에 직접 입력하는 경우(예: 로그인 양식)에는 단순화된 이름을 지원하는것이 바람직합니다. 이를 위하여 ID 정규화는 다음과 같이 수행되기를 *권장합니다* :

~~ActivityPub actors are generally one of the [ActivityStreams Actor Types](https://www.w3.org/TR/activitystreams-vocabulary/#actor-types), but they don't have to be. For example, a [Profile](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-profile) object might be used as an actor, or a type from an ActivityStreams extension. Actors are [retrieved](https://www.w3.org/TR/activitypub/#retrieving-objects) like any other Object in ActivityPub. Like other ActivityStreams objects, actors have an `id`, which is a URI. When entered directly into a user interface (for example on a login form), it is desirable to support simplified naming. For this purpose, ID normalization *SHOULD* be performed as follows:~~

1. 입력된 ID가 유효한 URI일 경우, 그대로 사용합니다.
2. 사용자가 `example.org/alice/`처럼 유효한 것으로 보이지만, 해당 URI에 대한 체계를 추가하지 않은 것으로 보이는 경우, 클라이언트는 `https`같은 기본 체계를 제공하려고 시도 *할 수도* 있습니다.
3. 위 사항들에 해당하지 않는 경우, 입력값은 유효하지 않은 것으로 간주됩니다.


~~1. If the entered ID is a valid URI, then it is to be used directly.~~

~~2. If it appears that the user neglected to add a scheme for a URI that would otherwise be considered valid, such as `example.org/alice/`, clients *MAY* attempt to provide a default scheme, preferably `https`.~~

~~3. Otherwise, the entered value should be considered invalid.~~

액터의 URI가 식별되면 이는 역 참조가 권장됩니다.

~~Once the actor's URI has been identified, it should be dereferenced.~~

[//]: # "Should를 Chapter2의 규칙에 따라 ~권장된다 로 번역하였습니다. 어감상 미묘한 부분이 있으므로 차후에 재검토하겠습니다.(TODO)"

>참고
>
>액티비티펍은 "사용자"와 액터들간의 관계를 강요하지 않으므로 수많은 구성들이 가능합니다. 다수의 인간 유저들이나 조직들이 액터를 제어할 수 있으며, 한명의 인간 또는 조직이 다수의 액터를 제어하는 경우가 있을 수 도 있습니다. 이와 유사하게 액터는 봇과 같은 소프트웨어나 자동화 된 프로세스를 나타낼 수 있습니다. 더 자세한 "사용자" 모델링은 동일한 개체에게 제어되는 액터들을 모아보거나 또는 하나의 액터가 다수의 대체 프로필들이나 상태들을 통하여 제공할 수 있는 것 처럼 구현의 재량에 달려 있습니다.

~~Note~~

~~ActivityPub does not dictate a specific relationship between "users" and Actors; many configurations are possible. There may be multiple human users or organizations controlling an Actor, or likewise one human or organization may control multiple Actors. Similarly, an Actor may represent a piece of software, like a bot, or an automated process. More detailed "user" modelling, for example linking together of Actors which are controlled by the same entity, or allowing one Actor to be presented through multiple alternate profiles or aspects, are at the discretion of the implementation.~~

[//]: # "'Note'를 '참고' 로 변역한것은 [(W3C)웹 콘텐츠 접근성 지침 2.1](http://www.kwacc.or.kr/WAI/wcag21/)를 참조하였습니다."
[//]: # "마지막 문장의 번역을 좀 과도하게 한 감이 있습니다. 차후에 재검토하겠습니다.(TODO)"

#### 4.1 액터 객체 ~~Actor objects~~

Actor objects *MUST* have, in addition to the properties mandated by [3.1 Object Identifiers](https://www.w3.org/TR/activitypub/#obj-id), the following properties:

**inbox**

* A reference to an [[ActivityStreams](https://www.w3.org/TR/activitypub/#bib-ActivityStreams)] [`OrderedCollection`](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-orderedcollection) comprised of all the messages received by the actor; see [5.2 Inbox](https://www.w3.org/TR/activitypub/#inbox).

**outbox**

* An [[ActivityStreams](https://www.w3.org/TR/activitypub/#bib-ActivityStreams)] [`OrderedCollection`](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-orderedcollection) comprised of all the messages produced by the actor; see [5.1 Outbox](https://www.w3.org/TR/activitypub/#outbox).

Implementations *SHOULD*, in addition, provide the following properties:

**following**

* A link to an [[ActivityStreams](https://www.w3.org/TR/activitypub/#bib-ActivityStreams)] collection of the actors that this actor is following; see [5.4 Following Collection](https://www.w3.org/TR/activitypub/#following)

**followers**

* A link to an [[ActivityStreams](https://www.w3.org/TR/activitypub/#bib-ActivityStreams)] collection of the actors that follow this actor; see [5.3 Followers Collection](https://www.w3.org/TR/activitypub/#followers).

Implementations *MAY* provide the following properties:

**liked**

* A link to an [[ActivityStreams](https://www.w3.org/TR/activitypub/#bib-ActivityStreams)] collection of objects this actor has liked; see [5.5 Liked Collection](https://www.w3.org/TR/activitypub/#liked).

> Example 9
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

Implementations *MAY*, in addition, provide the following properties:

**streams**

* A list of supplementary Collections which may be of interest.

**preferredUsername**

* A short username which may be used to refer to the actor, with no uniqueness guarantees.

**endpoints**

* A json object which maps additional (typically server/domain-wide) endpoints which may be useful either for this actor or someone referencing this actor. This mapping may be nested inside the actor document as the value or may be a link to a JSON-LD document with these properties. 

The `endpoints` mapping *MAY* include the following properties:

**proxyUrl**

* Endpoint URI so this actor's clients may access remote ActivityStreams objects which require authentication to access. To use this endpoint, the client posts an `x-www-form-urlencoded` id parameter with the value being the `id` of the requested ActivityStreams object.

**oauthAuthorizationEndpoint**

* If OAuth 2.0 bearer tokens [[RFC6749](https://www.w3.org/TR/activitypub/#bib-RFC6749)] [[RFC6750](https://www.w3.org/TR/activitypub/#bib-RFC6750)] are being used for authenticating [client to server interactions](https://www.w3.org/TR/activitypub/#client-to-server-interactions), this endpoint specifies a URI at which a browser-authenticated user may obtain a new authorization grant.

**oauthTokenEndpoint**

* If OAuth 2.0 bearer tokens [[RFC6749](https://www.w3.org/TR/activitypub/#bib-RFC6749)] [[RFC6750](https://www.w3.org/TR/activitypub/#bib-RFC6750)] are being used for authenticating [client to server interactions](https://www.w3.org/TR/activitypub/#client-to-server-interactions), this endpoint specifies a URI at which a client may acquire an access token.

**provideClientKey**

* If Linked Data Signatures and HTTP Signatures are being used for authentication and authorization, this endpoint specifies a URI at which browser-authenticated users may authorize a client's public key for [client to server interactions](https://www.w3.org/TR/activitypub/#client-to-server-interactions).

**signClientKey**

* If Linked Data Signatures and HTTP Signatures are being used for authentication and authorization, this endpoint specifies a URI at which a client key may be signed by the actor's key for a time window to act on behalf of the actor in interacting with foreign servers. 

**sharedInbox**

* An optional endpoint [used for wide delivery of publicly addressed activities and activities sent to followers](https://www.w3.org/TR/activitypub/#shared-inbox-delivery). `sharedInbox` endpoints *SHOULD* also be publicly readable `OrderedCollection` objects containing objects addressed to the [Public](https://www.w3.org/TR/activitypub/#public-addressing) special collection. Reading from the `sharedInbox` endpoint *MUST NOT* present objects which are not addressed to the `Public` endpoint.

> Note
>
> As the upstream vocabulary for ActivityPub, any applicable [[ActivityStreams](https://www.w3.org/TR/activitypub/#bib-ActivityStreams)] property may be used on ActivityPub Actors. Some ActivityStreams properties are particularly worth highlighting to demonstrate how they are used in ActivityPub implementations.
>**url**
>A link to the actor's "profile web page", if not equal to the value of `id`.
>
>**name**
>The preferred "nickname" or "display name" of the actor.
>
>**summary**
>A quick summary or bio by the user about themselves.
>
>**icon**
>A link to an image or an Image object which represents the user's profile picture (this may be a thumbnail).

> Note
>
> Properties containing natural language values, such as `name`, `preferredUsername`, or `summary`, make use of [natural language support defined in ActivityStreams](https://www.w3.org/TR/activitystreams-core/#naturalLanguageValues).
