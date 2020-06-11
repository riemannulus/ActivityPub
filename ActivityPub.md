## [ActivityPub](https://www.w3.org/TR/activitypub)
<abbr title="World Wide Web Consortium">W3C</abbr> Recommendation 23 January 2018

**This version:**
* https://www.w3.org/TR/2018/REC-activitypub-20180123/

**Latest published version:**
* https://www.w3.org/TR/activitypub/

**Latest editor's draft:**
* https://w3c.github.io/activitypub/

**Test suite:**
* https://test.activitypub.rocks/

**Implementation report:**
* https://activitypub.rocks/implementation-report

**Previous version:**
* https://www.w3.org/TR/2017/PR-activitypub-20171205/

**Editors:**
* [Christopher Lemmer Webber](https://dustycloud.org/)
* [Jessica Tallon](https://tsyesika.se/)

**Authors:**
* [Christopher Lemmer Webber](https://dustycloud.org/)
* [Jessica Tallon](https://tsyesika.se/)
* [Erin Shepherd](http://erinshepherd.net/)
* [Amy Guy](https://rhiaro.co.uk/)
* [Evan Prodromou](https://en.wikipedia.org/wiki/Evan_Prodromou)

**Repository:**
* [Git repository](https://github.com/w3c/activitypub)
* [Issues](https://github.com/w3c/activitypub/issues)
* [Commits](https://github.com/w3c/activitypub/commits/gh-pages)

Please check the [errata](https://www.w3.org/wiki/ActivityPub_errata) for any errors or issues reported since publication.

See also [translations](http://www.w3.org/2003/03/Translations/byTechnology?technology=activitypub).

[Copyright](https://www.w3.org/Consortium/Legal/ipr-notice#Copyright) © 2018 [<abbr title="World Wide Web Consortium">W3C</abbr>](https://www.w3.org/)® (<abbr title="Massachusetts Institute of Technology">[MIT](https://www.csail.mit.edu/)</abbr>, <abbr title="European Research Consortium for Informatics and Mathematics">[ERCIM](https://www.ercim.eu/)</abbr>, [Keio](https://www.keio.ac.jp/), [Beihang](http://ev.buaa.edu.cn/)). <abbr title="World Wide Web Consortium">W3C</abbr> [liability](https://www.w3.org/Consortium/Legal/ipr-notice#Legal_Disclaimer), [trademark](https://www.w3.org/Consortium/Legal/ipr-notice#W3C_Trademarks) and p[ermissive document license](https://www.w3.org/Consortium/Legal/2015/copyright-software-and-document) rules apply.

#### Abstract

The ActivityPub protocol is a decentralized social networking protocol based upon the [[ActivityStreams](https://www.w3.org/TR/activitypub/#bib-ActivityStreams)] 2.0 data format. It provides a client to server API for creating, updating and deleting content, as well as a federated server to server API for delivering notifications and content.

#### Status of This Document

This section describes the status of this document at the time of its publication. Other documents may supersede this document. A list of current <abbr title="World Wide Web Consortium">W3C</abbr> publications and the latest revision of this technical report can be found in the [<abbr title="World Wide Web Consortium">W3C</abbr> technical reports index](https://www.w3.org/TR/) at https://www.w3.org/TR/.

This document was published by the Social Web Working Group as a Recommendation.

All interested parties are invited to provide implementation and bug reports and other comments through the Working Group's [Issue tracker](https://github.com/w3c/activitypub/issues). These will be discussed by the [Social Web Community Group](http://www.w3.org/wiki/SocialCG) and considered in any future versions of this specification.

Please see the Working Group's [implementation report](https://activitypub.rocks/implementation-report).

This document has been reviewed by <abbr title="World Wide Web Consortium">W3C</abbr> Members, by software developers, and by other <abbr title="World Wide Web Consortium">W3C</abbr> groups and interested parties, and is endorsed by the Director as a <abbr title="World Wide Web Consortium">W3C</abbr> Recommendation. It is a stable document and may be used as reference material or cited from another document. <abbr title="World Wide Web Consortium">W3C</abbr>'s role in making the Recommendation is to draw attention to the specification and to promote its widespread deployment. This enhances the functionality and interoperability of the Web.

This document was produced by a group operating under the [<abbr title="World Wide Web Consortium">W3C</abbr> Patent Policy](https://www.w3.org/Consortium/Patent-Policy/). <abbr title="World Wide Web Consortium">W3C</abbr> maintains a [public list of any patent disclosures](https://www.w3.org/2004/01/pp-impl/72531/status) made in connection with the deliverables of the group; that page also includes instructions for disclosing a patent. An individual who has actual knowledge of a patent which the individual believes contains [Essential Claim(s)](https://www.w3.org/Consortium/Patent-Policy/#def-essential) must disclose the information in accordance with [section 6 of the <abbr title="World Wide Web Consortium">W3C</abbr> Patent Policy](https://www.w3.org/Consortium/Patent-Policy/#sec-Disclosure).

This document is governed by the [1 March 2017 <abbr title="World Wide Web Consortium">W3C</abbr> Process Document](https://www.w3.org/2017/Process-20170301/).

### Table of Contents

```
1. Overview
    1.1 Social Web Working Group
2. Conformance
    2.1 Specification Profiles
3. Objects
    3.1 Object Identifiers
    3.2 Retrieving objects
    3.3 The source property
4. Actors
    4.1 Actor objects
5. Collections
    5.1 Outbox
    5.2 Inbox
    5.3 Followers Collection
    5.4 Following Collection
    5.5 Liked Collection
    5.6 Public Addressing
    5.7 Likes Collection
    5.8 Shares Collection
6. Client to Server Interactions
    6.1 Client Addressing
    6.2 Create Activity
        6.2.1 Object creation without a Create Activity
    6.3 Update Activity
        6.3.1 Partial Updates
    6.4 Delete Activity
    6.5 Follow Activity
    6.6 Add Activity
    6.7 Remove Activity
    6.8 Like Activity
    6.9 Block Activity
    6.10 Undo Activity
    6.11 Delivery
    6.12 Uploading Media
7. Server to Server Interactions
    7.1 Delivery
        7.1.1 Outbox Delivery Requirements for Server to Server
        7.1.2 Forwarding from Inbox
        7.1.3 Shared Inbox Delivery
    7.2 Create Activity
    7.3 Update Activity
    7.4 Delete Activity
    7.5 Follow Activity
    7.6 Accept Activity
    7.7 Reject Activity
    7.8 Add Activity
    7.9 Remove Activity
    7.10 Like Activity
    7.11 Announce Activity (sharing)
    7.12 Undo Activity
A. Internationalization
B. Security Considerations
    B.1 Authentication and Authorization
    B.2 Verification
    B.3 Accessing localhost URIs
    B.4 URI Schemes
    B.5 Recursive Objects
    B.6 Spam
    B.7 Federation denial-of-service
    B.8 Client-to-server ratelimiting
    B.9 Client-to-server response denial-of-service
    B.10 Sanitizing Content
    B.11 Not displaying bto and bcc properties
C. Acknowledgements
D. References
    D.1 Normative references
    D.2 Informative references
```