[README로 돌아가기](README.md)

## 목차 (Table of Contents)

0. [액티비티펍](ActivityPubIntro.md) (ActivityPub)

- 개요 (Abstract) 
- 이 문서의 상태 (Status of This Document)

1. [개론](ActivityPubChapter1.md) (Overview)

- 1.1 소셜 웹 워킹 그룹 (Social Web Working Group)

2. [규칙](ActivityPubChapter2.md) (Conformance)

- 2.1 규격 개요 (Specification Profiles)

3. [객체](ActivityPubChapter3.md) (Objects)

- 3.1 객체 구분자 (Object Identifiers)
- 3.2 객체 회수 (Retrieving objects)
- 3.3 출저 속성 (The source property)

4. [액터](ActivityPubChapter4.md) (Actors)

- 4.1 액터 객체 (Actor objects)

5. [컬렉션](ActivityPubChapter5.md) (Collections)

- 5.1 아웃박스 (Outbox)
- 5.2 인박스 (Inbox)
- 5.3 팔로워 컬렉션 (Followers Collection)
- 5.4 팔로잉 컬렉션 (Following Collection)
- 5.5 좋아하는 컬렉션 (Liked Collection)
- 5.6 공개 전파 (Public Addressing)
- 5.7 좋아요 컬렉션 (Likes Collection)
- 5.8 공유 컬렉션 (Shares Collection)

6. [클라이언트-서버 간 상호작용](ActivityPubChapter6.md) (Client to Server Interactions)

- 6.1 클라이언트 처리 (Client Addressing)
- 6.2 생성 액티비티 (Create Activity)
  - 6.2.1 생성 액티비티 없이 객체 생성 (Object creation without a Create Activity)
- 6.3 갱신 액티비티 (Update Activity)
  - 6.3.1 부분 갱신 (Partial Updates)
- 6.4 삭제 액티비티 (Delete Activity)
- 6.5 팔로우 액티비티 (Follow Activity)
- 6.6 추가 액티비티 (Add Activity)
- 6.7 제거 액티비티 (Remove Activity)
- 6.8 좋아요 액티비티 (Like Activity)
- 6.9 차단 액티비티 (Block Activity)
- 6.10 실행취소 액티비티 (Undo Activity)
- 6.11 전송 (Delivery)
- 6.12 미디어 업로드 (Uploading Media)

7. [서버-서버 간 상호작용](ActivityPubChapter7.md) (Server to Server Interactions)

- 7.1 전송 (Delivery)
  - 7.1.1 서버-서버 간 아웃박스 전송 요구사항 (Outbox Delivery Requirements for Server to Server)
  - 7.1.2 인박스에서의 포워딩 (Forwarding from Inbox)
  - 7.1.3 공유 인박스 전송 (Shared Inbox Delivery)
- 7.2 생성 액티비티 (Create Activity)
- 7.3 갱신 액티비티 (Update Activity)
- 7.4 삭제 액티비티 (Delete Activity)
- 7.5 팔로우 액티비티 (Follow Activity)
- 7.6 수락 액티비티 (Accept Activity)
- 7.7 거절 액티비티 (Reject Activity)
- 7.8 추가 엑티비티 (Add Activity)
- 7.9 제거 액티비티 (Remove Activity)
- 7.10 좋아요 액티비티 (Like Activity)
- 7.11 전파 액티비티 (공유) (Announce Activity (sharing))
- 7.12 실행취소 액티비티 (Undo Activity)

A. [국제화](ActivityPubChapterA.md) (Internationalization)

B. [보안 고려사항](ActivityPubChapterB.md) (Security Considerations)

- B.1 인증 및 권한 (Authentication and Authorization)
- B.2 검증 (Verification)
- B.3 로컬호스트 URI를 접근하기 (Accessing localhost URIs)
- B.4 URI 스킴 (URI Schemes)
- B.5 재귀적 객체 (Recursive Objects)
- B.6 스팸 (Spam)
- B.7 연합 서비스 거부 (Federation denial-of-service)
- B.8 클라이언트-서버 간 속도 제한 (Client-to-server ratelimiting)
- B.9 클라이언트-서버 간 서비스 거부 응답 (Client-to-server response denial-of-service)
- B.10 컨텐츠 보안 (Sanitizing Content)
- B.11 bto와 bcc 속성을 표시하지 않기 (Not displaying bto and bcc properties)

C. [감사의 말](ActivityPubChapterC.md) (Acknowledgements)

D. [참고 문헌](ActivityPubChapterD.md) (References)

- D.1 규정 참고 문헌 (Normative references)
- D.2 정보 참고 문헌 (Informative references)
