### 6. Client to Server Interactions

Activities as defined by [[ActivityStreams](https://www.w3.org/TR/activitypub/#bib-ActivityStreams)] are the core mechanism for creating, modifying and sharing content within the social graph.

Client to server interaction takes place through clients posting Activities to an actor's [outbox](https://www.w3.org/TR/activitypub/#outbox). To do this, clients *MUST* discover the URL of the actor's outbox from their [profile](https://www.w3.org/TR/activitypub/#actor-objects) and then *MUST* make an HTTP `POST` request to this URL with the Content-Type of `application/ld+json; profile="https://www.w3.org/ns/activitystreams"`. Servers *MAY* interpret a Content-Type or Accept header of `application/activity+json` as equivalent to `application/ld+json; profile="https://www.w3.org/ns/activitystreams"` for client-to-server interactions. The request *MUST* be authenticated with the credentials of the user to whom the outbox belongs. The body of the `POST` request *MUST* contain a single Activity (which *MAY* contain embedded objects), or a single non-Activity object which [will be wrapped in a Create activity by the server](https://www.w3.org/TR/activitypub/#object-without-create).

>Example 11: Submitting an Activity to the Outbox
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

If an Activity is submitted with a value in the `id` property, servers *MUST* ignore this and generate a new `id` for the Activity. Servers *MUST* return a `201 Created` HTTP code, and unless the activity is transient, *MUST* include the new `id` in the `Location` header.

>Example 12: Outbox response to submitted Activity
>
>HTTP/1.1 201 Created
>
>Location: https://dustycloud.org/likes/345

The server *MUST* remove the `bto` and/or `bcc` properties, if they exist, from the ActivityStreams object before delivery, but *MUST* utilize the addressing originally stored on the `bto` / `bcc` properties for determining recipients in [delivery](https://www.w3.org/TR/activitypub/#delivery).

The server *MUST* then add this new Activity to the [outbox](https://www.w3.org/TR/activitypub/#outbox) collection. Depending on the type of Activity, servers may then be required to carry out further side effects. (However, there is no guarantee that time the Activity may appear in the outbox. The Activity might appear after a delay or disappear at any period). These are described per individual Activity below.

Attempts to submit objects to servers not implementing client to server support *SHOULD* result in a `405 Method Not Allowed` response.

HTTP caching mechanisms [[RFC7234](https://www.w3.org/TR/activitypub/#bib-RFC7234)] *SHOULD* be respected when appropriate, both in clients receiving responses from servers as well as servers sending responses to clients.

#### 6.1 Client Addressing

**Clients** are responsible for addressing new Activities appropriately. To some extent, this is dependent upon the particular client implementation, but clients must be aware that the server will only forward new Activities to addressees in the `to`, `bto`, `cc`, `bcc`, and `audience` fields.

The [Followers Collection](https://www.w3.org/TR/activitypub/#followers) and/or the [Public Collection](https://www.w3.org/TR/activitypub/#public-addressing) are good choices for the default addressing of new Activities.

Clients *SHOULD* look at any objects attached to the new Activity via the `object`, `target`, `inReplyTo` and/or `tag` fields, retrieve their `actor` or `attributedTo` properties, and MAY also retrieve their addressing properties, and add these to the `to` or `cc` fields of the new Activity being created. Clients *MAY* recurse through attached objects, but if doing so, *SHOULD* set a limit for this recursion. (Note that this does not suggest that the client should "unpack" collections of actors being addressed as individual recipients).

Clients *MAY* give the user the chance to amend this addressing in the UI.

For example, when Chris likes the following article by Amy:

>Example 13: An Article
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

the like is generated by the client as:

>Example 14: A Like of the Article
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

The receiving outbox can then perform [delivery](https://www.w3.org/TR/activitypub/#delivery) to not only the followers of Chris (the liker), but also to Amy, and Amy's followers and Evan, both of whom received the original article.

Clients submitting the following activities to an `outbox` *MUST* provide the `object` property in the activity: `Create`, `Update`, `Delete`, `Follow`, `Add`, `Remove`, `Like`, `Block`, `Undo`. Additionally, clients submitting the following activities to an outbox *MUST* also provide the `target` property: `Add`, `Remove`.

#### 6.2 Create Activity

The `Create` activity is used when posting a new object. This has the side effect that the object embedded within the Activity (in the `object` property) is created.

When a `Create` activity is posted, the `actor` of the activity SHOULD be copied onto the `object`'s `attributedTo` field.

A mismatch between addressing of the Create activity and its `object` is likely to lead to confusion. As such, a server *SHOULD* copy any recipients of the Create activity to its `object` upon initial distribution, and likewise with copying recipients from the object to the wrapping Create activity. Note that it is acceptable for the `object`'s addressing to be changed later without changing the `Create`'s addressing (for example via an `Update` activity).

##### 6.2.1 Object creation without a Create Activity

For client to server posting, it is possible to submit an object for creation without a surrounding activity. The server MUST accept a valid [[ActivityStreams](https://www.w3.org/TR/activitypub/#bib-ActivityStreams)] object that isn't a subtype of `Activity` in the POST request to the outbox. The server then *MUST* attach this `object` as the object of a [Create Activity](https://www.w3.org/TR/activitypub/#create-activity-outbox). For non-transient objects, the server *MUST* attach an `id` to both the wrapping `Create` and its wrapped `Object`.

>Note
>
>The `Location` value returned by the server should be the URL of the new Create activity (rather than the object).

Any `to`, `bto`, `cc`, `bcc`, and `audience` properties specified on the object *MUST* be copied over to the new Create activity by the server.

>Example 15: Object with audience targeting
>
>```json
>{
>  "@context": "https://www.w3.org/ns/activitystreams",
>  "type": "Note",
>  "content": "This is a note",
>  "published": "2015-02-10T15:04:55Z",
>  "to": ["https://example.org/~john/"],
>  "cc": ["https://example.com/~erik/followers",
>         "https://www.w3.org/ns/activitystreams#Public"]
>}
>```

The above example could be converted to this:

>Example 16: Create Activity wrapper generated by the server
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
>    "content": "This is a note",
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

#### 6.3 Update Activity

The `Update` activity is used when updating an already existing object. The side effect of this is that the `object` *MUST* be modified to reflect the new structure as defined in the update activity, assuming the actor has permission to update this object.

##### 6.3.1 Partial Updates

For client to server interactions, updates are partial; rather than updating the document all at once, any key value pair supplied is used to replace the existing value with the new value. This only applies to the top-level fields of the updated object. A special exception is for when the value is the json `null` type; this means that this field should be removed from the server's representation of the object.

Note that this behavior is for client to server interaction where the client is posting to the server only. Server to server interaction and updates from the server to the client should contain the entire new representation of the object, after the partial update application has been applied. See the description of the [Update activity for server to server interactions](https://www.w3.org/TR/activitypub/#update-activity-inbox) for more details.

#### 6.4 Delete Activity

The `Delete` activity is used to delete an already existing object. The side effect of this is that the server *MAY* replace the `object` with a `Tombstone` of the object that will be displayed in activities which reference the deleted object. If the deleted object is requested the server *SHOULD* respond with either the HTTP 410 Gone status code if a `Tombstone` object is presented as the response body, otherwise respond with a HTTP 404 Not Found.

A deleted object:

>Example 17
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

#### 6.5 Follow Activity

The `Follow` activity is used to subscribe to the activities of another actor.

The side effect of receiving this in an **outbox** is that the server *SHOULD* add the `object` to the `actor`'s [`following` Collection](https://www.w3.org/TR/activitypub/#following) when and only if an `Accept` activity is subsequently received with this `Follow` activity as its object.

#### 6.6 Add Activity

Upon receipt of an `Add` activity into the **outbox**, the server *SHOULD* add the `object` to the collection specified in the `target` property, unless:

* the `target` is not owned by the receiving server, and thus they are not authorized to update it.
* the `object` is not allowed to be added to the `target` collection for some other reason, at the receiving server's discretion.

#### 6.7 Remove Activity

Upon receipt of a `Remove` activity into the **outbox**, the server *SHOULD* remove the `object` from the collection specified in the `target` property, unless:

* the `target` is not owned by the receiving server, and thus they are not authorized to update it.
* the `object` is not allowed to be removed from the `target` collection for some other reason, at the receiving server's discretion.

#### 6.8 Like Activity

The `Like` activity indicates the `actor` likes the `object`.

The side effect of receiving this in an **outbox** is that the server *SHOULD* add the `object` to the `actor`'s [`liked` Collection](https://www.w3.org/TR/activitypub/#liked).

#### 6.9 Block Activity

The `Block` activity is used to indicate that the posting actor does not want another actor (defined in the `object` property) to be able to interact with objects posted by the actor posting the `Block` activity. The server *SHOULD* prevent the blocked user from interacting with any object posted by the actor.

Servers *SHOULD NOT* deliver Block Activities to their `object`.

#### 6.10 Undo Activity

The `Undo` activity is used to undo a previous activity. See the Activity Vocabulary documentation on [Inverse Activities and "Undo"](https://www.w3.org/TR/activitystreams-vocabulary/#inverse). For example, `Undo` may be used to undo a previous `Like`, `Follow`, or `Block`. The undo activity and the activity being undone *MUST* both have the same actor. Side effects should be undone, to the extent possible. For example, if undoing a `Like`, any counter that had been incremented previously should be decremented appropriately.

There are some exceptions where there is an existing and explicit "inverse activity" which should be used instead. `Create` based activities should instead use `Delete`, and `Add` activities should use `Remove`.

#### 6.11 Delivery

Federated servers *MUST* perform delivery on all Activities posted to the **outbox** according to [outbox delivery](https://www.w3.org/TR/activitypub/#outbox-delivery).

#### 6.12 Uploading Media

*This section is non-normative.*

Servers MAY support uploading document types to be referenced in activites, such as images, video or other binary data, but the precise mechanism is out of scope for this version of ActivityPub. The Social Web Community Group is refining the protocol in the [ActivityPub Media Upload report](https://www.w3.org/wiki/SocialCG/ActivityPub/MediaUpload).
