[README로 돌아가기](README.md)

### 목차 ~~Table of Contents~~

0. [서론](ActivityPubIntro.md)

1. [개론](ActivityPubChapter1.md) ~~Overview~~
* 1.1 소셜 웹 워킹 그룹 ~~Social Web Working Group~~

2. [준수](ActivityPubChapter2.md) ~~Conformance~~
* 2.1 규격 프로파일 ~~Specification Profiles~~

3. [객체](ActivityPubChapter3.md) ~~Objects~~
* 3.1 객체 구분자 ~~Object Identifiers~~
* 3.2 객체 회수 ~~Retrieving objects~~
* 3.3 소스 속성 ~~The source property~~

[//]: # "의견: 'source'를 `소스`로 번역하였습니다. 다른 대안이 나오기 전까지는 '소스'로 적어두겠습니다(TODO)"
[//]: # "참조: [(Google Support)소스 속성 보고서](https://support.google.com/analytics/answer/6033972?hl=ko)"
[//]: # "참조: [(IBM Knowledge Center)IBM i2, 속성 클래스 결합](https://www.ibm.com/support/knowledgecenter/ko/SSXVXZ_2.2.1/com.ibm.i2.anb.doc/combine_attribute_classes.html)"

4. [액터](ActivityPubChapter4.md) ~~Actors~~
* 4.1 액터 객체 ~~Actor objects~~

5. [컬렉션](ActivityPubChapter5.md) ~~Collections~~
* 5.1 보낼 편지함 ~~Outbox~~
* 5.2 받은 편지함 ~~Inbox~~
* 5.3 팔로워 모음 ~~Followers Collection~~
* 5.4 팔로잉 모음 ~~Following Collection~~
* 5.5 즐겨찾던 모음 ~~Liked Collection~~
* 5.6 공개 처리 ~~Public Addressing~~
* 5.7 즐겨찾는 모음 ~~Likes Collection~~
* 5.8 공유 모음 ~~Shares Collection~~

[//]: # "의견: ~box를 편지함이 아닌 다른 번역으로 대체하였으면 좋겠다고 생각이 듭니다. 다른 대안이 나오기 전까지는 '~편지함'으로 적어두겠습니다(TODO)"

[//]: # "의견: Followers / Following은 다른 SNS에서도 팔로워 / 팔로잉 으로 표기했음에 따라 팔로워 / 팔로잉 으로 번역하였습니다. 대안이 있을 경우 수정하겠습니다 (TODO)"
[//]: # "참조: [(Twitter Help) 팔로잉 관련 자주 묻는 질문](https://help.twitter.com/ko/using-twitter/following-faqs)"
[//]: # "참조: [(Facebook About) 팔로우](https://www.facebook.com/about/follow)"

[//]: # "의견: Addressing 을 '처리' 로 번역하였습니다. 차후에 해당 내역이 오류였을 경우 수정하겠습니다.(TODO)"

[//]: # "의견: 'Liked / Likes'를 '즐겨찾던 / 즐겨찾기'로 번역하였으나 원문 그대로 해석하면 '좋아했던/좋아하는' 또는 '마음에 들던 / 마음에 드는' 같이 해석해야 한다고 생각은 됩니다. 비슷하므로 해당 번역이 오류였다고 판단되면 수정하겠습니다(TODO)"
[//]: # "참조: [(Twitter Help) 트윗 마음에 들어하기](https://help.twitter.com/ko/using-twitter/liking-tweets-and-moments)"
[//]: # "참조: [(Facebook Help) '좋아요'가 무슨 의미인가요?](https://www.facebook.com/help/110920455663362)"

6. [클라이언트와 서버간 상호 작용](ActivityPubChapter6.md) ~~Client to Server Interactions~~
* 6.1 클라이언트 처리 ~~Client Addressing~~
* 6.2 생성 액티비티 ~~Create Activity~~
  * 6.2.1 생성 액티비티 없이 객체 생성 ~~Object creation without a Create Activity~~
* 6.3 갱신 액티비티 ~~Update Activity~~
  * 6.3.1 부분 갱신 ~~Partial Updates~~
* 6.4 삭제 액티비티 ~~Delete Activity~~
* 6.5 팔로 액티비티 ~~Follow Activity~~
* 6.6 추가 액티비티 ~~Add Activity~~
* 6.7 제거 액티비티 ~~Remove Activity~~
* 6.8 즐겨찾기 액티비티 ~~Like Activity~~
* 6.9 차단 액티비티 ~~Block Activity~~
* 6.10 되돌리기 액티비티 ~~Undo Activity~~
* 6.11 전송 ~~Delivery~~
* 6.12 미디어 업로드 ~~Uploading Media~~

[//]: # "의견: Addressing 을 '처리' 로 번역하였습니다. 차후에 해당 내역이 오류였을 경우 수정하겠습니다.(TODO)"
[//]: # "의견: Follow 는 Twitter에서도 '팔로'로 표기했음에 따라 '팔로' 로 번역하였습니다. 치후 해당 챕터 검토 후 '액티비티 따라가기' 처럼 다른 표현법으로 변경할지 검토하겠습니다.(TODO)"
[//]: # "의견: '미디어 올리기' 로도 표현이 가능하나 '미디어 업로드' 라고 번역하는 것이 의미를 가장 정확하게 전달할 것 같아 해당 내용으로 번역하였습니다. 차후 다른 표현법으로 변경할지 검토하겠습니다.(TODO)"

7. [서버와 서버간 상호 작용](ActivityPubChapter7.md) ~~Server to Server Interactions~~
* 7.1 전송 ~~Delivery~~
  * 7.1.1 서버와 서버간 보낼 편지함 전송 요구사항 ~~Outbox Delivery Requirements for Server to Server~~
  * 7.1.2 받은 편지함에서의 전송 ~~Forwarding from Inbox~~
  * 7.1.3 공유된 받은 편지함에서의 전송 ~~Shared Inbox Delivery~~
* 7.2 생성 액티비티 ~~Create Activity~~
* 7.3 갱신 액티비티 ~~Update Activity~~
* 7.4 삭제 액티비티 ~~Delete Activity~~
* 7.5 팔로 액티비티 ~~Follow Activity~~
* 7.6 수락 액티비티 ~~Accept Activity~~
* 7.7 거절 액티비티 ~~Reject Activity~~
* 7.8 추가 엑티비티 ~~Add Activity~~
* 7.9 제거 액티비티 ~~Remove Activity~~
* 7.10 즐겨찾기 액티비티 ~~Like Activity~~
* 7.11 전파 액티비티 (공유) ~~Announce Activity (sharing)~~
* 7.12 되돌리기 액티비티 ~~Undo Activity~~

[//]: # "의견: 7.1.x 번역이 전부 깔끔하지 않다고 판단됩니다. 차후 다른 표현법으로 변경할지 검토하겠습니다.(TODO)"


A. [국제화](ActivityPubChapterA.md) ~~Internationalization~~

B. [보안 고려사항](ActivityPubChapterB.md) ~~Security Considerations~~
* B.1 인증 및 권한 ~~Authentication and Authorization~~
* B.2 검증 ~~Verification~~
* B.3 로컬호스트 URI 접근 ~~Accessing localhost URIs~~
* B.4 URI 도식 ~~URI Schemes~~
* B.5 재귀적 객체 ~~Recursive Objects~~
* B.6 스팸 ~~Spam~~
* B.7 연합 서비스 거부 ~~Federation denial-of-service~~
* B.8 클라이언트-서버간 속도 제한 ~~Client-to-server ratelimiting~~
* B.9 클라이언트-서버간 서비스 거부 응답 ~~Client-to-server response denial-of-service~~
* B.10 컨텐츠 소독 ~~Sanitizing Content~~
* B.11 bto와 bcc 속성을 표시하지 않을 경우 ~~Not displaying bto and bcc properties~~

[//]: # "의견: 도식, 체계 말고도 스킴으로 변역해도 괜찮은것 같으나 일단은 '도식'으로 번역하겠습니다(TODO)"

[//]: # "의견: 소독 말고 정제나 다른 단어가 있을것 같으나 일단은 '소독'으로 번역하겠습니다(TODO)"

C. [감사의 말](ActivityPubChapterC.md) ~~Acknowledgements~~

D. [참고 문헌](ActivityPubChapterD.md) ~~References~~
* D.1 규정 참고 문헌 ~~Normative references~~
* D.2 정보 참고 문헌 ~~Informative references~~
