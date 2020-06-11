### 7. Server to Server Interactions

Servers communicate with other servers and propagate information across the social graph by posting activities to actors' [inbox](https://www.w3.org/TR/activitypub/#inbox) endpoints. An Activity sent over the network *SHOULD* have an `id`, unless it is intended to be transient (in which case it *MAY* omit the `id`).

`POST` requests (eg. to the inbox) MUST be made with a Content-Type of `application/ld+json; profile="https://www.w3.org/ns/activitystreams"` and `GET` requests (see also [3.2 Retrieving objects](https://www.w3.org/TR/activitypub/#retrieving-objects)) with an Accept header of `application/ld+json; profile="https://www.w3.org/ns/activitystreams"`. Servers *SHOULD* interpret a Content-Type or Accept header of `application/activity+json as equivalent to application/ld+json; profile="https://www.w3.org/ns/activitystreams"` for server-to-server interactions.

In order to propagate updates throughout the social graph, Activities are sent to the appropriate recipients. First, these recipients are determined through following the appropriate links between objects until you reach an actor, and then the Activity is inserted into the actor's *inbox* ([delivery](https://www.w3.org/TR/activitypub/#delivery)). This allows recipient servers to:

* conduct any side effects related to the Activity (for example, notification that an actor has liked an object is used to update the object's like count);
* deliver the Activity to recipients of the original object, to ensure updates are propagated to the whole social graph (see [inbox delivery](https://www.w3.org/TR/activitypub/#inbox-forwarding)).

Delivery is usually triggered by, for example:

* an Activity being created in an actor's [outbox](https://www.w3.org/TR/activitypub/#outbox) with their [Followers Collection](https://www.w3.org/TR/activitypub/#followers) as the recipient.
* an Activity being created in an actor's [outbox](https://www.w3.org/TR/activitypub/#outbox) with directly addressed recipients.
* an Activity being created in an actors's [outbox](https://www.w3.org/TR/activitypub/#outbox) with user-curated collections as recipients.
* an Activity being created in an actor's [outbox](https://www.w3.org/TR/activitypub/#outbox) or [inbox](https://www.w3.org/TR/activitypub/#inbox) which references another object.

Servers performing delivery to the `inbox` or `sharedInbox` properties of actors on other servers *MUST* provide the `object` property in the activity: `Create`, `Update`, `Delete`, `Follow`, `Add`, `Remove`, `Like`, `Block`, `Undo`. Additionally, servers performing server to server delivery of the following activities *MUST* also provide the target property: `Add`, `Remove`.

HTTP caching mechanisms [[RFC7234](https://www.w3.org/TR/activitypub/#bib-RFC7234)] SHOULD be respected when appropriate, both when receiving responses from other servers as well as sending responses to other servers.

#### 7.1 Delivery

The following is required by federated servers communicating with other federated servers only.

An activity is delivered to its targets (which are [actors](https://www.w3.org/TR/activitypub/#actors)) by first looking up the targets' inboxes and then posting the activity to those inboxes. Targets for delivery are determined by checking the [ActivityStreams audience targeting](https://www.w3.org/TR/activitystreams-vocabulary/#audienceTargeting); namely, the `to`, `bto`, `cc`, `bcc`, and `audience` fields of the activity.

The [inbox](https://www.w3.org/TR/activitypub/#inbox) is determined by first [retrieving the target actor's JSON-LD representation](https://www.w3.org/TR/activitypub/#retrieving-objects) and then looking up the `inbox` property. If a recipient is a `Collection` or `OrderedCollection`, then the server *MUST* dereference the collection (with the user's credentials) and discover inboxes for each item in the collection. Servers *MUST* limit the number of layers of indirections through collections which will be performed, which *MAY* be one.

Servers *MUST* de-duplicate the final recipient list. Servers *MUST* also exclude actors from the list which are the same as the `actor` of the Activity being notified about. That is, actors shouldn't have their own activities delivered to themselves.

>Note: Silent and private activities
>
>What to do when there are no recipients specified is not defined, however it's recommended that if no recipients are specified the object remains completely private and access controls restrict the access to object. If the object is just sent to the "public" collection the object is not delivered to any actors but is publicly viewable in the actor's outbox.

An HTTP POST request (with authorization of the submitting user) is then made to the [inbox](https://www.w3.org/TR/activitypub/#inbox), with the Activity as the body of the request. This Activity is added by the receiver as an `item` in the [inbox](https://www.w3.org/TR/activitypub/#inbox) OrderedCollection. Attempts to deliver to an inbox on a non-federated server *SHOULD* result in a `405 Method Not Allowed` response.

For federated servers performing delivery to a third party server, delivery *SHOULD* be performed asynchronously, and *SHOULD* additionally retry delivery to recipients if it fails due to network error.

**Note**: Activities being distributed between actors on the same origin may use any internal mechanism, and are not required to use HTTP.

>Note: Relationship to Linked Data Notifications
>
>While it is not required reading to understand this specification, it is worth noting that ActivityPub's targeting and delivery mechanism overlaps with the [Linked Data Notifications](https://www.w3.org/TR/ldn/) specification, and the two specifications may interoperably combined. In particular, the `inbox` property is the same between ActivityPub and Linked Data Notifications, and the targeting and delivery systems described in this document are supported by Linked Data Notifications. In addition to JSON-LD compacted ActivityStreams documents, Linked Data Notifications also supports a number of RDF serializations which are not required for ActivityPub implementations. However, ActivityPub implementations which wish to be more broadly compatible with Linked Data Notifications implementations may wish to support other RDF representations.

##### 7.1.1 Outbox Delivery Requirements for Server to Server

When objects are received in the [outbox](https://www.w3.org/TR/activitypub/#outbox) (for servers which support both [Client to Server interactions](https://www.w3.org/TR/activitypub/#client-to-server-interactions) and [Server to Server Interactions](https://www.w3.org/TR/activitypub/#server-to-server-interactions)), the server *MUST* target and deliver to:

* The `to`, `bto`, `cc`, `bcc` or `audience` fields if their values are individuals or Collections owned by the actor.

These fields will have been [populated appropriately by the client](https://www.w3.org/TR/activitypub/#client-addressing) which posted the Activity to the outbox.

##### 7.1.2 Forwarding from Inbox

>Note: Forwarding to avoid the ghost replies problem
>
>The following section is to mitigate the "ghost replies" problem which occasionally causes problems on federated networks. This problem is best demonstrated with an example.
>
>Alyssa makes a post about her having successfully presented a paper at a conference and sends it to her followers collection, which includes her friend Ben. Ben replies to Alyssa's message congratulating her and includes her followers collection on the recipients. However, Ben has no access to see the members of Alyssa's followers collection, so his server does not forward his messages to their inbox. Without the following mechanism, if Alyssa were then to reply to Ben, her followers would see Alyssa replying to Ben without having ever seen Ben interacting. This would be very confusing!

When Activities are received in the [inbox](https://www.w3.org/TR/activitypub/#inbox), the server needs to forward these to recipients that the origin was unable to deliver them to. To do this, the server *MUST* target and [deliver](https://www.w3.org/TR/activitypub/#delivery) to the values of `to`, `cc`, and/or `audience` if and only if all of the following are true:

* This is the first time the server has seen this Activity.
* The values of `to`, `cc`, and/or `audience` contain a Collection owned by the server.
* The values of `inReplyTo`, `object`, `target` and/or `tag` are objects owned by the server. The server *SHOULD* recurse through these values to look for linked objects owned by the server, and *SHOULD* set a maximum limit for recursion (ie. the point at which the thread is so deep the recipients followers may not mind if they are no longer getting updates that don't directly involve the recipient). The server *MUST* only target the values of `to`, `cc`, and/or `audience` on the original object being forwarded, and not pick up any new addressees whilst recursing through the linked objects (in case these addressees were purposefully amended by or via the client).

The server *MAY* filter its delivery targets according to implementation-specific rules (for example, spam filtering).

##### 7.1.3 Shared Inbox Delivery

For servers hosting many actors, delivery to all followers can result in an overwhelming number of messages sent. Some servers would also like to display a list of all messages posted publicly to the "known network". Thus ActivityPub provides an optional mechanism for serving these two use cases.

When an object is being delivered to the originating actor's followers, a server MAY reduce the number of receiving actors delivered to by identifying all followers which share the same [sharedInbox](https://www.w3.org/TR/activitypub/#sharedInbox) who would otherwise be individual recipients and instead deliver objects to said `sharedInbox`. Thus in this scenario, the remote/receiving server participates in determining targeting and performing delivery to specific inboxes.

Additionally, if an object is addressed to the [Public](https://www.w3.org/TR/activitypub/#public-addressing) special collection, a server *MAY* deliver that object to all known `sharedInbox` endpoints on the network.

Origin servers sending publicly addressed activities to `sharedInbox` endpoints MUST still deliver to actors and collections otherwise addressed (through `to`, `bto`, `cc`, `bcc`, and `audience`) which do not have a `sharedInbox` and would not otherwise receive the activity through the `sharedInbox` mechanism.

#### 7.2 Create Activity

Receiving a `Create` activity in an `inbox` has surprisingly few side effects; the activity should appear in the actor's `inbox` and it is likely that the server will want to locally store a representation of this activity and its accompanying object. However, this mostly happens in general with processing activities delivered to an `inbox` anyway.

#### 7.3 Update Activity

For server to server interactions, an `Update` activity means that the receiving server SHOULD update its copy of the `object` of the same `id` to the copy supplied in the `Update` activity. Unlike the [client to server handling of the Update activity](https://www.w3.org/TR/activitypub/#update-activity-outbox), this is not a partial update but a complete replacement of the object.

The receiving server *MUST* take care to be sure that the `Update` is authorized to modify its `object`. At minimum, this may be done by ensuring that the `Update` and its `object` are of same origin.

#### 7.4 Delete Activity

The side effect of receiving this is that (assuming the `object` is owned by the sending actor / server) the server receiving the delete activity *SHOULD* remove its representation of the `object` with the same `id`, and MAY replace that representation with a `Tombstone` object.

(Note that after an activity has been transmitted from an origin server to a remote server, there is nothing in the ActivityPub protocol that can enforce remote deletion of an object's representation).

#### 7.5 Follow Activity

The side effect of receiving this in an **inbox** is that the server *SHOULD* generate either an `Accept` or `Reject` activity with the Follow as the `object` and deliver it to the `actor` of the Follow. The `Accept` or `Reject` *MAY* be generated automatically, or *MAY* be the result of user input (possibly after some delay in which the user reviews). Servers *MAY* choose to not explicitly send a `Reject` in response to a `Follow`, though implementors ought to be aware that the server sending the request could be left in an intermediate state. For example, a server might not send a `Reject` to protect a user's privacy.

In the case of receiving an `Accept` referencing this `Follow` as the object, the server *SHOULD* add the `actor` to the object actor's [Followers Collection](https://www.w3.org/TR/activitypub/#followers). In the case of a `Reject`, the server *MUST NOT* add the actor to the object actor's [Followers Collection](https://www.w3.org/TR/activitypub/#followers).

>Note
>
>Sometimes a successful `Follow` subscription may occur but at some future point delivery to the follower fails for an extended period of time. Implementations should be aware that there is no guarantee that actors on the network will remain reachable and should implement accordingly. For instance, if attempting to deliver to an actor for perhaps six months while the follower remains unreachable, it is reasonable that the delivering server remove the subscriber from the `followers` list. Timeframes and behavior for dealing with unreachable actors are left to the discretion of the delivering server.

#### 7.6 Accept Activity

The side effect of receiving this in an **inbox** is determined by the type of the `object` received, and it is possible to accept types not described in this document (for example, an `Offer`).

If the `object` of an `Accept` received to an **inbox** is a `Follow` activity previously sent by the receiver, the server *SHOULD* add the `actor` to the receiver's [Following Collection](https://www.w3.org/TR/activitypub/#following).

#### 7.7 Reject Activity

The side effect of receiving this in an **inbox** is determined by the type of the `object` received, and it is possible to reject types not described in this document (for example, an `Offer`).

If the `object` of a `Reject` received to an **inbox** is a `Follow` activity previously sent by the receiver, this means the recipient did not approve the `Follow` request. The server *MUST NOT* add the `actor` to the receiver's [Following Collection](https://www.w3.org/TR/activitypub/#following).

#### 7.8 Add Activity

Upon receipt of an `Add` activity into the **inbox**, the server *SHOULD* add the `object` to the collection specified in the `target` property, unless:

* the `target` is not owned by the receiving server, and thus they can't update it.
* the `object` is not allowed to be added to the `target` collection for some other reason, at the receiver's discretion.

#### 7.9 Remove Activity

Upon receipt of a `Remove` activity into the **inbox**, the server *SHOULD* remove the `object` from the collection specified in the `target` property, unless:

* the `target` is not owned by the receiving server, and thus they can't update it.
* the `object` is not allowed to be removed to the `target` collection for some other reason, at the receiver's discretion.

#### 7.10 Like Activity

The side effect of receiving this in an **inbox** is that the server SHOULD increment the object's count of likes by adding the received activity to the [`likes`](https://www.w3.org/TR/activitypub/#likes) collection if this collection is present.

#### 7.11 Announce Activity (sharing)

Upon receipt of an `Announce` activity in an **inbox**, a server *SHOULD* increment the object's count of shares by adding the received activity to the [`shares`](https://www.w3.org/TR/activitypub/#shares) collection if this collection is present.

>Note
>
>The `Announce` activity is effectively what is known as "sharing", "reposting", or "boosting" in other social networks.

#### 7.12 Undo Activity

The `Undo` activity is used to undo the side effects of previous activities. See the ActivityStreams documentation on [Inverse Activities and "Undo"](https://www.w3.org/TR/activitystreams-vocabulary/#inverse). The scope and restrictions of the `Undo` activity are the same as for [the Undo activity in the context of client to server interactions](https://www.w3.org/TR/activitypub/#undo-activity-outbox), but applied to a federated context.
