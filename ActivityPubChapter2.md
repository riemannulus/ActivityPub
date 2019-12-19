
### 2. Conformance

As well as sections marked as non-normative, all authoring guidelines, diagrams, examples,   and notes in this specification are non-normative. Everything else in this specification is   normative. 

The key words  *MAY*, *MUST*, *MUST NOT*, *SHOULD*, and *SHOULD NOT* are    to be interpreted as described in [[RFC2119](https://www.w3.org/TR/activitypub/#bib-RFC2119)]. 

#### 2.1 Specification Profiles

This specification defines two closely related and interacting protocols:         

- A client to server protocol, or "Social API"

  - This protocol permits a client to act *on behalf* of a user.  For example, this protocol is used by a mobile phone application to interact with a social stream of the user's actor.           

- A server to server protocol, or "Federation Protocol"

  - This protocol is used to distribute activities between actors on different servers, tying them into the same social graph.

The ActivityPub specification is designed so that once either of these protocols are implemented, supporting the other is of very little additional effort. However, servers may still implement one without the other. This gives three conformance classes:         

- ActivityPub conformant Client
- This designation applies to any implementation of the entirety of the client portion of the client to server protocol.
- ActivityPub conformant Server
- This designation applies to any implementation of the entirety of the server portion of the client to server protocol.           
- ActivityPub conformant Federated Server
- This designation applies to any implementation of the entirety of the federation protocols.           

It is called out whenever a portion of the specification only applies to implementation of the federation protocol. In addition, whenever requirements are specified, it is called out whether they apply to the client or server (for the client-to-server protocol) or whether referring to a sending or receiving server in the server-to-server protocol.

