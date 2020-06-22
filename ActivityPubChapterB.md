[목차로 돌아가기](ActivityPubContents.md)

## B. 보안 고려사항 (Security Considerations)

*이 항목은 비표준입니다.*

~~*This section is non-normative.*~~

### B.1 인증 및 권한 (Authentication and Authorization)

액티비티펍은 두가지 목적으로 인증을 사용합니다; 첫번째로는 클라이언트를 서버에 인증하기 위해서이고, 두번째로는 연합 서버에서 서버를 서로 인증하는데 사용하기 위해서입니다.

~~ActivityPub uses authentication for two purposes; first, to authenticate clients to servers, and secondly in federated implementations to authenticate servers to each other.~~

불행하게도 표준화 당시에는 인증 메커니즘에 대한 강력하게 합의된 바가 없습니다. 인증에 대한 몇 가지 가능한 지침은 [소셜 웹 커뮤니티 그롭 인증 및 권한 부여 우수 사례 보고서](https://www.w3.org/wiki/SocialCG/ActivityPub/Authentication_Authorization)에 나와있습니다.

~~Unfortunately at the time of standardization, there are no strongly agreed upon mechanisms for authentication. Some possible directions for authentication are laid out [in the Social Web Community Group Authentication and Authorization best practices report](https://www.w3.org/wiki/SocialCG/ActivityPub/Authentication_Authorization).~~

### B.2 검증 (Verification)

서버는 클라이언트가 제출한 컨텐츠를 신뢰하여서는 아니되며, 연합 서버는 어떤 형태의 검증없이도 컨텐츠의 출저가 아닌 서버로부터 수신된 컨텐츠를 신뢰하여서는 안됩니다.

~~Servers should not trust client submitted content, and federated servers also should not trust content received from a server other than the content's origin without some form of verification.~~

서버는 새로운 컨텐츠가 실제로 컨텐츠를 게시하였다고 주장하는 액터에 의하여 게시되었는지, 그리고 액터가 요구하는 자원을 업데이트할 권한이 있는지 확인하여야 합니다. [3. 객체](https://www.w3.org/TR/activitypub/#obj) 및 [B.1 인증 및 권한부여](https://www.w3.org/TR/activitypub/#authorization)를 참고하시길 바랍니다.

~~Servers should be careful to verify that new content is really posted by the actor that claims to be posting it, and that the actor has permission to update the resources it claims to. See also [3. Objects](https://www.w3.org/TR/activitypub/#obj) and [B.1 Authentication and Authorization](https://www.w3.org/TR/activitypub/#authorization).~~

### B.3 로컬호스트 URI를 접근하기 (Accessing localhost URIs)

개발하는 동안 localhost에서 실행되는 프로세스에 테스트를 실행하는 것은 상당히 편리합니다. 그러나 프로덕션 클라이언트 또는 서버 인스턴스에서 로컬 호스트에 대한 요청을 허용하면 위험 할 수도 있습니다. 권한이 필요없는 localhost의 URI에 요청하면 localhost-전용으로 사용 가능한 것으로 간주되는 자원에 실수로 접근하거나 수정 할 수도 있습니다.

~~It is often convenient while developing to test against a process running on localhost. However, permitting requests to localhost in a production client or server instance can be dangerous. Making requests to URIs on localhost which do not require authorization may unintentionally access or modify resources assumed to be protected to be usable by localhost-only.~~

ActivityPub 서버 또는 클라이언트가 개발 목적으로 localhost URI에 대한 요청을 허용하는 경우, 기본값을 off로 설정하는 구성 설정을 고려하시길 바랍니다.

~~If your ActivityPub server or client does permit making requests to localhost URIs for development purposes, consider making it a configuration option which defaults to off.~~

### B.4 URI 스킴 (URI Schemes)

`http` 및`https` 외에도 많은 유형의 URI가 있습니다. 다양한 URI 스킴에서 fetch 요청을 처리하는 일부 라이브러리는 영리하게 처리하려고 하지만, `file`과 같이 바람직하지 않은 스킴에 참조를 시도 할 수도 있습니다. 클라이언트 및 서버 작성자는 라이브러리가 요청을 처리하는 방법을주의 깊게 확인하고 잠재적으로`http` 및`https`와 같은 특정하고 안전한 URI 유형들만 허용 목록(whitelist)에 추가하는것을 권장합니다.

~~There are many types of URIs aside from just `http` and `https`. Some libraries which handle fetching requests at various URI schemes may try to be smart and reference schemes which may be undesirable, such as `file`. Client and server authors should carefully check how their libraries handle requests, and potentially whitelist only certain safe URI types, such as `http` and `https`.~~

### B.5 재귀적 객체 (Recursive Objects)

서버는 객체를 해결하는 동안 얼마나 깊게 재귀를 하는지 제한하거나 재귀 참조를 사용하여 액티비티스트림 객체를 특별하게 처리해야합니다. 그렇지 않으면 서비스 거부(denial-of-service) 보안 취약점이 발생할 수 있습니다.

~~Servers should set a limit on how deep to recurse while resolving objects, or otherwise specially handle ActivityStreams objects with recursive references. Failure to properly do so may result in denial-of-service security vulnerabilities.~~

### B.6 스팸 (Spam)

스팸(spam)은 모든 네트워크, 특히 연합 네트워크에서 문제가 됩니다. 액티비티펍에서는 스팸 방지를 위한 특정한 메커니즘을 제공하지 않지만, 서버는 일종의 스팸 필터를 통하여 신뢰할 수 없는 로컬 사용자와 모든 원격 사용자가 들어오는 콘텐츠를 필터링하는 것이 권장됩니다.

~~Spam is a problem in any network, perhaps especially so in federated networks. While no specific mechanism for combating spam is provided in ActivityPub, it is recommended that servers filter incoming content both by local untrusted users and any remote users through some sort of spam filter.~~

### B.7 연합 서비스 거부 (Federation denial-of-service)

서버는 다른 연합 서버의 서비스 거부(denial-of-service) 공격에 대한 보호 기능을 구현해야합니다. 이는 속도 제한같은 메커니즘을 사용하여 보호 할 수 있습니다. 서버는 부작용과 관련된 활동에 대해 이 보호 기능을 구현할 때 특히 주의해야합니다. 서버는 또한 지수 백오프(exponential backoff) 방식을 사용하는 등 과도한 제출로 인하여 서버에 과부하가 걸리지 않도록 주의하는 것을 *권장합니다*.

~~Servers should implement protections against denial-of-service attacks from other, federated servers. This can be done using, for example, some kind of ratelimiting mechanism. Servers should be especially careful to implement this protection around activities that involve side effects. Servers *SHOULD* also take care not to overload servers with submissions, for example by using an exponential backoff strategy.~~

[//Comment]: # "'exponential backoff'는 AWS 및 GCP에서도 '지수 백오프'라고 표기하였으므로 해당 번역으로 번역하였습니다."
[//Comment]: # "출저: [AWS의 오류 재시도 횟수 및 지수 백오프](https://docs.aws.amazon.com/ko_kr/general/latest/gr/api-retries.html)"
[//Comment]: # "출저: [잘린 지수 백오프](https://cloud.google.com/storage/docs/exponential-backoff?hl=ko)"

### B.8 클라이언트-서버 간 속도 제한 (Client-to-server ratelimiting)

서버는 API 클라이언트가 제출하는 것에 대해 속도 제한(ratelimit)을 하는 것을 권장합니다. 이는 두 가지 목적으로 사용됩니다:

~~Servers should ratelimit API client submissions. This serves two purposes:~~

1. 악의적인 클라이언트가 서버에서 서비스 거부(denial-of-service) 공격을 수행하지 못하게 합니다.
2. 서버가 다른 서버의 [서비스 거부 보호](https://www.w3.org/TR/activitypub/#security-federation-dos)를 발동시킬 정도로 많은 액티비티들을 분배하지 않도록합니다.

~~1. It prevents malicious clients from conducting denial-of-service attacks on the server.~~

~~2. It ensures that the server will not distribute so many activities that it triggers another server's [denial-of-service protections](https://www.w3.org/TR/activitypub/#security-federation-dos).~~

### B.9 클라이언트-서버 간 서비스 거부 응답 (Client-to-server response denial-of-service)

대형 컬렉션으로 인해 클라이언트가 과부하되지 않도록 하려면 서버는 클라이언트로 반환되는 컬렉션 페이지의 크기를 제한해야합니다. 클라이언트는 예를 들어 시간 초과 및 오류 생성 등 악의적이거나 손상된 서버에 연결될 경우 처리하려는 응답의 크기를 제한할 준비가 되어 있기를 권장합니다.

~~In order to prevent a client from being overloaded by oversized Collections, servers should take care to limit the size of Collection pages they return to clients. Clients should still be prepared to limit the size of responses they are willing to handle in case they connect to malicious or compromised servers, for example by timing out and generating an error.~~

### B.10 컨텐츠 보안 (Sanitizing Content)

브라우저 (또는 기타 서식있는 텍스트 지원 응용 프로그램)에 렌더링되는 모든 액티비티 필드들은 크로스 사이트 스크립팅 공격을 방지하기 위해 마크 업이 포함된 필드를 보안처리 하여야 합니다.

~~Any activity field being rendered for browsers (or other rich text enabled applications) should take care to sanitize fields containing markup to prevent cross site scripting attacks.~~

[//]: # "'Sanitizing'은 본래 '소독' 또는 '정리'를 의미하지만 이 경우에는 '보안'이 알맞는 것 같아 해당 번역으로 번역하였습니다. 이는 공식 번역은 존재하지 않는 것으로 판단되나, npm에 있는 sanitize-html를 '입출력보안'이라 해석하는 다수의 문서에서 착안하였습니다."

### B.11 bto와 bcc 속성을 표시하지 않기 (Not displaying bto and bcc properties)

`bto`와`bcc`는 이미 [전달을 위해 반드시 제거](https://www.w3.org/TR/activitypub/#remove-bto-bcc-before-delivery)했어야 하지만 서버는 자체 스토리지 시스템에서 객체를 표현하는 방법을 자유롭게 결정할 수 있습니다. 그러나 `bto`와 `bcc`는 객체/액티비티의 원작자에게만 알려 지거나 보여지기 위한 것이므로 서버는 이를 표시하는 동안 이러한 속성들을 생략해야합니다.

~~`bto` and `bcc` already [must be removed for delivery](https://www.w3.org/TR/activitypub/#remove-bto-bcc-before-delivery), but servers are free to decide how to represent the object in their own storage systems. However, since `bto` and `bcc` are only intended to be known/seen by the original author of the object/activity, servers should omit these properties during display as well.~~
