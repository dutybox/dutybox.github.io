# CrewLife 개발 로드맵

> 2026-04-18 개정 — v0.2.3, Phase 1-E 카카오 로그인 완료 반영.

## Phase 1 — 환경 설정 + 공항 DB + 새 하단바 UI + 카카오 로그인

### 1-A 인프라 ✅ 완료
- [x] Flutter SDK 설치 및 `app/` 프로젝트 생성
- [x] Supabase 프로젝트 + `.env` + DB 마이그레이션 (`airports`, `profiles`, `schedules`)
- [x] 기본 탭 스캐폴딩 (4단 NavigationBar) — **이후 1-C에서 재작업**
- [x] 실기기 연결 확인 (`flutter run` OK, 2026-04-14)

### 1-B 공항 DB 시드 ✅ 완료 (2026-04-15)
- [x] `supabase/seeds/airports.sql` 137개 공항 적재
- [x] `airports` 테이블 스키마 단순화 (0002_simplify_airports.sql)

### 1-C 하단바 UI 재설계 ✅ 완료 (2026-04-15)
- [x] 기존 4단 `NavigationBar` 제거
- [x] 3슬롯 커스텀 하단바: 좌(친구/내 스케줄 토글) · 중앙(빠른 입력 FAB) · 우(설정)
- [x] 좌측/우측 사이드 드로어, 친구 화면 토글, 상단 AppBar 알림 벨 자리

### 1-D 항공사 마스터 데이터 ✅ 완료 (2026-04-15)
- [x] `airline_registry.dart` — 15개 항공사 시드 + 브랜드 컬러
- [x] `detectAirlineFromFlightNumber()` 유틸 + 단위 테스트
- [x] `app/assets/airlines/` 폴더 + README
- [x] 항공사 모듈 시스템 (`modules/`) — KE 완성, 14개 빈 모듈 (v0.1.8)

### 1-E 카카오 로그인 ✅ 완료 (v0.2.3, 2026-04-18)
- [x] Kakao Developers 앱 등록 + 네이티브/REST 앱키 발급
- [x] `kakao_flutter_sdk_user` 패키지 추가
- [x] 카카오 로그인 → Edge Function `kakao-auth` → Supabase 세션 발급
- [x] 앱 시작 시 세션 유효성 검증 (`refreshSession()`) + 스플래시 화면
- [x] `AuthService.fullReset()` — 카카오 unlink + Supabase signOut (처음 가입 테스트용)
- [x] 설정 드로어: 로그아웃 + 완전 초기화(개발용) 버튼

**Exit criteria**: 카카오 로그인 → 홈 진입 → 앱 재시작 시 자동 로그인 → 새 하단바 정상 동작 → Supabase 연결 확인. ✅

## Phase 2 — AI OCR + 수동 정정 + 빠른 입력 + 캘린더 연동

### 2-A AI OCR ✅ 완료 (v0.1.8, 2026-04-16)
- [x] Supabase Edge Function `parse-schedule` (Claude Sonnet 4.6 Vision)
- [x] 이미지 업로드 UI → 파싱 → LocalFlightStore → 달력 반영
- [x] 수동 정정 다이얼로그 (편명/공항 선택/시간 스크롤 피커/색상)
- [x] 공항 선택형 피커 (137개, 검색, ICN/GMP/PUS/CJU 최상단)
- [x] 편명 → 항공사 감지 → 인사말 자동 변경 + KE 환영 팝업
- [x] AI는 스케줄당 1회만 호출 (DB 플래그 가드 — 현재 비활성, 카카오 로그인 후 활성화)
- [x] 매직 바이트 기반 이미지 포맷 감지 (PNG/JPEG 불일치 해결)

### 2-B 수동 입력 ✅ 완료 (v0.2.2, 2026-04-17)
- [x] QuickAddSheet 실제 저장 연결 + 날짜 탭 → 수동 추가/수정/삭제 통합
- [x] 비행/오프 탭 강조 (56px 컬러 박스) + 편명 KE 고정 + 숫자만 입력
- [x] ATDO/OFF 분리 (OCR→ATDO, 수동→OFF)
- [x] 오프 저장 시 즉시 달력 복귀
- [ ] 카카오 로그인 완료됨 → `schedules` 테이블 직저장으로 전환 (LocalFlightStore → DB)

### 2-C 꾸미기
- [ ] 스티커, 자유 그리기, 메시지 (`signature` 패키지)
- [ ] 항공사 브랜드 배경/테두리 적용

### 2-D 외부 캘린더 연동
- [ ] Google Calendar OAuth + 캘린더 목록 선택 UI (구글 프로필 선택 느낌)
- [ ] Apple Calendar EventKit 권한 + 캘린더 목록 선택
- [ ] 설정 드로어에 "캘린더 연동" 진입점
- [ ] 동기화 방향: CrewLife → 외부 (1-way)

**비용 가드**: AI 호출은 최초 1회만. 재편집은 로컬 상태 변경으로만.

## Phase 3 — 공유 + 커뮤니티

### 3-A 스케줄 공유 (외부 전송)
- [ ] 현재 달력 화면을 이미지로 캡처 (`RepaintBoundary` + `RenderRepaintBoundary.toImage()`)
- [ ] 공유 채널: 에어드랍, 퀵쉐어, 카카오톡, 로컬 갤러리 저장 (`share_plus` + `image_gallery_saver`)
- [ ] 공유 버튼 위치: 현재 개발자용 초기화 버튼 자리 (AppBar 좌측) → 출시 시 교체
- [ ] 화면 캡처 없이 현재 보이는 그대로 공유 (스크린샷 불필요)

### 3-B 친구 시스템
- [ ] 연락처 권한 + 전화번호 기반 친구 탐색
- [ ] 친구 드로어에 **카카오톡으로 친구 초대** 버튼 추가 (`kakao_flutter_sdk_share`)
- [ ] 초대 코드 발급/검증 (신뢰 기반 인증 루트 C)
- [ ] 자유 그룹 생성 — 친구 여러 명을 내 맘대로 그룹으로 묶기

### 3-C 친구 스케줄 비교 뷰
- [ ] 2주 단위 리스트 뷰: 나 → 친구1 → 친구2 → … 순서대로 세로 나열
- [ ] 그룹 선택 시 해당 그룹 멤버만 표시
- [ ] 친구 스케줄 공유 권한 (전체/기간/비공개)

### 3-D 활동 피드
- [ ] 활동 소식 피드 + 응원 보내기 (AppBar 벨 아이콘 연결)
- [ ] 친구 드로어 내부 목록 실제 데이터 연결

## Phase 4 — 보안 + 수익화 + 출시

- [ ] 생체인증 (Face ID, 지문, PIN)
- [ ] AdMob 배너 + 보상형 (친구 10명 초과 시 슬롯 개방)
- [ ] 구독 결제 (월 1,100~1,900원) — 광고 제거
- [ ] 스케줄표 패턴 매칭 자동 승무원 인증 (루트 A)
- [ ] App Store / Play Store 제출

## Backlog (Phase 5+)

- 사원증 사진 인증 (루트 B) — 민감정보 자동 마스킹
- 다국어 지원 (영어, 일본어, 중국어)
- 팀/크루 그룹 채팅
- 캘린더 양방향 동기화
