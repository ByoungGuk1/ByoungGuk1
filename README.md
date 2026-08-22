## Profile

🧑 **Name** | SongByoungGuk <br />
📧 **Email** | bksong121212@gmail.com <br />

정상 흐름을 구현하는 데서 멈추지 않고, **데이터 정합성·동시성·재처리·멱등성**까지 고려하는 Java/Spring 백엔드 개발자입니다.

## Tech

- ⚙️ Backend | Java 17, Spring Boot, Spring MVC, Spring Security, Spring Batch, JPA/Hibernate, MyBatis
- 🗄️ Data | MariaDB, Oracle, MySQL, PostgreSQL, Redis
- 🔗 API / Integration | REST API, JWT, OAuth2, Swagger/OpenAPI, WebSocket/STOMP
- 🎨 Frontend | React, JavaScript, TypeScript, Thymeleaf, jQuery
- 🧪 Test / Infra | JUnit 5, Mockito, AssertJ, MockMvc, H2, Docker, Git/GitHub

## Links

- 💼 **Portfolio** | <a href="pdf/포트폴리오/종합.pdf">Portfolio (PDF)</a>
- 📄 **Notion** | https://www.notion.so/2b71ad5726a880249391e9c062cc1f53?source=copy_link <br />
- 💻 **Velog** | https://velog.io/@bksong9903 <br />


![](./profile-3d-contrib/profile-night-rainbow.svg)


## Team Projects

### MARIA | RIA 국내시장복귀계좌 관리 시스템

- Repository: [returns-7/maria](https://github.com/returns-7/maria)
- Period/Team: 2026.07~2026.08 · 5명
- Role: Account·Settlement 도메인, MyData 계좌 동기화 흐름
- Key Topics: CAS, Redis ZSET Retry Queue, Ownership Token, Spring Batch, REQUIRES_NEW, Append-only Retry
- Stack: Java 17, Spring Boot 3.5.16, MyBatis, MariaDB, PostgreSQL, Redis, Spring Batch, Thymeleaf

### WebNest | 알고리즘 학습·커뮤니티

- Repository: [Frontend](https://github.com/NullPoint-team/WebNest_front) | [Backend](https://github.com/NullPoint-team/WebNest_back)
- Period/Team: 2025.07~2025.11 · 5명 · 팀장
- Role: 인증·회원 백엔드, API 연동 기준, WebSocket/STOMP·LLM 기능
- Stack: Java, Spring Boot, Spring Security, JWT, OAuth2, MyBatis, Oracle, Redis, React

### EV119 | 응급·건강 정보 관리 플랫폼

- Repository: [Frontend](https://github.com/evee-EV119/EV119-react) | [Backend](https://github.com/evee-EV119/EV119-spring)
- Period/Team: 2025.11~2025.12 · 4명
- Role: 마이페이지, 건강정보·복용 약·알레르기·긴급 연락처 API
- Stack: Java, Spring Boot, Spring Security, JWT, JPA/Hibernate, QueryDSL, Oracle, React

## Project Details

<details>
<summary><strong>팀 프로젝트 · MARIA Account/Settlement 상세 보기</strong></summary>

### 프로젝트 개요

- RIA 계좌 심사부터 해외주식 입고·매도·가환전·확정산·세제혜택까지 처리하는 증권사 백오피스
- MARIA, MyData, Return Securities가 DB를 공유하지 않고 REST API로 통신하는 다중 시스템 구조
- Account 도메인과 Settlement 도메인, MyData 계좌 동기화 흐름 담당

### Account

- 고객별 1계좌, 계좌번호 중복 방지, 5천만 원 합산 한도와 상태 전이를 서비스 검증과 DB 제약으로 이중 방어
- `expectedCurrentLimit`를 조건으로 포함한 Compare-And-Set 방식으로 동시 한도 변경의 Lost Update 감지
- DB Commit 후 MyData 동기화 실패를 Redis ZSET Retry Queue에 예약하고 단계적 Backoff로 재처리
- `lockToken`과 `taskToken`을 분리하고 Lua Script에서 비교·삭제·재예약을 원자적으로 수행해 오래된 Worker의 상태 덮어쓰기 차단

### Settlement

- 매도 체결 트랜잭션에 `Propagation.MANDATORY`로 가환전을 참여시켜 매도만 남고 가환전이 누락되는 중간 상태 방지
- Guard Row `SELECT FOR UPDATE`와 `runId`로 동일 업무일의 중복 Batch 생성과 오래된 비동기 Job 실행 차단
- `ExecutionContext`에 커서를 저장하고 Keyset Pagination으로 PENDING Item을 100건씩 처리
- Item별 `REQUIRES_NEW` 독립 트랜잭션으로 성공 건을 보존하고 실패 건만 기록·재처리
- 기존 실패 행을 수정하지 않고 새 Item을 추가하는 Append-only Retry와 `FINALIZED` 재확인으로 추적성·멱등성 확보
- 환율 API를 DB 트랜잭션 밖에서 조회하고, 같은 Batch 내 정상·실패 결과를 공유해 외부 호출과 Lock 점유 시간 감소

### 설계 한계와 개선 방향

- DB Commit과 Redis Task 등록 사이의 Dual Write 구간은 Transactional Outbox 도입 후보
- Lock Watchdog·Lease 연장, Testcontainers 기반 장애 주입, Retry 운영 지표는 후속 개선 과제

---

<img width="1920" height="1080" alt="1" src="https://github.com/user-attachments/assets/d63146d8-bf22-43c1-81be-8814dae2dc55" />
<img width="1920" height="1080" alt="2" src="https://github.com/user-attachments/assets/0d867fa7-1999-4600-918f-6e6b1f994d53" />
<img width="1920" height="1080" alt="3" src="https://github.com/user-attachments/assets/dc3a6d52-d587-4d32-a428-c12de2511760" />
<img width="1920" height="1080" alt="4" src="https://github.com/user-attachments/assets/969c0bb1-902c-4557-994b-2c0fc3064a86" />
<img width="1920" height="1080" alt="5" src="https://github.com/user-attachments/assets/25258393-5b6e-4d8c-8dce-ca77322d8407" />
<img width="1920" height="1080" alt="6" src="https://github.com/user-attachments/assets/28735916-b9e2-4bdf-8c06-7e11b961b395" />
<img width="1920" height="1080" alt="7" src="https://github.com/user-attachments/assets/e976c916-b9ff-4ee5-a7a4-e11170558a5a" />
<img width="1920" height="1080" alt="8" src="https://github.com/user-attachments/assets/b3111d07-895e-4355-8b25-d979f9066293" />
<img width="1920" height="1080" alt="9" src="https://github.com/user-attachments/assets/cdec1063-26bf-4a16-9f7c-918859dc9108" />
<img width="1920" height="1080" alt="10" src="https://github.com/user-attachments/assets/2645d43f-9a8a-4f69-a57d-f7cc6a3fd99e" />
<img width="1920" height="1080" alt="11" src="https://github.com/user-attachments/assets/02945979-3f6d-4b17-8776-a4e70aaa70e2" />
<img width="1920" height="1080" alt="12" src="https://github.com/user-attachments/assets/dbb0bc3c-23c4-49c7-8d19-972860892ef7" />
<img width="1920" height="1080" alt="13" src="https://github.com/user-attachments/assets/4fce9e25-5b55-4bd4-acf2-5dce395230ee" />
<img width="1920" height="1080" alt="14" src="https://github.com/user-attachments/assets/9c5c6f55-7c34-4f89-8088-cb4e4725392d" />
<img width="1920" height="1080" alt="15" src="https://github.com/user-attachments/assets/297f84f5-b97a-4377-90d8-ee7bafdb8c16" />
<img width="1920" height="1080" alt="16" src="https://github.com/user-attachments/assets/2d67b06d-ea99-428b-9565-0fd2ab40191b" />
<img width="1920" height="1080" alt="17" src="https://github.com/user-attachments/assets/b94e1567-f91d-41c6-8ea0-af5a4fb588bd" />
<img width="1920" height="1080" alt="18" src="https://github.com/user-attachments/assets/0d27733e-7d6b-4f59-b19c-9ad9cf6eace3" />
<img width="1920" height="1080" alt="19" src="https://github.com/user-attachments/assets/f76b086e-30ab-4ced-8a5d-97244e5d927d" />
<img width="1920" height="1080" alt="20" src="https://github.com/user-attachments/assets/abdf4851-9079-4ff3-bd66-41408b5febac" />
<img width="1920" height="1080" alt="21" src="https://github.com/user-attachments/assets/79cdf3bc-603b-420b-a558-b14a69a2da20" />
<img width="1920" height="1080" alt="22" src="https://github.com/user-attachments/assets/9a21c624-2fe9-4065-9df0-8113dd91039a" />
<img width="1920" height="1080" alt="23" src="https://github.com/user-attachments/assets/85525b85-e822-46cb-9e94-5bf66fbb73c2" />
<img width="1920" height="1080" alt="24" src="https://github.com/user-attachments/assets/26abb64c-021e-4efc-b514-5dd05b864cfe" />
<img width="1920" height="1080" alt="25" src="https://github.com/user-attachments/assets/97cee07c-9242-44c4-9ce8-757ec62fc798" />
<img width="1920" height="1080" alt="26" src="https://github.com/user-attachments/assets/ab6ef4bc-d375-49ae-b718-afee745bb87e" />
<img width="1920" height="1080" alt="27" src="https://github.com/user-attachments/assets/f5e22586-e9ac-4b69-88a7-6fa998015368" />
<img width="1920" height="1080" alt="28" src="https://github.com/user-attachments/assets/8073458f-7c97-45ec-9d78-b6a072a902a0" />
<img width="1920" height="1080" alt="29" src="https://github.com/user-attachments/assets/88c90273-7a68-4340-b0f8-21f0c922ca26" />
<img width="1920" height="1080" alt="30" src="https://github.com/user-attachments/assets/63b16217-8f9b-4221-818c-eb11687ae3ec" />
<img width="1920" height="1080" alt="31" src="https://github.com/user-attachments/assets/f2be41a7-921f-41b3-9dd9-ee8744f64eeb" />
<img width="1920" height="1080" alt="32" src="https://github.com/user-attachments/assets/c44fa9fc-224f-44c2-8143-731886b1e731" />
<img width="1920" height="1080" alt="33" src="https://github.com/user-attachments/assets/c8053301-d48a-4e1d-b29e-478eea75b3c6" />
<img width="1920" height="1080" alt="34" src="https://github.com/user-attachments/assets/6ddbddf1-be2d-4e32-b980-21a27a893a8b" />
<img width="1920" height="1080" alt="35" src="https://github.com/user-attachments/assets/d6703359-2f2f-413c-9ceb-cefffd72c142" />
<img width="1920" height="1080" alt="36" src="https://github.com/user-attachments/assets/4d5b50b6-336c-498e-afa7-ae720aaa307b" />
<img width="1920" height="1080" alt="37" src="https://github.com/user-attachments/assets/84956b34-5a24-4748-9a79-3a449fa9c3b1" />
<img width="1920" height="1080" alt="38" src="https://github.com/user-attachments/assets/0800aa34-6799-4e09-86dd-787604d69a52" />
<img width="1920" height="1080" alt="39" src="https://github.com/user-attachments/assets/35de3c04-f93a-4e9e-b445-25553c4fceb5" />
<img width="1920" height="1080" alt="40" src="https://github.com/user-attachments/assets/d4061d8a-7fc3-49f3-8a34-482260dc44ca" />
<img width="1920" height="1080" alt="41" src="https://github.com/user-attachments/assets/f0ac8f16-eea5-4c4b-9b9a-8dad6e499ef4" />
<img width="1920" height="1080" alt="42" src="https://github.com/user-attachments/assets/a3117dce-1bfd-4302-8a95-33ff187e4ccd" />
<img width="1920" height="1080" alt="43" src="https://github.com/user-attachments/assets/2afb99a3-9bd3-46d1-bad8-82382ccb597e" />
<img width="1920" height="1080" alt="44" src="https://github.com/user-attachments/assets/611347c6-4f6c-4fb2-8cfc-ba42d6926dac" />
<img width="1920" height="1080" alt="45" src="https://github.com/user-attachments/assets/d7192e49-130b-40d4-bd71-67f8297eedf0" />
<img width="1920" height="1080" alt="46" src="https://github.com/user-attachments/assets/8ff01f4a-9563-4e69-b0e1-e9b0364407e2" />
<img width="1920" height="1080" alt="47" src="https://github.com/user-attachments/assets/8aef4f9e-bb10-45d2-bcae-1d334729b868" />
<img width="1920" height="1080" alt="48" src="https://github.com/user-attachments/assets/3a65ecc3-08f5-4125-a03e-983a5e941a9d" />
<img width="1920" height="1080" alt="49" src="https://github.com/user-attachments/assets/050fb36d-47ab-465c-894d-4db7ef5dc949" />
<img width="1920" height="1080" alt="50" src="https://github.com/user-attachments/assets/7502dce8-00c0-4455-91ce-5b91ca2656d5" />
<img width="1920" height="1080" alt="51" src="https://github.com/user-attachments/assets/a9887af8-4f1d-4579-8324-157a4218dd7d" />
<img width="1920" height="1080" alt="52" src="https://github.com/user-attachments/assets/5a8395d1-174d-4994-9208-d52e54568537" />
<img width="1920" height="1080" alt="53" src="https://github.com/user-attachments/assets/98c17ab4-673c-4cc2-b7cb-64c5ae8830f1" />
<img width="1920" height="1080" alt="54" src="https://github.com/user-attachments/assets/46618395-8cc7-4789-973d-33aeeb0f74d6" />
<img width="1920" height="1080" alt="55" src="https://github.com/user-attachments/assets/132a1dcb-44a5-495a-8802-69873956c49a" />
<img width="1920" height="1080" alt="56" src="https://github.com/user-attachments/assets/8a5e480f-1376-4755-8c37-e4869872cc50" />
<img width="1920" height="1080" alt="57" src="https://github.com/user-attachments/assets/d6fabe66-57e6-4666-ae98-71ab04654f2b" />
<img width="1920" height="1080" alt="58" src="https://github.com/user-attachments/assets/40b4e680-06de-401d-9c22-3877fd1cd427" />
<img width="1920" height="1080" alt="59" src="https://github.com/user-attachments/assets/fc492600-c80b-46b6-bf19-de89e496fb12" />
<img width="1920" height="1080" alt="60" src="https://github.com/user-attachments/assets/7b50e20c-960e-4d5a-b7a3-28267c26878a" />
<img width="1920" height="1080" alt="61" src="https://github.com/user-attachments/assets/fdf3f87c-1e71-4a93-bc7d-d7ba660799c5" />
<img width="1920" height="1080" alt="62" src="https://github.com/user-attachments/assets/016f0453-ec82-49d5-99af-932bda131aa0" />
<img width="1920" height="1080" alt="63" src="https://github.com/user-attachments/assets/3d704fbb-0f42-46e6-a8f0-951891cba5c0" />
<img width="1920" height="1080" alt="64" src="https://github.com/user-attachments/assets/7b6a850c-3847-4583-bbb0-4f9742e86660" />
<img width="1920" height="1080" alt="65" src="https://github.com/user-attachments/assets/cdf602e7-3d17-48b2-90e0-34bfd47597f0" />
<img width="1920" height="1080" alt="66" src="https://github.com/user-attachments/assets/1d6f180c-a693-4168-b439-286d3c07ea5b" />
<img width="1920" height="1080" alt="67" src="https://github.com/user-attachments/assets/d716910b-3afd-41f3-a6bf-da2fa413dc9c" />

</details>

<details>
<summary><strong>팀 프로젝트 #1 · WebNest 상세 보기</strong></summary>

## 팀 프로젝트 #1

WebNest - 10대를 대상으로, 프로그래밍을 처음 접하는 학생들도 부담 없이 들어올 수 있는 서비스

### 사용 기술

- Language : Java(JDK17), HTML, CSS, JavaScript <br />
- Server, Cloud : Apache Tomcat 9.0 <br />
- Framework : Spring Boot 3.2.x, React 18.x <br />
- DB : Oracle 21C, Redis 8.x <br />
- IDE : IntelliJ IDEA 2025.2.3, Visual Studio Code <br />
- API, Library : STOMP(WebSocket), Swagger, coolSMS, OpenAI, Monaco Editor, Swiper API, Stream API,OAuth2, Spring Security, JWT <br />
- DevOps, Tools : Git, GitHub, Docker, Figma, ERDCloud <br />

### 내 역할 (팀장)

1. 로그인, 회원가입 구현 <br />
2. OAuth2 소셜로그인 구현 <br />
3. JWT 인증 <br />
4. Redis refresh token 관리<br />
5. OpenAI API 연동<br />
6. WebSocket/STOMP 끝말잇기<br />
   <br />
   <img src="images/webnest/메인.png" alt="webnest_프로젝트_메인화면" width="600" />

### 핵심 기능

멀티게임 - LLM을 활용한 끝말잇기

### 트러블슈팅 #1

- 상황
  LLM 사용 시 데이터를 가져오지 못했는데에도 다음 코드가 실행되는상황

- 해결 방법
  Map을 통한 Cache 변수를 생성하여 값을 저장했습니다.
  최대 3번까지 요청하며, 필요한 값인 단어와 설명 부분을 Cache에 저장합니다.값을 정확하게 가지고 왔다면 설명을 바로 리턴해주고,
  값을 가져오지 못한 경우 확인을 위해 문자열 메세지를 바로 응답해 주었습니다.

- 해당 경험을 통해 알게된 점
  API 사용 시 정해진 요청 경로에 정해진 요청 값을 정해진 타입으로요청해야 하고, 응답을 받는 경우에도 정해진 이름과 타입으로 응답을받아야 한다는 것을 알았습니다.
  이전 OAuth2.0의 경우와 마찬가지로 값이 응답되기 전에 다음 로직을실행시켜버리는 비동기 문제가 발생 할 수 있기에 그 부분도 유의하며로직을 작성해야했습니다.
  따라서 API를 사용할 경우, 요청을 위한 객체와 응답을 위한 객체, 두가지가 필요하다는 것을 알게 되었습니다.

### 트러블슈팅 #2

- 상황
  소셜 회원 가입 시 기본 입력값이 정확하게 VO로 전달되지 않는 상황

- 해결방법
  OAuth2.0를 통해 들어오는 값을 데이터베이스에 저장하기 위해 만든 VO에 저장할 때,
  닉네임 또는 Provider 이름을 사용하여 중복되지 않는 임의의 값을 저장해두었습니다.
  그 후 데이터베이스에 사용자 등록을 마친 다음 바로 로그인이 적용되도록 해주었습니다.

- 해당 경험을 통해 알게된 점
  OAuth2.0의 경우 모든 사이트의 값이 동일한 값으로 전달되는 것이 아닌, 각 사이트에 정해진 이름으로 값을 전달해 주었습니다.
  또한, 값을 전달받는 즉시 이름을 바꿔 저장하는 경우,비동기 문제가 발생하여 값이 제대로 저장되지 않을 수 있다는 것을 알게 되었습니다.
  이를 통해 추후 값을 요청하고 응답하는 경우, 해당 로직이 동기적인지 비동기적인지 한차례 더 생각하게 되는 계기가 되었습니다.

### 간략 시스템 구성도

<img src="images/webnest/시스템구성도.png" alt="webnest_시스템구성도" width="600" />
<img src="images/webnest/서비스설계.png" alt="webnest_서비스설계" width="600" />

### 끝말잇기

<img src="images/webnest/끝말잇기.png" alt="webnest_끝말잇기" width="600" />

### 로그인 화면

<img src="images/webnest/로그인.png" alt="webnest_로그인" width="600" />

### ID/PW 찾기

<img src="images/webnest/IDPW찾기.png" alt="webnest_ID_PW_찾기" width="600" />

</details>

<details>
<summary><strong>팀 프로젝트 #2 · EV119 상세 보기</strong></summary>

## 팀 프로젝트 #2

EV119 - 공공데이터를 활용하여 빠르게 응급실에 도착하고 빅데이터를 활용하여 간단한 응급처치에 대한 조언을 구할 수 있는 서비스

### 사용 기술

- Language : Java(JDK17), HTML, CSS, JavaScript <br />
- Server, Cloud : Apache Tomcat 9.0 <br />
- Framework : Spring Boot 3.2.x, React 18.x <br />
- DB : Oracle 21C, Redis 8.x <br />
- IDE : IntelliJ IDEA 2025.2.3, Visual Studio Code <br />
- API, Library : JPA, OAuth2, Spring Security, JWT, OpenAI,카카오 맵 API, 응급실 정보 API , 외상센터 정보 API, ...<br />
- DevOps, Tools : Git, GitHub, Docker, Figma, ERDCloud <br />

### 내 역할

1. 마이페이지 - 회원정보 수정, 비밀번호 변경 <br />
2. 마이페이지 - 건강정보 조회 및 관리 <br />
3. 마이페이지 - 과거 병원 방문이력 <br />
4. 마이페이지 - 회원 탈퇴 <br />

<img src="images/ev119/메인.png" alt="ev119_프로젝_메인화면" width="600" />

### 핵심 기능

입력된 사용자의 정보를 활용하여 응급 상황 발생 시 적절한 조치 방법 제공

### 간략 시스템 구성도

<img src="images/ev119/시스템구성도.png" alt="ev119_시스템구성도" width="600" />

### 데이터베이스 구조

<img src="images/ev119/ERDCloud.png" alt="ev119_ERDCloud" width="600" />

### 기능명세서

<img src="images/ev119/기능명세서.png" alt="ev119_기능명세서" width="600" />

### UI/UX 가이드

<img src="images/ev119/UIUX가이드.png" alt="ev119_UI_UX_가이드" width="600" />

### 계정관리

<img src="images/ev119/계정관리.png" alt="ev119_계정관리" width="600" />

### 건강정보관리

<img src="images/ev119/건강정보관리.png" alt="ev119_건강정보관리" width="600" />

</details>
