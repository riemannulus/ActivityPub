### B. Security Considerations

*This section is non-normative.*

#### B.1 Authentication and Authorization

ActivityPub uses authentication for two purposes; first, to authenticate clients to servers, and secondly in federated implementations to authenticate servers to each other.

Unfortunately at the time of standardization, there are no strongly agreed upon mechanisms for authentication. Some possible directions for authentication are laid out [in the Social Web Community Group Authentication and Authorization best practices report](https://www.w3.org/wiki/SocialCG/ActivityPub/Authentication_Authorization).

#### B.2 Verification

Servers should not trust client submitted content, and federated servers also should not trust content received from a server other than the content's origin without some form of verification.

Servers should be careful to verify that new content is really posted by the actor that claims to be posting it, and that the actor has permission to update the resources it claims to. See also [3. Objects](https://www.w3.org/TR/activitypub/#obj) and [B.1 Authentication and Authorization](https://www.w3.org/TR/activitypub/#authorization).

#### B.3 Accessing localhost URIs

It is often convenient while developing to test against a process running on localhost. However, permitting requests to localhost in a production client or server instance can be dangerous. Making requests to URIs on localhost which do not require authorization may unintentionally access or modify resources assumed to be protected to be usable by localhost-only.

If your ActivityPub server or client does permit making requests to localhost URIs for development purposes, consider making it a configuration option which defaults to off.

#### B.4 URI Schemes

There are many types of URIs aside from just `http` and `https`. Some libraries which handle fetching requests at various URI schemes may try to be smart and reference schemes which may be undesirable, such as `file`. Client and server authors should carefully check how their libraries handle requests, and potentially whitelist only certain safe URI types, such as `http` and `https`.

#### B.5 Recursive Objects

Servers should set a limit on how deep to recurse while resolving objects, or otherwise specially handle ActivityStreams objects with recursive references. Failure to properly do so may result in denial-of-service security vulnerabilities.

#### B.6 Spam

Spam is a problem in any network, perhaps especially so in federated networks. While no specific mechanism for combating spam is provided in ActivityPub, it is recommended that servers filter incoming content both by local untrusted users and any remote users through some sort of spam filter.

#### B.7 Federation denial-of-service

Servers should implement protections against denial-of-service attacks from other, federated servers. This can be done using, for example, some kind of ratelimiting mechanism. Servers should be especially careful to implement this protection around activities that involve side effects. Servers *SHOULD* also take care not to overload servers with submissions, for example by using an exponential backoff strategy.

#### B.8 Client-to-server ratelimiting

Servers should ratelimit API client submissions. This serves two purposes:

1. It prevents malicious clients from conducting denial-of-service attacks on the server.
2. It ensures that the server will not distribute so many activities that it triggers another server's [denial-of-service protections](https://www.w3.org/TR/activitypub/#security-federation-dos).

#### B.9 Client-to-server response denial-of-service

In order to prevent a client from being overloaded by oversized Collections, servers should take care to limit the size of Collection pages they return to clients. Clients should still be prepared to limit the size of responses they are willing to handle in case they connect to malicious or compromised servers, for example by timing out and generating an error.

#### B.10 Sanitizing Content

Any activity field being rendered for browsers (or other rich text enabled applications) should take care to sanitize fields containing markup to prevent cross site scripting attacks.

#### B.11 Not displaying bto and bcc properties

`bto` and `bcc` already [must be removed for delivery](https://www.w3.org/TR/activitypub/#remove-bto-bcc-before-delivery), but servers are free to decide how to represent the object in their own storage systems. However, since `bto` and `bcc` are only intended to be known/seen by the original author of the object/activity, servers should omit these properties during display as well.
