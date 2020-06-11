### 5. Collections

[[ActivityStreams](https://www.w3.org/TR/activitypub/#bib-ActivityStreams)] defines the collection concept; ActivityPub defines several collections with special behavior. Note that ActivityPub makes use of [ActivityStreams paging](https://www.w3.org/TR/activitystreams-core/#paging) to traverse large sets of objects.

Note that some of these collections are specified to be of type [`OrderedCollection`](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-orderedcollection) specifically, while others are permitted to be either a [`Collection`](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-collection) or an [`OrderedCollection`](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-orderedcollection). An [`OrderedCollection`](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-orderedcollection) *MUST* be presented consistently in reverse chronological order.

>Note
>
>What property is used to determine the reverse chronological order is intentionally left as an implementation detail. For example, many SQL-style databases use an incrementing integer as an identifier, which can be reasonably used for handling insertion order in most cases. In other databases, an insertion time timestamp may be preferred. What is used isn't important, but the ordering of elements must remain intact, with newer items first. A property which changes regularly, such a "last updated" timestamp, should not be used.

#### 5.1 Outbox

The outbox is discovered through the `outbox` property of an [actor's](https://www.w3.org/TR/activitypub/#actors) profile. The `outbox` *MUST* be an [`OrderedCollection`](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-orderedcollection).

The outbox stream contains activities the user has published, subject to the ability of the requestor to retrieve the activity (that is, the contents of the outbox are filtered by the permissions of the person reading it). If a user submits a request without [Authorization](https://www.w3.org/TR/activitypub/#authorization) the server should respond with all of the [Public](https://www.w3.org/TR/activitypub/#public-addressing) posts. This could potentially be all relevant objects published by the user, though the number of available items is left to the discretion of those implementing and deploying the server.

The outbox accepts HTTP POST requests, with behaviour described in [Client to Server Interactions](https://www.w3.org/TR/activitypub/#client-to-server-interactions).

#### 5.2 Inbox

The inbox is discovered through the `inbox` property of an [actor's](https://www.w3.org/TR/activitypub/#actors) profile. The `inbox` *MUST* be an OrderedCollection.

The inbox stream contains all activities received by the actor. The server *SHOULD* filter content according to the requester's permission. In general, the owner of an inbox is likely to be able to access all of their inbox contents. Depending on access control, some other content may be public, whereas other content may require authentication for non-owner users, if they can access the inbox at all.

The server *MUST* perform de-duplication of activities returned by the inbox. Duplication can occur if an activity is addressed both to an actor's followers, and a specific actor who also follows the recipient actor, and the server has failed to de-duplicate the recipients list. Such deduplication *MUST* be performed by comparing the `id` of the activities and dropping any activities already seen.

The inboxes of actors on federated servers accepts HTTP POST requests, with behaviour described in [Delivery](https://www.w3.org/TR/activitypub/#delivery). Non-federated servers *SHOULD* return a 405 Method Not Allowed upon receipt of a POST request.

#### 5.3 Followers Collection

Every [actor](https://www.w3.org/TR/activitypub/#actors) *SHOULD* have a `followers` collection. This is a list of everyone who has sent a [Follow](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-follow) activity for the actor, added as a [side effect](https://www.w3.org/TR/activitypub/#follow-activity-outbox). This is where one would find a list of all the actors that are following the actor. The `followers` collection *MUST* be either an [`OrderedCollection`](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-orderedcollection) or a [`Collection`](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-collection) and MAY be filtered on privileges of an authenticated user or as appropriate when no authentication is given.

>Note: Default for notification targeting
>
>The follow activity generally is a request to see the objects an actor creates. This makes the Followers collection an appropriate default target for [delivery](https://www.w3.org/TR/activitypub/#delivery) of notifications.

#### 5.4 Following Collection

Every actor *SHOULD* have a `following` collection. This is a list of everybody that the actor has followed, added as a [side effect](https://www.w3.org/TR/activitypub/#follow-activity-outbox). The `following` collection *MUST* be either an [`OrderedCollection`](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-orderedcollection) or a [`Collection`](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-collection) and `MAY` be filtered on privileges of an authenticated user or as appropriate when no authentication is given.

#### 5.5 Liked Collection

Every actor `MAY` have a `liked` collection. This is a list of every object from all of the actor's Like activities, added as a [side effect](https://www.w3.org/TR/activitypub/#like-activity-outbox). The `liked` collection MUST be either an [`OrderedCollection`](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-orderedcollection) or a [`Collection`](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-collection) and MAY be filtered on privileges of an authenticated user or as appropriate when no authentication is given.

#### 5.6 Public Addressing

In addition to [[ActivityStreams](https://www.w3.org/TR/activitypub/#bib-ActivityStreams)] collections and objects, Activities may additionally be addressed to the special "public" collection, with the identifier `https://www.w3.org/ns/activitystreams#Public`. For example:

>Example 10
>
>```json
>{
>  "@context": "https://www.w3.org/ns/activitystreams",
>  "id": "https://www.w3.org/ns/activitystreams#Public",
>  "type": "Collection"
>}
>```

Activities addressed to this special URI shall be accessible to all users, without authentication. Implementations *MUST NOT* deliver to the "public" special collection; it is not capable of receiving actual activities. However, actors MAY have a [`sharedInbox`](https://www.w3.org/TR/activitypub/#sharedInbox) endpoint which is available for efficient shared delivery of public posts (as well as posts to followers-only); see [7.1.3 Shared Inbox Delivery](https://www.w3.org/TR/activitypub/#shared-inbox-delivery).

>Note
>
>Compacting an ActivityStreams object using the ActivityStreams JSON-LD context might result in `https://www.w3.org/ns/activitystreams#Public` being represented as simply `Public` or `as:Public` which are valid representations of the Public collection. Implementations which treat ActivityStreams objects as simply JSON rather than converting an incoming activity over to a local context using JSON-LD tooling should be aware of this and should be prepared to accept all three representations.

#### 5.7 Likes Collection

Every object *MAY* have a `likes` collection. This is a list of all `Like` activities with this object as the `object` property, added as a [side effect](https://www.w3.org/TR/activitypub/#like-activity-inbox). The likes collection *MUST* be either an [`OrderedCollection`](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-orderedcollection) or a [`Collection`](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-collection) and *MAY* be filtered on privileges of an authenticated user or as appropriate when no authentication is given.

>Note
>
>Care should be taken to not confuse the the [`likes`](https://www.w3.org/TR/activitypub/#likes) collection with the similarly named but different [`liked`](https://www.w3.org/TR/activitypub/#liked) collection. In sum:
>
>* **liked**: Specifically a property of actors. This is a collection of *Like* activities performed by the actor, added to the collection as a [side effect of delivery to the outbox](https://www.w3.org/TR/activitypub/#like-activity-outbox).
>* **likes**: May be a property of any object. This is a collection of *Like* activities referencing this object, added to the collection as a [side effect of delivery to the inbox](https://www.w3.org/TR/activitypub/#like-activity-inbox).

#### 5.8 Shares Collection

Every object *MAY* have a `shares` collection. This is a list of all `Announce` activities with this `object` as the `object` property, added as a [side effect](https://www.w3.org/TR/activitypub/#announce-activity-inbox). The `shares` collection *MUST* be either an [`OrderedCollection`](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-orderedcollection) or a [`Collection`](https://www.w3.org/TR/activitystreams-vocabulary/#dfn-collection) and *MAY* be filtered on privileges of an authenticated user or as appropriate when no authentication is given.
