### 3. Objects

Objects are the core concept around which both [[ActivityStreams](https://www.w3.org/TR/activitypub/#bib-ActivityStreams)] and ActivityPub are built. Objects are often wrapped in Activities and are contained in streams of Collections, which are themselves subclasses of Objects. See the [[Activity-Vocabulary](https://www.w3.org/TR/activitypub/#bib-Activity-Vocabulary)] document, particularly the [Core Classes](https://www.w3.org/TR/activitystreams-vocabulary/#types); ActivityPub follows the mapping of this vocabulary very closely. 

ActivityPub defines some terms in addition to those provided by ActivityStreams. These terms are provided in the ActivityPub [JSON-LD context](http://www.w3.org/TR/json-ld/#the-context) at `https://www.w3.org/ns/activitystreams`.         Implementers *SHOULD* include the ActivityPub context in their object definitions. Implementers *MAY* include additional context as appropriate. 

ActivityPub shares the same [URI / IRI conventions as in ActivityStreams](https://www.w3.org/TR/activitystreams-core/#urls).

Servers *SHOULD* validate the content they receive to avoid content spoofing attacks. (A server should do something at least as robust as checking that the object appears as received at its origin, but mechanisms         such as checking signatures would be better if available). No particular mechanism for verification is authoritatively specified by this document, but please see [Security Considerations](https://www.w3.org/TR/activitypub/#security-considerations) for some suggestions and good practices.

As an example, if example.com receives the activity

```json
{
  "@context": "https://www.w3.org/ns/activitystreams",
  "type": "Like",
  "actor": "https://example.net/~mallory",
  "to": ["https://hatchat.example/sarah/",
         "https://example.com/peeps/john/"],
  "object": {
    "@context": {"@language": "en"},
    "id": "https://example.org/~alice/note/23",
    "type": "Note",
    "attributedTo": "https://example.org/~alice",
    "content": "I'm a goat"
  }
}
```

it should dereference the `id` both to ensure that it exists and is a valid object, and that it is not misrepresenting the object. (In this example, Mallory could be spoofing an object allegedly posted
by Alice).



#### 3.1 Object Identifiers

 All Objects in [ActivityStreams] should have unique global identifiers. ActivityPub extends this requirement; all objects distributed by the ActivityPub protocol MUST have unique global identifiers, unless they are intentionally transient (short lived activities that are not intended to be able to be looked up, such as some kinds of chat messages or game notifications). These identifiers must fall into one of the following groups:

1. Publicly dereferencable URIs, such as HTTPS URIs, with their authority belonging to that of their originating server. (Publicly facing content SHOULD use HTTPS URIs).
2. An ID explicitly specified as the JSON null object, which implies an anonymous object (a part of its parent context)

Identifiers MUST be provided for activities posted in server to server communication, unless the activity is intentionally transient. However, for client to server communication, a server receiving an object posted to the outbox with no specified id SHOULD allocate an object ID in the actor's namespace and attach it to the posted object.

All objects have the following properties:

**id**
    The object's unique global identifier (unless the object is transient, in which case the id MAY be omitted). 
**type**
    The type of the object. 

#### 3.2 Retrieving objects

The HTTP GET method may be dereferenced against an object's id property to retrieve the activity. Servers MAY use HTTP content negotiation as defined in [RFC7231](https://www.w3.org/TR/activitypub/#bib-RFC7231) to select the type of data to return in response to a request, but MUST present the ActivityStreams object representation in response to `application/ld+json; profile="https://www.w3.org/ns/activitystreams"`, and SHOULD also present the ActivityStreams representation in response to `application/activity+json` as well. The client MUST specify an Accept header with the `application/ld+json; profile="https://www.w3.org/ns/activitystreams"` media type in order to retrieve the activity.

Servers *MAY* implement other behavior for requests which do not comply with the above requirement. (For example, servers may implement additional legacy protocols, or may use the same URI for both HTML and ActivityStreams representations of a resource).

Servers MAY require authorization as specified in B.1 [Authentication and Authorization](https://www.w3.org/TR/activitypub/#authorization), and may additionally implement their own authorization rules. Servers SHOULD fail requests which do not pass their authorization checks with the appropriate HTTP error code, or the 403 Forbidden error code where the existence of the object is considered private. An origin server which does not wish to disclose the existence of a private target MAY instead respond with a status code of 404 Not Found. 

#### 3.3 The source property

 In addition to all the properties defined by the [Activity-Vocabulary](https://www.w3.org/TR/activitypub/#bib-Activity-Vocabulary), ActivityPub extends the `Object` by supplying the `source` property. The `source` property is intended to convey some sort of source from which the `content` markup was derived, as a form of provenance, or to support future editing by clients. In general, clients do the conversion from `source` to `content`, not the other way around.

The value of `source` is itself an object which uses its own `content` and `mediaType` fields to supply source information. 

```json
{
  "@context": ["https://www.w3.org/ns/activitystreams",
               {"@language": "en"}],
  "type": "Note",
  "id": "http://postparty.example/p/2415",
  "content": "<p>I <em>really</em> like strawberries!</p>",
  "source": {
    "content": "I *really* like strawberries!",
    "mediaType": "text/markdown"}
}
```

	Note: What to do when clients can't meaningfully handle a mediaType?
	
	In general, it's best to let a user edit their original post in the same source format they originally composed it in. But not all clients can reliably provide a nice interface for all source types, and since clients are expected to do the conversion from source to content, some clients may work with a media type that another client does not know how to work with. While a client could feasibly provide the content markup to be edited and ignore the source, this means that the user will lose the more desirable form of the original source in any future revisions. A client doing so should thus provide a minimally obtrusive warning cautioning that the original source format is not understood and is thus being ignored.
	
	For example, Alyssa P. Hacker likes to post to her ActivityPub powered blog via an Emacs client she has written, leveraging Org mode. Later she switches to editing on her phone's client, which has no idea what text/x-org is or how to render it to HTML, so it provides a text box to edit the original content instead. A helpful warning displays above the edit area saying, "This was originally written in another markup language we don't know how to handle. If you edit, you'll lose your original source!" Alyssa decides the small typo fix isn't worth losing her nice org-mode markup and decides to make the update when she gets home.
