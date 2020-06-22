[README로 돌아가기](README.md)

# ActivityPub 문서 한글 번역 가이드라인

* 임시로 작성한 문서입니다.

## 1. 번역 단어관련

* 타 문서에서 음차로 번역되나, 이 문서에서는 한글로 번역한 단어가 다수 존재합니다.

원문 | 번역 | 비고
-- | -- | --
Vocabulary | 어휘
A to B (Server to Client...) | A - B 간
post | 게시
origin | (원) 발신지
receive | 전달 받다
List | 목록
Access | 접근
property | 속성
posting | 게시
Undo | 실행취소 | '되돌리기'는 'revert'
remove | 제거 | '삭제'는 'delete'
object | 객체
target | 대상 (목표물) | targeting은 음차를 사용하겠습니다
return | 반환
delivery | 전달  | '배송'이나 '송달'이 공식적인 번역
dereference | 역 참조
transient | 일시적 | '임시'는 'temporary'
likes/liked/like | 좋아요 / 좋아하는 / 좋은(좋아요) | 오판을 방지하기 위해 {번역(원문)} 양식을 따를 것
public | 공개
private | 비공개
Federated | 연합
Addressed | 지정하다, 전파하다, 처리하다, 발송하다 | 문맥에 따라 번역될수 있는 단어가 너무 많습니다
the ~ / a ~ | 주어진 ~ / ~ | 주어가 다수 존재할때만 적절히 사용

### 1-1. 음차를 사용하는 번역

원문 | 번역
-- | --
actor | 액터 
Follower/Following | 팔로워/팔로잉
Wrapping | 래핑
Activity | 액티비티 {예외적으로 Chapter1에서는 '활동'도 사용}
Inbox/Outbox | 인박스/아웃박스
Endpoint | 엔드포인트
Collection | 컬렉션
Profile | 프로필
Context | 컨텍스트
Link | 링크
targeting | 타게팅
Hosting | 호스팅
Schemes | 스킴

### 1-2. 원문 그대로 사용하는 번역

원문 | 비고
-- | --
Alyssa, Ben... 등 사람이름 | Chapter1 제외
body | 예: POST/요청의 body


## 2. 번역 일관성

RFC2119 (Chapter2에 정의되어 있습니다)
* *~수도 있다 (MAY)*,
* *반드시 (MUST)*, 
* *절대 하지 말아야 (MUST NOT)*, 
* *권장된다 (SHOULD)*, 
* *권장되지 않는다 (SHOULD NOT)*

## 3. markdown 등 그 외 관련

굳이 'ActivityStreams' / 'ActivityPub'를 번역하여야 하는가?
- 일단은 번역된 단어로 작성하였습니다.

`[WORD](LINK)`
- WORD
  - {번역한 단어}를 사용하되, 의미전달이 애매할 경우는 {번역(원문)}
- LINK
  - LINK는 원문 링크를 따르기

\`~\` 
- 단어 자체가 강조된 부분이므로 번역하지 않고 원어 그대로 적어두기
- 몇몇 Chapter에 보면 `object`를... 객체를... 이 한 문장/문단안에 있는 경우가 있습니다
    - 이 경우 해당 `백틱` 이후 첫 등장하는 단어에 번역(원문)으로 적겠습니다

주석 양식
```
[//TODO]: # "TODO"
[//Comment]: # "Comment"
[//Link]: # "Link"
```
