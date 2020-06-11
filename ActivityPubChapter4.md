### 4. Actors

ActivityPub actors are generally one of the [ActivityStreams Actor Types](https://www.w3.org/TR/activitystreams-vocabulary/#actor-types), but they don't have to be. For example, a Profile object might be used as an actor, or a type from an ActivityStreams extension. Actors are [retrieved](https://www.w3.org/TR/activitypub/#retrieving-objects) like any other Object in ActivityPub. Like other ActivityStreams objects, actors have an `id`, which is a URI. When entered directly into a user interface (for example on a login form), it is desirable to support simplified naming. For this purpose, ID normalization *SHOULD* be performed as follows:

1. If the entered ID is a valid URI, then it is to be used directly.
2. If it appears that the user neglected to add a scheme for a URI that would otherwise be considered valid, such as `example.org/alice/`, clients *MAY* attempt to provide a default scheme, preferably `https`.
3. Otherwise, the entered value should be considered invalid.

Once the actor's URI has been identified, it should be dereferenced.

> Note
>
> ActivityPub does not dictate a specific relationship between "users" and Actors; many configurations are possible. There may be multiple human users or organizations controlling an Actor, or likewise one human or organization may control multiple Actors. Similarly, an Actor may represent a piece of software, like a bot, or an automated process. More detailed "user" modelling, for example linking together of Actors which are controlled by the same entity, or allowing one Actor to be presented through multiple alternate profiles or aspects, are at the discretion of the implementation.

#### 4.1 Actor objects

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
