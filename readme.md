# Today I Learned (TIL)

매일 무엇을 배웠는지 정리하고 기록합니다.

> 꾸준히 하면 좋은 일이 생긴다!

### History

- [2019년 11월](https://github.com/jhleed/TIL/tree/master#2019년-11월)
- [2019년 10월](https://github.com/jhleed/TIL#2019%EB%85%84-10%EC%9B%94)
- [2018년 10월](https://github.com/jhleed/TIL#2018%EB%85%84-10%EC%9B%94)
- [2018년 9월](https://github.com/jhleed/TIL#2018%EB%85%84-9%EC%9B%94)
- [2017년 10월](https://github.com/jhleed/TIL#2017%EB%85%84-10%EC%9B%94)
- [2017년 9월](https://github.com/jhleed/TIL#2017%EB%85%84-09%EC%9B%94)
- [2017년 8월](https://github.com/jhleed/TIL#2017%EB%85%84-08%EC%9B%94)

# 2019년 11월

## 2019.11.13 (수)

### Redis 의 Upsert

- `Redis`도 마찬가지로 Upsert를 해줄 일이 있어서 방법을 알아봤다.
- `Redis`는 `Increase` 연산이 없고 덮어쓰기만 된다고 한다.
- 따라서 값을 먼저 읽고, 값을 더한 후, 덮어쓰기를 해야 한다.
- 이 때, 멀티 쓰레딩 환경에서는 문제가 발생할 수 있다. 이 경우 처리 방법은 아래 2가지가 있다.
    1. 대규모 트래픽인 경우 → 스레드에 순서를 줘서 데이터를 분산 처리하는 방식 (정확하게 이해를 한 건 아니라서 추후 찾아봐야겠음)
    2. 그냥 `Lock` → 성능 저하 고려해야 함
- 이번 경우는 클릭 데이터라서 트래픽이 많지 않고, 일정이 촉박하므로 Lock을 거는 방식으로 해결하기로 했다.

### 데이터베이스의 격리 수준(Transaction Isolation Level)

- 어제 DB 격리 수준으로 머리 싸매고 고민했더니 꿈에서도 격리 수준이 나왔다.
- 어제는 그렇게 머리가 아팠는데 지금은 정리가 된 기분이다. 신기하다. 가끔 이럴 때가 있다.
- 동료분과 이야기를 나눴는데, 이번 Upsert의 경우는 내가 설정한 격리 수준(2단계 : 커밋된 데이터 읽기)정도면 충분하다고 하셨다.

### IntelliJ 폰트 설정

- 인텔리제이에 일부 한글 폰트가 깨져서 나오는 현상을 해결했다.
    - [노션 메모](https://www.notion.so/jhleed/IntelliJ-1ece00e066ca4fc5a33a0a5d516de8df)

- 렌더링 안되는 폰트에 대해서 서식을 지정하는 `Fallback font` 옵션에 대해서 알게 되었다.
- 생활코딩에 질문하니 답을 금방 찾을 수 있었다. 역시 집단지성 ..

![](Untitled-378fe25c-592b-4b35-88cb-f16bb48dae74.png)

## 2019.11.12 (화)

### 데이터베이스의 격리 수준(Transaction Isolation Level)

- [Notion 정리](https://www.notion.so/jhleed/MSSQL-19b9814c094f4a7ab9744716a23e3816)
- RDB에 대한 경험이 많지 않다보니, 이런 기본적인 지식들이 부족하다. 뭔지는 알고 있는데, 와닿지가 않는다고 할까..
- 틈틈히 정리해야겠다.

### MSSM 쿼리 바로가기 단축키 설정

- `MSSM`을 쓸 일이 많아서 자주 쓰는 기능들을 단축키로 정리했다.
- 특히 `sp_depends` 랑 `sp_helptext` 그리고 테이블의 row 샘플 보기가 꿀인 듯 하다.
    - 다른 분들은 `sp_helpindex` 도 많이 쓰시는 듯 하니 참고하자.
- 나는 아래와 같이 맞췄다. 또 필요한 게 생기면 추가해야지.

![](Untitled-7d551d76-6107-450f-a0b4-d6241af8bae6.png)

### Spring Boot & Redis 연동

- 을 하려다가 포트오픈이 안되어 있어서 보류했다. 곧 PC 교체때문에 IP도 바뀔텐데 그 이후에 해야겠다.

### 기술 토론

- 동료분들과 아침에 커피를 마시며 기술 토론을 했다.
- `JPA` : `Table`이 객체지향적으로 분리가 잘 되어 있어야 효과가 극대화 됨
- `Clean Architecture` : 해결하려는 문제(도메인)에 적합한 형태로 사용하는 것이 좋음, 모든 개념을 다 도입하면 오버 엔지니어링으로 가기 쉬움

### Java 수양록

- [Java에서 시간 객체를 다루는 법을 정리](https://www.notion.so/jhleed/7a6c614d0ed34280b53b8e8a5f42896e?v=ef7eda9e75ca4be8a374111d118d5bb9)

## 2019.11.11 (월)

### 이상한 테스트

- 잘못된 로직이 있는데 테스트가 정상적으로 동작하고 있다.
- 버그가 발생하는 것 보다 더 심각한 최악의 케이스다.
- `Test Double` 을 너무 과다하게 사용하다 보니 이런 폐해가 발생하는 듯 하다.
    - `Test Double` 의 사용을 줄여야 하나..

### 사이드 프로젝트

- 다시 사이드 프로젝트에 관심이 간다.. 이런 변덕쟁이.

### 아이패드 세트 장만

- 생산성 향상을 위해 아이패드 풀 세트 장만! (키보드 + 펜슬)
    - 기존에도 아이패드 + 펜슬을 잘 쓰고 있었기에 가성비로 치면 망이지만.. 생산성을 위해서는 아낌없이 투자한다!
        - ~~(사실 애플에 뇌가 지배당한 것은 아닐까..?)~~

## 2019.11.10 (일)

### MSSQL JDBC Fork

- 지난 번 나를 헷갈리게 했던 에러 메세지를 변경하고자 Fork 했다.
    - [바로 이 부분이다.](https://github.com/jhleed/mssql-jdbc/blob/master/src/main/java/com/microsoft/sqlserver/jdbc/SQLServerCallableStatement.java#L1283)
- 마이크로소프트의 오픈소스 기여자가 꼭 되고 싶다.

## 2019.11.09 (토)

### 컴주개 모임

- 생산적이고 재미있었던 모임
- `RESTful`에서 왜 `STATELESS`가 중요한지
- `SPOF`에 대한 설명과 예시
- 소켓통신에 대한 설명과 예시
- 다음 모임에서는 기술발표에 대한 비중을 조금 더 늘려보기로 했다.

## 2019.11.08 (금)

### 계속되는 삽질

- `SP`실행 권한을 정상적으로 부여했음에도 불구하고, 동일한 현상이 발생한다.
- 좀 더 리서칭을 해보니 `MSSQL JDBC` 자체에도 이슈가 있는 듯 했다.
    - [이슈 링크](https://github.com/microsoft/mssql-jdbc/issues/1112)
- 우선 사내 위키 가이드를 따라, 사용 방법을 좀 바꿨더니 DB에는 붙는 것 같은데 이번엔 다른 에러가 난다.
    - `Caused by: com.microsoft.sqlserver.jdbc.SQLServerException: '{' 근처의 구문이 잘못되었습니다.`
    - 이건 또 무슨 말인가.. 디버깅으로 넘어가는 call 값을 보니 아래와 같이 뜨던데 이게 원인인가.

        ![](Untitled-6e3e4440-d146-499f-85dd-48247d523cb0.png)

    - MS의 예외 메세지는 가끔 너무 불친절하다는 느낌을 받을때가 있다.

## 2019.11.07 (목)

### MSSQL JDBC Driver 분석

- `Java`에서 `MSSQL`의 `Stored Procudure`를 실행시키려고 하는데 자꾸 무슨 파라미터가 정의되어 있지 않다고 뜬다. 분명히 정의되어 있는데.
- 이상해서 소스를 파고들어서 분석해봤다. `SP`를 실행시킬 수 없을때 발생하는 예외인데, 메세지만 읽으면 의미가 완전 다르다.
- 왜 `SP` 실행이 안될까 생각해봤는데, `SP`에 실행권한이 부여되어 있지 않은 것이 원인인 듯 하다.
- 이렇게 오픈소스 깊게 파본적은 오랜만이다. 그리고, 생각보다 할만하다. 이걸 계기로 `MSSQL JDBC`에 컨트리뷰션을 할 수 있었으면 좋겠다.

### 오픈소스 커뮤니티 코덕

- 민수의 소개로 [코덕](https://co-duck.com/)이란 서비스를 알게됬다.
- 깃허브와 연동해서 페이스북과 같은 SNS처럼 보여주는 서비스인데 재밌다.

![](Untitled-f89bdb8f-2914-4755-9cdd-af1d81ef58ce.png)

- 개발 블로그랑 연동도 된다.

![](Untitled-278002dd-9baf-4d06-9601-a7bd868b69fc.png)

### 오픈소스

- 후배들이 일일커밋 등 깃허브 활동을 왕성하게 하는 것을 보고 자극 받았다.
- 나도 적극적으로 오픈소스 활동에 참여해볼까 싶다. 어디서부터 시작해야할지는 잘 모르겠지만 우선 사이드 프로젝트를 위한 [아이디어 뱅크 저장소](https://github.com/jhleed/IdeaBank)를 만들었다.
- [오픈소스 룰 저장소](https://github.com/jhleed/My-OpenSource-Rule)도 만들었다.
    - 그리고, **앞으로 오픈소스부터 영어를 적극 활용하기로 결심했다.**
        - 코딩과 영어를 자연스럽게 늘리는 방법엔 이것만큼 좋은게 없는 것 같다. 영어로 오픈소스 활동을 하는 것.
        - 전에는 영어 잘 못하는데 이렇게 써도 되나..라는 걱정이 있었는데, 지금 생각해보면 어차피 한글로도 주석 맞춤법은 많이 틀린다. 그냥 마음 편하게 쓰기로 한다.

## 2019.11.06 (수)

### 테스팅에 대한 고민을 많이 했다.

- 엔지니어링에서 테스팅이란, 가장 이상적인 방법보다는 현 상황에 최저의 비용으로 최대의 효과를 뽑아낼 수 있는 방법인 것 같다.
- 이제는 Mocking Library에 의존하지 않다보니 어떤 언어, 어떤 환경에서도 테스트를 작성할 수 있을 것 같다.
    - 이전에는 `.NET`에서 `DI`를 하지 못해서 쩔쩔맸었는데 이제 더 이상 그런일은 없다.
- 언제나 읽는 [규원님의 테스팅 관련 글](https://justhackem.wordpress.com/2016/05/23/unit-integration-acceptance-and-functional-testing/amp/), 그리고 구글의 [Just Say No to More End-to-End Tests](https://testing.googleblog.com/2015/04/just-say-no-to-more-end-to-end-tests.html) 라는 글이 인상적이었다.
    - 테스팅 피라미드

![](Untitled-a4d79f61-721b-40b1-89dd-d128919b1f6b.png)

### 노션 정리 방식 변경

- 기술 정리 방식을 기존 페이지 단위에서 아래처럼 테이블 단위로 바꿨다.
- 제일 좋은 점은 한눈에 태그별로 정렬해서 볼 수 있다는 것

![](Untitled-ff94f14c-b309-4231-9914-e352ab1391cd.png)

### 도메인 로직에 빠뜨린 부분이 있네?

- 요구사항 하나를 잘못 이해해서 설계를 고쳐야 할 부분이 조금 있다.

### 코딩 시작

- 힘든 백업을 끝내고 코딩을 시작한다.

## 2019.11.05 (화)

### 포맷 백업의 고통

- 몇 가지 백업을 안한 부분이 또 생각났다.
    - `MSSQL CLIENT`, `PIG TOOLBOX`,

## 2019.11.04 (월)

### 컴퓨터 세팅

- 포맷 후 컴퓨터 세팅
- 사내 프로그램이 많아서 생각보다 쉽게 백업이 잘 안 된다. 내일 출근후 동료분들 오시면 동료분 로컬 PC 세팅 참고해서 해결해야겠다. 오늘은 할 수 있는 일을 하자.

## 2019.11.03 (일)

### 멋쟁이 사자처럼 발표자료 업로드

- [슬라이드쉐어에 발표자료를 업로드 했다.](https://www.slideshare.net/JamesLee218/ss-189866386)
    - 발표자료 플랫폼으로 스피커 덱 vs 슬라이드 쉐어 설문조사를 했는데 슬라이드 쉐어가 압도적으로 많았다.
- 슬라이드 쉐어의 문제점들
    - 발표자료에 한글이 들어가 있으면 업로드가 안 됨
    - 이미지가 너무 크거나 용량이 크면 업로드가 일부 짤림 (이미지가 안 보인다거나..
        - 애니메이션 단계는 빼고, 이미지 품질을 조정 (최상 → 우수)해봤지만 문제는 여전하다
        - [해결 방법을 찾았다.](https://item4.blog/2016-10-31/Way-to-Use-Homeland-Fonts-on-SlideShare/)
            - [웹 서비스 버전](https://beomi.github.io/SlideShare_Character_Updater/#/)

## 2019.11.01 (금)

### 도커와 쿠버네티스

- [http://www.itworld.co.kr/news/135282](http://www.itworld.co.kr/news/135282)

> 만약 컨테이너가 크루즈선의 승객이라면, 쿠버네티스는 크루즈선의 선장이다.

# 2019년 10월

## 2019.10.31 (목)

### Mockito 제거하고 순수 Mock 객체로 대신하기

- Mockito를 제거했다.
- 아직은 테스트가 구현에 의존적이지만 그래도 테스트 속도가 훨씬 빨라졌다!
- Stub를 사용함으로써 테스트가 더 유연해졌다.
    - 여러 파라미터를 넘겨도 그 중 테스트 하고 싶은 값만 테스트 하면 되므로

## 2019.10.30 (수)

### Redis

- `Redis Cluster`는 `Master-Master` 구조와는 다르게 `DB`단위로 분할되어 있지 않다.
- 그래서 `CLI Client`로 접속해서 봐야 한다.

### Remarketing Ad Bisuness

- 동료분께 리마케팅 비즈니스에 대한 설명을 들었다.
- `SSP`, `Ad Exchange`는 대강 알고 있었지만 DSP와 어떻게 얽히는지 실제 예시로 설명을 들으니까 더 이해가 잘 되었다.
- 그쪽은 수요에 비해 공급이 적어서 기회의 땅이기도 하다.
    - 개발자들이 하기는 좀 짜치는(?)데, 운영 인력들이 하기에는 기술적인 지식이 필요한..
- 재미있었다. 종종 이렇게 비즈니스적인 이해도를 높힐 수 있었으면 좋겠다.

### 자바에서의 Json 직렬화

- `Jackson`은 자바에서 `Json`을 직렬화 하는데 사용하는 보편적인 도구이지만, `Time Format`에 문제가 있다고 한다. 그래서 보통 `Custom Serializer`를 만드는 듯 하다.
    - `jsonMapperJava8DateTimeModule`
- 참고 문서
    - [https://perfectacle.github.io/2018/01/16/jackson-local-date-time-serialize/](https://perfectacle.github.io/2018/01/16/jackson-local-date-time-serialize/)

### DTO, VO

- `VO(Value Object)`는 값 그대로 의미를 가지는 객체, **Read-Only**이다.
- `DTO(Data Transfer Object)`는 **전송되는 데이터의 컨테이너**이다.
- 참고 문서
    - [https://ijbgo.tistory.com/9](https://ijbgo.tistory.com/9)

### 테스트와 설계

- 설계를 살짝 바꿨는데 테스트가 왕창 깨졌다. 코드와 테스트의 결합이 너무 높은 것이 원인이었다.
- 관련하여 페이스북에 아래와 같은 글을 썼다.

![](Untitled-7dc14a6c-93fe-481d-9609-d42b07dc21f2.png)

- Mock을 완전히 없애보려고 했는데 쉽지 않다. void 타입 메소드가 호출됬는지를 검증하려고 하면 결국 Mock이 필요하다.
    - 이와 관련하여 [좋은 글](https://medium.com/@SlackBeck/mock-object란-무엇인가-85159754b2ac)을 발견하여 정리하고 있다.
    - [https://www.notion.so/jhleed/Testing-Void-Mock-Object-7744c5ed86b24a9ea4e59b186c5aa8c9](https://www.notion.so/jhleed/Testing-Void-Mock-Object-7744c5ed86b24a9ea4e59b186c5aa8c9)

## 2019.10.29 (화)

### Deview 2일차 참석

- 예전에 데뷰 참석했을때는 딴 세상 이야기인줄 알았는데, 이베이에 온 이후로는 와닿는 내용들이 많아졌다. 그래서 굉장히 재밌게 들었다.
- 특히, 아래의 세션들이 좋았다.
    - 실패에서 배우는 SRE(사이트 신뢰성 엔지니어링)
        - 장애가 발생했을때 아낄 수 있는 시간은 최대한 아끼자
        - 실패에서 배운다! Post-Moterm
        - 특히, 장애 패턴을 학습해서 그래프와 정규식을 매칭시켜 장애 타입을 식별해서 담당자에게 알려주는 부분 매우 인상깊었다.
    - 속도를 위한 MongoDB
        - 컴파운드 인덱스, 그리고 쿼리 플래너의 동작 방식에 대해 배웠다.
        - 내가 삽질했을 수도 있는 내용이었는데, 미리 앞서 겪어본 선배 개발자분들의 이야기를 듣는게 정말 값진 시간인 것 같다.

## 2019.10.28 (월)

### Java Optional

- Java Optional에 관한 3부작 포스팅을 봤다.
    - [자바8 Optional 1부: 빠져나올 수 없는 null 처리의 늪](https://www.daleseo.com/java8-optional-before/)
    - [자바8 Optional 2부: null을 대하는 새로운 방법](https://www.daleseo.com/java8-optional-after/)
    - [자바8 Optional 3부: Optional을 Optional답게](https://www.daleseo.com/java8-optional-effective/)
- `Optional` 은 NPE을 우아하게 방어하는 것이 목적이다.
    - `Null` 체크를 하자니 코드가 지저분해진다.
    - 함수형 언어에서 해법을 찾은 것이 바로 `Optional`
        - 포인트는 `Null` 이라는 개념을 하나의 `Type`으로 다루는 것
- 기존에 조건문들을 `orElse` 등의 API로 대체한다.
- Stream과도 비슷하게 사용할 수 있다.
- 예시 (Notion Markdown Export는 소스코드 주석이 안먹어서 이미지로 대체..)

![](Untitled-fb8b6618-2e36-4a54-abe9-67cec19dae85.png)

## 2019.10.27 (일)

- 멋쟁이 사자처럼 발표

## 2019.10.26 (토)

- 멋쟁이 사자처럼 발표 연습

## 2019.10.25 (금)

### 실리콘밸리 top5 회사 합격 후기

- [https://medium.com/@jubileekim/실리콘밸리-top5-회사-합격-후기-c50640b26eab](https://medium.com/@jubileekim/%EC%8B%A4%EB%A6%AC%EC%BD%98%EB%B0%B8%EB%A6%AC-top5-%ED%9A%8C%EC%82%AC-%ED%95%A9%EA%B2%A9-%ED%9B%84%EA%B8%B0-c50640b26eab)
- 동기부여

### 커넥션 풀과 스레드 풀

- [https://d2.naver.com/helloworld/5102792](https://d2.naver.com/helloworld/5102792)

### 자바 병목 현상 분석하기 (스레드 덤프)

- [https://www.slideshare.net/cc612/ch10-48277394?from_m_app=ios](https://www.slideshare.net/cc612/ch10-48277394?from_m_app=ios)

### 자바스크립트에서 메모리 관리

- [https://developer.mozilla.org/ko/docs/Web/JavaScript/Memory_Management](https://developer.mozilla.org/ko/docs/Web/JavaScript/Memory_Management)

## 2019.10.24 (목)

아래의 사항을 복습했다. 

자바를 다시 쓰기 시작했으므로 기본적인 것들 중 잘못 알고 있는 것은 없는지 다시 체크해본다.

- 서블릿 (멀티 스레드 환경)에서의 Thread-Safe
    - [https://jojoldu.tistory.com/118](https://jojoldu.tistory.com/118)
    - [http://stackoverflow.com/questions/9555842/why-servlets-are-not-thread-safe/](http://stackoverflow.com/questions/9555842/why-servlets-are-not-thread-safe/)
- 서블릿 생명주기
- Session 생명주기
    - Session hijacking

## ~ 2019.10.23 (수)

- 며칠간 테이블 모델링, 그리고 모듈간 데이터 플로우를 설계했다.
    - 처음에 생각했었던 것 보다 범위가 많이 커졌다.
- 문서화의 장점을 체감했다.
    - 노트에 펜으로 끄적거리는 것 보다 Wiki의 Giffy Tool을 이용해서 모듈간의 데이터 흐름을 표현했다.
    - 확실히 한 눈에 들어오기도 좋고, 공유하기도 좋다.

## 2019.10.21 (월)

### 테스트가 개발 생산성으로 이어지다.

핵심 로직이 유의미한 테스트 커버리지를 가지게 되었다. 

이후 개발 생산성이 급격하게 향상되는 것이 느껴진다. 

1. 버그를 빠르게 찾아낸다. → 삽질에 걸리는 시간이 줄어든다.
2. 과감한 리팩토링이 가능해진다. → 좋은 구조로 이어진다. → 생산성이 향상된다.

그리고 심리적인 안정감 또한 큰 것 같다. 

## 2019.10.20 (일)

*뭐에 집중해야 할 지 혼란스럽다. 하고 싶은건 많은데* ..

### 도서관 - 오브젝트, 도메인 주도 설계 핵심을 읽었다.

**오브젝트**

- 객체지향의 핵심은 상속이 아니라 객체간의 책임과 협력이다.
- 정보를 가지고 있는 객체에 책임을 줘야 한다.
- 한 객체에 책임이 너무 많이 몰리면 응집력이 떨어지므로 객체를 분리해야 한다.

**도메인 주도 설계 핵심**

- `DDD`의 핵심을 요약해서 전달해주는 책이다.
- 한번 쭉 읽고나서 나에게 필요한 부분, 더 알고싶은 부분을 에릭 에반스의 `DDD` 책으로 깊이 이쎅 공부하면 좋을 듯.
- 궁금한 것 : DDD에서 말하는 `Entity`는 `JPA`에서 말하는 `Entity`와 무엇이 다른가
    - `Entity`에 책임을 넣어도 되는가? (데이터 + 로직)

## 2019.10.19 (토)

### 모의 객체를 통한 인수 테스트 구축하기

- `Mockito` 라이브러리 의존성 문제를 겪었고, 해결했다.
    - [트러블 슈팅 노트](https://www.notion.so/jhleed/Mokito-java-lang-NoSuchMethodError-c64951706b1c43d296662e0e58f16121)

테스트를 짜는데 자꾸 난관에 부딫친다. 

- 내 설계에 뭔가 문제가 있다는 신호다. 리팩토링을 하는 것이 좋겠다.

설계가 좋을수록 모의 객체를 덜 사용하고도 테스트가 가능하다. 아래 글도 참고하자.

- [https://jojoldu.tistory.com/320](https://jojoldu.tistory.com/320)

비즈니스 로직은 각 도메인 객체에 위임하고 서비스 레이어는 외부 레파지토리를 올바른 파라미터로 호출하는지 확인한다. (아직은 서비스에 별 비즈니스 로직이 없어서.)

`Mockito`의 여러가지 사용법을 익히는 중이다. 

- 미리 정해진 값을 Return하는 `Given()`
- 의존하는 객체가 메소드를 호출하는지를 검증하는 `verify()`
    - 해당 메소드의 파라미터를 검사하는 `ArgumentCaptor` 와`capture()`
        - `verify`로는 넘겨주는 파라미터에 값 비교 (deep equals)이 되지 않는다. 이럴 때 `ArgumentCapture`를 쓰면 된다. → [참고 링크](https://stackoverflow.com/questions/1142837/verify-object-attribute-value-with-mockito)

## 2019.10.18 (금)

### 테스트 테스트 테스트..

- 테스트 설정을 하는데 스프링에서 테스트 대역 설정을 하는 법을 많이 잊어버려서 삽질을 하는 중이다.
- 외부 모듈과 통신하는 부분을 제외하고 로직만 테스트를 하려고 하는데, Spring 작동 방식이 부분부분 기억나서, Bean을 생성할 수 없다는 예외를 자주 만난다.

## 2019.10.17 (목)

### 객체에 어떻게 책임을 더 많이 넘길까

아래 방법들을 고민 중이다.

- 객체에 책임을 더 많이 넘기는 방법
- 외부 세계로부터의 의존성을 분리하는 방법
    - 외부 세계로부터의 데이터를 하나의 객체로 모아서 업무 로직을 처리하는 객체에 파라미터로 던지는 방법을 생각 중이다.
        - 전달하는 파라미터 객체만 Mocking 해주면 될 것 같은데.. 음.

### DTO, VO, Entity 고민

- 비즈니스 로직을 Entity에다가 담으려고 하는데, 적합한 방법을 고민 중

### Report Batch Job 메모리 이슈 해결

- 내가 한 건 아니고 동료분께서 처리하신 부분에 대한 내용을 공유받았다.
- 이전에 내가 구현했었던 Job에서 메모리가 폭증해서 일부 데이터를 처리하지 못하는 현상이 지속되었다고 한다.
- 원인은, 특정 케이스에서 처리량이 급격히 늘어나서 메모리를 다 잡아먹어버려서 처리가 안되었었던 것
- 구현할 당시에는 데이터가 그렇게 많아질 줄 예상하지 못했는데, 앞으로는 그런 부분도 고려해야겠다.

## 2019.10.16 (수)

### 도메인 객체에 책임 부여

- 이전의 나와 달라진 점이 있다면 도메인 객체들에게 책임을 더 많이 부여하기 시작했다는 것이다. 즉, 보다 객체지향적으로 로직을 구현하기 시작했다.
    - 이전에는 서비스 레이어에서 대부분의 비즈니스 로직을 담당했다. 객체지향 언어를 씀에도 불구하고 서비스 레이어에서는 마치 절차지향처럼 구현한 것이다.
    - 이제는 도메인 객체들을 생성하고 각 객체에게 책임을 분배한다. 그리고 각 객체는 서로 조화를 이루며 비즈니스 로직을 구현하도록 한다.
    - 조영호님의 책 [객체지향의 사실과 오해](http://www.yes24.com/Product/goods/18249021?scode=018)에서 많은 영향을 받은 것 같다.

### 섣부른 추상화를 막자

- 처음에 인터페이스 기반으로 구조를 잡다가 대부분 구현체로 바꿨다.
- 아직 스펙이 완전하지 않기 때문에 섣부른 추상화보다는 개발 이후 리팩토링을 통해 코드 품질을 올리는 전략으로 가기로 했다.
    - 내가 뭘 구현하는지도 모르면 안되잖아? 추상화는 구현 대상의 주요 특징을 뽑아내는 것인데, 스펙이 완전하지 않은 상태에서는 손실이 더 클 것이라는 판단이다.
- 리팩토링을 효율적으로 하려면 테스트 커버리지가 탄탄해야겠지. 그래서 테스트는 정말 꼼꼼히 짜기로 했다.

### 테스트 도입

- 내가 맡은 모듈에 `단위 테스트(Unit Test)`를 도입 중이다.
    - 자동화 테스트가 주는 안정감 오랜만이다. 편안하다.
    - 그리고 레거시 프레임워크에서 벗어나 스프링을 쓰니 참 좋다. 고향에 온 기분이다. :)
    - 원래는 바로 인수 테스트로 들어가려고 했는데, 구조가 애매해서 우선 단위 테스트부터 적용하고 이후에 인수 테스트를 넣으려 한다.
        - 인수 테스트를 할 구조를 아직 못 잡겠다. 객체 간 어떤식으로 의존성을 줘야 하나 고민된다.

### 클린 아키텍쳐 도입 고민

- 내가 맡은 모듈에 클린 아키텍쳐 구조를 도입할까 고민했다.
    - 오버 엔지니어링이 될 거 같다는 우려가 살짝 든다. 그렇게 많은 로직을 포함하고 있지 않기 때문이다.
    - 차라리, `Use Case Layer`만 둬서 `인수 테스트`만 할까 싶다.

### 테스트 전략

![](Untitled-62d94b15-24f4-4d99-aa61-9e3ae42082d7.png)

- 그 동안 `인수 테스트(Acceptance Test)`와 `통합 테스트(Integration Test)`의 개념에 대해 오해를 가지고 있다는 사실을 깨달았다.
    - **통합 테스트**는 DB 등과 같은 외부 모듈들을 포함하여 테스트하는 것이다.
        - 이전에는 다른 모듈(ex : 다른 클래스)을 포함하면 통합 테스트인줄 알았다.
    - **인수 테스트**는 이해 관계자(ex : 기획자 또는 사용자)가 이해할 수 있는 비즈니스 시나리오를 의미한다.
        - 위 그림에는 `BDD`를 인수 테스트의 예로 들었다.
        - 인수 테스트는 외부 모듈을 테스트하지 않는다.
            - *그러면 Mocking을 한다는 것인가..?*
- [참고 문서](https://github.com/mattia-battiston/clean-architecture-example#testing-strategy)

## 2019.10.15 (화)

### 클린 아키텍쳐 도입 고민

- 어떻게 내 프로젝트에 클린 아키텍쳐를 도입할지 고민 중이다.
- 인프라나 외부 세계가 확정되지 않은 상태지만, 비즈니스 로직이라도 먼저 만들고 싶다.
- 추후에 테스트를 용이하게 하기 위함도 있다.
- 아래 문서를 참고했다.
    - [깃허브](https://www.notion.so/jhleed/Today-I-Learned-TIL-4adb6f1f86ea4e6da8d49eb599f75b92)
    - [슬라이드](https://www.slideshare.net/mattiabattiston/real-life-clean-architecture-61242830)

### Spring-Kafka 적용

- `spring-kafka`를 적용했다.
- 처음에는 `kafka-clients` 를 썼는데 이상 없이 동작하는 것을 보고 그냥 이대로 쓸까..? 하는 유혹이 잠깐 들었다. 그러나 그냥 쓰기에는 단점들이 너무 많았다.
- 몇 번의 삽질을 거쳐 `spring-kafka`로 바꿨는데, 확실히 스프링을 쓰면 비즈니스 로직에만 집중할 수 있다는게 느껴진다.

### HTTP/3 초안이 나왔다고?

- `HTTP/3`의 초안이 나왔다고 해서 아웃사이더님의 [한글 번역본](https://bagder.gitbooks.io/http3-explained/ko/)을 읽어봤다. 놀랍게도 UDP위에 구현했다고 한다. 자연스럽게 **"그럼 신뢰성 보장은 어떻게 해?"** 라는 생각이 들게 되는데 **UDP 위에 새로운 계층을 추가함으로써 신뢰성 문제를 해결**했다고 한다.

![](Untitled-b0a03fe6-c3eb-40f7-b44a-d33d0790ff94.png)

- `HTTP/2`가 나온것도 얼마 안 된 것 같은데 벌써 `HTTP/3`의 초안이 나왔다니 기술의 변화 속도가 빠르다는게 체감된다.
- 그 외 읽어본 문서
    - [QUIC과 HTTP/3 - 1. UDP기반 전송 프로토콜의 대두](https://www.notion.so/jhleed/Today-I-Learned-TIL-4adb6f1f86ea4e6da8d49eb599f75b92)
    - [HTTP/3은 왜 UDP3을 선택한 것일까?](https://evan-moon.github.io/2019/10/08/what-is-http3/)

## 2019.10.14 (월)

### 능력있는 개발자

- [능력있는 개발자는 어떻게 알아볼 수 있습니까](https://okky.kr/article/370719)? 라는 글을 봤는데 공감이 많이 됬다. 내가 어느 단계에 있는지 생각을 해보게 된다.
- 사실 몇년 전에도 봤던 글인데, 그 때는 안 와닿았던 것들이 지금은 와 닿는 것들이 많았다.
- 특히 공감이 되었던 부분은 이렇다.
    - 개발자들의 레벨을 나누는 방법
    - 지식은 가르칠 수 있지만 태도는 가르치기 어려움
    - 표면을 보는 사람에게 본질을 보는 사람의 말은 뜬구름처럼 들린다는 것
    - ...

### 전략 패턴으로 의존성 분리하기

- 광고 벤더사를 추가하는 작업이 있었다. 기존 벤더사와 대부분의 비즈니스 로직은 동일하고 일부가 다르다.
    - **기존 코드에 간단하게 로직을 추가해줄 수 있었지만 벤더사를 하나 추가했을때 미치는 사이드 이펙트가 염려되었다.**
- **위 이유로 전략 패턴을 도입했다.** 기존 벤더사에서 인터페이스를 뽑아내고 각 벤더사 구현체는 해당 인터페이스를 구현하게 했다.
    - 이제 A와 B 벤더사는 서로 느슨한 결합을 가진다. 로직을 변경해도 서로에게 영향을 미치지 않는다.

### 멋쟁이 사자처럼 피드백

- 멋쟁이 사자처럼에서 발표할 자료에 대한 1차 피드백이 들어왔다. 꽤 좋은 평가를 받았다.
    - .. 어제는 이렇게 만들어도 될까? 했는데 지금 보니까 꽤 잘 만든 것 같다. (....)
    - 업계 동향 같은 내용도 추가했으면 좋겠는데, 발표 시간안에 다 할 수 있을지 모르겠다. 이 부분은 별도의 자료로 첨부할까 싶다. (유튜브 등)

### 새로운 기술 스택 도입 가능!

- [호환성 문제로 신규 프로젝트에 새로운 기술 스택을 도입할 수 없다고 생각했는데](https://github.com/jhleed/TIL/tree/master#20191012) 몇 가지 테스트 결과 새로운 기술 스택을 도입할 수 있게 되었다.
    - 새로운 기술이란 자바, 스프링 부트와 클라우드 네이티브 인프라다.
        - 이미 여러 서비스 회사에서 사용하는 기술 스택이라 별로 대단한게 없어 보일수도 있지만, 팀에서 처음 도입하는 기술인 만큼 (내겐 익숙하지만) 큰 도전이라고 생각한다.
    - Kafka 1.1.1 클라이언트는 JDK 1.8 버전만 지원 가능하다고 명시되어 있었는데, 테스트 결과 JDK 11에서도 사용 가능한 것으로 확인되었다. 자바의 하위 호환성 보장이란..

### 변수명 짓는거 너무 어려움

- 어떤 이름을 지어야 이 객체가 뭘 하는지를 명확하게 나타낼 수 있을까. 늘 고민된다.
- [변수와 메소드 이름의 좋은 예를 구글링 해봤다.](https://dzone.com/articles/best-practices-variable-and)
    - 별로 만족스럽지 않다. 이건 너무 범용적인 경우다.
    - 차라리 유명 오픈소스를 까서 보는게 어떨까 싶다.

### 작업 시작 전 리팩토링

- 기존 프로젝트에 새로운 기능을 추가하는데 리팩토링부터 시작한다. (가능한 범위 내에서)
    - **경험상 이렇게 하는게 결국 개발 생산성을 훨씬 높게 만든다.**
- 단, 테스트 코드가 없기 때문에 매우 짧은 주기로 리팩토링을 시행했다.
- 오늘은 아래의 리팩토링을 했다.
    - 단일 책임 원칙 : 하나의 모듈은 하나의 책임만
        - 지금은 클래스 단위로 쪼개기 어려워서 함수 단위로 쪼갰다.
    - 비슷한 맥락의 데이터들은 클래스로 합치기
    - 테스트하기 어려운 부분과 쉬운 부분을 분리
        - 어느 부분까지 테스트 대상에 포함할지 결정

### 2019.10.13 (일)

**멋사 발표자료 작성**

- 오늘도 회사에 왔다. 회사 일 때문은 아니고 10월 27일에 발표할 멋사 자료를 만들러 왔다. 집에서 할까 했지만 34층의 아름다운 뷰를 보고 싶었다.
- **주니어 개발자를 위한 격언**이라는 거창한 주제인데, 발표 시간은 15분이다. 짧은 시간에 어떤 내용을 담아야 강렬한 메세지를 줄 수 있을까..
    - 일단 1차로 작성해서 피드백 요청드렸다. 세 시간 정도 걸렸는데 생각보다 재밌게 만들었다. 근데 기술적 내용도 없고, 너무 짧은 시간에 많은 내용을 표현하려다 보니 내용이 추상적인 감이 있다. 과연 도움이 될까 싶은 기분이 든다.. 좀 고민해보고 필요하면 다시 고쳐야겠다.

**불안함을 원동력으로**

- 내가 젤 자신 없는 것은 휴식이다. 쉬면 어색하고, 불안하다. 쉬는 시간에 뭐라도 하나 더 해야할 것 같다.
    - 휴식을 취하면서도 마음 한켠에는 계속 일 생각이다.
    - 얼마 전까지는 이게 이상하다고 생각했다. 쉴땐 쉬고, 놀땐 놀아야 되는데 계속 일(정확히는 생산적인 무언가)생각을 놓지를 못하는 내가 싫었던 적도 있다.
- 그런데 이제는 생각을 좀 고쳐 먹기로 했다.
    - 내가 지금까지 성취해온 것들 중 많은 부분이 이 불안함이 원동력이 되었을 것이다.
    - 어떻게 보면 이건 남들에게 없는 나만의 축복(?)일 수도 있다.
    - 이 불안함을 성취의 원동력으로 삼고 더 높은 곳으로 향하기로 했다. 나를 있는 수용하자.

### 2019.10.12

**주말에 회사에 나와서 코딩**

- 요즘 여러모로 일이 겹쳐서 스케줄이 폭주하고 있다. 그래도 작년에 비하면 훨씬 즐겁다. 기술도 새롭고 도전적이다! 힘내서 올해 마무리 잘 해야지.

**결국 새로운 기술 스택을 포기하다.**

- 위에 주말에 출근해도 즐겁다고 적었는데, 저녁에 퇴근할 때는 기분이 많이 가라앉았다.
- 새 프로젝트에는 기존 `.NET` 대신 `Java`, `Spring Boot (Batch)` + `Cloud Native Infra` 로 새로운 기술 스택을 도입하려고 했었다.
- **그런데 몇 가지 테스트 도중 위험한 의존성 문제를 발견해서 결국 도입을 포기했다.**
    - JDK 버전 차이로 인한 문제다. 자세히 적기엔 좀 부담스럽고, 여튼 도입하면 여러가지 이슈들이 생길 수 있는 상황.
    - 해결할 수는 있는데 애초에 내가 새로운 기술을 도입하며 계산했던 리스크보다 커진 상황이었다. **내 기술 욕심을 충족시키자고 프로젝트에 큰 리스크를 지게 할 수는 없는 법.** 결국 기존 기술 스택을 유지하기로 했다.
    - 올바른 결정이지만 솔직히 마음 한 구석에 아쉬움이 남는다. 그렇지만 돈을 받고 일하는 이상 회사 프로페셔널의 마인드를 유지해야 한다.

**TIL 잔디 심기, 업로드를 더 편하게**

- 오늘 TIL Repo에 master push를 했음에도 불구하고 잔디밭에 표시가 되지 않는다! 동기화에 시간이 좀 걸리는 것인가? 딱히 잔디밭에 집착은 없지만 그래도 꽉 채우면 예쁠 것 같다. 혹시 모르지, 동기부여가 될 지도? :)

![](Untitled-a272da03-dff0-46ef-8099-b0d2d87a7b79.png)

- [알고 보니](https://plplim.tistory.com/3) **회사 컴퓨터와 개인 깃허브에 등록된 프로필이 달라서 발생한 문제**였다.
    - 회사 컴퓨터는 회사 프로필로 저장되어 있으니까
- 그렇다고 회사 컴퓨터의 깃 프로필을 개인 계정으로 바꿀 수도 없고 TIL Repository 수준에서 User를 설정하는 방법을 알아봐야겠다.
    - 생각보다 쉽게 해결했다. `.git/config` 파일에 user를 아래와 같이 추가해주고 Push 했더니 잔디가 잘 기록된다. 해보진 않았지만. `git config —local` 명령어로도 잘 동작할 듯?

    ![](Untitled-f14068a7-5da3-4754-a6af-ab1948df6a8a.png)

- 이어서 Window OS에서 Push 할때마다 귀찮게 발생하는 `HttpRequestException encounterd` 오류를 없앴다. **Git Window에서 보안 이슈가 있어 패치를 한 것이 원인**이고, [깃 크리덴셜 매니저를 업데이트 해줬더니](https://github.com/Microsoft/Git-Credential-Manager-for-Windows/releases/tag/v1.14.0) 깔끔하게 해결됬다. **이제 TIL 업로드를 더 편하게 할 수 있다!**

### **2019.10.11**

**스택오버 플로우에 [첫 질문](https://stackoverflow.com/questions/58333080/which-is-the-better-combination-spring-boot-with-kafka)을 했다.** 

- 부족한 영어 실력이지만 그래서 더 스택 오버 플로우를 이용하려고 한다. 영어를 자주 쓰다 보면 자연스럽게 늘겠지.
- 두려움을 극복하는 것부터 시작이다.

**적절한 기술 스택 결정하기**

- 실시간 컨슈밍 Job을 만드려고 하는데 경험이 없어서 아래 중 어떤 조합으로 갈지 고민된다.
    1. Spring Boot Batch + Kafka Consumer
    2. Spring Boot + Kafka Consumer
- Spring Boot + Kafka 로 시작하고 필요하면 마이그레이션을 하는 방향으로 생각이 기울고 있다. 데이터 특성상 TPS도 그다지 높지 않고 ..
    - Spring Batch를 써보지는 않았지만 일괄처리에 초점이 맞춰져 있을 것 같다는 생각이 드는데..

**실시간 광고 제어 시스템**

- 비즈니스 플로우를 그렸다. 고수준의 로직이지만 놓친 건 없어 보인다.
    - 클린 아키텍쳐에서 배운 것 대로 데이터베이스에 의존되는 설계를 하지 않았다. 비즈니스 로직과 저장소는 완전 독립적인 존재이다. 비즈니스 로직은 저장소에 의존적이지 않다.
- 저장소로 무엇을 쓰는가를 결정해야 한다. 현재까지는 성능이 그다지 우려되는 상황은 아니기 때문에 인메모리 저장소를 쓰건 RDB를 쓰건 큰 상관은 없다. 운영성을 고려하여 RDB로 기울고 있다. (나중에 쿼리로 데이터 보기 편함)
    - 그렇지만 다른 시스템은 Redis로 부터 데이터를 읽어오는 것이 편하다. 그래서 RDB 및 Redis에 각각 데이터를 저장해주기로 했다. 이 방법이 진짜 맞는지는 고민을 좀 더 해봐야 할 듯.
    - Redis는 TTL을 걸어서 데이터를 초기화시키 편하다는 장점이 있다. (이 데이터는 매일 자정마다 초기화되어야 한다.)

### ~19.10.10

**이번 프로젝트는 내게 여러모로 도전적이다.** 

내 모듈에는 팀 최초로 도입하는 기술 스택이 많다.  `Java` 뿐 아니라 클라우드 네이티브를 도입한다. 

`C#` 을 써보니 딱히 `Java`가 좋다고 생각이 들지는 않는다. 오히려 `C#`에 비해서는 언어적으로 세련되지 못한 부분들이 있다. 하지만 그럼에도 불구하고 `C#`을 버리는 이유는, 결정적으로 플랫폼 차이다. `.NET CORE` + `Azure` 를 쓰면 이런 문제는 없겠지만 우리 회사의 방향이 점차 `Java`로 가고 있기 때문에 기존 .NET에 대한 플랫폼 지원이 부족하다. `Java`와 `Spring`, 그리고 오픈쉬프트와 

목표는 내 모듈부터 시작해서 하나씩 `MSA`지향적인 구조로 바꾸는 것이다. 성공할지는 모르겠다. 하지만 시작도 안하면 100% 실패한다. 

**Git bash를 사용하던 중 이상한 경험을 했다.** 

- [shell script에서 환경변수가 제대로 설정 안되는 현상](https://www.notion.so/jhleed/shell-script-108c237c26cb45f9b4c12df08265f464)

Git Bash의 버그가 강력하게 의심된다. 

한참 삽질하다가 할일이 바빠서 그냥 디렉토리 부분은 하드코딩으로 넣었다. 할일도 많은데 더 이상 시간 낭비할 수는 없다. 집에가서 맥북으로 다시 시도해봐야겠다.

### ~19.10.07

1년만에 TIL을 쓴다. 배움에 대한 기록은 `Notion`에 지나칠 꾸준히 하고 있지만 전부 Private라서 다른 곳에 공개되지 않는다.
타인이 볼 수 있도록 글을 쓰게 되면 좀 더 기록이 꼼꼼해지지 않을까 싶어서 다시 Github에 TIL 기록을 남기기로 한다.

에디터로 어떤 것을 쓸 지가 고민이다. (벌써?)
텍스트만 올리려면 그냥 아무 에디터나 쓰면 그만이지만, 이미지를 자주 쓰는 나의 특성상 매번 이미지 파일을 다운로드 받고, 경로를 지정해주는 일은 너무나도 귀찮기 때문. 귀찮은건 질색.

애정하는 `Notion`에서 바로 요 TIL Repository로 업로드를 할 수 있으면 좋겠다.
Notion에서 Export까지만 수동으로 해주고 해당 경로에서 이미지를 한꺼번에 옮긴 다음에 TIL Repository로 Push를 해주는 스크립트를 짜볼까 한다.

## 2018년 10월

### ~18.10.28

- 선기형과 빅데이터 플랫폼 스터디 결성
    - 아직 자세한 계획은 없지만.. 선기형은 `ELK` & `Kafka`, 나는 `Hadoop Ecosystem` & `Kafka`
    - 드디어 나도 내가 필요한 부분을 함께 공부할 동지가 생겼다!
- 글또 시작
    - 11월 17일에 첫 모임이 예정되어 있다.
    - 학생분들도 많고, 네임드 서비스를 운영하는 회사에서 오신 분들도 많다.
    - 데이터에 관심을 가지신 분들이 많은 것 같은데 좋은 자극 받아가야겠다.
- Kafka` 개념을 학습
    - 카프카 looks like 실시간 데이터 스트리밍 플랫폼
    - 11번가에서 실제로 카프카를 사용하고 있는 사례, 11번가는 에어비엔비, 넷플릭스 등을 참고하였음
    - MSA와 카프카의 조합, 카프카를 backbone으로 사용하고 각 서비스들은 카프카를 이용하여 느슨하게 통신한다.
        - 특정 서비스가 장애가 발생해도, 전체 장애로 퍼지는 것을 방지한다.
            - 이 부분은 카프카가 있어서 장애 전파 방지가 된다기 보다는.. 카프카를 이용함으로 이러한 장애 전파 방지 처리가 더 쉬워졌다는 의미인 것 같다. (어차피 서비스 죽으면 response가 없을 것이기 때문에..)
        - 서비스 장애가 복구가 되면 카프카에 모아두었던 request들을 다시 전송한다.
            - kafka가 데이터를 디스크에 안정적으로 저장하기 때문에 이런 것이 가능
            - 이건 꽤 좋은 것 같다.
- ELK 개념을 학습하였음
    - 희끄무레하게 알고 있던 것을 정의와 사용처를 명확하게 하니 좋았다.

### ~18.10.26

여기서부터는 작성 편의상 음슴체로 다시 회귀하였습니다.

- 광고 리포트 Batch Job에 테스트 코드를 적용하고, DB와의 의존성 분리에 성공<u>(한 것 같다)</u>
- 100% 커버리지는 아니지만 에러 포인트를 잡아내는 것이 그 전에 비해 훨씬 수월해졌다. 야호.
    - 불안정한 Apache kylin…후.~
- 광고 리포트 시스템에 테스트를 작성하기까지의 고민과 그 과정들은 TIL에 담기에는 너무 길어서 별도의 글로 정리해야겠다.

## 2018년 9월

### 18.09.29

- 이베이에 온 뒤로는 `.NET` 환경에서만 개발해서 복습도 할 겸 오랜만에 `Spring Boot` 와 `JPA`를 써서 간단한 API 서버를 만들어 보았습니다. 오랜만에 사용해보니 생각보다 삽질을 많이 했습니다.
- `Testing`에 대한 고민 - DB와의 의존성을 느슨하기 위한 방법들에 대한 고민
    - Repository단에서는 실제 Connection을 사용, 비즈니스 로직 단계에서는 `Mocking` 을 사용해볼까?
        - Repository 단에서 실제로 값을 받아오는 것을 검증하는 단위 테스트를 작성
        - Repository 단위 테스트를 믿고, 비즈니스 로직에서는 `Mocking` 사용
    - 테스트를 위한 별도의 스키마를 생성하는 방법도 고려해봤지만 그다지 좋은 솔루션은 아닌 듯
        - 테이블 변경/추가 시 2벌씩 해야 하기 때문..
- 테스트의 가독성을 좀 더 좋게 하도록 `BDD` 고려 - `Spock` 라이브러리
- 그 외
    - 머신러닝, Redis를 공부하려고 했는데 API 만드는 부분이 테스팅이 너무 재밌다 보니 온종일 저것들만 만들었다..
    - 재밌는 공부, 중요한 공부 사이의 저울질이 어렵다.

### ~ 18.09.28

- 개인적으로 진행하는 프로젝트에 `MSA` 를 적용하고 있습니다. 사실 `MSA`라고 부르기 민망할 정도의 작은 모듈들의 집합이긴 합니다.
    - 이런 작은 규모에도 `MSA`를 적용하는 이유는 아래와 같습니다.
        - 경험
        - 추후 다른 개인 프로젝트에서 해당 모듈들을 재활용하기 위함
    - <u>MSA 적용 후기 : 결국 생산성 문제로 모놀리틱 구조로 회귀하였습니다. 그냥 서비스를 개발하고 추후에 API를 분리하는 것이 더 이득일거라는 생각이 드네요.</u>
- 현재 Account / Board 기능들을 구현 중에 있습니다.
    - 추후 더 적용해볼 만한 기능들이 생각나면 좋을 텐데 잘 떠오르지 않네요.
- 적용된 기술 스택은 `NodeJs`/`Spring Boot`/`ORM` /`VUE.js`/`AWS - EC2` /`REDIS` 입니다.

### 2018.01 ~ 2018.09

`Today I Learned`를 거의 1년 만에 다시 시작합니다.

8개월 동안 새로운 환경에서 여러 가지로 많이 배우며 도전했습니다.

입사 초반에는 정말 새로운 환경에서 배울 게 너무 많아 매일매일 한계까지 짜내어지는 기분이었습니다. (개인 노트에 적기도 바빠서 TIL에 따로 정리해서 기록하지 못했다는 핑계를)

처음 맡은 프로젝트에 극한의 일정과 범위가 할당되었습니다.

회사에서도 야근을 지양하는 분위기고, 개인적으로도 야근을 좋아하는 성격은 아니지만 주어진 일정 내에 맡은 일을 반드시 처리해야 한다는 책임감(부담감) 때문에 처음으로 주말 출근과 휴일 출근을 반복적으로 하는 경험도 했습니다. (한 달 반 정도?)

시간과 품질 사이에서 정말 많은 고민과 갈등을 겪었고 심신도 많이 피곤했지만 **프로젝트는 결국 런칭에 성공했습니다. 정말 뿌듯했습니다.**

지금은 예전부터 도전하고 싶었던 빅 데이터(`Hadoop Ecosystem`) 쪽을 맡게 되어 한창 공부하고 적용하고 있습니다.

- 개인적으로는 인공지능 - `Machine Learning` 쪽을 꽤 재밌게 공부하고 있습니다. 이 분야도 꼭 실무를 경험하고 싶습니다)

요약하면 아래와 같습니다.

- **Business**
    - 광고 플랫폼 - 매력적인 도메인
- **Technology**
    - `Linux` & `Spring` 기반에서 `Window` &amp`.NET Framework`로 전환
    - 빅 데이터 핸들링 - `Hadoop Ecosystem` & `Lambda Architecture`
    - 인공지능 - `Machine Learning` - 아직 실무 X, 개인적으로 공부 중
- **Projects**
    - 새로운 광고 플랫폼 개발 및 런칭 (OBT)
        - 담당 파트 : Account / Report
    - 기존 광고 시스템 유지보수
        - 일 매출 억 단위 유지 중

## 2017년 10월

### ~ 17.10.26

- `신림 프로그래머` 스터디 그룹에서 `Java9`와 `Spring5`를 공부하였고 [회고록](https://github.com/jhleed/study-java9-and-spring5)을 작성하였습니다.
    - [Reactive Programming with JDK 9 Flow API](http://jhleed.tistory.com/99)문서도 번역해 보았습니다.
- 별 생각없이 쓰고 있던 `Logging`에 대해서 왜 쓰는지, 어떤 것이 좋은지를 생각해보는 시간을 가졌습니다.
- `System.out.println`을 썼을때의 단점과 로깅 라이브러리인 `log4j` 여기서 더 진보한 `logback`과 로깅 라이브러리들을 쉽게 갈아끼우기 위한 (파사드, facade 패턴) `slf4j` 등..
- 처음으로 외주를 시작하였습니다. 그리고 개인적으로 좋은 팀도 생겼습니다. 금액이 많고 적고를 떠나서 회사에서 주는 월급 외의 나의 능력으로 버는 수입이 추가되었다는 것에 의미를 두어보려고 합니다.

### ~ 17.10.17

- REST API를 RESTful하게 쓰고 있는가에 대하여 반성하게 하는 글을 보았습니다. [그런 REST API로 괜찮은가](http://slides.com/eungjun/rest#/)
- 요점은 로이 필딩이 제시한 REST한 조건을 만족시켜서 제대로 RESTful한 API를 만들던지 혹은 API의 이름을 바꾸라는 것입니다. (HTTP API? Web API?)
- 여유롭지 않은 재정상황이었지만 아이패드 + 애플펜슬 세트를 구매하였습니다. 생산성을 올리는 일에는 아낌없이 투자하려고 합니다.

---

## 2017년 09월

### 17.09.24

**NodeJS Testing**

- 코딩 테스트 문제를 `javascript`를 이용하여 풀기로 하였습니다. 따라서 `javascript` 에서 사용할 `TDD` 도구가 필요했고 `mocha`라는 테스팅 라이브러리를 알게 되었습니다.
- `IntelliJ` 에서는 `mocha`를 지원해주기 때문에 편하게 테스트할 수 있습니다. 단, 상용 버전이고 `NodeJS Plugin`이 설치되어 있어야 합니다.
- `mocha`는 `BDD`과 `TDD`를 모두 지원하는데 개념적인 차이 이외에 어떤 기능적인 차이가 있는지 알아봐야 할 것 같습니다.

**Testing**

- Kent Beck의 `Test Driven Development: By Example`을 읽어보았습니다. TDD를 하는 방법 자체는 익숙하였지만 그 이면에 담긴 철학적인 내용의 이해를 하는 데 도움이 되었습니다.
- 이전에는 단순히 알고리즘 문제를 푸는 것으로 시작했는데 요즘에는 실무에 조금씩 적용을 하고 있고, 효율성을 느끼고 있습니다. **역시 기술은 적용할 때 뿌듯함이 느껴집니다.**

---

### 17.09.22

**Java**

- `Annotation`의 사용법을 복습하였고 사용 이유에 대해서 다시 한번 생각해보았습니다.
- `Annotation`, `Reflection`과 같이 사용법은 알지만 근본적인 존재 이유를 모르고 사용하는 기술들이 있습니다. 이런 기술들은 안다고 할 수 없기에 지속적인 학습이 필요하다고 생각합니다.

**input tag file copy**

- 하나의 `<input type=file>` 에서 다른 `<input type=file>`으로 파일을 복사하려면 아래처럼 간단한 코드로 처리가 가능합니다. **동료의 시간을 아껴줄 수 있어서 좋았습니다.**

    //btn1에 파일을 올리면 btn2로 복사.
     $("#btn1").on("change", function () {
    	$("#btn2")[0].files = this.files;
    });

---

### 17.09.21

**Github**

- [TIL Repository](https://github.com/jhleed/TIL)를 정리하였습니다.

**IntelliJ**

- 서브모듈을 구성하는 방법에 대해서 고민을 하였습니다.

---

### 17.09.20

**Python 보다는 Js를**

- 최근 자바를 이용하여 코딩 테스트를 연습하던 도중 자바를 대체할 언어를 찾았습니다. 동료인 태환님은 `Python`으로 알고리즘을 푸는데 간결한 문법이 매우 좋다고 하였습니다.
- 저도 `Python`을 고민하다가 결국 `Javascript`로 가기로 하였습니다. `Javascript`는 이미 쓰고있는 언어이기 때문에 별도의 학습 비용이 필요 없고 어느정도 간결하기 때문입니다.

**AWS**

- Awsomeday를 다녀온 뒤 회사에서 브리핑을 하기로 하여 관련 기술 몇 가지를 복습하였습니다.
    - AWS 보안 및 접근제어 관리 서비스 - IAM
    - AWS의 관리형 서비스 및 데이터베이스 서비스 - RDS ,DynamoDB
    - 확장성 있는 아키텍처 구축을 위한 AWS 서비스

**IntelliJ**

- [Module 끼리 Grouping 을 하는 방법](https://www.jetbrains.com/help/idea/grouping-modules.html)을 찾아내었습니다. 덕분에 프로젝트 구성을 좀 더 가독성 있게 구성 할 수 있게 되었습니다.

**Github**

- 학습하는 기술에 대해서 별도의 리파지토리를 운영하고 있습니다.
- 리파지토리의 이름은 study-XX 의 형태를 가지고 있습니다. (ex : study-nodes, study-polymer)
- 이것을 그냥 study 라는 리파지토리 내에 하나로 합쳐버리면 어떨까 싶습니다.

---

### 17.09.13

**MongoDB**

- `Replica Set`과 `Spring Boot`의 연동이 드디어 끝났습니다. MongoDB Instance에서 Replica Set을 설정해줄때 IP를 AWS의 `Public DNS (IPv4)`에 연결해 줘야 합니다.
- 처음에 이 작업을 시작할때는 이정도 디비 세팅정도 책보고 하면 금방 하겠지 했는데, 예상외의 변수가 계속 생겨서 시간이 굉장히 지체되었습니다. (네트워크 방화벽 문제라던지, 권한 문제 등..) 인프라와 DB 쪽을 설정하는 경험이 거의 없었기에 발생한 일이라 생각합니다. 이런 것도 경험할수록 많이 늘겠지요.

**NodeJS**

- 정적인 파일을 요청할 수 있는 권한을 주려고 합니다. 구글에 찾아보니 여러 가지 방법이 있습니다. 어떤 방법이 적합할지는 고민을 해보아야 할 것 같습니다. 프로젝트에서 `NodeJS`의 비중이 작으므로 최대한 단순한 방법을 택하려고 합니다.

### 17.09.11

**MongoDB**

- Replica Set에 인증 처리를 하려고 합니다. Replica Set을 설정할 수 있는 권한을 설정하려면 `cluster admin` 정보가 필요하다는 것을 알게 되었습니다.
- 이전에는 데이터가 꼬이면 데이터가 저장된 디렉토리를 날려버리는 방식으로 (데이터가 얼마 없는 개발 중인 서버라서 가능) 해결을 했었는데 이는 `Wiretiger.wt` 파일도 같이 날려버리므로 문제를 야기할 수 있다는 것을 알았습니다.

**TIL**

- 지난주 금요일에는 처음으로 TIL 기록을 빼먹었습니다. 당일 약속이 있었기 때문입니다.
- 기억을 되짚어서 적으려고 하면 적을 수 있지만 그렇게 하면 마치 숙제 같은 느낌이 들 것 같습니다. **장기적으로 꾸준하게 TIL 을 쓰기 위하여 매일 의무적으로 써야 한다는 강박관념을 버리려 합니다.**

**Maven**

- `maven wrapper` 에 대한 이야기를 동료들었습니다. maven이 별도로 설치되어 있지 않아도 의존성을 가져와서 빌드 할 수 있다고 합니다. 배포에 필요한 과정을 간소화 시켜주는 도구인 것 같습니다.

**IntelliJ Bug**

- Spring Boot 에서 컨트롤러를 만들었는데 테스트 코드에서 의존성 주입이 되지 않는다는 경고 메세지가 나왔습니다. 그러나 실제로 실행을 시켜보면 정상적으로 의존성 주입이 잘 됩니다.
- 맥에서는 이런 현상이 발생하지 않는데, 윈도우에서는 이런 현상이 발생합니다. IntelliJ 자체 버그인 것 같습니다. IntelliJ에 대한 신뢰도가 조금 떨어진 듯한 느낌이 드네요. 이래서 릴리즈되는 소프트웨어에는 사소한 버그도 용납이 되지 않는 것 같습니다. 신뢰도의 문제니까요.

**Spring Boot Testing**

- [Spring Boot의 Testing에 대해서 잘 정리된 글](http://meetup.toast.com/posts/124)이 있어서 해당 글을 참고하여 Spring Boot의 Testing을 공부해 보려고 합니다. 잠깐 훑어봤는데 굉장히 다양한 기능들이 있습니다. `JUnit` 과 `MockMvc` 정도만 사용해봤는데 어떤 기능들이 있는지, 어떻게 테스팅 방법이 개선되었을지 궁금합니다.

---

### 17.09.07

**알고리즘과 TDD**

- 카카오 블라인드 테스트 example 문제를 풀어보았습니다. TDD 방식이 많은 도움이 되었습니다.
- 복잡한 로직을 분할 정복 (Divide and conquer)으로 해결할 수 있었고, 일단 작성한 기능은 정상적으로 동작한다는 보장이 되었기에 성능 개선 등의 리팩토링을 과감하게 시도할 수 있었습니다. 게다가 어느 정도 익숙해지니 TDD 방식으로 코딩을 하는 것에 생산성의 저하가 거의 느껴지지 않았습니다.
- TDD는 설계를 보조해주는 도구로써 참 좋은 것 같습니다. 다만, 현재 저의 레벨에서는 TDD가 설계 그 자체가 될 수는 없는 것 같습니다. 추상적인 알고리즘을 머릿속으로 그려 놓고, 그것을 구체화하는 도구로써는 아주 좋은 것 같습니다.

**개발자 커리어에 관한 조언**

- 이제 대학을 졸업하는 후배가 개발자의 커리어에 관해서 조언을 구하기에 제가 아는 선에서 열심히 조언해 줬습니다. 신입 개발자 때부터 삽질을 참 많이 해왔는데 후배가 조언을 잘 받아들여서 삽질을 조금이나마 덜 한다면 뿌듯할 것 같습니다.

**NodeJS**

- **모든 기술에는 Why가 제일 중요하다고 생각합니다.** [아웃사이더님의 NodeJS 프로그래밍](https://blog.outsider.ne.kr/745) 책으로 NodeJS 의 특징을 학습하였고 어떤 맥락에서 Node를 쓰면 적합한지를 이해하였습니다.
- Node의 역사를 공부하다가 Node가 라이언 달의 **개인 프로젝트로 시작되었음을** 알게 되었습니다. 예전에 개발자로 큰 돈을 버는 방법 중 하나로 개인 프로젝트가 큰 가치를 가진 것이 입증되었을 때.. 라는 글을 보았는데 이와 일맥 상통하는 것 같습니다.

**그 외**

- IntelliJ SSH 연결을 하는 방법을 배웠고 [블로그에 정리](http://jhleed.tistory.com/98)하였습니다. 매번 터미널과 IDE를 번갈아서 보는 귀찮음을 해소하여 주었네요.
- brew cask [install target name]를 하여 유틸리티를 설치하는 방법을 배웠습니다. 왜 개발자들이 맥을 선호하는지 점점 알 것 같습니다.

---

### 17.09.06

**MongoDB**

- Replica Set의 Auto Fail over (and recovery) 작업 마무리
    - Process id 를 주기적으로 체크하여 종료시 자동으로 재시작해주는 shell script를 작성하였습니다. 간단한 로직인데 쉘 스크립트를 쓸 일이 많지 않다 보니 문법을 찾아가면서 작업을 했습니다. 쉘 스크립트를 작성할 일이 많지 않기 때문에 당분간은 이렇게 찾아가면서 해야겠습니다.
    - 주기적으로 특정 작업을 반복하는 기능은 linux crontab 을 이용하였습니다.
- 이로써 클라우드에 MongoDB 를 세팅하는 며칠간의 삽질이 종료되었습니다. 현재는 서비스에서 처리해야 할 데이터가 많지 않기 때문에 샤딩과 같은 처리는 하지 않았지만 MongoDB에 점점 익숙해지고 있는 것 같습니다. 다음 단계를 배울 준비가 되어 있다고 느낍니다.

**JWT**

- 프론트 서버를 별도로 분리하고 데이터를 REST API 로 요청하는 구조입니다.
- API 인증 방식으로 JWT 를 사용하였는데 token 값을 node 를 거치지 않고 바로 브라우저 단으로 저장 (session storage)하려고 합니다.
- token을 클라이언트 단으로 저장하는 것은 좋지 않은 것을 알고 있지만 아래와 같은 이유로 브라우저단에 보관하려고 합니다. 현재는 이렇고, 이후에 생각이 바뀔 수도 있을 것 같습니다.
    1. token에 민감한 정보가 담겨 있지 않음.
    2. jwt 의 만료 시간이 짧고, 브라우저를 종료하면 보관된 정보가 사라진다. (session storage)
    3. 프로젝트의 특성상 하나의 컴퓨터 (브라우저)를 여러 유저가 사용하지 않는다.
    4. jwt 를 처리하는 로직을 별도로 구성하지 않는 것이 추후 REST API 서버를 사용할 다른 프로젝트를 구축하는데에 유리하다. (동일한 로직을 재구성할필요가 없으므로)

**NodeJs**

- 라우팅 등의 간단한 기능을 처리하는 백엔드가 필요하기에 서버 플랫폼으로 NodeJs 를 사용하기로 하였습니다. 노드에 익숙하지 않기에 라우팅이라던지, 레이아웃 구성 방법 등을 공부해볼 예정입니다.
- 개인적으로 서버를 구축하는데에 동적 언어를 사용하는 것을 싫어하는 편입니다. 안정성이 중요한 서버에서는 아직까지는 역시 자바 등의 type safety한 언어를 쓰는 것이 낫지 않나 싶습니다. Spring Boot를 사용하면 생산성도 그렇게 떨어지지 않는 것 같습니다.

---

### 17.09.05

**개발 문화와 업무 효율**

- 누가 어떤 개발업무를 맡고 있고, 일이 어떻게 진행되어가는지를 확인할 수 없었습니다. **이를 해결하기 위한 방안으로 Trello를 제시하였고 지금은 팀원분들이 다 같이 만족하며 사용하고 있습니다.** 무료임에도 상당히 많은 기능을 사용할 수 있습니다.
- 연구실의 개발 문화를 향상시키는데에 기여한 것 같아 뿌듯합니다.

**Message API**

- 브라우저의 탭 간에 데이터를 전달할 수 있는 [Message API](https://caniuse.com/#search=message) 라는 것을 알게 되었습니다. 현재로써는 어떤 용도로만 쓰는지만 알고 있는 상태이고 사용하기 위해서는 학습이 필요합니다.

---

### 17.09.04

**Baby step (small step)**

- 테스트할 때 작은 것부터 하면 오류를 금방 찾을 수 있는 것 같습니다. 대량 데이터를 한 번에 실행하면 어디서부터 잘못되었는지를 찾기가 힘듭니다.

**Testable 한 코드 작성**

- 단위 테스트는 독립적인 하나의 기능을 테스트해야 하는데 함수 하나에 여러 가지 기능이 들어가 있으면 해당 기능을 테스트하는 의미가 반감되게 됩니다. (단일 기능 동작 보장)
- 이를 최대한 활용하기 위해서는 메서드를 독립적인 기능 단위로 쪼개고 (SRP) 각 메서드마다 테스트 코드를 만드는 것이 나을 것 같습니다.
- 위의 사항들은 이론적으로는 알고 있었는데 실제 실무에 적용하다 보면서 직접 겪다보니 더 와닿는 것 같습니다.

**MongoDB**

- AWS에 Replica Set 의 기본적인 설정을 완료하였습니다.
- Replica 서버들끼리 연결이 안되서 며칠동안 삽질을 했는데 결국 AWS의 Security Group의 inbound 설정 문제였습니다.
- 네트워크 문제면 mongodb의 bound 부분이랑 infra의 방화벽 설정의 문제일 가능성이 가장 큰 것 같습니다. 앞으로 이처럼 연결 거부 문제가 생기면 이 부분에 포커스를 맞춰야겠습니다.

---

### 17.09.03

**Jenkins**

- 개인 AWS 에 CI를 설치하였습니다. [잘 정리된 메뉴얼](http://sanketdangi.com/post/62715793234/install-configure-jenkins-on-amazon-linux)을 따라하다보니 그렇게 어렵지 않게 설치 할 수 있었습니다. 개인 프로젝트에 적용할 [지속적 배포(Continuous delivery, CD)](https://aws.amazon.com/ko/devops/continuous-delivery/)의 첫 걸음이 되었으면 합니다.

**SMS (Simple Message Server)**

- 메일을 전송해주는 심플한 서버를 생성하였습니다. 어렵지 않은 작업이었지만 이전에 메일 서버를 만들어 본 적이 없었기에 의미가 있었습니다. 다음번에는 SMS를 전송해주는 서버를 만들어보려고 합니다.

### 17.09.02

**Algorithm**

- hackerrank에서 문제를 조금 풀어보았습니다. 30 days of code라는 기본 튜토리얼인데 하루에 1개씩 문제를 제공해줍니다. 현재 2일차까지 풀었는데 문제 자체는 매우 쉽습니다. 이 코스는 아마 입문자를 대상으로 한 것 같습니다. 그러나 영어를 해석하는데에 시간이 조금 걸리긴 합니다. 개발자에게도 영어공부가 꾸준히 필요한 것을 체감합니다. 입문자 레벨을 그만 공부하고 Data structure / Algorithm 을 중급과정부터 풀어보고 싶은데 한번 시작했기 때문에 어떻게든 끝내보고 싶은 오기도 생기게 되네요.

---

### 17.09.01

- **실서버의 데이터를 대량으로 수정할 일이 생겼습니다.** 현재 사용하고 있는 MongoDB의 특성상 한번에 쿼리를 날려 변경하는 것이 불가능하였고 (스크립트를 사용한다면 되긴 하지만) 이런 요구사항이 언제 다시 발생할 지 모르기 때문에 직접 코드를 수정하는 로직을 API 서버에 작성하였습니다. 프로덕션 데이터를 수정한다는 것은 매우 민감한 일이였기에 테스트 코드를 단계별로 작성하고, 잘못되었을 경우를 대비해서 원래 상태로 돌려놓는 리커버리 코드도 작성하였습니다. 데이터베이스에 접근하기 때문에 단위 테스트를 한번 돌릴때 5분정도가 소요되었습니다. 현재 읽고 있는 `Effective Unit Testing` 에서는 단위 테스트를 작성할때에 데이터베이스 접근을 피하는 것을 권고하는데 이런 경우에는 어떻게 해결해야 할지 알아봐야겠습니다.
- 동료인 태환님으로부터 알고리즘 연습 등을 할 수 있는 사이트인 [hackerrank](https://www.hackerrank.com/)를 소개받았습니다. 단순히 알고리즘등을 연습할 수 있는 것이 아니라. 특정 회사들에서 알고리즘 대회 등을 주최하는데 여기에서 우수한 성적을 올리면 상금 등으로 보상을 주거나 라이센스를 줘서 입사시 혜택을 받을 수가 있다고 합니다. 알고리즘 연습에 외재적 보상이 따르면 동기부여에 보탬이 될 것 같습니다.

## 2017년 08월

### 17.08.31

- 여전히 `MongoDB Replica Set`과 씨름을 하였습니다. AWS에 떠있는 3대의 서버 중 1대에 제대로 네트워크가 연결이 되지 않는 현상이 발견되었습니다. 공식 메뉴얼대로 똑같이 따라했는데 안되면 어디서부터 잘못됬는지 찾기 어렵고 참 난감합니다.
- `AWS`에 `부트스트래핑`이라는게 있다는 것을 팀원분에게 들었습니다. 또 멀티 인스턴스에 동시에 커맨드를 날릴 수 있는 기능도 있다고 합니다. AWS 는 정말 편한 클라우드 서비스이면서 배울 것도 참 많은 것 같습니다.
- 조대협님께서 쓰신 [대용량 아키텍쳐와 성능 튜닝](http://www.yes24.com/24/goods/17018954) 책을 빌렸습니다. 전부 이해하기에는 아직 어렵고 REST API 설계와 NoSQL 설계 부분을 참고해보려고 합니다.

---

### 17.08.30

- 주로 `MongoDB Replica Set` 을 학습하고, 설정하는데에 많은 시간을 사용했습니다.
- 화요일에 고민했었던 프로젝트를 결국 `NodeJS` + `Polymer Library`로 구성하기로 했습니다. 프론트의 비중이 큰 프로젝트이므로 Node로 가볍게 라우팅만 해주는 기능을 제공하고 백엔드 로직은 별도의 서버에서 처리하여 API 형식으로 제공받는 것으로 결론지었습니다.

---

### 17.08.29

- 크리스티나 초도로우의 `MongoDB 완벽 가이드`라는 책을 참고하여 MongoDB Replication Set을 공부하고 있습니다. 간단한 Example을 만드는 것까지는 간단했는데 실제로 구상하려니까 고려사항이 많은 것 같습니다.
- 개발자커리어 Young Community 연합세미나 이력서 피드백을 정리하여 [블로그에 포스팅](http://jhleed.tistory.com/97)하고 페이스북에 공유했는데 생각했던 것보다 많은 분들(1000분)이 블로그를 방문해주셨습니다.
- 회사에서 진행하고 있는 프로젝트의 Back end 로 NodeJs를 쓸지, Spring boot를 쓸지 고민이 됩니다. (Front end는 Polymer 라이브러리를 사용)
    - 가벼운 경량 백 엔드로 NodeJs 를 사용하고 싶으나 NodeJs 에 익숙하지 않음
    - 차라리 익숙한 Spring을 쓰는 것이 생산성이 나을 듯한데 그나마 그중 가볍게 사용할 수 있는 Spring Boot을 사용하려고 함
    - 고민 1 : View 단은 JSP를 사용하게 될 듯한데 Web Component Library인 Polymer와 올바른 궁합인가? (HTML import)
        - JSP가 아니라 thymeleaf를 사용하는 방법도 고려해 볼 것
    - 고민 2 : WAS는 정적 자원을 제공하는데 최적화되어있지 않다. 이 문제는 어떻게 해결해야 하는가.

---

### 17.08.28

[Polymer의 style 부분을 학습하고 정리하였습니다.](https://github.com/jhleed/study-polymer/tree/master/style)

개인적으로 사용하는 REST API 서버의 프레임워크를 spring에서 spring boot로 변경하였습니다.
별다른 충돌 없이 매끄럽게 변경되었습니다. git에서 ignoring하는 JRebel의 rebel.xml 부분만 조금 신경썼으면 완벽했을 것 같습니다.

대학을 졸업하고 첫 직장에서 개발을 처음 알려주신 [넷스루의 오재훈 개발이사님](https://www.facebook.com/jaehoon.oh.503)과 저녁을 먹었습니다.
테스트와 소프트웨어의 품질, 개발 문화에 대한 이야기를 나누었고 아주 재밌었습니다.

---

### 17.08.27

어제 개발자커리어 세미나를 다녀오고 나서 github, blog를 재정비해야 되겠다고 생각했습니다.

여기에는 선배 개발자 세션에서 오마이트립 CTO로 계신 [이규원](https://justhackem.wordpress.com/) 님의 영향이 컸습니다.

> 잘 관리된 깃허브와 블로그가 아니라면 이력서에 쓰지 마세요.

스스로 블로그가 잘 정리되어있었는지 생각을 해보는 계기가 되었습니다.

지금까지 딱히 남에게 보여주기 위한 글이라기보다는 제 생각을 정리하는 글이 많았기에 이력서를 검토하시는 분의 기준으로는 정성이 부족한 포스팅이 감점 요소일 것이라는 생각도 조금 들긴 합니다. 우려도 조금 되고요.

그러나 **결국 블로그는 이대로 내버려 두려고 합니다. 잘못된 정보가 아닌 한 누군가에게는 도움이 될 것이라는 생각**에서입니다.

---

### 17.08.26

[개발자커리어 Young Community 연합세미나](https://onoffmix.com/event/108861)에 참여하였습니다.

- 초반 강의 대부분은 주로 대학생들, 혹은 1~2년 차를 대상으로 해서 크게 와닿는 부분은 없었습니다.
- 그렇지만 선배 개발자분들의 이력서 피드백이라든지 사회를 보시는 분의 피드백은 큰 도움이 되었습니다. 이력서를 수정하고 블로그를 재정비해야겠다는 생각이 들었습니다.

강의를 마치고 친구와 강남에서 열리는 IT인들의 치맥 파티에 갔습니다. 유익한 정보 등을 얻으려고 갔는데 결국 치맥만 실컷 먹고 돌아왔습니다 (..)

---

### 17.08.25

### MongoDB

Replica set에 대한 개념을 알아보았습니다. [이곳](https://unagi44.wordpress.com/2015/09/10/mongodb-replica-set%EC%9D%98-%ED%95%84%EC%9A%94%EC%84%B1-3/)에 설명이 참 잘 되어 있었습니다.

### Spring Boot

Spring Boot와 JRebel 연동이 제대로 안 되는 현상을 [해결](http://jhleed.tistory.com/96)하였습니다.

[IntelliJ에서 Spring Boot를 생성하는 기능](http://blog.saltfactory.net/creating-springboot-project-in-intellij/)이 참 잘 되어 있다고 생각했습니다.
생성부터 기본적으로 필요한 플러그인 (롬복 등….)을 한 번에 가져와 줍니다.

Spring Boot의 생산성을 직접 체험했습니다.
게다가 `IntelliJ`와 같은 Idea를 쓰면 `application.property`에 자동완성까지 지원해줍니다.
토이 프로젝트라든지, 외주를 처리할 때는 스프링 부트를 쓰면 **구현에 드는 시간이 많이 줄어들 것 같습니다.**