# 1. Introduction

본 문서는 우리 조가 개발하고자 하는 시스템 **“Let’s Study Now! (LSN)”**의  
Software Design Specification(SDS) 문서이다.  
LSN은 **언제든지 실시간으로 스터디 그룹에 참여하고 다른 사용자들과 함께 공부할 수 있는 웹 기반 스터디 매칭 플랫폼**이다.

---

## 📘 시스템 개요

본 시스템은 사용자가 **회원가입 및 로그인만 하면 즉시 접속 가능한 오픈 스터디룸**,  
**동일한 공부 시간을 설정한 사용자들끼리 자동으로 매칭되는 그룹 스터디룸**,  
그리고 **URL 공유를 통한 초대 기반 매칭 기능**을 중심으로 설계되었다.  

이를 통해 사용자는 혼자 공부할 때보다 **더 높은 집중력과 지속적인 동기부여**를 얻을 수 있도록 돕는다.  
본 SDS 문서는 LSN의 전체적인 설계를 제시하며, **요구사항 분석 결과를 다양한 관점에서** 나타내고 있다.

---

## ⚙️ 시스템 구조

LSN은 **웹 기반 클라이언트-서버 구조**로 구성된다.

- **Client (React)**  
  브라우저에서 동작하며, 다음 기능들을 제공한다:  
  - 실시간 화상 및 음성 연결  
  - 타이머 상태 표시  
  - 체크리스트 및 디데이 관리  

- **Server (Spring Boot API)**  
  - 사용자 계정 관리  
  - 그룹 매칭 로직 처리  
  - 스터디 기록 저장  
  - 실시간 방 상태 동기화  

- **Database (DB)**  
  사용자, 스터디룸, 체크리스트, 디데이, 매칭 기록 등의 엔티티로 구성된다.

---

## 🌟 주요 특징

LSN의 가장 핵심적인 특징은 **“언제든지 즉시 매칭할 수 있는 시스템”**이다.

- 로그인 후 대기 없이 바로 **오픈 스터디룸 입장 가능**  
- 동일한 공부 시간을 설정한 사용자끼리 **자동 매칭 기능** 제공  
- **URL 공유 초대 시스템**으로 개인 스터디룸 개설 가능  
- 스터디룸 내에서 다음 기능 활용 가능: 
  - 공부/휴식 상태 표시  
  - 타이머 및 상태메시지 등록  

결과적으로 **시간과 공간의 제약 없이 학습 가능한 실시간 스터디 플랫폼**으로,  
본 문서는 LSN의 **요구사항이 구현 과정에서 어떻게 반영되는지에 대한 기준 문서**로 활용된다.


# 2. Use Case Analysis

> This document summarizes the Use Case Diagram and detailed Use Case Descriptions for **Let’s Study Now! (LSN)**.  
> 본 장에서는 LSN 시스템의 Use Case Diagram과 각 Use Case의 상세 설명을 제공한다. 

---

## 2.1 Use Case Diagram

- 액터: **User** (일반 사용자), **Admin** (관리자)  
- 주요 기능: 회원가입(Sign Up), 로그인(Login), 로그아웃(Logout), 프로필 관리(Profile Management), 친구 초대(Friend Invite), 오픈/그룹 스터디룸 입장(Join Open/Group Study Room), 체크리스트 관리(Checklist), 화상 연결(Video), 알림(Notification) 등.

<img width="800" height="800" alt="image" src="https://github.com/FAITRUEE/Let-s-Study-Now/blob/main/SDS/2.%20Use%20case%20analysis/images/usecase_diagram.png?raw=true" />

---

## 2.2 Use Case Descriptions

---

## Use case #1: 로그인 (Login)
**Summary**  
사용자가 등록된 ID와 비밀번호로 시스템에 로그인한다.

**Primary Actor**  
User

**Preconditions**  
사용자는 회원가입이 완료되어 있어야 한다.

**Trigger**  
로그인 버튼 클릭

**Main success scenario**
1. 사용자가 ID, 비밀번호 입력
2. 로그인 버튼 클릭
3. 서버에서 인증 수행
4. 인증 성공 시 메인(스케줄) 페이지로 리디렉션

**Extensions**
- 입력란 공백 → 입력 요청 메시지 표시.
- 정보 불일치 → 오류 메시지 표시.

---

## Use case #2: 회원가입 (Sign Up)
**Summary**  
필수/선택 정보를 입력하여 계정을 생성한다.

**Primary Actor**  
User

**Preconditions**  
시스템 접속 가능 상태

**Trigger**  
회원가입 버튼 클릭

**Main success scenario**
1. 필수정보 입력
2. ID 중복검사 등 검증
3. 프로필 사진 업로드(선택)
4. 저장 후 가입 완료, 로그인 페이지로 이동

**Extensions**
- 필수정보 누락/유효성 오류 → 에러 처리 및 안내.

---

## Use case #3: 로그아웃 (Logout)
**Summary**  
사용자의 세션을 종료하고 로그아웃 상태로 만든다.

**Primary Actor**  
User

**Preconditions**  
사용자가 로그인되어 있어야 함

**Trigger**  
로그아웃 버튼 클릭

**Main success scenario**
1. 로그아웃 버튼 클릭
2. 세션/JWT 무효화
3. 클라이언트 토큰 삭제 응답
4. 로그인 페이지로 이동

**Extensions**
- 이미 로그아웃된 상태 → 안내 후 메인 이동
- 토큰 무효화 오류 → 에러 메시지 및 로깅

---

## Use case #4: 상태 메시지 등록 (Status Message)
**Summary**  
사용자가 현재 상태/기분을 메시지로 등록/수정한다.

**Primary Actor**  
User

**Preconditions**  
로그인 상태

**Trigger**  
상태 메시지 설정 메뉴 접근

**Main success scenario**
1. 입력 후 등록 클릭
2. 유효성 검사 후 DB 저장
3. 프로필 및 참여자 목록에 반영

**Extensions**
- 글자 수 초과 → 저장 거부/경고
- 등록 취소 → 변경 없음

---

## Use case #5: 친구 초대 공유 (Invite URL)
**Summary**  
고유 초대 URL을 생성해 외부로 공유한다.

**Primary Actor**  
User

**Preconditions**  
로그인 상태

**Trigger**  
친구 초대 기능 클릭

**Main success scenario**
1. 초대 URL 생성 및 표시
2. 사용자 복사/공유
3. 수신자가 접근 시 가입/로그인 흐름과 연동

**Extensions**
- URL 생성 실패 → 에러 안내
- 유효기간 만료 → 만료 메시지
- 이미 친구인 경우 → 별도 처리

---

## Use case #6: 마이 프로필 조회/수정 (Profile View/Edit)
**Summary**  
사용자가 자신의 프로필을 조회하고 수정한다.

**Primary Actor**  
User

**Preconditions**  
로그인 상태

**Trigger**  
마이 프로필 메뉴 클릭

**Main success scenario**
1. 현재 정보 표시
2. 수정 후 저장 → DB 반영
3. 성공 알림 표시

**Extensions**
- 프로필 사진 업로드 실패 → 기본 이미지 유지
- 자기소개 글자 수 초과 → 오류

---

## Use case #7: 자기소개 작성 (Introduce Yourself)
**Summary**  
자기소개를 작성/수정한다.

**Primary Actor**  
User

**Preconditions**  
로그인 및 프로필 페이지 접근

**Trigger**  
자기소개 입력 후 저장

**Main success scenario**
1. 입력 후 저장
2. 글자수(최대 200자) 검증
3. DB에 저장 및 반영

**Extensions**
- 글자수 초과 → 경고/저장 제한

---

## Use case #8: 아이디/이메일/나이 표시 (Show Basic Info)
**Summary**  
사용자의 ID, 이메일, 나이를 표시한다.

**Primary Actor**  
User

**Preconditions**  
로그인 상태, 프로필 페이지 로드

**Trigger**  
프로필 페이지 로딩

**Main success scenario**
1. DB에서 정보 조회
2. 화면에 정확히 표시

**Extensions**
- 필수 정보 누락 → '정보 없음' 표시
- 나이 미제공 → 비공개 또는 공란 처리

---

## Use case #9: 공부 분야 선택 (Select Study Subjects)
**Summary**  
사용자가 관심있는 공부 분야를 하나 이상 선택한다.

**Primary Actor**  
User

**Preconditions**  
로그인 상태

**Trigger**  
공부 분야 선택 섹션 클릭

**Main success scenario**
1. 목록에서 선택
2. 저장 → DB 반영
3. 알림 및 화면 반영

**Extensions**
- 목록 로드 실패 → 이전 목록 사용 또는 오류 안내
- 최소/최대 선택 수 제한

---

## Use case #10: 프로필 사진 업로드 (Upload Profile Picture)
**Summary**  
로컬에서 이미지를 선택해 프로필 사진으로 등록/변경한다.

**Primary Actor**  
User

**Preconditions**  
로그인 상태

**Trigger**  
업로드 버튼 클릭

**Main success scenario**
1. 이미지 선택
2. 파일 형식 검증(JPEG/PNG/GIF 등)
3. 서버 저장 및 URL 반영
4. 미리보기 및 성공 알림

**Extensions**
- 형식/크기 오류 → 업로드 거부
- 업로드 실패 → 에러 메시지

---

## Use case #11: 계정 설정 (Account Settings)
**Summary**  
비밀번호/이메일/아이디 변경, 탈퇴, 알림 설정 등 계정 관련 설정을 관리한다.

**Primary Actor**  
User

**Preconditions**  
로그인 상태

**Trigger**  
계정 설정 메뉴 진입

**Main success scenario**
1. 변경 항목 선택 및 입력
2. 검증 후 저장
3. 성공 알림

**Extensions**
- 비밀번호 변경/이메일 변경/탈퇴 관련 각종 오류 처리

---

## Use case #12: 비밀번호 변경 (Change Password)
**Summary**  
기존 비밀번호 확인 후 새 비밀번호로 변경한다.

**Primary Actor**  
User

**Preconditions**  
로그인 상태

**Trigger**  
비밀번호 변경 선택

**Main success scenario**
1. 현재 비밀번호, 새 비밀번호 입력
2. 현재 비밀번호 확인 및 보안 규칙 검증
3. 암호화 후 DB 저장
4. 성공 알림

**Extensions**
- 현재 비밀번호 불일치 → 에러
- 새 비밀번호 규칙 미충족 → 에러
- 저장 실패 → 에러

---

## Use case #13: 이메일 변경 (Change Email)
**Summary**  
등록된 이메일을 새 이메일로 변경한다(인증 포함).

**Primary Actor**  
User

**Preconditions**  
로그인 상태, 신규 이메일 사용 가능

**Trigger**  
이메일 변경 요청

**Main success scenario**
1. 새 이메일 입력
2. 이메일 형식 및 중복 검사
3. 인증 절차(메일) 수행
4. DB 업데이트 및 성공 알림

**Extensions**
- 형식 오류/중복 → 에러
- 인증 실패 → 에러

---

## Use case #14: 아이디 변경 (Change Username)
**Summary**  
본인 인증 후 새로운 아이디로 변경한다.

**Primary Actor**  
User

**Preconditions**  
로그인 상태

**Trigger**  
아이디 변경 선택

**Main success scenario**
1. 새 아이디 입력
2. 규칙 및 중복 검사
3. DB 업데이트 및 알림 (재로그인 요구 가능)

**Extensions**
- 규칙 위반/중복 → 에러

---

## Use case #15: 계정 탈퇴 (Delete Account)
**Summary**  
본인 인증 후 계정을 탈퇴하고 관련 데이터를 삭제 또는 비활성화한다.

**Primary Actor**  
User

**Preconditions**  
로그인 상태

**Trigger**  
계정 탈퇴 선택

**Main success scenario**
1. 비밀번호 재입력 등 본인 확인
2. 삭제/비활성화 처리
3. 삭제 완료 알림 및 강제 로그아웃

**Extensions**
- 비밀번호 불일치 → 에러
- DB 처리 실패 → 에러 및 안내

---

## Use case #16: 알림 설정 (Notification Settings)
**Summary**  
알림 종류·수신여부·수단 등을 설정한다.

**Primary Actor**  
User

**Preconditions**  
로그인 상태

**Trigger**  
알림 설정 메뉴 선택

**Main success scenario**
1. 알림 유형 선택/토글
2. 저장 → DB 반영
3. 성공 알림

**Extensions**
- 필수 알림 해제 시 제약 안내

---

## Use case #17: 오픈 스터디룸 입장 (Enter Open Study Room)
**Summary**  
오픈 스터디룸 목록 조회, 생성, 참여, 삭제 등의 공개 스터디 활동을 관리한다.

**Primary Actor**  
User

**Preconditions**  
로그인 상태

**Trigger**  
오픈 스터디룸 메뉴 클릭

**Main success scenario**
1. 목록 조회
2. 방 생성/참여/삭제 등 수행
3. 활동 완료 후 페이지 이탈

**Extensions**
- 생성/참여/삭제 실패 시 에러 처리

---

## Use case #18: 오픈 스터디룸 만들기 (Create Open Study Room)
**Summary**  
조건을 설정해 공개 스터디 방을 생성한다.

**Primary Actor**  
User

**Preconditions**  
로그인 상태

**Trigger**  
방 만들기 진입

**Main success scenario**
1. 필수 항목 입력(제목, 분야, 인원 등)
2. 검증 성공 시 방 저장 및 즉시 목록에 표시
3. 생성자에게 방장 권한 부여 및 입장

**Extensions**
- 필수 항목 누락 → 생성 거부

---

## Use case #19: 오픈 스터디 방 목록 조회 (List Open Study Rooms)
**Summary**  
생성된 오픈 스터디룸 목록을 카드 형태로 실시간 표시한다.

**Primary Actor**  
User

**Preconditions**  
로그인 상태

**Trigger**  
오픈 스터디룸 페이지 진입

**Main success scenario**
1. DB에서 방 목록 조회
2. 카드 형태로 표시(참여인원, 분야 등)
3. 실시간 업데이트 반영

**Extensions**
- 필터/검색 기능
- 실시간 업데이트 실패 시 수동 새로고침 안내

---

## Use case #20: 오픈 스터디 방 참여 (Join Open Study Room)
**Summary**  
목록에서 방을 선택하여 입장한다.

**Primary Actor**  
User

**Preconditions**  
로그인 상태

**Trigger**  
참여 버튼 클릭

**Main success scenario**
1. 정원/삭제 여부/중복 참여 확인
2. 조건 충족 시 멤버로 추가 및 입장
3. 참여자 수 자동 증가

**Extensions**
- 정원 초과 → 입장 불가
- 이미 삭제된 방 → 에러

---

## Use case #21: 오픈 스터디 방 삭제 (Auto Delete Open Room)
**Summary**  
참여자가 없는 방을 자동으로 감지해 삭제한다(시간 기준 등).

**Primary Actor**  
System / User

**Preconditions**  
해당 방이 생성되어 있어야 함

**Trigger**  
빈방 상태 또는 특정 시간 경과 등 트리거 발생

**Main success scenario**
1. 삭제 조건 감지
2. 사전 안내(타이머) 후 삭제
3. 목록에서 제거

**Extensions**
- 타이머 중 다른 참여자 입장 → 삭제 취소/안내

---

## Use case #22: 오픈스터디 방 화상연결 (Open Room Video)
**Summary**  
스튜디오룸 내 화상 연결 ON/OFF 제어 및 실시간 반영.

**Primary Actor**  
User

**Preconditions**  
로그인 및 방 입장 상태

**Trigger**  
카메라 버튼 클릭

**Main success scenario**
1. 기본값(OFF) 설정
2. 사용자 토글 시 상태 변경 및 실시간 전파
3. ON: 영상 송출, OFF: 닉네임/프로필 표시

**Extensions**
- 장치 인식 실패 → 에러 메시지
- 네트워크 지연 → 연결 불안정 표시

---

## Use case #23: 에러 처리 및 로깅 (Error Handling & Logging)
**Summary**  
시스템 오류 및 예외를 탐지하고 적절히 처리·로깅한다.

**Primary Actor**  
System

**Trigger**  
네트워크/서버/권한 등 예외 발생

**Main success scenario**
1. 에러 탐지
2. 로그 준비 및 기록(중앙 또는 로컬)
3. 사용자에게 적절한 피드백 제공
4. 정상 상태로 복구 시도

**Extensions**
- 로깅 정보 획득 실패 → 최소 정보 저장
- 중앙 로깅 통신 실패 → 로컬 저장/재시도 큐

---

## Use case #24: 그룹스터디 방 입장 (Enter Group Study Room)
**Summary**  
그룹 소속 사용자가 그룹 기반 스터디 방을 생성/참여/관리한다.

**Primary Actor**  
User

**Preconditions**  
로그인 및 그룹 소속

**Trigger**  
그룹 스터디 메뉴 클릭

**Main success scenario**
1. 소속 그룹의 방 목록 표시
2. 생성/참여/멤버 확인 등 수행

**Extensions**
- 권한 오류/접근 실패 → 에러

---

## Use case #25: 그룹스터디 방 생성 및 종료 (Create/End Group Study)
**Summary**  
그룹에 속한 사용자가 방 생성, 설정된 시간 종료 시 방을 자동 종료한다.

**Primary Actor**  
User

**Preconditions**  
로그인 및 그룹 소속

**Trigger**  
그룹 스터디에서 방 만들기 클릭

**Main success scenario**
1. 필수 정보 입력(인원, 분야, 공부시간 등)
2. 성공 시 생성자 입장
3. 공부시간 종료 시 자동 종료 및 퇴장

**Extensions**
- 제목 미입력/필수항목 누락 → 에러

---

## Use case #26: 그룹 생성 (Create Group)
**Summary**  
새 그룹을 생성하고 권한을 설정한다.

**Primary Actor**  
User

**Preconditions**  
로그인 상태

**Trigger**  
그룹 만들기 버튼 클릭

**Main success scenario**
1. 그룹명 입력
2. DB 등록 및 생성자에게 방장 권한 부여

**Extensions**
- 이름 미입력/중복 → 에러

---

## Use case #27: 그룹 삭제 (Delete Group)
**Summary**  
그룹장이 자신이 만든 그룹을 영구 삭제한다(멤버 없어야 함).

**Primary Actor**  
Group Owner

**Preconditions**  
로그인 및 방장 권한, 그룹 내 다른 멤버 없어야 함

**Trigger**  
그룹 삭제 버튼 클릭

**Main success scenario**
1. 삭제 전 확인 절차
2. 최종 동의 시 그룹 영구 삭제

**Extensions**
- 다른 멤버 존재 시 삭제 거부

---

## Use case #28: 그룹 멤버 확인 (Check Group Members)
**Summary**  
그룹의 멤버 목록 조회, 초대 링크 발급, 멤버 추방 등 관리 기능.

**Primary Actor**  
Group Owner / User

**Preconditions**  
로그인 및 그룹 스터디 페이지 접근

**Trigger**  
그룹 멤버 확인 메뉴 클릭

**Main success scenario**
1. 멤버 리스트 표시
2. 초대 링크 복사 및 발급
3. 초대 수락 시 자동 멤버 추가

**Extensions**
- 추방 시 확인 절차
- 초대 발급 오류 → 에러

---

## Use case #29: 그룹스터디 목록 조회 (Group Study List)
**Summary**  
사용자가 소속된 그룹의 스터디 방 목록을 조회한다.

**Primary Actor**  
User

**Preconditions**  
로그인 및 그룹 소속

**Trigger**  
그룹 스터디 목록 메뉴 클릭

**Main success scenario**
1. 자신이 속한 그룹의 스터디 목록만 표시
2. 카드 형태로 핵심 정보 제공

**Extensions**
- 다중 그룹 소속 시 필터링
- 참여 인원 초과 방 표시

---

## Use case #30: 그룹스터디 참여 및 종료 (Join/End Group Study)
**Summary**  
그룹 스터디에 입장하고 설정된 시간 종료 시 자동 퇴장한다.

**Primary Actor**  
User

**Preconditions**  
로그인 및 방 존재

**Trigger**  
참여 버튼 클릭 또는 시간 종료

**Main success scenario**
1. 입장 요청 및 멤버 확인
2. 입장 성공
3. 설정된 시간이 종료되면 자동 퇴장

**Extensions**
- 네트워크 오류 → 참여 실패
- 비멤버 접근 시 차단

---

## Use case #31: 그룹스터디 화상연결 (Group Study Video)
**Summary**  
그룹 스터디 내에서 화상 연결을 제어한다.

**Primary Actor**  
User

**Preconditions**  
로그인 및 그룹 방 입장

**Trigger**  
카메라 버튼 클릭

**Main success scenario**
1. 기본값 OFF
2. 토글 시 실시간 상태 반영
3. ON: 영상 송출 / OFF: 닉네임 표시

**Extensions**
- 장치 인식 오류 → 에러

---

## Use case #32: 참여자 목록 확인 (View Participants)
**Summary**  
현재 스터디방에 접속한 참여자 목록을 확인한다.

**Primary Actor**  
User

**Preconditions**  
방에 입장해 있어야 함

**Trigger**  
참여자 목록 요청

**Main success scenario**
1. 현재 접속 중인 참여자 목록 표시
2. 프로필/상태 메시지 등 함께 표시

**Extensions**
- 목록 로드 실패 → 오류 메시지

---

## Use case #33: 그룹스터디 참여 시간 확인 (Check Participation Time)
**Summary**  
사용자가 스터디룸에 머문 시간을 조회한다.

**Primary Actor**  
User

**Preconditions**  
로그인 및 방 접속

**Trigger**  
참여 시간 확인 요청

**Main success scenario**
1. 입장 시점부터 현재까지 시간 계산
2. 화면에 표시(분 단위 자동 갱신 가능)

**Extensions**
- 시간 계산 오류 → 오류 안내

---

## Use case #34: 공부/휴식 모드 전환 (Study/Break Mode)
**Summary**  
사용자가 공부 또는 휴식 모드로 상태를 전환하여 다른 참여자에게 표시한다.

**Primary Actor**  
User

**Preconditions**  
로그인 및 방 접속

**Trigger**  
모드 토글 또는 타이머에 의한 자동 전환

**Main success scenario**
1. 모드 변경 시 실시간 반영
2. 타이머 연동 시 자동 전환

**Extensions**
- 동기화 문제 발생 시 연결 재시도

---

## Use case #35: 참여자 목록 확인(상세) (Participant List Details)
**Summary**  
참여자의 상세 정보(닉네임, 상태, 참여 시간 등)를 확인한다.

**Primary Actor**  
User

**Preconditions**  
방 접속

**Trigger**  
참여자 목록 열람

**Main success scenario**
1. 각 참여자에 대한 상세 정보 표시

**Extensions**
- 개인정보 표시 제한에 따른 비공개 처리

---

## Use case #36: 참여 시간 표시 (Show Participation Time)
**Summary**  
각 참여자의 참여 시간을 표시한다.

**Primary Actor**  
System / User

**Preconditions**  
방 입장 기록 존재

**Trigger**  
참여자 목록 로드

**Main success scenario**
1. 입장 시각 기반으로 머문 시간 계산 후 표시

**Extensions**
- 동기화 문제로 임시 값 표시

---

## Use case #37: 타이머 설정 및 알림 (Timer & Alerts)
**Summary**  
공부/휴식 타이머 설정과 완료 시 알림 제공.

**Primary Actor**  
User

**Preconditions**  
방 접속

**Trigger**  
타이머 설정 또는 시작

**Main success scenario**
1. 타이머 설정 및 시작
2. 완료 시 알림(음성/팝업) 전송

**Extensions**
- 네트워크 지연 시 재동기화

---

## Use case #38: 공부/휴식 모드 자동화 (Auto Mode via Timer)
**Summary**  
타이머에 따라 공부/휴식 모드를 자동 전환한다.

**Primary Actor**  
System

**Preconditions**  
타이머 설정

**Trigger**  
타이머 만료

**Main success scenario**
1. 모드 자동 전환 및 실시간 반영

**Extensions**
- 재동기화 실패 시 사용자 수동 전환 필요

---

## Use case #39: 이메일 인증(가입/변경/비밀번호 찾기) - (Email Auth) *(partial, see #44)*  
**Summary**  
이메일 기반 인증이 필요한 흐름에서 코드 또는 링크를 발송하여 소유권을 검증한다.

(세부 흐름은 #44 이메일 인증에 상세 기술)

---

## Use case #40: 체크 리스트 생성 (Create Checklist)
**Summary**  
특정 날짜에 체크리스트를 생성해 개인 목표를 설정한다.

**Primary Actor**  
User

**Preconditions**  
로그인 및 체크리스트 관리 페이지 접근

**Trigger**  
생성 버튼 클릭

**Main success scenario**
1. 날짜 선택 후 생성
2. 내용 작성 및 저장 → 달력 반영

**Extensions**
- 내용 미입력 → 저장 거부

---

## Use case #41: 체크 리스트 수정 (Edit Checklist)
**Summary**  
작성된 체크리스트를 수정한다.

**Primary Actor**  
User

**Preconditions**  
수정할 체크리스트 존재

**Trigger**  
수정 버튼 클릭

**Main success scenario**
1. 날짜 선택 및 수정 창 오픈
2. 수정 후 저장 → DB 반영

**Extensions**
- 수정할 체크리스트 없음 → 안내
- 수정 내용 미입력 → 안내

---

## Use case #42: 체크 리스트 조회 (View Checklist)
**Summary**  
작성된 체크리스트를 조회한다.

**Primary Actor**  
User

**Preconditions**  
체크리스트 존재

**Trigger**  
조회 요청

**Main success scenario**
1. 해당 날짜의 체크리스트 표시

**Extensions**
- 조회 실패 → 에러 안내

---

## Use case #43: 체크 리스트 삭제 (Delete Checklist)
**Summary**  
선택한 체크리스트를 삭제한다.

**Primary Actor**  
User

**Preconditions**  
삭제할 체크리스트 선택

**Trigger**  
삭제 버튼 클릭

**Main success scenario**
1. 삭제 대상 선택
2. 최종 확인 다이얼로그
3. 삭제 시 DB에서 제거 및 목록 갱신

**Extensions**
- 삭제할 항목 없음 → 버튼 비활성화
- 삭제 취소 → 변경 없음

---

## Use case #44: 이메일 인증 (Email Verification)
**Summary**  
입력한 이메일 주소의 유효성 및 소유권을 확인하기 위해 인증 코드 또는 링크를 발송하고 확인한다.

**Primary Actor**  
User

**Preconditions**
- 인증할 이메일 주소 입력
- 시스템이 해당 주소로 메일 발송 가능 상태

**Trigger**
- 회원가입, 이메일 변경, 비밀번호 찾기 등 이메일 인증 필요 시

**Main success scenario**
1. 사용자가 인증 요청 이메일 주소 입력
2. 시스템이 유효기간이 설정된 인증 코드 발송
3. 사용자에게 코드 입력 필드 제공
4. 사용자가 이메일에서 코드 확인 후 입력 및 확인 클릭
5. 시스템이 코드 일치 검증
6. 검증 성공 시 "이메일 인증 완료" 메시지 표시

**Extensions**
- 이메일 발송 실패 → 발송 실패 메시지 표시(주소 확인 요청).
- 인증 코드 불일치 → 오류 메시지 표시.
- 링크/코드 만료 → 만료 안내 및 재발송 옵션 제공.


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


## 🔄 4. Sequence Diagram

> 모든 Sequence Diagram은 특정 Use Case와 대응되며, 시나리오 흐름을 따른다.  
> 클래스 다이어그램의 관계 및 operation과 일치하며, 시스템 객체 간 메시지 교환 과정을 표현한다.

---

### 🧍‍♂️ 4.1 회원 기능 Sequence Diagram

<img width="535" height="321" alt="image" src="https://github.com/user-attachments/assets/f3eb07f1-6539-418a-a877-1bf868e020cf" />

#### 🔐 4.1.1 로그인
1. 사용자가 로그인 요청을 전송  
2. `MemberController` → `MemberService` 로직 수행  
3. `MemberRepository`에서 ID 조회  
4. 비밀번호 불일치 시 오류 반환  
5. 일치 시 `JWT_Handler`를 통해 인증 토큰 생성  
6. `HttpOnly Cookie`에 토큰 저장 후 로그인 성공 응답  


<img width="535" height="321" alt="image" src="https://github.com/user-attachments/assets/1e536c3b-3cf7-44bd-b73d-3e93d53a40a3" />

#### 🧾 4.1.2 회원등록
1. 사용자 요청 → `MemberController`  
2. `MemberService`에서 아이디 중복 확인  
3. 중복 시 실패 응답  
4. 비밀번호 암호화 후 DB 저장  
5. 성공 시 회원가입 완료 응답 반환  


<img width="540" height="312" alt="image" src="https://github.com/user-attachments/assets/12e6c0a9-7c5c-4449-9bf4-46391cbd029d" />

#### ✏️ 4.1.3 프로필 수정
1. 사용자 요청 → `MemberController` → `MemberUpdateService`  
2. 사용자 정보 조회  
3. 데이터 수정(프로필 이미지, 자기소개 등)  
4. 트랜잭션 종료 시 DB 업데이트  
5. 수정 완료 응답 반환  


<img width="532" height="346" alt="image" src="https://github.com/user-attachments/assets/ca8f7506-a5e9-420b-aedd-3456ac2aa4e0" />

#### 🔑 4.1.4 비밀번호 변경
1. 사용자 요청 → `MemberController`  
2. 현재 비밀번호 검증 → 불일치 시 실패  
3. 새 비밀번호 일치 여부 확인  
4. 암호화 후 DB 업데이트  
5. 성공 시 변경 완료 응답  


<img width="543" height="354" alt="image" src="https://github.com/user-attachments/assets/4d879346-abbe-42a9-a03d-46522e87ae1d" />

#### 📧 4.1.5 이메일 변경
1. 비밀번호 검증 후 통과 시  
2. 새 이메일 중복 확인  
3. 중복 시 실패, 통과 시 DB 업데이트  
4. 성공 응답 반환  


<img width="541" height="268" alt="image" src="https://github.com/user-attachments/assets/8bc368a7-d480-4ba0-b802-b91f1d508b42" />

#### 🗑️ 4.1.6 계정 삭제
1. 사용자 비밀번호 확인  
2. 불일치 시 실패 응답  
3. 일치 시 `MemberRepository.delete(user)` 호출  
4. 계정 삭제 성공 응답 반환  

---

### 💬 4.2 오픈 스터디 Sequence Diagram

<img width="531" height="377" alt="image" src="https://github.com/user-attachments/assets/7a27c7f7-50f5-4752-8cd4-c27704480609" />

#### 🏠 4.2.1 방 생성
1. 사용자 요청 → `OpenStudyRoomController`  
2. 중복 참여 여부 검사  
3. 자동 삭제 타이머 설정  
4. 방 및 첫 참여자 DB 저장  
5. 성공 응답 반환  

<img width="523" height="281" alt="image" src="https://github.com/user-attachments/assets/a020b9fb-e205-474f-a3c2-6aeae2491866" />

#### 📋 4.2.2 방 목록 조회
1. 요청 → `OpenStudyRoomController`  
2. DB에서 활성 방 목록 조회  
3. DTO로 변환 후 응답  


<img width="533" height="484" alt="image" src="https://github.com/user-attachments/assets/96cbb8db-4daa-4ce9-b30d-c314ec89aa5d" />

#### 🙋 4.2.3 방 참여
1. 기존 참여 여부 검증  
2. 방 상태 및 인원 확인  
3. 참여자 등록  
4. 인원 2명 이상 시 자동 삭제 예약 취소  

#### ⏱️ 4.2.4 방 자동 정리(Scheduler)
1. 5분 이상 혼자 있는 방 자동 삭제  
2. 삭제 예약 시간 경과 시 추가 삭제 수행  

---

### 👥 4.3 그룹 스터디 Sequence Diagram

<img width="563" height="284" alt="image" src="https://github.com/user-attachments/assets/1f629834-4ba6-423a-bbc8-f22b149fb021" />

#### ➕ 4.3.1 그룹 생성
1. 그룹 이름 검증  
2. `GroupRepository`에 저장  
3. DTO 변환 후 성공 응답  

<img width="549" height="365" alt="image" src="https://github.com/user-attachments/assets/6956dedb-526d-4c97-8a21-55a99e1183d8" />

#### 👤 4.3.2 멤버 추가
1. 그룹 존재 및 중복 멤버 여부 확인  
2. 중복 시 실패, 신규면 저장  
3. 성공 응답 반환  

<img width="538" height="405" alt="image" src="https://github.com/user-attachments/assets/214f3a2d-5740-481b-8e10-2e816878e35a" />

#### 🚫 4.3.3 멤버 추방
1. 리더 권한 및 자기추방 여부 검증  
2. 존재 확인 후 DB 삭제  
3. 성공 시 응답 반환  


<img width="516" height="494" alt="image" src="https://github.com/user-attachments/assets/2b53ccf3-9edf-4724-a5d4-b7deac2eee3a" />

#### ❌ 4.3.4 그룹 삭제
1. 그룹 존재 및 리더 권한 확인  
2. 멤버 수 1명 초과 시 실패  
3. 삭제 성공 시 응답 반환  


<img width="535" height="312" alt="image" src="https://github.com/user-attachments/assets/f38faf20-1b02-445f-9da0-c145fdb9c949" />

#### 🏗️ 4.3.5 스터디방 생성
1. 그룹 존재 및 요청자 멤버 여부 확인  
2. 유효성 검증 후 방 저장  
3. 생성자를 첫 참여자로 등록  
4. 성공 응답 반환  

<img width="538" height="382" alt="image" src="https://github.com/user-attachments/assets/e3bf1b7c-c506-401f-a2e6-36da1d9ebfbc" />

#### 🚪 4.3.6 스터디방 입장
1. 방 존재, 멤버 여부, 중복 입장 여부 확인  
2. 인원수 및 상태 검증  
3. 참여자 등록 후 응답 반환  

<img width="531" height="376" alt="image" src="https://github.com/user-attachments/assets/ed209111-a743-448c-b66b-75574ca18165" />

#### 🏃 4.3.7 스터디방 퇴장
1. 존재 및 참여 여부 확인  
2. 인원수 감소, 참여 기록 삭제  
3. 성공 응답 반환  

<img width="537" height="363" alt="image" src="https://github.com/user-attachments/assets/33cc134c-c2e7-4e97-a74c-b2ac00bf012d" />

#### 🔚 4.3.8 스터디방 종료
1. 방 상태 ENDED로 변경  
2. 모든 참여 기록 삭제  
3. 성공 응답 반환  

<img width="535" height="294" alt="image" src="https://github.com/user-attachments/assets/cb16ea79-a38b-4919-b68a-2e351342a99d" />

#### 🔍 4.3.9 스터디방 조회
1. ID 기반 그룹 조회  
2. DTO 변환 후 응답  
> 같은 구조로 그룹 멤버/스터디방/참여자 조회도 동일하게 처리  

---

### ✅ 4.4 체크리스트 Sequence Diagram

<img width="534" height="421" alt="image" src="https://github.com/user-attachments/assets/f9990374-767e-4ce8-a58b-0868a9ea06fb" />

#### 🗒️ 4.4.1 체크리스트 생성
1. 입력값 유효성 검사  
2. 사용자 인증 확인  
3. DB 저장 후 DTO 반환  

<img width="539" height="356" alt="image" src="https://github.com/user-attachments/assets/49801cb0-7081-4fea-b6d1-5fc5f480deb9" />

#### ✏️ 4.4.2 체크리스트 수정
1. 항목 존재 및 권한 확인  
2. 내용 업데이트 후 DB 저장  
3. 성공 응답 반환  


<img width="518" height="216" alt="image" src="https://github.com/user-attachments/assets/5a490979-6d61-43c1-8275-b213e8527241" />

#### 🔎 4.4.3 체크리스트 조회
1. 선택 날짜 기준 조회  
2. DTO 리스트 변환 후 응답  
<img width="528" height="173" alt="image" src="https://github.com/user-attachments/assets/fabb2e6d-d9f9-4d50-981f-5b5d5f87702b" />


#### 🗓️ 4.4.4 월별 체크리스트 조회
1. 월별 존재 날짜 목록 조회  
2. 성공 응답 반환  

<img width="528" height="296" alt="image" src="https://github.com/user-attachments/assets/8fcb76cf-3b82-40f5-8684-21a2bfba37bc" />

#### 🗑️ 4.4.5 체크리스트 삭제
1. 항목 존재 및 권한 검증  
2. DB 삭제 후 성공 응답  

<img width="528" height="283" alt="image" src="https://github.com/user-attachments/assets/512f1f29-54f5-47f7-9d9f-0b040a81a4fd" />

#### ✅ 4.4.6 완료/미완료 상태 변경
1. 항목 존재 및 권한 검증  
2. 상태 반전 후 DB 업데이트  
3. 성공 응답 반환  

# 5. State Machine Diagram

![Full System State Diagram](https://github.com/damins0406/Let-s-Study-Now_doc/blob/main/SDS/5.%20State%20machine%20diagram/images/FullSystemStateDiagram.png?raw=true)
![Study Room Lifecycle State Diagram](https://github.com/damins0406/Let-s-Study-Now_doc/blob/main/SDS/5.%20State%20machine%20diagram/images/StudyRoomLifecycleStateDiagram.png?raw=true)


## 5.1 서론
본 문서는 "Let's Study Now" 온라인 스터디 플랫폼의 전체 시스템 상태 다이어그램을 설명한다. 상태 다이어그램은 시스템이 생명주기 동안 거치는 상태와 그 상태 간의 전이를 표현하며, 사용자 인증, 프로필 관리, 체크리스트 관리, 스터디 룸 운영, 실시간 타이머 관리, 그리고 자동화된 룸 생명주기 관리를 포함한다.

## 5.2 최상위 상태 구조
시스템은 미인증사용자와 인증된사용자 두 개의 최상위 상태로 구분된다. 미인증사용자는 로그인 성공 시 JWT 토큰을 발급받아 인증된사용자 상태로 전이하며, 로그아웃 시 다시 미인증사용자로 복귀한다.

## 5.3 미인증사용자 상태
미인증사용자 상태는 로그인화면대기, 회원가입진행중, 로그인시도중 세 개의 하위 상태를 가진다. 로그인화면대기에서 회원가입 요청 시 회원가입진행중으로 전이하고, 가입 완료/실패 시 다시 로그인화면대기로 복귀한다. 로그인 요청 시 로그인시도중 상태로 전이하며, 인증 정보가 일치하고 탈퇴하지 않은 회원이면 JWT 토큰을 발급하고 인증된사용자로 전이하고, 실패 시 로그인화면대기로 복귀한다.

## 5.4 인증된사용자 - 메인메뉴
인증된사용자는 대시보드를 초기 상태로 가지며, 대시보드에서 프로필 수정, 체크리스트, 그룹 스터디, 오픈 스터디 네 가지 메뉴로 직접 전이할 수 있다. 각 기능 완료 후에는 다시 대시보드로 복귀한다.

## 5.5 프로필편집모드

 **5.5.1 프로필 조회 및 분기**

프로필편집모드 진입 시 프로필조회 상태에서 현재 프로필 정보(email, username, profileImage, studyField, bio, level)를 표시한다. 프로필조회에서 프로필정보수정, 비밀번호변경, 회원탈퇴 중 하나를 선택하여 전이한다.

 **5.5.2 프로필정보수정**

수정폼입력 상태에서 studyField, bio, profileImage를 수정할 수 있다. 이미지 선택 시 이미지업로드처리 상태로 전이하며, 기존 이미지를 S3에서 삭제(기본 이미지 제외)하고 새 이미지를 업로드한 후 URL을 업데이트한다. 업로드 완료 후 수정폼입력으로 복귀하고, 저장 버튼 클릭 시 프로필저장완료를 거쳐 프로필조회로 복귀한다.

**5.5.3 비밀번호변경**

비밀번호입력폼에서 currentPassword, newPassword, newPasswordCheck를 입력하고 변경 버튼을 클릭하면 비밀번호검증(Choice State)으로 전이한다. currentPassword가 불일치하거나 newPassword와 newPasswordCheck가 다르면 비밀번호입력폼으로 복귀하고, 검증 통과 시 passwordEncoder로 암호화하여 저장 후 비밀번호변경완료를 거쳐 프로필조회로 복귀한다.

**5.5.4 회원탈퇴**

탈퇴확인폼에서 비밀번호를 입력하고 탈퇴 버튼 클릭 시 탈퇴검증(Choice State)으로 전이한다. 비밀번호 불일치 시 탈퇴확인폼으로 복귀하고, 검증 통과 시 탈퇴처리 상태로 전이하여 Soft Delete를 수행한다(deleted=true, email과 username에 UUID 추가, bio와 profileImage 초기화). 탈퇴처리 완료 후 시스템에서 자동 로그아웃된다.

## 5.6 체크리스트관리

**5.6.1 날짜 선택**

체크리스트관리 진입 시 날짜선택화면에서 월별 캘린더를 표시하며, getDaysWithChecklistByMonth() 호출로 체크리스트가 있는 날짜를 강조 표시한다. 특정 날짜 선택 시 getChecklistByDate()를 호출하여 체크리스트목록 상태로 전이한다.

**5.6.2 체크리스트 작업**

체크리스트목록 상태는 목록표시를 초기 상태로 가지며, 네 가지 작업으로 전이할 수 있다. 체크리스트생성은 새 항목 추가 버튼 클릭 시 createChecklist()를 호출하여 targetDate, content, isCompleted=false로 생성 후 목록표시로 복귀한다. 체크리스트수정은 항목 클릭 시 권한 검증(memberId 일치) 후 updateChecklist()로 content를 수정하고 목록표시로 복귀한다. 완료상태토글은 체크박스 클릭 시 toggleChecklist()로 isCompleted를 반전(true↔false)시키고 즉시 목록표시로 복귀한다. 체크리스트삭제는 삭제 버튼 클릭 시 권한 검증 후 deleteChecklist()로 DB에서 Hard Delete하고 목록표시로 복귀한다.

## 5.7 스터디 룸 선택

**5.7.1 그룹스터디모드**

대시보드에서 그룹 스터디 클릭 시 그룹스터디모드로 전이한다. 그룹목록조회에서 그룹 선택 시 GroupMember 테이블에서 권한을 확인하고 그룹스터디룸목록으로 전이한다. 방 생성 버튼 클릭 시 그룹스터디룸생성폼으로 전이하여 방 이름, 공부 분야, 공부 시간(1~5시간), 최대 인원(2~10명)을 입력하고 제출하면 StudyRoom 엔티티를 생성하며 생성자는 자동 입장(currentMembers=1)하고 endTime을 설정(createdAt + studyHours)한다. 기존 방 선택 시 그룹스터디룸선택을 거쳐 그룹스터디룸입장대기로 전이한다.

**5.7.2 오픈스터디모드**

대시보드에서 오픈 스터디 클릭 시 오픈스터디모드로 전이한다. 오픈스터디선택화면에서 방 생성 선택 시 오픈스터디룸생성폼으로 전이하여 제목(최대 30자), 설명(최대 200자), 공부 분야, 최대 인원(2~10명)을 입력하고 제출하면 OpenStudyRoom 엔티티를 생성하며 생성자는 자동 입장(currentParticipants=1)하고 aloneTimerStartedAt을 현재 시간으로 설정하여 5분 타이머를 시작한다. 방 참가 선택 시 오픈스터디룸목록으로 전이하며, 공부 분야 필터를 적용하여 오픈스터디룸필터링과 상호 전이할 수 있다. 방 선택 시 오픈스터디룸선택을 거쳐 오픈스터디룸입장대기로 전이한다.

## 5.8 스터디세션중

**5.8.1 세션 초기화**

그룹스터디모드 또는 오픈스터디모드에서 방 입장 성공 시 스터디세션중으로 전이하며, 자동으로 StudySession(studyType, roomId, mode="STUDY", studyMinutes=0)과 PersonalTimer(timerMode=BASIC, timerStatus=STUDYING, totalStudySeconds=0, sessionStartTime=현재시간)를 생성한다.

**5.8.2 타이머 모드**

타이머작동중 상태는 타이머모드선택(Choice State)에서 시작하여 BASIC이면 기본모드_활성으로, POMODORO이면 뽀모도로모드_활성으로 전이한다.

**5.8.3 기본모드_활성**

공부중_기본과 휴식중_기본 사이를 toggleStatus() 호출로 전이한다. 공부중_기본에서 휴식중_기본으로 전이 시 sessionStartTime부터 현재까지의 시간을 계산하여 totalStudySeconds에 누적하고 timerStatus를 RESTING으로 변경하며 sessionStartTime을 갱신한다. 휴식중_기본에서 공부중_기본으로 전이 시 timerStatus를 STUDYING으로 변경하고 sessionStartTime을 갱신하며, 휴식 시간은 누적하지 않는다.

**5.8.4 뽀모도로모드_활성**

뽀모도로_공부중과 뽀모도로_휴식중 사이를 자동으로 전이한다. 뽀모도로_공부중 상태에서 사용자가 설정한 studyMinutes가 경과하면 changePomodoroStatus(RESTING)를 호출하여 현재까지 공부 시간을 누적하고 timerStatus를 RESTING으로 변경하며 sessionStartTime을 갱신한다. 뽀모도로_휴식중 상태에서 restMinutes가 경과하면 changePomodoroStatus(STUDYING)를 호출하여 timerStatus를 STUDYING으로 변경하고 sessionStartTime을 갱신한다.

**5.8.5 모드 전환**

기본모드_활성에서 뽀모도로모드_활성으로 전환 시 startPomodoroMode()를 호출하여 PomodoroSetting 존재를 확인하고, 현재 공부 중이면 시간을 누적한 후 timerMode를 POMODORO로 변경하며 sessionStartTime을 갱신한다. 뽀모도로모드_활성에서 기본모드_활성으로 전환 시 stopPomodoroMode()를 호출하여 현재 공부 중이면 시간을 누적한 후 timerMode를 BASIC으로 변경하며 sessionStartTime을 갱신한다.

**5.8.6 채팅기능**

타이머작동중과 병렬로 실행되는 채팅기능 상태는 일반채팅(MessageType=TALK)을 초기 상태로 가진다. 질문 작성 시 질문작성(MessageType=QUESTION, isSolved=false)으로 전이하고, 답변 작성 시 답변작성(MessageType=ANSWER, refId=질문ID, isSelected=false)으로 전이한다. 질문자가 답변 채택 시 markAsSelected()를 호출하여 답변의 isSelected를 true로, 질문의 isSolved를 true로 변경하며 질문자의 adoptionCount를 증가시키고 질문해결 상태로 전이한다.

**5.8.7 세션 종료 처리**

방 퇴장, 방 종료, 방 삭제 이벤트 발생 시 세션종료처리로 전이한다. 타이머종료 상태에서 endTimer()를 호출하여 현재 공부 중이면 마지막 세션 시간을 누적하고 totalStudySeconds를 계산한다. 학습기록저장 상태에서 오늘 날짜의 StudyHistory를 조회하여 기존 기록이 있으면 totalStudySeconds를 누적하고 없으면 새로 생성한다. 레벨업처리 상태에서 StudySession의 endTime을 기록하고 공부 시간당 경험치를 계산하여 회원의 totalExp를 증가시키며 레벨을 업데이트한 후 PersonalTimer를 DB에서 삭제한다. 세션 종료 완료 후 메인메뉴(대시보드)로 복귀한다.


## 5.9 오픈스터디룸 생명주기

오픈스터디룸 생명주기는 사용자 인터랙션과 독립적으로 작동하며 스케줄러가 1분마다 체크한다.

**5.9.1 ACTIVE 상태**

ACTIVE_오픈 상태(RoomStatus=ACTIVE, aloneTimerStartedAt 설정)에서 스케줄러가 생성자혼자5분체크(Choice State)로 전이시킨다. 5분 경과하지 않았거나 참여자가 2명 이상이면 ACTIVE_오픈으로 복귀하고, aloneTimerStartedAt 기준 5분 경과하고 currentParticipants가 1이면 DELETED_오픈_혼자로 전이하여 deleteAloneRoom()을 호출해 생성자의 세션/타이머를 종료하고 모든 참여자를 삭제한 후 status를 DELETED로 변경(Soft Delete)한다.

**5.9.2 PENDING_DELETE 상태**

참여자 퇴장 후 currentParticipants가 1 이하이면 scheduleDelete()를 호출하여 PENDING_DELETE_오픈으로 전이하며 status를 PENDING_DELETE로 변경하고 deleteScheduledAt을 현재시간+5분으로 설정한다. 새 참여자 입장으로 currentParticipants가 2 이상이 되면 cancelDeleteSchedule()를 호출하여 ACTIVE_오픈으로 복귀하며 status를 ACTIVE로 변경하고 deleteScheduledAt을 null로 설정한다. 스케줄러가 삭제예정시간체크(Choice State)로 전이시키고, deleteScheduledAt이 경과하고 currentParticipants가 1 이하이면 DELETED_오픈_예정으로 전이하여 deleteRoom()을 호출해 모든 참여자의 세션/타이머를 종료하고 모든 참여자를 삭제한 후 status를 DELETED로 변경(Soft Delete)한다.


## 5.10 그룹스터디룸 생명주기

그룹스터디룸 생명주기는 독립적으로 작동하며 스케줄러가 주기적으로 체크한다.

**5.10.1 ACTIVE 및 종료**

ACTIVE_그룹 상태(RoomStatus="ACTIVE", endTime=createdAt+studyHours)에서 종료조건체크(Choice State)로 전이한다. autoEndExpiredRooms() 스케줄러가 endTime 경과를 감지하면 ENDED_자동으로 전이하여 모든 참여자의 세션/타이머를 종료하고 모든 참여자를 삭제한 후 status를 "ENDED"로 변경하고 StudyRoom을 DB에서 Hard Delete(완전 삭제)한다. 방장이 deleteRoom()을 호출하고 방장만 남아있으면(currentMembers==1) DELETED_그룹으로 전이하여 방장의 세션/타이머를 종료하고 방장을 삭제한 후 StudyRoom을 DB에서 Hard Delete한다.


# 6. User Interface Prototype

이 문서는 **Let's Study Now** 프로젝트의 각 페이지별 사용자 인터페이스(UI) 프로토타입을 설명한다.  
디자인과 문장은 개발에 따라 일부 변경될 수 있으나, 전반적인 구성은 동일하다.

---

## 🏠 6.1 메인 페이지

위 그림은 메인 페이지 화면이다. 서비스의 주요 페이지들(오픈 스터디, 그룹 스터디, 체크리스트)로 신속하게 이동할 수 있도록 안내하는 기능이다. 화면 상단의 **지금 시작하기** 버튼은 새로운 사용자를 회원가입 페이지로 연결하는 진입점이다.

서비스의 핵심 기능인 **오픈 스터디**는 누구나 쉽게 참여하여 실시간으로 학습 현황을 공유하며 집중하는 협력 학습 방식이다. 이에 연결된 **오픈 스터디 참여하기** 버튼은 해당 스터디 목록 페이지로 바로 안내하는 역할을 한다.

다른 방식인 **그룹 스터디**는 리더가 계획을 세우고 출석 및 자료 공유를 관리하여 그룹원들이 꾸준히 공부하도록 돕는 조직적인 학습 환경이다. 새로운 그룹을 만들고자 할 때 **그룹 스터디 만들기** 버튼을 사용한다.

마지막으로, **체크리스트** 섹션은 사용자가 일일 학습 목표를 설정하고 달성률을 관리하여 학습 루틴을 완성하게 돕는 중요한 기능이다. 이 체크리스트의 상세 내용을 확인하거나 관리하고자 할 때 **체크리스트 관리하기** 버튼을 이용한다.

<img width="844" height="1978" alt="image" src="https://github.com/Let-s-Study-Now/Let-s-Study-Now_doc/blob/main/SDS/6.%20User%20interface%20prototype/images/ui_main.png?raw=true" />

---

## 🔐 6.2 로그인 페이지

위 그림은 로그인 페이지 화면이다. 아이디와 비밀번호를 입력 시 로그인을 하며 메인 페이지로 이동한다. 계정이 없으면 회원가입 페이지로 바로 이동할 수 있다.

<img width="950" height="500" alt="image" src="https://github.com/Let-s-Study-Now/Let-s-Study-Now_doc/blob/main/SDS/6.%20User%20interface%20prototype/images/ui_login.png?raw=true" />

---

## 🧾 6.3 회원가입 페이지

위 그림은 회원가입 페이지 화면이다. 아이디, 나이, 이메일, 비밀번호, 비밀번호 확인, 관심 공부 분야, 자기소개를 입력하여 회원가입할 수 있다.

**관심 공부 분야**는 1~5개 중 선택해야 하고, **나이**와 **자기소개**는 입력 필수 사항이 아닌 선택 사항이다. 이미 계정이 있으면 로그인 화면으로 이동할 수 있다.

<img width="950" height="500" alt="image" src="https://github.com/Let-s-Study-Now/Let-s-Study-Now_doc/blob/main/SDS/6.%20User%20interface%20prototype/images/ui_register.png?raw=true" />

---

## 📚 6.4 오픈 스터디 방 페이지

위 그림은 오픈 스터디 방 페이지 화면이다. 활성화된 방과 참여 가능한 방, 총 참여자를 나타내고 있다. 새로 생긴 방을 최신화할 수 있도록 **새로고침** 버튼이 있고, **방 만들기** 버튼이 있다.

<img width="950" height="500" alt="image" src="https://github.com/Let-s-Study-Now/Let-s-Study-Now_doc/blob/main/SDS/6.%20User%20interface%20prototype/images/ui_openstudy.png?raw=true" />

---

## ➕ 6.4.1 방 만들기

**방 만들기** 버튼에서는 제목과 설명, 최대 인원 수, 공부 분야 등을 선택할 수 있다. **방 설명**은 필수 사항이 아닌 선택 사항이다.

<img width="950" height="500" alt="image" src="https://github.com/Let-s-Study-Now/Let-s-Study-Now_doc/blob/main/SDS/6.%20User%20interface%20prototype/images/ui_openstudy_createroom.png?raw=true" />

---

## 📖 6.4.2 오픈 스터디 방 입장

오픈 스터디 방에 입장하면 사용자는 **학습 타이머**를 통해 '공부중' 또는 '휴식중' 버튼을 눌러 자신의 학습 시간을 정확하게 기록할 수 있으며, 이 기록된 시간은 추후 개인의 **공부 레벨 업데이트**에 반영된다.

효율적인 집중을 돕기 위해 **뽀모도로(학습 주기 타이머)** 기능이 제공된다. 이 타이머를 활용하여 작업(25분), 짧은 휴식(5분), 긴 휴식(15분) 주기를 정해두고 학습이 가능하며, 설정된 시간에 도달하면 종료 알림이 생성된다.

또한, 학습 환경을 조성하기 위해 **학습 음악 기능**을 통해 백색소음, 분위기 음악, 자연 음악 등을 실행시킬 수 있다.

다른 사용자들과의 소통을 위해서는 **질문 모드 채팅 시스템**이 마련되어 있어, 문제에 대한 답변을 위한 채팅 칸이 따로 지정되어 분리가 가능하다.

스터디를 함께 할 친구를 초대하려면 **초대 버튼**을 눌러 자동으로 생성되는 초대 링크를 공유하면 된다. 현재 방에 참여 중인 학습자 목록은 **사람 아이콘**을 클릭하여 확인할 수 있다.

스터디를 마칠 때, 일반 참여자는 **나가기 버튼**을 사용하여 방을 종료하며, 방장은 **방 삭제 기능**을 통해 스터디 방을 영구적으로 삭제할 수 있다.

<img width="950" height="500" alt="image" src="https://github.com/Let-s-Study-Now/Let-s-Study-Now_doc/blob/main/SDS/6.%20User%20interface%20prototype/images/ui_openstudyroom.png?raw=true" />

---

## 👥 6.5 그룹 스터디 방 페이지

위 그림은 그룹 스터디 방 화면이다. 그룹을 만들어 방을 만들 수 있다. 사용자는 **그룹 이름(필수 사항)**을 입력하여 새로운 그룹을 만들 수 있다.

그룹 생성이 완료되면, **내 그룹** 탭에 새로 추가한 그룹의 정보가 카드 형식으로 표시된다. 이 그룹 카드에는 정보를 최신 상태로 유지하기 위한 **새로고침 아이콘**, 친구를 초대할 수 있는 **초대 링크 아이콘**, 그리고 그룹을 삭제할 수 있는 **그룹 삭제 아이콘**이 포함되어 있다.

또한, 카드의 **멤버 보기** 버튼을 통해 현재 그룹에 속해 있는 멤버 목록을 확인할 수 있다.

<img width="950" height="500" alt="image" src="https://github.com/Let-s-Study-Now/Let-s-Study-Now_doc/blob/main/SDS/6.%20User%20interface%20prototype/images/ui_groupstudy.png?raw=true" />

---

## ➕ 6.5.1 그룹 만들기

사용자가 **그룹 만들기** 버튼을 클릭하면 '새 그룹 만들기' 탭이 나타난다. 이 탭에서 그룹 이름을 작성하여 새로운 그룹을 생성할 수 있다. 그룹을 만드는 행위는 나중에 초대 링크를 사용하여 친구를 초대하고 함께 공부할 기반을 마련하는 목적을 가진다.

<img width="950" height="500" alt="image" src="https://github.com/Let-s-Study-Now/Let-s-Study-Now_doc/blob/main/SDS/6.%20User%20interface%20prototype/images/ui_groupstudy_creategroup.png?raw=true" />

---

## 📖 6.5.2 스터디 방 목록

**스터디 방** 탭에 들어가면 현재 진행 중인 그룹 스터디 방의 목록이 카드 형식으로 사용자에게 표시된다. 각 카드에는 해당 스터디 방에 대한 간략한 정보가 포함되어 있어, 사용자는 인원수, 스터디 진행 상태, 그리고 스터디의 주제인 공부 분야 등을 즉시 확인할 수 있다.

원하는 스터디 방을 발견하면, 해당 카드의 **입장하기** 버튼을 눌러 곧바로 스터디 방에 참여할 수 있다.

<img width="950" height="500" alt="image" src="https://github.com/Let-s-Study-Now/Let-s-Study-Now_doc/blob/main/SDS/6.%20User%20interface%20prototype/images/ui_groupstudy_roomlist.png?raw=true" />

---

## ➕ 6.5.3 스터디 방 생성

사용자는 스터디 방을 생성할 수 있으며, 이 과정에서 몇 가지 필수 정보를 입력해야만 스터디 방 목록에 해당 방이 정상적으로 표시된다.

스터디 방을 만들기 위해서는 먼저 **그룹 추가** 섹션에서 미리 생성된 그룹을 선택해야 한다. 그룹 선택 후에는 해당 스터디 방의 **방 제목**을 입력하고, 스터디의 주제가 될 **공부 분야**를 명시해야 한다.

마지막으로, 방에 참여할 **참여 인원**과 **목표 공부 시간**을 필수적으로 입력해야 한다. 이러한 모든 정보가 빠짐없이 입력되었을 때, 비로소 스터디 방 탭에서 생성된 스터디 방 목록을 확인할 수 있다.

<img width="950" height="500" alt="image" src="https://github.com/Let-s-Study-Now/Let-s-Study-Now_doc/blob/main/SDS/6.%20User%20interface%20prototype/images/ui_groupstudy_createroom.png?raw=true" />

---

## 📖 6.5.4 그룹 스터디 방 입장

그룹 스터디 방에 입장하면 사용자는 **학습 타이머**를 통해 '공부중' 또는 '휴식중' 버튼을 눌러 자신의 학습 시간을 정확하게 기록할 수 있으며, 이 기록된 시간은 추후 개인의 **공부 레벨 업데이트**에 반영된다.

효율적인 집중을 돕기 위해 **뽀모도로(학습 주기 타이머)** 기능이 제공된다. 이 타이머를 활용하여 작업(25분), 짧은 휴식(5분), 긴 휴식(15분) 주기를 정해두고 학습이 가능하며, 설정된 시간에 도달하면 종료 알림이 생성된다.

또한, 학습 환경을 조성하기 위해 **학습 음악 기능**을 통해 백색소음, 분위기 음악, 자연 음악 등을 실행시킬 수 있다.

다른 사용자들과의 소통을 위해서는 **질문 모드 채팅 시스템**이 마련되어 있어, 문제에 대한 답변을 위한 채팅 칸이 따로 지정되어 분리가 가능하다.

스터디를 함께 할 친구를 초대하려면 **초대 버튼**을 눌러 자동으로 생성되는 초대 링크를 공유하면 된다. 현재 방에 참여 중인 학습자 목록은 **사람 아이콘**을 클릭하여 확인할 수 있다.

스터디를 마칠 때, 일반 참여자는 **나가기 버튼**을 사용하여 방을 종료하며, 방장은 **방 삭제 기능**을 통해 스터디 방을 영구적으로 삭제할 수 있다.

<img width="950" height="500" alt="image" src="https://github.com/Let-s-Study-Now/Let-s-Study-Now_doc/blob/main/SDS/6.%20User%20interface%20prototype/images/ui_groupstudyroom.png?raw=true" />

---

## 🗓️ 6.6 체크리스트 페이지

위 그림은 체크리스트 화면이다. 사용자가 날짜별로 학습 목표를 설정하고 달성률을 확인하며 루틴을 관리할 수 있다. 사용자가 설정한 체크리스트의 목표 달성 여부는 메인 페이지의 체크리스트 탭에서 **달성률**을 통해 쉽게 확인할 수 있다.

<img width="950" height="500" alt="image" src="https://github.com/Let-s-Study-Now/Let-s-Study-Now_doc/blob/main/SDS/6.%20User%20interface%20prototype/images/ui_checklist.png?raw=true" />

---

## ✏️ 6.6.1 체크리스트 생성 및 관리

사용자는 화면 왼쪽에 위치한 **캘린더**를 이용하여 원하는 날짜를 선택할 수 있다. **체크리스트 생성** 버튼을 통해 새로운 항목을 추가하면, 해당 항목이 화면에 띄워진다.

사용자는 항목 옆의 **체크**를 통해 완료한 항목을 쉽게 정리할 수 있으며, 필요에 따라 항목을 **수정**하거나 **삭제**하는 것도 가능하다.

<img width="950" height="500" alt="image" src="https://github.com/Let-s-Study-Now/Let-s-Study-Now_doc/blob/main/SDS/6.%20User%20interface%20prototype/images/ui_checklist_add.png?raw=true" />

---

## 🙍 6.7 마이 페이지

위 그림은 사용자가 자신의 정보를 관리할 수 있는 **마이 페이지** 화면이다. 이 페이지 내의 **프로필 정보** 탭에서는 사용자가 자신의 개인 정보를 유연하게 변경할 수 있는 기능을 제공한다.

구체적으로, 사용자는 **프로필 이미지 변경**, **이메일 수정**, **닉네임 변경**, 학습 관심 분야인 **공부 분야 수정**, 그리고 자신을 소개하는 **자기소개 내용 변경**이 가능하다.

<img width="950" height="500" alt="image" src="https://github.com/Let-s-Study-Now/Let-s-Study-Now_doc/blob/main/SDS/6.%20User%20interface%20prototype/images/ui_mypage.png?raw=true" />

---

## 🔑 6.7.1 비밀번호 변경

**현재 비밀번호**, **새 비밀번호**, **새 비밀번호 확인**을 입력하여 비밀번호를 변경할 수 있다.

<img width="950" height="500" alt="image" src="https://github.com/Let-s-Study-Now/Let-s-Study-Now_doc/blob/main/SDS/6.%20User%20interface%20prototype/images/ui_mypage_password.png?raw=true" />

---

## 🗑️ 6.7.2 계정 탈퇴

사용자는 자신의 계정을 삭제할 수 있는 기능을 사용할 수 있다. 이 기능을 실행하면 시스템은 사용자에게 정말로 탈퇴할 것인지 **경고 메시지**를 제공하여 최종 확인을 거친다. 사용자가 경고 메시지를 확인하고 절차를 완료하면 탈퇴가 가능하다.

<img width="950" height="500" alt="image" src="https://github.com/Let-s-Study-Now/Let-s-Study-Now_doc/blob/main/SDS/6.%20User%20interface%20prototype/images/ui_mypage_delete.png?raw=true" />


# 7. Implementation requirements

---

## 7.1 Client platform requirements
- **OS**: Windows 7+, macOS
- **Browser**: Chrome, Safari, Firefox etc..

## 7.2 Server platform requirements
- **Runtime**: Java 17 JDK
- **Database**: MySQL 8.0+
- **Cloud Platform**: Microsoft Azure

## 7.3 Developer H/W platform requirements
- **CPU**: intel i3+
- **RAM**: 4GB+
- **Storage**: 20GB+

## 7.4 Developer S/W platform requirements
- **Backend**
    - Implement Language: Java 17
    - Development environment: IntelliJ IDEA
    - Framework: Spring Boot 3.2.5
- **Frontend**
    - Implement Language: JavaScript/TypeSript (React 18.x+)
    - Development environment: Visual Studio Code
- **API Design**
    - Swagger

---
---

# 📘 Glossary

| 용어 | 설명 |
|------|------|
| **React** | 본 프로젝트의 클라이언트를 개발하기 위해 사용되는 JavaScript 라이브러리로, 사용자 인터페이스를 구축하기 위해 컴포넌트 기반의 방식을 사용한다. |
| **Spring Boot** | 본 프로젝트의 서버를 개발하기 위해 사용되는 Java 프레임워크로, API 서버 구현, 데이터베이스 연동, 비즈니스 로직 처리 등을 담당한다. |
| **API** | 클라이언트와 서버 간의 데이터 송수신을 위한 표준화된 통신 규약. RESTful API 방식으로 구현되며 HTTP 메서드를 사용한다. |
| **Entity** | 데이터베이스의 테이블과 1:1 매핑되는 Java 객체로, 실제 데이터를 저장하는 구조를 의미한다. |
| **Logging** | 시스템에서 발생하는 모든 이벤트, 오류, 주요한 작업 등을 기록하는 것. |
| **Redirection** | 사용자에게서 요청이 들어왔을 때 페이지나 URL로 자동으로 이동시키는 것. |
| **Database** | 사용자 정보, 스터디룸 정보, 매칭 기록 등 시스템의 모든 데이터를 저장하고 관리하는 시스템. 본 프로젝트에서는 관계형 데이터베이스(MySQL)를 사용한다. |
| **URL** | 특정 자원(예: API, 이미지 등)의 주소. 본 프로젝트의 주요 스터디룸 기능에서는 스터디룸마다 사용자를 구분하기 위한 고유한 URL이 생성된다. |
| **Toggle** | ON/OFF로 전환하는 기능. 음성/화상 연결, 알림 설정 등에서 특정 기능을 켜고 끄는 데 사용된다. |
| **DTO (Data Transfer Object)** | 패키지 간 데이터를 전달하기 위한 객체로, 필요한 데이터만 담아서 전송하는 데 사용된다. |
| **DTD (Document Type Definition)** | XML 문서의 구조와 규칙을 정의하는 파일. 본 프로젝트에서는 주로 데이터 형식 검증 용도로 사용된다. |
| **Interface** | Java에서 클래스 간의 공통된 동작을 정의하는 추상화된 타입. `Repository`, `Service` 등의 Interface로 코드 구조를 명확히 분리한다. |
| **Repository** | 데이터베이스와 직접 연결되어 데이터를 조작하는 계층. Entity 객체를 기반으로 데이터의 CRUD(Create, Read, Update, Delete)를 수행한다. |
| **JWT (JSON Web Token)** | 토큰을 사용하여 사용자를 인증한다. 서버가 아닌 클라이언트 측에서 인증 상태를 유지할 수 있게 해준다. |
| **Controller** | 클라이언트의 HTTP 요청을 `Service`로 전달하고, `Service`의 반환값을 JSON 형태로 클라이언트에게 응답하는 패키지. |
| **HttpOnly Cookie** | JavaScript 코드에서 접근할 수 없도록 보호되는 쿠키로, 보안 강화를 위해 사용된다. |
| **Swagger** | RESTful API를 정의, 설명, 생성, 시각화, 사용하기 위한 표준 도구. API 문서화를 자동화한다. |


# 9. References

---

1.  Spring Team, "Spring Boot 3.2.5 Reference Documentation," n.d.,
    [https://docs.spring.io/spring-boot/docs/3.2.5/reference/html/](https://docs.spring.io/spring-boot/docs/3.2.5/reference/html/)

2.  React Team, "React Documentation," n.d.,
    [https://react.dev/](https://react.dev/)

3.  springdoc-openapi team, "springdoc-openapi v2.5.0 Documentation," n.d.,
    [https://springdoc.org/](https://springdoc.org/)

4.  Oracle, "MySQL 8.0 Reference Manual," n.d.,
    [https://dev.mysql.com/doc/refman/8.0/en/](https://dev.mysql.com/doc/refman/8.0/en/)

5.  Oracle, "Java 17 Documentation," n.d.,
    [https://docs.oracle.com/en/java/javase/17/](https://docs.oracle.com/en/java/javase/17/)

6.  GitHub, "GitHub Docs," n.d.,
    [https://docs.github.com/ko](https://docs.github.com/ko)

7.  Microsoft, "Azure Documentation," n.d.,
    [https://docs.microsoft.com/ko-kr/azure/](https://docs.microsoft.com/ko-kr/azure/)
