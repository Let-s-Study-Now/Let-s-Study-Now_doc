# 6. User Interface Prototype

이 문서는 **Let's Study Now** 프로젝트의 각 페이지별 사용자 인터페이스(UI) 프로토타입을 설명한다.  
디자인과 문장은 개발에 따라 일부 변경될 수 있으나, 전반적인 구성은 동일하다.

---

## 🏠 1. 메인 페이지

위 그림은 메인 페이지 화면이다. 서비스의 주요 페이지들(오픈 스터디, 그룹 스터디, 체크리스트)로 신속하게 이동할 수 있도록 안내하는 기능이다. 화면 상단의 **지금 시작하기** 버튼은 새로운 사용자를 회원가입 페이지로 연결하는 진입점이다.

서비스의 핵심 기능인 **오픈 스터디**는 누구나 쉽게 참여하여 실시간으로 학습 현황을 공유하며 집중하는 협력 학습 방식이다. 이에 연결된 **오픈 스터디 참여하기** 버튼은 해당 스터디 목록 페이지로 바로 안내하는 역할을 한다.

다른 방식인 **그룹 스터디**는 리더가 계획을 세우고 출석 및 자료 공유를 관리하여 그룹원들이 꾸준히 공부하도록 돕는 조직적인 학습 환경이다. 새로운 그룹을 만들고자 할 때 **그룹 스터디 만들기** 버튼을 사용한다.

마지막으로, **체크리스트** 섹션은 사용자가 일일 학습 목표를 설정하고 달성률을 관리하여 학습 루틴을 완성하게 돕는 중요한 기능이다. 이 체크리스트의 상세 내용을 확인하거나 관리하고자 할 때 **체크리스트 관리하기** 버튼을 이용한다.

<img width="844" height="1978" alt="image" src="https://github.com/Let-s-Study-Now/Let-s-Study-Now_doc/blob/main/SDS/6.%20User%20interface%20prototype/images/ui_main.png?raw=true" />

---

## 🔐 2. 로그인 페이지

위 그림은 로그인 페이지 화면이다. 아이디와 비밀번호를 입력 시 로그인을 하며 메인 페이지로 이동한다. 계정이 없으면 회원가입 페이지로 바로 이동할 수 있다.

<img width="950" height="500" alt="image" src="https://github.com/Let-s-Study-Now/Let-s-Study-Now_doc/blob/main/SDS/6.%20User%20interface%20prototype/images/ui_login.png?raw=true" />

---

## 🧾 3. 회원가입 페이지

위 그림은 회원가입 페이지 화면이다. 아이디, 나이, 이메일, 비밀번호, 비밀번호 확인, 관심 공부 분야, 자기소개를 입력하여 회원가입할 수 있다.

**관심 공부 분야**는 1~5개 중 선택해야 하고, **나이**와 **자기소개**는 입력 필수 사항이 아닌 선택 사항이다. 이미 계정이 있으면 로그인 화면으로 이동할 수 있다.

<img width="950" height="500" alt="image" src="https://github.com/Let-s-Study-Now/Let-s-Study-Now_doc/blob/main/SDS/6.%20User%20interface%20prototype/images/ui_register.png?raw=true" />

---

## 📚 4. 오픈 스터디 방 페이지

위 그림은 오픈 스터디 방 페이지 화면이다. 활성화된 방과 참여 가능한 방, 총 참여자를 나타내고 있다. 새로 생긴 방을 최신화할 수 있도록 **새로고침** 버튼이 있고, **방 만들기** 버튼이 있다.

<img width="950" height="500" alt="image" src="https://github.com/Let-s-Study-Now/Let-s-Study-Now_doc/blob/main/SDS/6.%20User%20interface%20prototype/images/ui_openstudy.png?raw=true" />

---

## ➕ 4-1. 방 만들기

**방 만들기** 버튼에서는 제목과 설명, 최대 인원 수, 공부 분야 등을 선택할 수 있다. **방 설명**은 필수 사항이 아닌 선택 사항이다.

<img width="950" height="500" alt="image" src="https://github.com/Let-s-Study-Now/Let-s-Study-Now_doc/blob/main/SDS/6.%20User%20interface%20prototype/images/ui_openstudy_createroom.png?raw=true" />

---

## 📖 4-2. 오픈 스터디 방 입장

오픈 스터디 방에 입장하면 사용자는 **학습 타이머**를 통해 '공부중' 또는 '휴식중' 버튼을 눌러 자신의 학습 시간을 정확하게 기록할 수 있으며, 이 기록된 시간은 추후 개인의 **공부 레벨 업데이트**에 반영된다.

효율적인 집중을 돕기 위해 **뽀모도로(학습 주기 타이머)** 기능이 제공된다. 이 타이머를 활용하여 작업(25분), 짧은 휴식(5분), 긴 휴식(15분) 주기를 정해두고 학습이 가능하며, 설정된 시간에 도달하면 종료 알림이 생성된다.

또한, 학습 환경을 조성하기 위해 **학습 음악 기능**을 통해 백색소음, 분위기 음악, 자연 음악 등을 실행시킬 수 있다.

다른 사용자들과의 소통을 위해서는 **질문 모드 채팅 시스템**이 마련되어 있어, 문제에 대한 답변을 위한 채팅 칸이 따로 지정되어 분리가 가능하다.

스터디를 함께 할 친구를 초대하려면 **초대 버튼**을 눌러 자동으로 생성되는 초대 링크를 공유하면 된다. 현재 방에 참여 중인 학습자 목록은 **사람 아이콘**을 클릭하여 확인할 수 있다.

스터디를 마칠 때, 일반 참여자는 **나가기 버튼**을 사용하여 방을 종료하며, 방장은 **방 삭제 기능**을 통해 스터디 방을 영구적으로 삭제할 수 있다.

<img width="950" height="500" alt="image" src="https://github.com/Let-s-Study-Now/Let-s-Study-Now_doc/blob/main/SDS/6.%20User%20interface%20prototype/images/ui_openstudyroom.png?raw=true" />

---

## 👥 5. 그룹 스터디 방 페이지

위 그림은 그룹 스터디 방 화면이다. 그룹을 만들어 방을 만들 수 있다. 사용자는 **그룹 이름(필수 사항)**을 입력하여 새로운 그룹을 만들 수 있다.

그룹 생성이 완료되면, **내 그룹** 탭에 새로 추가한 그룹의 정보가 카드 형식으로 표시된다. 이 그룹 카드에는 정보를 최신 상태로 유지하기 위한 **새로고침 아이콘**, 친구를 초대할 수 있는 **초대 링크 아이콘**, 그리고 그룹을 삭제할 수 있는 **그룹 삭제 아이콘**이 포함되어 있다.

또한, 카드의 **멤버 보기** 버튼을 통해 현재 그룹에 속해 있는 멤버 목록을 확인할 수 있다.

<img width="950" height="500" alt="image" src="https://github.com/Let-s-Study-Now/Let-s-Study-Now_doc/blob/main/SDS/6.%20User%20interface%20prototype/images/ui_groupstudy.png?raw=true" />

---

## ➕ 5-1. 그룹 만들기

사용자가 **그룹 만들기** 버튼을 클릭하면 '새 그룹 만들기' 탭이 나타난다. 이 탭에서 그룹 이름을 작성하여 새로운 그룹을 생성할 수 있다. 그룹을 만드는 행위는 나중에 초대 링크를 사용하여 친구를 초대하고 함께 공부할 기반을 마련하는 목적을 가진다.

<img width="950" height="500" alt="image" src="https://github.com/Let-s-Study-Now/Let-s-Study-Now_doc/blob/main/SDS/6.%20User%20interface%20prototype/images/ui_groupstudy_creategroup.png?raw=true" />

---

## 📖 5-2. 스터디 방 목록

**스터디 방** 탭에 들어가면 현재 진행 중인 그룹 스터디 방의 목록이 카드 형식으로 사용자에게 표시된다. 각 카드에는 해당 스터디 방에 대한 간략한 정보가 포함되어 있어, 사용자는 인원수, 스터디 진행 상태, 그리고 스터디의 주제인 공부 분야 등을 즉시 확인할 수 있다.

원하는 스터디 방을 발견하면, 해당 카드의 **입장하기** 버튼을 눌러 곧바로 스터디 방에 참여할 수 있다.

<img width="950" height="500" alt="image" src="https://github.com/Let-s-Study-Now/Let-s-Study-Now_doc/blob/main/SDS/6.%20User%20interface%20prototype/images/ui_groupstudy_roomlist.png?raw=true" />

---

## ➕ 5-3. 스터디 방 생성

사용자는 스터디 방을 생성할 수 있으며, 이 과정에서 몇 가지 필수 정보를 입력해야만 스터디 방 목록에 해당 방이 정상적으로 표시된다.

스터디 방을 만들기 위해서는 먼저 **그룹 추가** 섹션에서 미리 생성된 그룹을 선택해야 한다. 그룹 선택 후에는 해당 스터디 방의 **방 제목**을 입력하고, 스터디의 주제가 될 **공부 분야**를 명시해야 한다.

마지막으로, 방에 참여할 **참여 인원**과 **목표 공부 시간**을 필수적으로 입력해야 한다. 이러한 모든 정보가 빠짐없이 입력되었을 때, 비로소 스터디 방 탭에서 생성된 스터디 방 목록을 확인할 수 있다.

<img width="950" height="500" alt="image" src="https://github.com/Let-s-Study-Now/Let-s-Study-Now_doc/blob/main/SDS/6.%20User%20interface%20prototype/images/ui_groupstudy_createroom.png?raw=true" />

---

## 📖 5-4. 그룹 스터디 방 입장

그룹 스터디 방에 입장하면 사용자는 **학습 타이머**를 통해 '공부중' 또는 '휴식중' 버튼을 눌러 자신의 학습 시간을 정확하게 기록할 수 있으며, 이 기록된 시간은 추후 개인의 **공부 레벨 업데이트**에 반영된다.

효율적인 집중을 돕기 위해 **뽀모도로(학습 주기 타이머)** 기능이 제공된다. 이 타이머를 활용하여 작업(25분), 짧은 휴식(5분), 긴 휴식(15분) 주기를 정해두고 학습이 가능하며, 설정된 시간에 도달하면 종료 알림이 생성된다.

또한, 학습 환경을 조성하기 위해 **학습 음악 기능**을 통해 백색소음, 분위기 음악, 자연 음악 등을 실행시킬 수 있다.

다른 사용자들과의 소통을 위해서는 **질문 모드 채팅 시스템**이 마련되어 있어, 문제에 대한 답변을 위한 채팅 칸이 따로 지정되어 분리가 가능하다.

스터디를 함께 할 친구를 초대하려면 **초대 버튼**을 눌러 자동으로 생성되는 초대 링크를 공유하면 된다. 현재 방에 참여 중인 학습자 목록은 **사람 아이콘**을 클릭하여 확인할 수 있다.

스터디를 마칠 때, 일반 참여자는 **나가기 버튼**을 사용하여 방을 종료하며, 방장은 **방 삭제 기능**을 통해 스터디 방을 영구적으로 삭제할 수 있다.

<img width="950" height="500" alt="image" src="https://github.com/Let-s-Study-Now/Let-s-Study-Now_doc/blob/main/SDS/6.%20User%20interface%20prototype/images/ui_groupstudyroom.png?raw=true" />

---

## 🗓️ 6. 체크리스트 페이지

위 그림은 체크리스트 화면이다. 사용자가 날짜별로 학습 목표를 설정하고 달성률을 확인하며 루틴을 관리할 수 있다. 사용자가 설정한 체크리스트의 목표 달성 여부는 메인 페이지의 체크리스트 탭에서 **달성률**을 통해 쉽게 확인할 수 있다.

<img width="950" height="500" alt="image" src="https://github.com/Let-s-Study-Now/Let-s-Study-Now_doc/blob/main/SDS/6.%20User%20interface%20prototype/images/ui_checklist.png?raw=true" />

---

## ✏️ 6-1. 체크리스트 생성 및 관리

사용자는 화면 왼쪽에 위치한 **캘린더**를 이용하여 원하는 날짜를 선택할 수 있다. **체크리스트 생성** 버튼을 통해 새로운 항목을 추가하면, 해당 항목이 화면에 띄워진다.

사용자는 항목 옆의 **체크**를 통해 완료한 항목을 쉽게 정리할 수 있으며, 필요에 따라 항목을 **수정**하거나 **삭제**하는 것도 가능하다.

<img width="950" height="500" alt="image" src="https://github.com/Let-s-Study-Now/Let-s-Study-Now_doc/blob/main/SDS/6.%20User%20interface%20prototype/images/ui_checklist_add.png?raw=true" />

---

## 🙍 7. 마이 페이지

위 그림은 사용자가 자신의 정보를 관리할 수 있는 **마이 페이지** 화면이다. 이 페이지 내의 **프로필 정보** 탭에서는 사용자가 자신의 개인 정보를 유연하게 변경할 수 있는 기능을 제공한다.

구체적으로, 사용자는 **프로필 이미지 변경**, **이메일 수정**, **닉네임 변경**, 학습 관심 분야인 **공부 분야 수정**, 그리고 자신을 소개하는 **자기소개 내용 변경**이 가능하다.

<img width="950" height="500" alt="image" src="https://github.com/Let-s-Study-Now/Let-s-Study-Now_doc/blob/main/SDS/6.%20User%20interface%20prototype/images/ui_mypage.png?raw=true" />

---

## 🔑 7-1. 비밀번호 변경

**현재 비밀번호**, **새 비밀번호**, **새 비밀번호 확인**을 입력하여 비밀번호를 변경할 수 있다.

<img width="950" height="500" alt="image" src="https://github.com/Let-s-Study-Now/Let-s-Study-Now_doc/blob/main/SDS/6.%20User%20interface%20prototype/images/ui_mypage_password.png?raw=true" />

---

## 🗑️ 7-2. 계정 탈퇴

사용자는 자신의 계정을 삭제할 수 있는 기능을 사용할 수 있다. 이 기능을 실행하면 시스템은 사용자에게 정말로 탈퇴할 것인지 **경고 메시지**를 제공하여 최종 확인을 거친다. 사용자가 경고 메시지를 확인하고 절차를 완료하면 탈퇴가 가능하다.

<img width="950" height="500" alt="image" src="https://github.com/Let-s-Study-Now/Let-s-Study-Now_doc/blob/main/SDS/6.%20User%20interface%20prototype/images/ui_mypage_delete.png?raw=true" />



