# 3. Class Diagram

---

## 3.1 DB 관리 Class diagram

## DB Class Diagram

<img width="2000" height="2000" alt="Image" src="https://github.com/Let-s-Study-Now/Let-s-Study-Now_doc/blob/main/SDS/3.%20Class%20diagram/images/ClassDiagram_1.png?raw=true" />

### MemberRepository

**Description:** 회원 엔티티를 관리하는 인터페이스

**Operations**

| **Name**           | **Argument**      | **Returns**        | **Description**            |
| ------------------ | ----------------- | ------------------ | -------------------------- |
| `existsByEmail`    | `String mail`     | `boolean`          | 회원 이메일 중복 여부 확인 |
| `existsByUsername` | `String username` | `boolean`          | 회원 아이디 중복 여부 확인 |
| `findByUsername`   | `String username` | `Optional<Member>` | 아이디로 회원 조회         |

---

### MemberService

**Description:** 회원가입, 로그인, 프로필 조회 처리

**Attributes**

| **Name**                       | **Type**                       | **Visibility** | **Description**  |
| ------------------------------ | ------------------------------ | -------------- | ---------------- |
| `memberRepository`             | `MemberRepository`             | private        | 회원 데이터 접근 |
| `passwordEncoder`              | `PasswordEncoder`              | private        | 비밀번호 암호화  |
| `authenticationManagerBuilder` | `AuthenticationManagerBuilder` | private        | 인증 매니저 빌더 |

**Operations**

| **Name**          | **Argument**                    | **Returns**  | **Description**           |
| ----------------- | ------------------------------- | ------------ | ------------------------- |
| `loginService`    | `LoginDto, HttpServletResponse` | `void`       | 로그인 및 JWT 발급 처리   |
| `registerService` | `RegisterDto`                   | `void`       | 회원가입 처리             |
| `profileService`  | `CustomUser`                    | `ProfileDto` | 로그인 사용자 프로필 조회 |

---

### MemberUpdateService

**Description:** 회원 정보 수정/이메일·비밀번호 변경/탈퇴

**Attributes**

| **Name**           | **Type**           | **Visibility** | **Description**  |
| ------------------ | ------------------ | -------------- | ---------------- |
| `memberRepository` | `MemberRepository` | private        | 회원 데이터 접근 |
| `passwordEncoder`  | `PasswordEncoder`  | private        | 비밀번호 암호화  |

**Operations**

| **Name**               | **Argument**                    | **Returns**  | **Description**              |
| ---------------------- | ------------------------------- | ------------ | ---------------------------- |
| `updateProfileService` | `CustomUser, ProfileUpdateDto`  | `ProfileDto` | 프로필 이미지/분야/소개 수정 |
| `updateEmail`          | `CustomUser, EmailChangeDto`    | `void`       | 이메일 변경                  |
| `changePassword`       | `CustomUser, PasswordChangeDto` | `void`       | 비밀번호 변경                |
| `deleteAccount`        | `CustomUser, AccountDeleteDto`  | `void`       | 회원 탈퇴                    |

---

### MemberController

**Description:** 회원 관련 REST API 컨트롤러

**Attributes**

| **Name**              | **Type**              | **Visibility** | **Description**         |
| --------------------- | --------------------- | -------------- | ----------------------- |
| `memberService`       | `MemberService`       | private        | 가입/로그인 서비스      |
| `memberUpdateService` | `MemberUpdateService` | private        | 프로필/비번/탈퇴 서비스 |

**Operations**

| **Name**         | **Argument**                                   | **Returns**         | **Description**    |
| ---------------- | ---------------------------------------------- | ------------------- | ------------------ |
| `loginAct`       | `LoginDto, BindingResult, HttpServletResponse` | `ResponseEntity<?>` | 로그인 요청 처리   |
| `registerAct`    | `RegisterDto, BindingResult`                   | `ResponseEntity<?>` | 회원가입 요청 처리 |
| `profile`        | `Authentication`                               | `ResponseEntity<?>` | 내 프로필 조회     |
| `updateProfile`  | `ProfileUpdateDto, Authentication`             | `ResponseEntity<?>` | 프로필 수정        |
| `updateEmail`    | `EmailChangeDto, Authentication`               | `ResponseEntity<?>` | 이메일 변경        |
| `changePassword` | `PasswordChangeDto, Authentication`            | `ResponseEntity<?>` | 비밀번호 변경      |
| `accountDelete`  | `AccountDeleteDto, Authentication`             | `ResponseEntity<?>` | 회원 탈퇴          |

---

## 3.2 DTO Class diagram

<img width="2342" height="1134" alt="Image" src="https://github.com/Let-s-Study-Now/Let-s-Study-Now_doc/blob/main/SDS/3.%20Class%20diagram/images/ClassDiagram_2.png?raw=true" />

### RegisterDto

**Description:** 회원가입 입력 데이터

**Attributes**

| **Name**        | **Type**  | **Visibility** | **Description**            |
| --------------- | --------- | -------------- | -------------------------- |
| `email`         | `String`  | private        | 이메일(중복 불가)          |
| `username`      | `String`  | private        | 로그인 아이디(2~12자)      |
| `password`      | `String`  | private        | 비밀번호(영/숫/특, 6~15자) |
| `checkPassword` | `String`  | private        | 비밀번호 확인              |
| `age`           | `Integer` | private        | 선택 입력                  |
| `profileImage`  | `String`  | private        | 프로필 이미지 경로         |
| `studyField`    | `String`  | private        | 전공/공부 분야             |
| `bio`           | `String`  | private        | 자기소개                   |

**Operations**

| **Name**    | **Argument** | **Returns** | **Description**         |
| ----------- | ------------ | ----------- | ----------------------- |
| `isCheckPw` | `-`          | `boolean`   | 비밀번호/확인 일치 검증 |

---

### LoginDto

**Description:** 로그인 인증 정보

**Attributes**

| **Name**   | **Type** | **Visibility** | **Description** |
| ---------- | -------- | -------------- | --------------- |
| `username` | `String` | private        | 아이디          |
| `password` | `String` | private        | 비밀번호        |

---

### ProfileDto

**Description:** 프로필 조회 응답

**Attributes**

| **Name**       | **Type**  | **Visibility** | **Description**  |
| -------------- | --------- | -------------- | ---------------- |
| `email`        | `String`  | private        | 이메일           |
| `username`     | `String`  | private        | 아이디           |
| `password`     | `String`  | private        | (보호 필요 필드) |
| `age`          | `Integer` | private        | 나이             |
| `profileImage` | `String`  | private        | 이미지 경로      |
| `studyField`   | `String`  | private        | 분야             |
| `bio`          | `String`  | private        | 소개             |

---

### ProfileUpdateDto

**Description:** 프로필 수정 입력

**Attributes**

| **Name**       | **Type** | **Visibility** | **Description**  |
| -------------- | -------- | -------------- | ---------------- |
| `profileImage` | `String` | private        | 이미지 경로      |
| `studyField`   | `String` | private        | 분야             |
| `bio`          | `String` | private        | 소개(최대 200자) |

---

### EmailChangeDto

**Description:** 이메일 변경 입력

**Attributes**

| **Name**          | **Type** | **Visibility** | **Description**      |
| ----------------- | -------- | -------------- | -------------------- |
| `newEmail`        | `String` | private        | 새 이메일            |
| `currentPassword` | `String` | private        | 본인확인용 현재 비번 |

---

### PasswordChangeDto

**Description:** 비밀번호 변경 입력

**Attributes**

| **Name**           | **Type** | **Visibility** | **Description**    |
| ------------------ | -------- | -------------- | ------------------ |
| `currentPassword`  | `String` | private        | 현재 비번          |
| `newPassword`      | `String` | private        | 새 비번(규칙 동일) |
| `newPasswordCheck` | `String` | private        | 새 비번 확인       |

---

### AccountDeleteDto

**Description:** 회원 탈퇴 입력

**Attributes**

| **Name**   | **Type** | **Visibility** | **Description**       |
| ---------- | -------- | -------------- | --------------------- |
| `password` | `String` | private        | 탈퇴 시 비밀번호 확인 |

---

## 3.3 인증/보안 Class diagram

<img width="3860" height="1836" alt="Image" src="https://github.com/Let-s-Study-Now/Let-s-Study-Now_doc/blob/main/SDS/3.%20Class%20diagram/images/ClassDiagram_3.png?raw=true" />

### CustomUser

**Description:** Spring `User` 확장, 사용자 고유 정보 보유

**Attributes**

| **Name** | **Type** | **Visibility** | **Description** |
| -------- | -------- | -------------- | --------------- |
| `email`  | `String` | public         | 사용자 이메일   |
| `id`     | `Long`   | public         | 사용자 PK       |

**Operations**

| **Name**           | **Argument**                                   | **Returns** | **Description**    |
| ------------------ | ---------------------------------------------- | ----------- | ------------------ |
| `CustomUser`(ctor) | `String, String, Collection<GrantedAuthority>` | `public`    | 상위 `User` 초기화 |

---

### MyUserDetailsService

**Description:** 사용자 정보를 로드하여 `UserDetails` 생성

**Attributes**

| **Name**           | **Type**           | **Visibility** | **Description** |
| ------------------ | ------------------ | -------------- | --------------- |
| `memberRepository` | `MemberRepository` | private        | 회원 조회       |

**Operations**

| **Name**             | **Argument** | **Returns**   | **Description**                         |
| -------------------- | ------------ | ------------- | --------------------------------------- |
| `loadUserByUsername` | `String`     | `UserDetails` | 아이디로 회원 조회 후 `CustomUser` 반환 |

---

### JwtUtil

**Description:** JWT 생성/검증 유틸

**Attributes**

| **Name** | **Type**    | **Visibility** | **Description**  |
| -------- | ----------- | -------------- | ---------------- |
| `key`    | `SecretKey` | private static | 서명 키          |
| `secret` | `String`    | private        | 환경변수 주입 키 |

**Operations**

| **Name**       | **Argument**     | **Returns** | **Description**                         |
| -------------- | ---------------- | ----------- | --------------------------------------- |
| `init`         | `-`              | `void`      | 인코딩 키 디코딩하여 `SecretKey` 초기화 |
| `createToken`  | `Authentication` | `String`    | 인증 정보 기반 JWT 생성                 |
| `extractToken` | `String`         | `Claims`    | JWT에서 Claims 추출                     |

---

### JwtFilter

**Description:** 요청당 1회 실행, JWT 유효성 검사

**Operations**

| **Name**           | **Argument**                                           | **Returns** | **Description**                     |
| ------------------ | ------------------------------------------------------ | ----------- | ----------------------------------- |
| `doFilterInternal` | `HttpServletRequest, HttpServletResponse, FilterChain` | `void`      | 쿠키에서 JWT 추출/검증 후 인증 저장 |

---

### SecurityConfig

**Description:** 인증/인가 설정

**Operations**

| **Name**          | **Argument**   | **Returns**           | **Description**                   |
| ----------------- | -------------- | --------------------- | --------------------------------- |
| `passwordEncoder` | `-`            | `PasswordEncoder`     | `BCryptPasswordEncoder` Bean 등록 |
| `filterChain`     | `HttpSecurity` | `SecurityFilterChain` | 세션 비활성화/필터/예외/권한 설정 |

---

### WebConfig

**Description:** CORS 설정 (React 통신 허용)

**Operations**

| **Name**          | **Argument**   | **Returns** | **Description**                   |
| ----------------- | -------------- | ----------- | --------------------------------- |
| `addCorsMappings` | `CorsRegistry` | `void`      | Origin/메서드/헤더/쿠키 허용 설정 |

---

## 3.4 오픈 스터디 기능 Class diagram

<img width="9110" height="2870" alt="Image" src="https://github.com/Let-s-Study-Now/Let-s-Study-Now_doc/blob/main/SDS/3.%20Class%20diagram/images/ClassDiagram_4.png?raw=true" />

### OpenStudyRoomController

**Description:** 오픈 스터디 관련 API

**Attributes**

| **Name**               | **Type**               | **Visibility** | **Description** |
| ---------------------- | ---------------------- | -------------- | --------------- |
| `openStudyRoomService` | `OpenStudyRoomService` | private        | 로직 처리       |
| `memberRepository`     | `MemberRepository`     | private        | 사용자 조회     |

**Operations**

| **Name**        | **Argument**       | **Returns** | **Description**   |
| --------------- | ------------------ | ----------- | ----------------- |
| `createRoom`    | `dto, user`        | `void`      | 방 생성           |
| `getRoomList`   | `-`                | `void`      | 활성 방 목록 조회 |
| `joinRoom`      | `Long, CustomUser` | `void`      | 방 참여 처리      |
| `leaveRoom`     | `Long, CustomUser` | `void`      | 방 나가기         |
| `getRoomDetail` | `Long`             | `void`      | 방 상세 조회      |

---

### OpenStudyRoomService

**Description:** 오픈 스터디 비즈니스 로직

**Attributes**

| **Name**                | **Type**                    | **Visibility** | **Description** |
| ----------------------- | --------------------------- | -------------- | --------------- |
| `roomRepository`        | `OpenStudyRoomRepository`   | private        | 방 데이터       |
| `participantRepository` | `RoomParticipantRepository` | private        | 참여자 데이터   |

**Operations**

| **Name**               | **Argument**       | **Returns**         | **Description**               |
| ---------------------- | ------------------ | ------------------- | ----------------------------- |
| `createRoom`           | `dto, user`        | `OpenStudyRoom`     | 방 생성 + 생성자 자동 참여    |
| `getRoomList`          | `-`                | `List`              | 활성/삭제예정 방 최신순 조회  |
| `joinRoom`             | `Long, CustomUser` | `RoomJoinResultDto` | 중복/정원/상태 검증 후 참여   |
| `leaveRoom`            | `Long, Member`     | `void`              | 나가기, 인원 수 따라 삭제예약 |
| `getRoomById`          | `Long`             | `OpenStudyRoom`     | ID로 상세 조회(없으면 예외)   |
| `getRoomToDelete`      | `-`                | `List`              | 삭제 예정 시간 지난 방        |
| `getAloneRoomsExpired` | `-`                | `List`              | 생성자 혼자 5분 경과 방       |
| `deleteRoom`           | `Long`             | `void`              | 삭제 예정 방 실제 삭제        |
| `deleteAloneRoom`      | `Long, String`     | `void`              | 혼자 있는 방 삭제(사유 로깅)  |

---

### OpenStudyRoom (Entity)

**Description:** 오픈 스터디 방 정보

**Attributes**

| **Name**              | **Type**                | **Visibility** | **Description**       |
| --------------------- | ----------------------- | -------------- | --------------------- |
| `id`                  | `Long`                  | private        | PK                    |
| `title`               | `String`                | private        | 제목                  |
| `description`         | `String`                | private        | 설명(≤30자)           |
| `studyField`          | `String`                | private        | 공부 분야             |
| `maxParticipants`     | `int`                   | private        | 최대 인원             |
| `currentParticipants` | `int`                   | private        | 현재 인원             |
| `creator`             | `Member`                | private        | 생성자                |
| `status`              | `RoomStatus`            | private        | 상태                  |
| `createdAt`           | `LocalDateTime`         | private        | 생성 시각             |
| `deleteScheduledAt`   | `LocalDateTime`         | private        | 삭제 예정 시각        |
| `aloneTimerStartedAt` | `LocalDateTime`         | private        | 혼자 타이머 시작 시각 |
| `participants`        | `List<RoomParticipant>` | private        | 참여자 리스트         |

**Operations**

| **Name**                  | **Argument** | **Returns** | **Description**             |
| ------------------------- | ------------ | ----------- | --------------------------- |
| `incrementParticipants`   | `-`          | `void`      | 입장 시 +1                  |
| `decrementParticipants`   | `-`          | `void`      | 퇴장 시 -1 (0 미만 방지)    |
| `isFull`                  | `-`          | `boolean`   | 정원 초과 여부              |
| `scheduleDelete`          | `-`          | `void`      | 5분 후 삭제예약 설정        |
| `cancelDeleteSchedule`    | `-`          | `void`      | 삭제예약 취소, 활성 복구    |
| `startAloneTimer`         | `-`          | `void`      | 방 생성 시 타이머 시작      |
| `resetAloneTimer`         | `-`          | `void`      | 두 번째 입장 시 타이머 해제 |
| `delete`                  | `-`          | `void`      | 상태를 DELETED로 변경       |
| `isJoinable`              | `-`          | `boolean`   | 참여 가능 상태 여부         |
| `isAloneTimerExpired`     | `-`          | `boolean`   | 혼자 5분 경과 여부          |
| `isDeleteScheduleExpired` | `-`          | `boolean`   | 삭제 예정 시각 경과 여부    |

---

### OpenStudyRoomRepository

**Description:** 오픈 스터디 방용 Repository

**Operations**

| **Name**                             | **Argument**                | **Returns** | **Description**       |
| ------------------------------------ | --------------------------- | ----------- | --------------------- |
| `findByStatusOrderByCreatedAtDesc`   | `RoomStatus status`         | `List`      | 활성 방 최신순        |
| `findByStatusInOrderByCreatedAtDesc` | `List<RoomStatus>`          | `List`      | 여러 상태 최신순      |
| `findRoomsToDelete`                  | `RoomStatus, LocalDateTime` | `List`      | 삭제 예정 경과 방     |
| `findAloneRoomsExpired`              | `LocalDateTime`             | `List`      | 혼자 5분 경과 방      |
| `findEmptyActiveRooms`               | `RoomStatus`                | `List`      | 참여자 0인 활성 방    |
| `findSingleParticipantPendingRooms`  | `-`                         | `List`      | 1명 남은 삭제 대기 방 |

---

### RoomCleanupScheduler

**Description:** 스케줄러로 오래된 방 자동 삭제

**Attributes**

| **Name**               | **Type**               | **Visibility** | **Description** |
| ---------------------- | ---------------------- | -------------- | --------------- |
| `openStudyRoomService` | `OpenStudyRoomService` | private        | 서비스 의존성   |

**Operations**

| **Name**               | **Argument** | **Returns** | **Description**             |
| ---------------------- | ------------ | ----------- | --------------------------- |
| `deleteAloneRooms`     | `-`          | `void`      | 혼자 5분 경과 방 자동 삭제  |
| `deleteScheduledRooms` | `-`          | `void`      | 삭제 예정 경과 방 자동 삭제 |

---

### RoomParticipant (Entity)

**Description:** 방 참여자

**Attributes**

| **Name**   | **Type**        | **Visibility** | **Description** |
| ---------- | --------------- | -------------- | --------------- |
| `id`       | `Long`          | private        | PK              |
| `room`     | `OpenStudyRoom` | private        | 방              |
| `member`   | `Member`        | private        | 회원            |
| `joinedAt` | `LocalDateTime` | private        | 참여 시각       |

---

### RoomParticipantRepository

**Description:** 방 참여자 Repository

**Operations**

| **Name**                    | **Argument** | **Returns** | **Description**             |
| --------------------------- | ------------ | ----------- | --------------------------- |
| `existsByRoomIdAndMemberId` | `Long, Long` | `boolean`   | 중복 참여 확인              |
| `findByRoomId`              | `Long`       | `List`      | 방별 참여자 조회            |
| `findActiveRoomByMemberId`  | `Long`       | `Optional`  | 회원이 참여 중인 활성 방    |
| `findByRoomIdAndMemberId`   | `Long, Long` | `Optional`  | 특정 회원 참여 정보         |
| `deleteByRoomId`            | `Long`       | `void`      | 방 삭제 시 참여자 일괄 삭제 |

---

## 3.5 그룹 스터디 Class diagram

### 3.5.1 Domain

<img width="1360" height="2680" alt="Image" src="https://github.com/Let-s-Study-Now/Let-s-Study-Now_doc/blob/main/SDS/3.%20Class%20diagram/images/ClassDiagram_5.png?raw=true" />

#### Group (Entity)

**Description:** 스터디 그룹 기본 정보

**Attributes**

| **Name**    | **Type**        | **Visibility** | **Description** |
| ----------- | --------------- | -------------- | --------------- |
| `id`        | `Long`          | private        | PK              |
| `groupName` | `String`        | private        | 그룹명          |
| `leaderId`  | `Long`          | private        | 리더 ID         |
| `createdAt` | `LocalDateTime` | private        | 생성일시        |

**Operations(요약)**: `Group(name, leaderId)`, getters

---

#### GroupMember (Entity)

**Description:** 그룹 소속 멤버

**Attributes**

| **Name**   | **Type**        | **Visibility** | **Description** |
| ---------- | --------------- | -------------- | --------------- |
| `id`       | `Long`          | private        | PK              |
| `groupId`  | `Long`          | private        | 그룹 ID         |
| `memberId` | `Long`          | private        | 멤버 ID         |
| `role`     | `String`        | private        | LEADER/MEMBER   |
| `joinedAt` | `LocalDateTime` | private        | 참여 일시       |

**Operations(요약)**: `GroupMember(groupId, memberId, role)`, `isLeader()`

---

#### StudyRoom (Entity)

**Description:** 그룹 내 스터디방

**Attributes**

| **Name**         | **Type**        | **Visibility** | **Description** |
| ---------------- | --------------- | -------------- | --------------- |
| `id`             | `Long`          | private        | PK              |
| `groupId`        | `Long`          | private        | 그룹 ID         |
| `roomName`       | `String`        | private        | 방 이름         |
| `studyField`     | `String`        | private        | 분야            |
| `studyHours`     | `Integer`       | private        | 1~5시간         |
| `maxMembers`     | `Integer`       | private        | 최대 인원       |
| `currentMembers` | `Integer`       | private        | 현재 인원       |
| `creatorId`      | `Long`          | private        | 생성자 ID       |
| `createdAt`      | `LocalDateTime` | private        | 생성 시각       |
| `endTime`        | `LocalDateTime` | private        | 종료 시각       |
| `status`         | `String`        | private        | ACTIVE/ENDED    |

**Operations**

| **Name**            | **Argument** | **Returns** | **Description**     |
| ------------------- | ------------ | ----------- | ------------------- |
| `addParticipant`    | `-`          | `void`      | 입장 시 인원 증가   |
| `removeParticipant` | `-`          | `void`      | 퇴장 시 인원 감소   |
| `end`               | `-`          | `void`      | 상태를 ENDED로 변경 |
| `isFull`            | `-`          | `boolean`   | 정원 도달 여부      |
| `isEnded`           | `-`          | `boolean`   | 종료 여부           |

---

#### StudyRoomParticipant (Entity)

**Description:** 스터디방 참여 기록

**Attributes**

| **Name**      | **Type**        | **Visibility** | **Description** |
| ------------- | --------------- | -------------- | --------------- |
| `id`          | `Long`          | private        | PK              |
| `studyRoomId` | `Long`          | private        | 방 ID           |
| `memberId`    | `Long`          | private        | 멤버 ID         |
| `joinedAt`    | `LocalDateTime` | private        | 입장 시각       |

---

### 3.5.2 DTO

<img width="3600" height="7000" alt="Image" src="https://github.com/Let-s-Study-Now/Let-s-Study-Now_doc/blob/main/SDS/3.%20Class%20diagram/images/ClassDiagram_6.png?raw=true" />

#### AddGroupMemberRequest

| **Name**   | **Type** | **Visibility** | **Description** |
| ---------- | -------- | -------------- | --------------- |
| `groupId`  | `Long`   | private        | 대상 그룹 ID    |
| `memberId` | `Long`   | private        | 추가 멤버 ID    |

#### CreateGroupRequest

| **Name**    | **Type** | **Visibility** | **Description** |
| ----------- | -------- | -------------- | --------------- |
| `groupName` | `String` | private        | 그룹명          |
| `leaderId`  | `Long`   | private        | 리더 ID         |

#### CreateStudyRoomRequest

| **Name**     | **Type**  | **Visibility** | **Description** |
| ------------ | --------- | -------------- | --------------- |
| `groupId`    | `Long`    | private        | 그룹 ID         |
| `roomName`   | `String`  | private        | 방 이름         |
| `studyField` | `String`  | private        | 분야            |
| `studyHours` | `Integer` | private        | 시간(1~5)       |
| `maxMembers` | `Integer` | private        | 최대 인원       |
| `creatorId`  | `Long`    | private        | 생성자 ID       |

#### GroupMemberResponse

| **Name**   | **Type**        | **Visibility** | **Description** |
| ---------- | --------------- | -------------- | --------------- |
| `id`       | `Long`          | private        | 레코드 ID       |
| `memberId` | `Long`          | private        | 멤버 ID         |
| `role`     | `String`        | private        | 역할            |
| `joinedAt` | `LocalDateTime` | private        | 참여 일시       |

#### GroupResponse

| **Name**    | **Type**        | **Visibility** | **Description** |
| ----------- | --------------- | -------------- | --------------- |
| `id`        | `Long`          | private        | 그룹 PK         |
| `groupName` | `String`        | private        | 그룹명          |
| `leaderId`  | `Long`          | private        | 리더 ID         |
| `createdAt` | `LocalDateTime` | private        | 생성일시        |

#### StudyRoomResponse

| **Name**           | **Type**        | **Visibility** | **Description**     |
| ------------------ | --------------- | -------------- | ------------------- |
| `id`               | `Long`          | private        | 방 PK               |
| `groupId`          | `Long`          | private        | 그룹 ID             |
| `roomName`         | `String`        | private        | 방 이름             |
| `studyField`       | `String`        | private        | 분야                |
| `studyHours`       | `Integer`       | private        | 시간                |
| `maxMembers`       | `Integer`       | private        | 최대 인원           |
| `currentMembers`   | `Integer`       | private        | 현재 인원           |
| `creatorId`        | `Long`          | private        | 생성자 ID           |
| `createdAt`        | `LocalDateTime` | private        | 생성 시각           |
| `endTime`          | `LocalDateTime` | private        | 종료 시각           |
| `status`           | `String`        | private        | ACTIVE/ENDED        |
| `remainingMinutes` | `Long`          | private        | (ACTIVE 시) 남은 분 |

---

### 3.5.3 Services

<img width="6042" height="828" alt="Image" src="https://github.com/Let-s-Study-Now/Let-s-Study-Now_doc/blob/main/SDS/3.%20Class%20diagram/images/ClassDiagram_7.png?raw=true" />

#### GroupService

**Operations**

| **Name**       | **Argument**         | **Returns**           | **Description**                   |
| -------------- | -------------------- | --------------------- | --------------------------------- |
| `createGroup`  | `CreateGroupRequest` | `GroupResponse`       | 그룹 생성/저장/응답               |
| `getGroup`     | `Long`               | `GroupResponse`       | 그룹 단건 조회                    |
| `getMyGroups`  | `Long`               | `List<GroupResponse>` | 내가 만든 그룹 목록               |
| `getAllGroups` | `-`                  | `List<GroupResponse>` | 전체 그룹 목록                    |
| `deleteGroup`  | `Long, Long`         | `void`                | 리더이면서 다른 멤버 없을 때 삭제 |

#### GroupMemberService

**Operations**

| **Name**          | **Argument**            | **Returns**                 | **Description**               |
| ----------------- | ----------------------- | --------------------------- | ----------------------------- |
| `addMember`       | `AddGroupMemberRequest` | `GroupMemberResponse`       | 중복 금지 후 추가             |
| `getGroupMembers` | `Long`                  | `List<GroupMemberResponse>` | 멤버 목록 조회                |
| `removeMember`    | `Long, Long, Long`      | `void`                      | 리더 권한으로 추방(본인 제외) |

#### StudyRoomService

**Operations**

| **Name**              | **Argument**             | **Returns**               | **Description**             |
| --------------------- | ------------------------ | ------------------------- | --------------------------- |
| `createRoom`          | `CreateStudyRoomRequest` | `StudyRoomResponse`       | 검증 포함 생성              |
| `getRoom`             | `Long`                   | `StudyRoomResponse`       | 단건 조회                   |
| `getGroupRooms`       | `Long`                   | `List<StudyRoomResponse>` | 그룹별 목록                 |
| `getAllActiveRooms`   | `-`                      | `List<StudyRoomResponse>` | 전체 활성 목록              |
| `joinRoom`            | `Long, Long`             | `void`                    | 멤버/정원/종료 확인 후 입장 |
| `leaveRoom`           | `Long, Long`             | `void`                    | 퇴장 처리                   |
| `endRoom`             | `Long`                   | `void`                    | 종료 및 참여자 정리         |
| `autoEndExpiredRooms` | `-`                      | `void`                    | 종료 시각 경과 방 자동 종료 |

---

### 3.5.4 Controllers

<img width="4706" height="786" alt="Image" src="https://github.com/Let-s-Study-Now/Let-s-Study-Now_doc/blob/main/SDS/3.%20Class%20diagram/images/ClassDiagram_8.png?raw=true" />

#### GroupController

| **Name**       | **Argument**         | **Returns**                           | **Description**                |
| -------------- | -------------------- | ------------------------------------- | ------------------------------ |
| `createGroup`  | `CreateGroupRequest` | `ResponseEntity<GroupResponse>`       | POST `/api/groups`             |
| `getGroup`     | `Long`               | `ResponseEntity<GroupResponse>`       | GET `/api/groups/{groupId}`    |
| `getAllGroups` | `-`                  | `ResponseEntity<List<GroupResponse>>` | GET `/api/groups`              |
| `getMyGroups`  | `Long`               | `ResponseEntity<List<GroupResponse>>` | GET `/api/groups/my`           |
| `deleteGroup`  | `Long, Long`         | `ResponseEntity<Void>`                | DELETE `/api/groups/{groupId}` |

#### GroupMemberController

| **Name**       | **Argument**                  | **Returns**                                 | **Description**                                   |
| -------------- | ----------------------------- | ------------------------------------------- | ------------------------------------------------- |
| `addMember`    | `Long, AddGroupMemberRequest` | `ResponseEntity<GroupMemberResponse>`       | POST `/api/groups/{groupId}/members`              |
| `getMembers`   | `Long`                        | `ResponseEntity<List<GroupMemberResponse>>` | GET `/api/groups/{groupId}/members`               |
| `removeMember` | `Long, Long, Long`            | `ResponseEntity<Void>`                      | DELETE `/api/groups/{groupId}/members/{memberId}` |

#### StudyRoomController

| **Name**        | **Argument**             | **Returns**                               | **Description**                                  |
| --------------- | ------------------------ | ----------------------------------------- | ------------------------------------------------ |
| `createRoom`    | `CreateStudyRoomRequest` | `ResponseEntity<StudyRoomResponse>`       | POST `/api/study-rooms`                          |
| `getRoom`       | `Long`                   | `ResponseEntity<StudyRoomResponse>`       | GET `/api/study-rooms/{roomId}`                  |
| `getGroupRooms` | `Long`                   | `ResponseEntity<List<StudyRoomResponse>>` | GET `/api/study-rooms/group/{groupId}`           |
| `getAllRooms`   | `-`                      | `ResponseEntity<List<StudyRoomResponse>>` | GET `/api/study-rooms`                           |
| `joinRoom`      | `Long, Long`             | `ResponseEntity<Void>`                    | POST `/api/study-rooms/{roomId}/join?memberId=`  |
| `leaveRoom`     | `Long, Long`             | `ResponseEntity<Void>`                    | POST `/api/study-rooms/{roomId}/leave?memberId=` |
| `endRoom`       | `Long`                   | `ResponseEntity<Void>`                    | POST `/api/study-rooms/{roomId}/end`             |

---

### 3.5.5 Repositories

<img width="5868" height="702" alt="Image" src="https://github.com/Let-s-Study-Now/Let-s-Study-Now_doc/blob/main/SDS/3.%20Class%20diagram/images/ClassDiagram_9.png?raw=true" />

#### GroupRepository

| **Name**          | **Argument** | **Returns**       | **Description**      |
| ----------------- | ------------ | ----------------- | -------------------- |
| `findByLeaderId`  | `Long`       | `List<Group>`     | 리더가 만든 그룹     |
| `findByGroupName` | `String`     | `Optional<Group>` | 그룹명으로 단건 조회 |

#### GroupMemberRepository

| **Name**                     | **Argument** | **Returns**             | **Description** |
| ---------------------------- | ------------ | ----------------------- | --------------- |
| `findByGroupId`              | `Long`       | `List<GroupMember>`     | 그룹 멤버 전체  |
| `findByGroupIdAndMemberId`   | `Long, Long` | `Optional<GroupMember>` | 단건 조회       |
| `countByGroupId`             | `Long`       | `long`                  | 인원 수         |
| `deleteByGroupIdAndMemberId` | `Long, Long` | `void`                  | 특정 멤버 삭제  |

#### StudyRoomRepository

| **Name**                 | **Argument**   | **Returns**       | **Description**  |
| ------------------------ | -------------- | ----------------- | ---------------- |
| `findByGroupId`          | `Long`         | `List<StudyRoom>` | 그룹의 모든 방   |
| `findByGroupIdAndStatus` | `Long, String` | `List<StudyRoom>` | 상태별 조회      |
| `findByCreatorId`        | `Long`         | `List<StudyRoom>` | 사용자가 만든 방 |

#### StudyRoomParticipantRepository

| **Name**                         | **Argument** | **Returns**                      | **Description**  |
| -------------------------------- | ------------ | -------------------------------- | ---------------- |
| `findByStudyRoomId`              | `Long`       | `List<StudyRoomParticipant>`     | 방의 모든 참여자 |
| `findByStudyRoomIdAndMemberId`   | `Long, Long` | `Optional<StudyRoomParticipant>` | 특정 멤버 참여   |
| `countByStudyRoomId`             | `Long`       | `long`                           | 참여자 수        |
| `deleteByStudyRoomIdAndMemberId` | `Long, Long` | `void`                           | 특정 멤버 삭제   |

---

## 3.6 체크리스트 Class diagram

<img width="4823" height="2772" alt="Image" src="https://github.com/Let-s-Study-Now/Let-s-Study-Now_doc/blob/main/SDS/3.%20Class%20diagram/images/ClassDiagram_10.png?raw=true" />

**Description:** 체크리스트 도메인의 구조와 주요 동작을 한눈에 정리한다.

### Checklist (Entity)

**Attributes**

| Name        | Type      | Visibility | Description               |
| ----------- | --------- | ---------- | ------------------------- |
| id          | Long      | private    | 체크리스트 PK             |
| member      | Member    | private    | 소유 사용자(FK=member_id) |
| targetDate  | LocalDate | private    | 대상 날짜                 |
| content     | String    | private    | 항목 내용                 |
| isCompleted | boolean   | private    | 완료 여부                 |

**Operations**

| Name          | Argument          | Returns | Description   |
| ------------- | ----------------- | ------- | ------------- |
| complete      | -                 | void    | 완료로 전환   |
| uncomplete    | -                 | void    | 미완료로 전환 |
| updateContent | String newContent | void    | 내용 수정     |

---

### DTOs

**ChecklistCreateDto** — 생성 요청  
**Attributes**

| Name       | Type      | Validation                            | Description |
| ---------- | --------- | ------------------------------------- | ----------- |
| targetDate | LocalDate | `@NotNull("날짜를 선택해야 합니다.")` | 대상 날짜   |
| content    | String    | `@NotBlank("내용을 입력해주세요")`    | 내용        |

**ChecklistUpdateDto** — 내용 수정 요청  
**Attributes**

| Name    | Type   | Validation                          | Description |
| ------- | ------ | ----------------------------------- | ----------- |
| content | String | `@NotBlank("내용을 입력해주세요.")` | 수정 내용   |

**ChecklistResponseDto** — 조회/생성/수정 응답  
**Attributes**

| Name        | Type      | Description |
| ----------- | --------- | ----------- |
| id          | Long      | PK          |
| targetDate  | LocalDate | 대상 날짜   |
| content     | String    | 항목 내용   |
| isCompleted | boolean   | 완료 여부   |

---

### ChecklistRepository

**Operations**

| Name                               | Argument                                      | Returns             | Description           |
| ---------------------------------- | --------------------------------------------- | ------------------- | --------------------- |
| findByMemberIdAndTargetDate        | Long memberId, LocalDate date                 | List<Checklist>     | 특정 날짜 조회        |
| findByIdAndMemberId                | Long id, Long memberId                        | Optional<Checklist> | 소유자 검증 단건 조회 |
| findByMemberIdAndTargetDateBetween | Long memberId, LocalDate start, LocalDate end | List<Checklist>     | 기간 조회             |
| save                               | Checklist                                     | Checklist           | 저장/수정             |
| deleteById                         | Long id                                       | void                | 삭제                  |

---

### ChecklistService

**Operations**

| Name           | Argument                                            | Returns                    | Description      |
| -------------- | --------------------------------------------------- | -------------------------- | ---------------- |
| create         | Long memberId, ChecklistCreateDto                   | ChecklistResponseDto       | 항목 생성        |
| update         | Long memberId, Long checklistId, ChecklistUpdateDto | ChecklistResponseDto       | 내용 수정        |
| toggleComplete | Long memberId, Long checklistId                     | ChecklistResponseDto       | 완료/미완료 전환 |
| getDaily       | Long memberId, LocalDate date                       | List<ChecklistResponseDto> | 하루치 조회      |
| getRange       | Long memberId, LocalDate start, LocalDate end       | List<ChecklistResponseDto> | 기간 조회        |
| delete         | Long memberId, Long checklistId                     | void                       | 삭제             |

---

### ChecklistController (REST)

**Endpoints**

| Method | Path                                            | Request            | Response                   | Description |
| ------ | ----------------------------------------------- | ------------------ | -------------------------- | ----------- |
| POST   | /api/checklists                                 | ChecklistCreateDto | ChecklistResponseDto       | 생성        |
| PUT    | /api/checklists/{id}                            | ChecklistUpdateDto | ChecklistResponseDto       | 내용 수정   |
| PATCH  | /api/checklists/{id}/complete                   | —                  | ChecklistResponseDto       | 완료 토글   |
| GET    | /api/checklists?date=YYYY-MM-DD                 | —                  | List<ChecklistResponseDto> | 하루치 조회 |
| GET    | /api/checklists?start=YYYY-MM-DD&end=YYYY-MM-DD | —                  | List<ChecklistResponseDto> | 기간 조회   |
| DELETE | /api/checklists/{id}                            | —                  | 204 No Content             | 삭제        |

---

## 3.7 타이머 기능 Class diagram

### 3.7.1 Entity Class diagram

**타이머 Entity 다이어그램**

<img width="2000" alt="Timer Entity Diagram" src="https://github.com/Let-s-Study-Now/Let-s-Study-Now_doc/blob/main/SDS/3.%20Class%20diagram/images/Timer_Entity_Diagram.png?raw=true" />

#### PersonalTimer
**Class Description:** 개인 타이머 엔티티. 사용자의 공부 시간을 실시간으로 측정하고 관리하는 Class

**Attributes:**

| Name | Description | Type | Visibility |
|------|-------------|------|------------|
| id | 타이머 고유 식별자 | Long | private |
| memberId | 사용자 ID | Long | private |
| roomId | 현재 입장한 방 ID | Long | private |
| timerMode | 타이머 모드 (기본 모드 or 뽀모도로 모드) | TimerMode | private |
| timerStatus | 타이머 상태 (공부 중 or 휴식 중) | TimerStatus | private |
| sessionStartTime | 현재 세션 시작 시간 | LocalDateTime | private |
| totalStudySeconds | 총 누적 공부 시간 (초 단위) | Long | private |
| createdAt | 타이머 생성 시간 (방 입장 시간) | LocalDateTime | private |
| updatedAt | 마지막 업데이트 시간 | LocalDateTime | private |

**Operations:**

| Name | Description | Argument | Returns |
|------|-------------|----------|---------|
| toggleStatus | 수동 토글: 공부 ↔ 휴식 전환 (기본 모드에서만 가능) | none | void |
| switchToPomodoroMode | 뽀모도로 모드로 전환 | none | void |
| switchToBasicMode | 기본 모드로 전환 | none | void |
| changePomodoroStatus | 뽀모도로 상태 변경 (공부 ↔ 휴식) | TimerStatus | void |
| accumulateStudyTime | 현재까지의 공부 시간 누적 (private) | none | void |
| endTimer | 타이머 종료 (방 퇴장 시). 마지막 세션 시간 누적 | none | void |
| getCurrentSessionSeconds | 현재 세션의 경과 시간을 초 단위로 반환 | none | long |

---

#### PomodoroSetting
**Class Description:** 뽀모도로 설정 엔티티. 사용자별 공부/휴식 시간 설정을 저장하는 Class

**Attributes:**

| Name | Description | Type | Visibility |
|------|-------------|------|------------|
| id | 설정 고유 식별자 | Long | private |
| memberId | 사용자 ID (1:1 관계, unique) | Long | private |
| studyMinutes | 공부 시간 (분 단위) | Integer | private |
| restMinutes | 휴식 시간 (분 단위) | Integer | private |
| createdAt | 설정 생성 시간 | LocalDateTime | private |
| updatedAt | 설정 업데이트 시간 | LocalDateTime | private |

**Operations:**

| Name | Description | Argument | Returns |
|------|-------------|----------|---------|
| updateSetting | 뽀모도로 설정 업데이트 | Integer, Integer | void |
| validateMinutes | 시간 유효성 검증 (1~120분 범위). private 메소드 | Integer, Integer | void |
| getStudySeconds | 공부 시간을 초 단위로 반환 | none | long |
| getRestSeconds | 휴식 시간을 초 단위로 반환 | none | long |

---

#### StudyHistory
**Class Description:** 공부 기록 엔티티. 날짜별 누적 공부 시간을 저장하는 Class

**Attributes:**

| Name | Description | Type | Visibility |
|------|-------------|------|------------|
| id | 기록 고유 식별자 | Long | private |
| memberId | 사용자 ID | Long | private |
| studyDate | 공부한 날짜 | LocalDate | private |
| totalStudySeconds | 해당 날짜의 총 공부 시간 (초 단위) | Long | private |
| createdAt | 기록 생성 시간 | LocalDateTime | private |
| updatedAt | 기록 업데이트 시간 | LocalDateTime | private |

**Operations:**

| Name | Description | Argument | Returns |
|------|-------------|----------|---------|
| addStudyTime | 공부 시간 추가. 0 이상의 값만 허용 | Long | void |
| getFormattedTime | 시간을 "HH:MM:SS" 형식의 문자열로 반환 | none | String |

---

#### TimerMode
**Enum Description:** 타이머 모드 열거형. 기본 모드와 뽀모도로 모드를 구분하는 Enum

**Values:**

| Name | Description |
|------|-------------|
| BASIC | 기본 모드 (수동 토글) |
| POMODORO | 뽀모도로 모드 (자동 전환) |

---

#### TimerStatus
**Enum Description:** 타이머 상태 열거형. 공부 중과 휴식 중 상태를 구분하는 Enum

**Values:**

| Name | Description |
|------|-------------|
| STUDYING | 공부 중 (시간 누적 ON) |
| RESTING | 휴식 중 (시간 누적 OFF) |

---

### 3.7.2 DTO Class diagram

**타이머 DTO 다이어그램**

<img width="2000" alt="Timer DTO Diagram" src="https://github.com/Let-s-Study-Now/Let-s-Study-Now_doc/blob/main/SDS/3.%20Class%20diagram/images/Timer_DTO_Diagram.png?raw=true" />

#### PomodoroSettingRequest
**Class Description:** 뽀모도로 설정 요청 DTO. 공부/휴식 시간을 설정할 때 사용하는 Class

**Attributes:**

| Name | Description | Type | Visibility |
|------|-------------|------|------------|
| studyMinutes | 공부 시간 (분 단위) | Integer | private |
| restMinutes | 휴식 시간 (분 단위) | Integer | private |

**Operations:**

| Name | Description | Argument | Returns |
|------|-------------|----------|---------|
| PomodoroSettingRequest | 모든 필드를 초기화하는 생성자 | Integer, Integer | none |
| getStudyMinutes | 공부 시간을 조회하는 Getter | none | Integer |
| getRestMinutes | 휴식 시간을 조회하는 Getter | none | Integer |

---

#### TimerStatusResponse
**Class Description:** 타이머 상태 응답 DTO. 현재 타이머의 전체 정보를 반환하는 Class

**Attributes:**

| Name | Description | Type | Visibility |
|------|-------------|------|------------|
| timerId | 타이머 ID | Long | private |
| memberId | 사용자 ID | Long | private |
| roomId | 방 ID | Long | private |
| timerMode | 타이머 모드 | TimerMode | private |
| timerStatus | 타이머 상태 | TimerStatus | private |
| currentSessionSeconds | 현재 세션 경과 시간 (초) | Long | private |
| totalStudySeconds | 총 누적 공부 시간 (초) | Long | private |
| totalStudyTime | 총 누적 공부 시간 (HH:MM:SS 형식) | String | private |

**Operations:**

| Name | Description | Argument | Returns |
|------|-------------|----------|---------|
| TimerStatusResponse | PersonalTimer 엔티티로부터 DTO 생성 | PersonalTimer | none |
| formatSeconds | 초를 HH:MM:SS 형식으로 변환. private static 메소드 | Long | String |

---

#### PomodoroSettingResponse
**Class Description:** 뽀모도로 설정 응답 DTO. 저장된 뽀모도로 설정 정보를 반환하는 Class

**Attributes:**

| Name | Description | Type | Visibility |
|------|-------------|------|------------|
| id | 설정 ID | Long | private |
| memberId | 사용자 ID | Long | private |
| studyMinutes | 공부 시간 (분) | Integer | private |
| restMinutes | 휴식 시간 (분) | Integer | private |

**Operations:**

| Name | Description | Argument | Returns |
|------|-------------|----------|---------|
| PomodoroSettingResponse | PomodoroSetting 엔티티로부터 DTO 생성 | PomodoroSetting | none |

---

#### StudyTimeResponse
**Class Description:** 누적 공부 시간 응답 DTO. 총 누적 시간과 오늘 공부 시간을 반환하는 Class

**Attributes:**

| Name | Description | Type | Visibility |
|------|-------------|------|------------|
| totalStudySeconds | 총 누적 공부 시간 (초) | Long | private |
| totalStudyTime | 총 누적 공부 시간 (HH:MM:SS 형식) | String | private |
| todayStudySeconds | 오늘 공부 시간 (초) | Long | private |
| todayStudyTime | 오늘 공부 시간 (HH:MM:SS 형식) | String | private |

**Operations:**

| Name | Description | Argument | Returns |
|------|-------------|----------|---------|
| of | 초 단위 시간으로부터 DTO 생성. static factory 메소드 | Long, Long | StudyTimeResponse |
| formatSeconds | 초를 HH:MM:SS 형식으로 변환. private static 메소드 | Long | String |

---

### 3.7.3 Services Class diagram

**타이머 Service 다이어그램**

<img width="2000" alt="Timer Service Diagram" src="https://github.com/Let-s-Study-Now/Let-s-Study-Now_doc/blob/main/SDS/3.%20Class%20diagram/images/Timer_Service_Diagram.png?raw=true" />

#### PersonalTimerService
**Class Description:** 개인 타이머 비즈니스 로직을 처리하는 Service Class

**Attributes:**

| Name | Description | Type | Visibility |
|------|-------------|------|------------|
| personalTimerRepository | 개인 타이머 repository | PersonalTimerRepository | private |
| pomodoroSettingRepository | 뽀모도로 설정 repository | PomodoroSettingRepository | private |
| studyHistoryRepository | 공부 기록 repository | StudyHistoryRepository | private |

**Operations:**

| Name | Description | Argument | Returns |
|------|-------------|----------|---------|
| startTimer | 타이머 시작 (방 입장 시). 모든 참여자는 공부 상태로 시작 | Long, Long, boolean | TimerStatusResponse |
| endTimer | 타이머 종료 (방 퇴장 시). 누적 시간을 StudyHistory에 저장하고 타이머 삭제 | Long | void |
| toggleTimer | 수동 토글 (공부 ↔ 휴식). 기본 모드에서만 작동 | Long | TimerStatusResponse |
| startPomodoroMode | 뽀모도로 모드 시작. 뽀모도로 설정이 필요함 | Long | TimerStatusResponse |
| stopPomodoroMode | 뽀모도로 모드 종료. 기본 모드로 전환 | Long | TimerStatusResponse |
| changePomodoroStatus | 뽀모도로 상태 자동 전환 (공부 ↔ 휴식) | Long, TimerStatus | TimerStatusResponse |
| getTimerStatus | 현재 타이머 상태 조회 | Long | TimerStatusResponse |
| getStudyTime | 누적 시간 조회 (총 누적 시간 + 오늘 공부 시간) | Long | StudyTimeResponse |
| saveToStudyHistory | StudyHistory에 공부 시간 저장. private 메소드 | Long, Long | void |

---

#### PomodoroSettingService
**Class Description:** 뽀모도로 설정 비즈니스 로직을 처리하는 Service Class

**Attributes:**

| Name | Description | Type | Visibility |
|------|-------------|------|------------|
| pomodoroSettingRepository | 뽀모도로 설정 repository | PomodoroSettingRepository | private |

**Operations:**

| Name | Description | Argument | Returns |
|------|-------------|----------|---------|
| saveSetting | 뽀모도로 설정 저장/업데이트. 첫 설정 시 생성, 이미 있으면 업데이트 | Long, PomodoroSettingRequest | PomodoroSettingResponse |
| getSetting | 뽀모도로 설정 조회. 설정이 없으면 null 반환 | Long | PomodoroSettingResponse |
| hasSettings | 뽀모도로 설정 존재 여부 확인 | Long | boolean |

---

### 3.7.4 Controllers Class diagram

**타이머 Controller 다이어그램**

<img width="2000" alt="Timer Controller Diagram" src="https://github.com/Let-s-Study-Now/Let-s-Study-Now_doc/blob/main/SDS/3.%20Class%20diagram/images/Timer_Controller_Diagram.png?raw=true" />

#### PersonalTimerController
**Class Description:** 개인 타이머 API 엔드포인트를 제공하는 Controller Class

**Attributes:**

| Name | Description | Type | Visibility |
|------|-------------|------|------------|
| personalTimerService | 개인 타이머 서비스 | PersonalTimerService | private |
| pomodoroSettingService | 뽀모도로 설정 서비스 | PomodoroSettingService | private |

**Operations:**

| Name | Description | Argument | Returns |
|------|-------------|----------|---------|
| startTimer | 타이머 시작 (방 입장 시). POST /api/timer/start | CustomUser, Long, boolean | ResponseEntity\<TimerStatusResponse\> |
| endTimer | 타이머 종료 (방 퇴장 시). POST /api/timer/end | CustomUser | ResponseEntity\<Void\> |
| toggleTimer | 수동 토글 (공부 ↔ 휴식). POST /api/timer/toggle | CustomUser | ResponseEntity\<TimerStatusResponse\> |
| startPomodoroMode | 뽀모도로 모드 시작. POST /api/timer/pomodoro/start | CustomUser | ResponseEntity\<TimerStatusResponse\> |
| stopPomodoroMode | 뽀모도로 모드 종료. POST /api/timer/pomodoro/stop | CustomUser | ResponseEntity\<TimerStatusResponse\> |
| changePomodoroStatus | 뽀모도로 상태 전환. POST /api/timer/pomodoro/change-status | CustomUser, TimerStatus | ResponseEntity\<TimerStatusResponse\> |
| getTimerStatus | 현재 타이머 상태 조회. GET /api/timer/status | CustomUser | ResponseEntity\<TimerStatusResponse\> |
| getStudyTime | 누적 공부 시간 조회. GET /api/timer/study-time | CustomUser | ResponseEntity\<StudyTimeResponse\> |
| savePomodoroSetting | 뽀모도로 설정 저장. POST /api/timer/pomodoro/settings | CustomUser, PomodoroSettingRequest | ResponseEntity\<PomodoroSettingResponse\> |
| getPomodoroSetting | 뽀모도로 설정 조회. GET /api/timer/pomodoro/settings | CustomUser | ResponseEntity\<PomodoroSettingResponse\> |

---

### 3.7.5 Repository Class diagram

**타이머 Repository 다이어그램**

<img width="2000" alt="Timer Repository Diagram" src="https://github.com/Let-s-Study-Now/Let-s-Study-Now_doc/blob/main/SDS/3.%20Class%20diagram/images/Timer_Repository_Diagram.png?raw=true" />

#### PersonalTimerRepository
**Class Description:** PersonalTimer 엔티티용 repository, 사용자/방 기준 조회 기능을 제공하는 Class

**Attributes:** None

**Operations:**

| Name | Description | Argument | Returns |
|------|-------------|----------|---------|
| findByMemberId | 사용자의 활성 타이머 조회 | Long | Optional\<PersonalTimer\> |
| existsByMemberId | 사용자의 활성 타이머 존재 여부 확인 | Long | boolean |
| findByRoomId | 특정 방의 모든 타이머 조회 | Long | List\<PersonalTimer\> |
| deleteByMemberId | 사용자의 활성 타이머 삭제 | Long | void |

---

#### PomodoroSettingRepository
**Class Description:** PomodoroSetting 엔티티용 repository, 사용자 기준 조회 기능을 제공하는 Class

**Attributes:** None

**Operations:**

| Name | Description | Argument | Returns |
|------|-------------|----------|---------|
| findByMemberId | 사용자의 뽀모도로 설정 조회 | Long | Optional\<PomodoroSetting\> |
| existsByMemberId | 사용자의 뽀모도로 설정 존재 여부 확인 | Long | boolean |

---

#### StudyHistoryRepository
**Class Description:** StudyHistory 엔티티용 repository, 날짜별 조회 및 누적 시간 계산 기능을 제공하는 Class

**Attributes:** None

**Operations:**

| Name | Description | Argument | Returns |
|------|-------------|----------|---------|
| findByMemberIdAndStudyDate | 특정 날짜의 공부 기록 조회 | Long, LocalDate | Optional\<StudyHistory\> |
| findByMemberIdOrderByStudyDateDesc | 사용자의 모든 공부 기록 조회 (최신순) | Long | List\<StudyHistory\> |
| findByMemberIdAndStudyDateBetween | 사용자의 특정 기간 공부 기록 조회 | Long, LocalDate, LocalDate | List\<StudyHistory\> |
| getTotalStudySecondsByMemberId | 사용자의 총 누적 공부 시간 계산 (초). @Query 사용 | Long | Long |
| getTotalStudySecondsByMemberIdAndDateBetween | 특정 기간 누적 공부 시간 계산 (초). @Query 사용 | Long, LocalDate, LocalDate | Long |

---

## 3.8 채팅 기능 Class diagram

### 3.8.1 Entity Class diagram

**채팅 Entity 다이어그램**

<img width="2000" alt="Chat Entity Diagram" src="https://github.com/Let-s-Study-Now/Let-s-Study-Now_doc/blob/main/SDS/3.%20Class%20diagram/images/Chat_Entity_Diagram.png?raw=true" />

#### ChatMessage
**Class Description:** 채팅 메시지 엔티티. 스터디방의 채팅 메시지를 저장하고 관리하는 Class

**Attributes:**

| Name | Description | Type | Visibility |
|------|-------------|------|------------|
| id | 메시지 고유 식별자 | Long | private |
| roomType | 방 타입 (OPEN, GROUP) | ChatRoomType | private |
| roomId | 방 번호 | Long | private |
| sender | 보낸 사람 | String | private |
| message | 메시지 내용 | String | private |
| type | 메시지 타입 | MessageType | private |
| sentAt | 메시지 전송 시간 | LocalDateTime | private |
| refId | 답변 원본 질문 ID | Long | private |
| isSolved | 해결 여부 | Boolean | private |
| isSelected | 채택 여부 | Boolean | private |

**Operations:**

| Name | Description | Argument | Returns |
|------|-------------|----------|---------|
| markAsSolved | 질문을 해결 상태로 변경 | none | void |
| markAsSelected | 답변을 채택 상태로 변경 | none | void |

---

#### ChatRoomType
**Enum Description:** 채팅방 타입 열거형. 오픈스터디와 그룹스터디를 구분하는 Enum

**Values:**

| Name | Description |
|------|-------------|
| OPEN | 오픈스터디 |
| GROUP | 그룹스터디 |

---

#### MessageType
**Enum Description:** 메시지 타입 열거형. 채팅 메시지의 종류를 구분하는 Enum

**Values:**

| Name | Description |
|------|-------------|
| ENTER | 입장 메시지 |
| TALK | 일반 대화 |
| LEAVE | 퇴장 메시지 |
| IMAGE | 이미지 메시지 |
| QUESTION | 질문 메시지 |
| ANSWER | 답변 메시지 |
| SOLVE | 해결 메시지 |

---

### 3.8.2 DTO Class diagram

**채팅 DTO 다이어그램**

<img width="2000" alt="Chat DTO Diagram" src="https://github.com/Let-s-Study-Now/Let-s-Study-Now_doc/blob/main/SDS/3.%20Class%20diagram/images/Chat_DTO_Diagram.png?raw=true" />

#### ChatMessageRequest
**Class Description:** 채팅 메시지 요청 DTO. 클라이언트가 메시지를 전송할 때 사용하는 Class

**Attributes:**

| Name | Description | Type | Visibility |
|------|-------------|------|------------|
| type | 메시지 타입 | MessageType | private |
| roomType | 방 타입 | ChatRoomType | private |
| roomId | 방 번호 | Long | private |
| message | 메시지 내용 | String | private |
| refId | 답변일 경우 질문 ID (옵션) | Long | private |

**Operations:**

| Name | Description | Argument | Returns |
|------|-------------|----------|---------|
| getType | 메시지 타입을 조회하는 Getter | none | MessageType |
| getRoomType | 방 타입을 조회하는 Getter | none | ChatRoomType |
| getRoomId | 방 번호를 조회하는 Getter | none | Long |
| getMessage | 메시지 내용을 조회하는 Getter | none | String |
| getRefId | 참조 질문 ID를 조회하는 Getter | none | Long |
| setType | 메시지 타입을 설정하는 Setter | MessageType | void |
| setRoomType | 방 타입을 설정하는 Setter | ChatRoomType | void |
| setRoomId | 방 번호를 설정하는 Setter | Long | void |
| setMessage | 메시지 내용을 설정하는 Setter | String | void |
| setRefId | 참조 질문 ID를 설정하는 Setter | Long | void |

---

#### ChatMessageResponse
**Class Description:** 채팅 메시지 응답 DTO. 서버가 클라이언트에게 메시지 정보를 반환하는 Class

**Attributes:**

| Name | Description | Type | Visibility |
|------|-------------|------|------------|
| messageId | 메시지 ID | Long | private |
| type | 메시지 타입 | MessageType | private |
| roomType | 방 타입 | ChatRoomType | private |
| roomId | 방 번호 | Long | private |
| sender | 서버가 인증정보로 채워 넣은 진짜 보낸 사람 | String | private |
| message | 메시지 내용 | String | private |
| sentAt | 전송 시간 | LocalDateTime | private |
| refId | 참조 질문 ID | Long | private |
| isSolved | 해결 여부 | Boolean | private |
| isSelected | 채택 여부 | Boolean | private |

**Operations:**

| Name | Description | Argument | Returns |
|------|-------------|----------|---------|
| from | ChatMessage 엔티티로부터 DTO 생성. static factory 메소드 | ChatMessage | ChatMessageResponse |

---

### 3.8.3 Services Class diagram

**채팅 Service 다이어그램**

<img width="2000" alt="Chat Service Diagram" src="https://github.com/Let-s-Study-Now/Let-s-Study-Now_doc/blob/main/SDS/3.%20Class%20diagram/images/Chat_Service_Diagram.png?raw=true" />

#### ChatService
**Class Description:** 채팅 비즈니스 로직을 처리하는 Service Class

**Attributes:**

| Name | Description | Type | Visibility |
|------|-------------|------|------------|
| chatRepository | 채팅 메시지 repository | ChatRepository | private |
| memberRepository | 회원 repository | MemberRepository | private |

**Operations:**

| Name | Description | Argument | Returns |
|------|-------------|----------|---------|
| saveMessage | 메시지 저장. 입장/질문/답변 타입별 검증 및 처리 | ChatMessageRequest, String | ChatMessageResponse |
| solveQuestion | 질문 해결 완료 처리. 선택적으로 답변 채택 및 채택수 증가 | Long, Long, String | ChatMessage |
| deleteMessage | 메시지 삭제. 작성자만 삭제 가능 | Long, String | void |
| getChatHistory | 채팅 내역 조회. 페이징 처리하여 최신순으로 반환 | Long, ChatRoomType, int, int | List\<ChatMessage\> |

---

### 3.8.4 Controllers Class diagram

**채팅 Controller 다이어그램**

<img width="2000" alt="Chat Controller Diagram" src="https://github.com/Let-s-Study-Now/Let-s-Study-Now_doc/blob/main/SDS/3.%20Class%20diagram/images/Chat_Controller_Diagram.png?raw=true" />

#### ChatController
**Class Description:** 채팅 API 및 WebSocket 엔드포인트를 제공하는 Controller Class

**Attributes:**

| Name | Description | Type | Visibility |
|------|-------------|------|------------|
| messagingTemplate | WebSocket 메시지 전송 템플릿 | SimpMessageSendingOperations | private |
| chatService | 채팅 서비스 | ChatService | private |
| s3Service | S3 파일 업로드 서비스 | S3Service | private |

**Operations:**

| Name | Description | Argument | Returns |
|------|-------------|----------|---------|
| message | 메시지 전송 처리. WebSocket /pub/chat/message | ChatMessageRequest, Principal | void |
| solveQuestion | 질문 해결 및 답변 채택. PATCH /api/chat/message/{messageId}/solve | Long, Long, CustomUser | ResponseEntity\<String\> |
| deleteMessage | 채팅 메시지 삭제. DELETE /api/chat/message/{messageId} | Long, CustomUser | ResponseEntity\<String\> |
| getChatHistory | 채팅 내역 조회. GET /api/chat/room/{roomId} | Long, ChatRoomType, int, int | ResponseEntity\<List\<ChatMessage\>\> |
| uploadChatImage | 채팅 이미지 업로드. POST /api/chat/image | MultipartFile | ResponseEntity\<String\> |

---

### 3.8.5 Repository Class diagram

**채팅 Repository 다이어그램**

<img width="2000" alt="Chat Repository Diagram" src="https://github.com/Let-s-Study-Now/Let-s-Study-Now_doc/blob/main/SDS/3.%20Class%20diagram/images/Chat_Repository_Diagram.png?raw=true" />

#### ChatRepository
**Class Description:** ChatMessage 엔티티용 repository, 방별 메시지 조회 기능을 제공하는 Class

**Attributes:** None

**Operations:**

| Name | Description | Argument | Returns |
|------|-------------|----------|---------|
| findByRoomIdAndRoomTypeOrderBySentAtDesc | 특정 방의 채팅 내역을 최신순으로 조회. 페이징 지원 | Long, ChatRoomType, Pageable | Slice\<ChatMessage\> |

---

## 3.9 스터디 세션 기능 Class diagram

### 3.9.1 Entity Class diagram

**스터디 세션 Entity 다이어그램**

<img width="2000" alt="Session Entity Diagram" src="https://github.com/Let-s-Study-Now/Let-s-Study-Now_doc/blob/main/SDS/3.%20Class%20diagram/images/Session_Entity_Diagram.png?raw=true" />

#### StudySession
**Class Description:** 공부 세션 엔티티. 회원이 스터디방에서 공부한 시간을 기록하고 관리하는 Class

**Attributes:**

| Name | Description | Type | Visibility |
|------|-------------|------|------------|
| id | 세션 고유 식별자 | Long | private |
| member | 공부하는 회원 | Member | private |
| studyType | 스터디 타입 ("OPEN_STUDY" 또는 "GROUP_STUDY") | String | private |
| roomId | 스터디방 ID (오픈스터디 또는 그룹스터디의 방 ID) | Long | private |
| startTime | 세션 시작 시간 | LocalDateTime | private |
| endTime | 세션 종료 시간 (null이면 아직 진행 중) | LocalDateTime | private |
| mode | 현재 모드 ("STUDY" 또는 "REST") | String | private |
| lastModeChangeTime | 마지막 모드 변경 시간 | LocalDateTime | private |
| studyMinutes | 실제 공부한 시간 (분 단위) | Integer | private |

**Operations:**

| Name | Description | Argument | Returns |
|------|-------------|----------|---------|
| addStudyMinutes | 공부 시간 추가 (분 단위) | int | void |
| endSession | 세션 종료 | none | void |
| isActive | 세션이 진행 중인지 확인 | none | boolean |

---

### 3.9.2 DTO Class diagram

**스터디 세션 DTO 다이어그램**

<img width="2000" alt="Session DTO Diagram" src="https://github.com/Let-s-Study-Now/Let-s-Study-Now_doc/blob/main/SDS/3.%20Class%20diagram/images/Session_DTO_Diagram.png?raw=true" />

#### SessionStartRequestDto
**Class Description:** 세션 시작 요청 DTO. 공부 세션 시작 시 사용하는 Class

**Attributes:**

| Name | Description | Type | Visibility |
|------|-------------|------|------------|
| studyType | 스터디 타입 ("OPEN_STUDY" 또는 "GROUP_STUDY") | String | private |
| roomId | 스터디방 ID | Long | private |

**Operations:**

| Name | Description | Argument | Returns |
|------|-------------|----------|---------|
| studyType | 스터디 타입을 조회하는 accessor | none | String |
| roomId | 방 ID를 조회하는 accessor | none | Long |

---

#### SessionResponseDto
**Class Description:** 세션 응답 DTO. 공부 세션 정보를 반환하는 Class

**Attributes:**

| Name | Description | Type | Visibility |
|------|-------------|------|------------|
| sessionId | 세션 ID | Long | private |
| memberId | 회원 ID | Long | private |
| studyType | 스터디 타입 | String | private |
| roomId | 방 ID | Long | private |
| mode | 현재 모드 | String | private |
| studyMinutes | 공부 시간 (분) | Integer | private |
| startTime | 시작 시간 | LocalDateTime | private |
| endTime | 종료 시간 | LocalDateTime | private |
| isActive | 활성 상태 여부 | Boolean | private |

**Operations:**

| Name | Description | Argument | Returns |
|------|-------------|----------|---------|
| from | StudySession 엔티티로부터 DTO 생성. static factory 메소드 | StudySession | SessionResponseDto |

---

#### SessionEndResultDto
**Class Description:** 세션 종료 결과 DTO. 세션 종료 시 레벨업 결과를 포함하여 반환하는 Class

**Attributes:**

| Name | Description | Type | Visibility |
|------|-------------|------|------------|
| sessionId | 세션 ID | Long | private |
| studyMinutes | 총 공부 시간 (분) | Integer | private |
| leveledUp | 레벨업 여부 | Boolean | private |
| newLevel | 새 레벨 (레벨업 안했으면 null) | Integer | private |

**Operations:** None (record class)

---

#### LevelInfoDto
**Class Description:** 레벨 정보 DTO. 회원의 레벨 관련 상세 정보를 반환하는 Class

**Attributes:**

| Name | Description | Type | Visibility |
|------|-------------|------|------------|
| memberId | 회원 ID | Long | private |
| username | 회원 이름 | String | private |
| currentLevel | 현재 레벨 | Integer | private |
| totalExp | 총 누적 경험치 (분) | Integer | private |
| currentLevelExp | 현재 레벨에서 획득한 경험치 (분) | Integer | private |
| requiredExpForNextLevel | 다음 레벨까지 필요한 총 경험치 (분) | Integer | private |
| remainingExp | 다음 레벨까지 남은 경험치 (분) | Integer | private |
| progress | 현재 레벨 진행률 (0~100%) | Double | private |

**Operations:** None (record class)

---

### 3.9.3 Services Class diagram

**스터디 세션 Service 다이어그램**

<img width="2000" alt="Session Service Diagram" src="https://github.com/Let-s-Study-Now/Let-s-Study-Now_doc/blob/main/SDS/3.%20Class%20diagram/images/Session_Service_Diagram.png?raw=true" />

#### StudySessionService
**Class Description:** 공부 세션 관리 서비스. Timer와 연동하여 실제 공부 시간을 측정하고 레벨업 처리하는 Class

**Attributes:**

| Name | Description | Type | Visibility |
|------|-------------|------|------------|
| studySessionRepository | 스터디 세션 repository | StudySessionRepository | private |
| memberRepository | 회원 repository | MemberRepository | private |
| levelUpService | 레벨업 서비스 | LevelUpService | private |
| personalTimerRepository | 개인 타이머 repository | PersonalTimerRepository | private |

**Operations:**

| Name | Description | Argument | Returns |
|------|-------------|----------|---------|
| startStudySession | 공부 세션 시작 (공부 모드로 시작) | Long, String, Long | StudySession |
| endStudySession | 공부 세션 종료 및 레벨업 처리. Timer에서 실제 공부 시간 가져와서 처리 | Long | SessionEndResultDto |
| getActiveSession | 회원의 활성 세션 조회 | Long | StudySession |

---

#### LevelUpService
**Class Description:** 레벨업 서비스. 공부 시간을 경험치로 변환하고 레벨업을 처리하는 Class

**Attributes:**

| Name | Description | Type | Visibility |
|------|-------------|------|------------|
| memberRepository | 회원 repository | MemberRepository | private |

**Operations:**

| Name | Description | Argument | Returns |
|------|-------------|----------|---------|
| getRequiredMinutesForLevel | 특정 레벨에서 다음 레벨로 올라가기 위해 필요한 시간 계산 (분 단위). 레벨의 10의 자리수 + 1 × 10시간 | int | int |
| calculateTotalExpForLevel | 특정 레벨에 도달하기 위해 필요한 총 누적 경험치 계산. private 메소드 | int | int |
| addStudyTimeAndCheckLevelUp | 공부 시간 추가 및 레벨업 처리. 여러 레벨 한번에 올라갈 수 있음 | Long, int | boolean |
| getLevelInfo | 회원의 레벨 정보 상세 조회 | Long | LevelInfoDto |

---

### 3.9.4 Controllers Class diagram

**스터디 세션 Controller 다이어그램**

<img width="2000" alt="Session Controller Diagram" src="https://github.com/Let-s-Study-Now/Let-s-Study-Now_doc/blob/main/SDS/3.%20Class%20diagram/images/Session_Controller_Diagram.png?raw=true" />

#### StudySessionController
**Class Description:** 공부 세션 및 레벨업 API 엔드포인트를 제공하는 Controller Class

**Attributes:**

| Name | Description | Type | Visibility |
|------|-------------|------|------------|
| studySessionService | 스터디 세션 서비스 | StudySessionService | private |
| levelUpService | 레벨업 서비스 | LevelUpService | private |

**Operations:**

| Name | Description | Argument | Returns |
|------|-------------|----------|---------|
| startSession | 공부 세션 시작. POST /api/study-sessions/start | CustomUser, SessionStartRequestDto | ResponseEntity\<SessionResponseDto\> |
| endSession | 공부 세션 종료 및 레벨업 처리. POST /api/study-sessions/{sessionId}/end | Long | ResponseEntity\<SessionEndResultDto\> |
| getActiveSession | 내 활성 세션 조회. GET /api/study-sessions/active | CustomUser | ResponseEntity\<SessionResponseDto\> |
| getLevelInfo | 내 레벨 정보 조회. GET /api/study-sessions/level | CustomUser | ResponseEntity\<LevelInfoDto\> |

---

### 3.9.5 Repository Class diagram

**스터디 세션 Repository 다이어그램**

<img width="2000" alt="Session Repository Diagram" src="https://github.com/Let-s-Study-Now/Let-s-Study-Now_doc/blob/main/SDS/3.%20Class%20diagram/images/Session_Repository_Diagram.png?raw=true" />

#### StudySessionRepository
**Class Description:** StudySession 엔티티용 repository, 회원별 활성 세션 조회 기능을 제공하는 Class

**Attributes:** None

**Operations:**

| Name | Description | Argument | Returns |
|------|-------------|----------|---------|
| findByMemberIdAndEndTimeIsNull | 회원의 활성 세션 조회 (종료되지 않은 세션) | Long | Optional\<StudySession\> |

---
