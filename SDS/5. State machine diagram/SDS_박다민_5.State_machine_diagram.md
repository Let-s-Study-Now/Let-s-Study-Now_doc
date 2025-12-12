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
