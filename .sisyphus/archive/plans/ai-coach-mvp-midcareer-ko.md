# 중장년층 리스킬링을 위한 AI 코치 MVP (공모전 실행 계획)

## TL;DR

> **요약**: 재직자/중장년 학습자가 역량 격차를 진단하고, 개인화된 주간 학습 경로를 받고, 진행 피드백을 받을 수 있는 공모전 제출용 AI 코칭 웹앱을 구축한다.
>
> **산출물**:
> - Next.js + Supabase 기반 MVP (인증, 진단, 코칭 경로, 주간 피드백)
> - WSL 우선 개발 워크플로우 및 검증 스크립트
> - 공모전 제출용 AI 보고서 증빙 산출물 및 QA 캡처
>
> **예상 공수**: 중간
> **병렬 실행**: YES — 3개 웨이브 + 최종 검증 웨이브
> **크리티컬 패스**: T1 → T4 → T8 → T10 → T12 → F1-F4

---

## 배경(Context)

### 원요청
KIT 바이브코딩 공모전을 위해, AI 시대 교육 격차를 줄이는 AI 교육 에이전트 아이디어와 실행 전략을 수립한다. 우선순위는 재직자/중장년 학습자의 실용적 도입이며, 이후 전 연령 확장 및 시니어 친화 UX를 고려한다.

### 인터뷰 요약
**핵심 논의**:
- 타깃 전략 확정: **전 연령 적용 가능 + 시니어 친화 UX**, 단 MVP의 1차 페르소나는 **중장년 직장인**.
- 제품 범위 확정: **AI 코칭 웹앱** (진단 → 개인화 경로 → 주간 피드백).
- 기술 스택 확정: **Next.js + Supabase**.
- 검증 방식 확정: **구현 후 테스트**(완전 TDD 아님), 단 에이전트 실행 QA 시나리오는 필수.
- 운영 모델 확정: **Windows 호스트 + WSL 중심 실행**.
- gstack 활용 확정: **운영 패턴만 차용**, 전체 포크 금지.

**조사 결과**:
- `garrytan/gstack`은 공개 저장소이며 프로세스 레퍼런스로 활용 가능.
- 공모전 평가지표는 AI 협업 증빙과 현장 적용 가능성을 높게 평가함.

### Metis 리뷰 반영사항
**식별된 갭(보완 완료)**:
- 범위 확장(scope creep) 위험 → 엄격한 MVP 범위 + 명시적 Must-NOT-Have 경계 추가
- 접근성 모호성 → 계량 가능한 시니어 친화 UX 점검 항목 추가
- 테스트 모호성 → "기능 슬라이스 구현 후 즉시 자동 테스트/QA 수행"으로 해석 고정
- gstack 드리프트 위험 → 허용 차용 목록을 명시

---

## 작업 목표(Work Objectives)

### 핵심 목표
재직자/중장년 학습자의 리스킬링 실행 품질을 실제로 개선하는 AI 코칭 MVP를 시연 가능 상태로 출시하고, 접근성 고려 설계 및 운영 완성도를 공모전 심사 기준에 맞춰 입증한다.

### 구체적 산출물
- 웹앱 라우트: 온보딩, 진단, 학습 계획, 주간 피드백, 진척도 뷰
- Supabase 기반 인증 및 사용자 단위 데이터 격리(RLS)
- 안전한 폴백 동작을 포함한 AI 코칭 파이프라인
- 공모전/보고서 재사용을 위한 `.sisyphus/evidence/` 증빙 패키지

### 완료 정의(Definition of Done)
- [ ] 사용자가 로그인 후 진단을 완료하고, 개인화된 주간 계획을 수신하며, 주간 진행 상황을 기록할 수 있음
- [ ] 각 플로우에 자동 검증 결과가 존재함(Playwright/API/tests)
- [ ] DB 정책 점검에서 사용자 간 데이터 노출이 없음
- [ ] WSL 환경에서 build/typecheck/test 명령이 통과함

### Must Have
- 중장년 페르소나에 맞춘 명확한 온보딩
- 개인화 주간 계획 생성(결정론적 폴백 포함)
- 진행 변화(delta) 표시가 있는 주간 피드백 루프
- 접근성 기본 점검(키보드 플로우 + 확대 + 가독성 높은 UI 문구)
- MVP 데이터 전략: 사용자 입력 프로필/진단 + 샘플 시드 데이터(대규모 외부 데이터 수집 없음)

### Must NOT Have (가드레일)
- 전체 LMS 구축 금지(강좌/수업 관리/복잡한 콘텐츠 저작 기능)
- 취업 보장/채용 보장 또는 법률·의료 자문성 동작 금지
- gstack 전체 프레임워크 재구현/벤더링 금지
- 명시적 블로커 없이는 추가 인프라(큐/벡터DB/마이크로서비스) 도입 금지
- MVP v1에서 이력서 파일 파싱 파이프라인 금지

---

## 검증 전략(필수)

> **사람 개입 0(Zero Human Intervention)** — 모든 검증은 에이전트가 실행한다.

### 테스트 의사결정
- **기반 인프라 존재 여부**: NO (부트스트랩 필요 가정)
- **자동화 테스트 방식**: 구현 후 테스트(슬라이스별 즉시 검증)
- **프레임워크**: Vitest + Playwright

### QA 정책
- **프론트/UI**: Playwright 플로우 + 스크린샷
- **API/백엔드**: curl 점검 + JSON assertion
- **DB/보안**: Supabase 정책 검증 스크립트
- **증빙**: `.sisyphus/evidence/task-{N}-{slug}.*` 경로에 산출물 저장

---

## 실행 전략(Execution Strategy)

### gstack 패턴 차용(본 프로젝트 선택 항목)

> 전체 포크는 제외한다. 아래는 **패턴 이식(pattern transplant)**만 적용한다.

1. **워크플로우 커맨드 세트(office-hours 스타일)**
   - 프로젝트 로컬 프롬프트 커맨드를 다음 용도로 정의: discovery, plan review, QA, release check.
   - 커맨드 의도 예시:
     - `office-hours`: 문제 프레이밍 + 범위 가드레일 질문
     - `plan-review`: 현재 TODO 웨이브 기준 아키텍처/리스크 체크리스트
     - `qa-run`: 필수 시나리오 번들을 실행하고 증빙 캡처
     - `ship-check`: 제출 전 감사(테스트, 시크릿, 마감 안전 커밋)

2. **WSL 에이전트 운영 팩**
   - WSL 전용 실행 규칙, tmux 세션 네이밍, 산출물 경로를 표준화한다.
   - 모든 검증 명령은 WSL 내부에서 실행하고 로그를 `.sisyphus/evidence/`에 저장한다.

3. **검증/릴리스 체크리스트**
   - 공모전 제약 반영 단일 제출 전 체크리스트 추가:
     - 공개 저장소 가시성
     - API 키 유출 없음
     - 마감 시점 커밋 감사
     - 라이브 URL 헬스 체크
     - AI 보고서 완성도

4. **문서 템플릿 번들**
   - 다음 문서의 재사용 템플릿 준비:
     - AI 협업 보고서
     - 데모 스크립트(3분 버전 + 폴백 스크립트)
     - 심사기준 매핑 테이블(기술 완성도, AI 효율성, 기획력, 창의성)

### 병렬 실행 웨이브

웨이브 1 (기반 — 즉시 시작):
- T1. 프로젝트 스캐폴드 + WSL 런북
- T2. 디자인 토큰 + 시니어 친화 UI 베이스라인
- T3. 데이터 모델 + Supabase 스키마 + RLS 베이스라인
- T4. 인증 + 세션 기반
- T5. 테스트/부트스트랩 구성(Vitest + Playwright + evidence 디렉터리)

웨이브 2 (핵심 기능 — 웨이브 1 이후 최대 병렬):
- T6. 온보딩 + 진단 플로우 UI
- T7. AI 코칭 서비스 계약(contract) + 폴백 처리
- T8. 개인화 주간 계획 엔진
- T9. 주간 피드백 수집 + 진행 타임라인
- T10. API 엔드포인트 + 서버 액션 통합

웨이브 3 (통합 + 공모전 제출 준비):
- T11. E2E 연결 + UX 하드닝
- T12. 공모전 산출물 패키지(AI 보고서 입력, 스크린샷, 데모 스크립트)
- T13. 워크플로우 커맨드 템플릿(office-hours/plan-review/qa-run/ship-check)
- T14. WSL 운영 표준(tmux/세션/로깅 규칙)
- T15. 제출 전 릴리스 체크리스트 강제
- T16. 문서 템플릿 번들 최종화

최종 웨이브(FINAL, 병렬 리뷰 게이트):
- F1 계획 준수 감사
- F2 코드 품질 리뷰
- F3 실제 QA 실행
- F4 범위 충실도 점검

크리티컬 패스: T1 → T4 → T8 → T10 → T11 → T12 → F1-F4
최대 동시 실행 수: 5

### 의존성 매트릭스(전체)
- T1: 선행 없음 | 후속 영향 T6,T10,T11,T12
- T2: 선행 없음 | 후속 영향 T6,T11
- T3: 선행 없음 | 후속 영향 T8,T9,T10
- T4: 선행 T1,T3 | 후속 영향 T6,T10,T11
- T5: 선행 T1 | 후속 영향 T11,T12,F2,F3
- T6: 선행 T2,T4 | 후속 영향 T11
- T7: 선행 T1 | 후속 영향 T8,T10,T11
- T8: 선행 T3,T7 | 후속 영향 T10,T11
- T9: 선행 T3 | 후속 영향 T10,T11
- T10: 선행 T1,T3,T4,T7,T8,T9 | 후속 영향 T11,T12
- T11: 선행 T2,T4,T5,T6,T7,T8,T9,T10 | 후속 영향 T12,F1,F2,F3,F4
- T12: 선행 T1,T5,T10,T11 | 후속 영향 F1,F3
- T13: 선행 T1,T5 | 후속 영향 T15,T16,F1
- T14: 선행 T1 | 후속 영향 T15,F2,F3
- T15: 선행 T5,T10,T12,T13,T14 | 후속 영향 F1,F3
- T16: 선행 T12,T13 | 후속 영향 F1

### 에이전트 배치 요약
- 웨이브 1: T1/T5=`quick`, T2=`visual-engineering`, T3/T4=`unspecified-high`
- 웨이브 2: T6=`visual-engineering`, T7/T8=`deep`, T9/T10=`unspecified-high`
- 웨이브 3: T11=`deep`, T12=`writing`
- 최종: F1=`oracle`, F2=`unspecified-high`, F3=`unspecified-high`(+playwright), F4=`deep`

---

## TODO

- [ ] 1. 프로젝트 스캐폴드 + WSL 런북

  **수행 내용**:
  - Next.js 앱 구조 초기화 및 WSL 우선 명령 규약 적용
  - `.sisyphus/evidence/` 하위 구조 및 명령 인덱스 추가

  **금지 사항(Must NOT do)**:
  - 로컬 + 호스티드 Supabase 베이스라인을 넘는 인프라 추가 금지

  **권장 에이전트 프로필**:
  - Category: `quick` (초기 프로젝트 셋업)
  - Skills: `[]`

  **병렬화**: YES (웨이브 1) | 선행: 없음 | 후속 영향: T6,T10,T11,T12

  **참고**:
  - `https://github.com/garrytan/gstack` - 프로세스 영감용(전체 도입 금지)
  - `https://nextjs.org/docs` - 앱 스캐폴딩 및 런타임 패턴

  **수용 기준(Acceptance Criteria)**:
  - WSL에서 `npm run dev` 실행 시 앱이 정상 시작됨
  - 증빙: `.sisyphus/evidence/task-1-wsl-dev-start.txt`

  **QA 시나리오**:
  - 정상 경로: WSL에서 앱 실행, 홈 라우트 HTTP 200 확인, 터미널 로그 캡처
  - 실패 경로: 환경변수 누락 시 명확한 시작 오류 메시지 출력

- [ ] 2. 시니어 친화 디자인 베이스라인

  **수행 내용**:
  - 타이포 스케일, 여백, 대비 기준, 단순 네비게이션 패턴 정의
  - 큰 탭 타깃 및 가독성 높은 문구를 위한 재사용 UI 프리미티브 작성

  **금지 사항(Must NOT do)**:
  - 픽셀 단위 과도 디자인 및 비기능적 시각 범위 확장 금지

  **권장 에이전트 프로필**:
  - Category: `visual-engineering`
  - Skills: `[]`

  **병렬화**: YES (웨이브 1) | 선행: 없음 | 후속 영향: T6,T11

  **참고**:
  - `https://www.w3.org/WAI/WCAG22/quickref/` - 계량 가능한 접근성 기준

  **수용 기준(Acceptance Criteria)**:
  - 핵심 화면이 키보드 내비게이션 및 포커스 가시성 점검을 통과
  - 증빙: `.sisyphus/evidence/task-2-a11y-baseline.png`

  **QA 시나리오**:
  - 정상 경로: 키보드 탭 순서가 주요 컨트롤에 순차 도달
  - 실패 경로: 200% 확대 시 핵심 컨트롤이 사라지지 않음

- [ ] 3. Supabase 스키마 + RLS 베이스라인

  **수행 내용**:
  - profile, diagnosis, weekly_plan, feedback_log 테이블 정의
  - 사용자 단위 엄격 격리를 위한 RLS 정책 적용

  **금지 사항(Must NOT do)**:
  - 사용자 코칭 데이터에 대한 공개 공유 쓰기 경로 금지

  **권장 에이전트 프로필**:
  - Category: `unspecified-high`
  - Skills: `[]`

  **병렬화**: YES (웨이브 1) | 선행: 없음 | 후속 영향: T8,T9,T10

  **참고**:
  - `https://supabase.com/docs/guides/database/postgres/row-level-security`

  **수용 기준(Acceptance Criteria)**:
  - 정책 테스트 스크립트에서 사용자 A가 사용자 B 행을 읽지 못함
  - 증빙: `.sisyphus/evidence/task-3-rls-check.json`

  **QA 시나리오**:
  - 정상 경로: 인증 사용자는 본인 레코드만 읽기 가능
  - 실패 경로: 타 사용자 질의는 0행/권한 거부 반환

- [ ] 4. 인증/세션 기반

  **수행 내용**:
  - 로그인/로그아웃 및 보호된 앱 셸 라우팅 구현
  - 세션 만료 처리 및 재인증 프롬프트 보장

  **금지 사항(Must NOT do)**:
  - 블로커가 아닌 한 MVP에서 소셜 로그인 확장 금지

  **권장 에이전트 프로필**:
  - Category: `unspecified-high`
  - Skills: `[]`

  **병렬화**: YES (웨이브 1) | 선행: T1,T3 | 후속 영향: T6,T10,T11

  **참고**:
  - `https://supabase.com/docs/guides/auth`

  **수용 기준(Acceptance Criteria)**:
  - 세션이 없으면 보호 라우트 접근 불가
  - 증빙: `.sisyphus/evidence/task-4-auth-flow.png`

  **QA 시나리오**:
  - 정상 경로: 로그인 성공 후 대시보드로 리다이렉트
  - 실패 경로: 만료 토큰 발생 시 클린 재로그인 상태 강제

- [ ] 5. 테스트 및 증빙 하네스 부트스트랩

  **수행 내용**:
  - Vitest 및 Playwright 설정
  - CI/로컬 공통 산출물 저장 경로 표준화

  **금지 사항(Must NOT do)**:
  - 재사용 가능한 커맨드 래퍼 없는 일회성 취약 스크립트 금지

  **권장 에이전트 프로필**:
  - Category: `quick`
  - Skills: `[]`

  **병렬화**: YES (웨이브 1) | 선행: T1 | 후속 영향: T11,T12,F2,F3

  **참고**:
  - `https://playwright.dev/docs/intro`
  - `https://vitest.dev/guide/`

  **수용 기준(Acceptance Criteria)**:
  - `npm run test` 및 `npx playwright test`로 베이스라인 스위트 실행 가능
  - 증빙: `.sisyphus/evidence/task-5-test-bootstrap.txt`

  **QA 시나리오**:
  - 정상 경로: 클린 실행에서 테스트 명령 통과
  - 실패 경로: 의도적 실패 스펙 추가 시 CI 명령이 올바르게 실패

- [ ] 6. 온보딩 + 진단 플로우

  **수행 내용**:
  - 중장년 온보딩 폼(역할, 목표, 주당 학습 가능 시간) 구현
  - 진단 문항 및 점수 정규화 로직 추가

  **금지 사항(Must NOT do)**:
  - MVP에서 5분 초과 장문 설문 금지

  **권장 에이전트 프로필**:
  - Category: `visual-engineering`
  - Skills: `[]`

  **병렬화**: YES (웨이브 2) | 선행: T2,T4 | 후속 영향: T11

  **참고**:
  - `## 배경 > 인터뷰 요약` (본 계획 문서) - 페르소나/범위 의사결정

  **수용 기준(Acceptance Criteria)**:
  - 사용자가 5개 이하 화면에서 온보딩 + 진단 완료
  - 증빙: `.sisyphus/evidence/task-6-onboarding-flow.mp4`

  **QA 시나리오**:
  - 정상 경로: 유효 입력 시 진단 결과 페이지 도달
  - 실패 경로: 필수값 누락 시 명확한 인라인 오류 표시

- [ ] 7. AI 코칭 서비스 계약 + 폴백

  **수행 내용**:
  - 주간 계획 생성용 프롬프트 계약 정의
  - 타임아웃/빈 응답 대비 폴백 템플릿 추가

  **금지 사항(Must NOT do)**:
  - 설명 가능한 섹션 라벨 없이 불투명 블랙박스 출력 금지

  **권장 에이전트 프로필**:
  - Category: `deep`
  - Skills: `[]`

  **병렬화**: YES (웨이브 2) | 선행: T1 | 후속 영향: T8,T10,T11

  **참고**:
  - `https://platform.openai.com/docs/guides/prompt-engineering`

  **수용 기준(Acceptance Criteria)**:
  - AI 호출 실패 시 안전한 주간 폴백 템플릿 반환
  - 증빙: `.sisyphus/evidence/task-7-ai-fallback.json`

  **QA 시나리오**:
  - 정상 경로: AI 응답이 기대 구조 섹션으로 파싱됨
  - 실패 경로: 제공자 타임아웃 시 5초 이내 폴백 + 사용자 친화 문구 노출

- [ ] 8. 개인화 주간 계획 엔진

  **수행 내용**:
  - 진단 결과 + 목표를 주간 계획 블록으로 변환
  - 계획 버전 저장 및 재생성 로직 구현

  **금지 사항(Must NOT do)**:
  - 사용자 확인 없는 완전 자동 비가역 쓰기 금지

  **권장 에이전트 프로필**:
  - Category: `deep`
  - Skills: `[]`

  **병렬화**: YES (웨이브 2) | 선행: T3,T7 | 후속 영향: T10,T11

  **참고**:
  - `https://supabase.com/docs/guides/database`

  **수용 기준(Acceptance Criteria)**:
  - 재생성 시 기존 이력 보존 + 신규 버전 생성
  - 증빙: `.sisyphus/evidence/task-8-plan-versioning.txt`

  **QA 시나리오**:
  - 정상 경로: 사용자에게 주간 과제 + 근거가 포함된 계획 생성
  - 실패 경로: 잘못된 진단 페이로드는 명시적 검증 오류로 거부

- [ ] 9. 주간 피드백 및 진행 타임라인

  **수행 내용**:
  - 체크인 폼 및 주간 진행 타임라인 시각화 구현
  - 단순 추세(개선/유지/리스크) 계산

  **금지 사항(Must NOT do)**:
  - 사용자 입력 기반 측정치를 벗어나는 예측성 주장 금지

  **권장 에이전트 프로필**:
  - Category: `unspecified-high`
  - Skills: `[]`

  **병렬화**: YES (웨이브 2) | 선행: T3 | 후속 영향: T10,T11

  **참고**:
  - `## 작업 목표 > Must Have` (본 계획 문서) - 주간 피드백 요구사항 출처

  **수용 기준(Acceptance Criteria)**:
  - 사용자가 주간 상태를 제출하고 타임라인 변화 확인 가능
  - 증빙: `.sisyphus/evidence/task-9-weekly-feedback.png`

  **QA 시나리오**:
  - 정상 경로: 신규 체크인이 즉시 타임라인에 반영
  - 실패 경로: 동일 일자 중복 제출은 병합/업데이트 규칙으로 처리

- [ ] 10. API/서버 통합 레이어

  **수행 내용**:
  - UI 플로우를 백엔드 액션/엔드포인트와 연결
  - 경계 지점에서 인증 검사 및 입력 검증 보장

  **금지 사항(Must NOT do)**:
  - 클라이언트 측 권한 DB 직접 접근 금지

  **권장 에이전트 프로필**:
  - Category: `unspecified-high`
  - Skills: `[]`

  **병렬화**: YES (웨이브 2) | 선행: T1,T3,T4,T7,T8,T9 | 후속 영향: T11,T12

  **참고**:
  - `https://nextjs.org/docs/app/building-your-application/data-fetching`

  **수용 기준(Acceptance Criteria)**:
  - 인증되지 않은 보호 API 호출은 401 반환
  - 증빙: `.sisyphus/evidence/task-10-api-auth.txt`

  **QA 시나리오**:
  - 정상 경로: 인증 호출이 기대 스키마 반환
  - 실패 경로: 잘못된 페이로드는 필드 단위 오류 포함 400 반환

- [ ] 11. 엔드투엔드 통합 + UX 하드닝

  **수행 내용**:
  - 전체 사용자 여정 연결 및 치명적 UX 마찰 제거
  - 키보드/포커스/확대/가독성 메시지 중심 접근성 하드닝

  **금지 사항(Must NOT do)**:
  - 무관한 기능 확장(마켓플레이스, 커뮤니티, 엔터프라이즈 관리자) 금지

  **권장 에이전트 프로필**:
  - Category: `deep`
  - Skills: `[]`

  **병렬화**: NO (웨이브 3) | 선행: T2,T4,T5,T6,T7,T8,T9,T10 | 후속 영향: T12,F1,F2,F3,F4

  **참고**:
  - `https://www.w3.org/WAI/WCAG22/quickref/`

  **수용 기준(Acceptance Criteria)**:
  - Playwright E2E 실행에서 전체 사용자 여정 통과
  - 증빙: `.sisyphus/evidence/task-11-e2e-suite.txt`

  **QA 시나리오**:
  - 정상 경로: 로그인 → 진단 → 계획 → 피드백 → 타임라인 전체 통과
  - 실패 경로: AI 타임아웃 시뮬레이션에서 폴백 노출 + 워크플로우 복구 가능

- [ ] 12. 공모전 제출 산출물 패키지

  **수행 내용**:
  - AI 보고서 입력자료, 아키텍처 요약, 증빙 인덱스 준비
  - 심사기준 정렬 데모 스크립트 준비

  **금지 사항(Must NOT do)**:
  - 증빙 경로 없는 비검증 주장 금지

  **권장 에이전트 프로필**:
  - Category: `writing`
  - Skills: `[]`

  **병렬화**: NO (웨이브 3 종료) | 선행: T1,T5,T10,T11 | 후속 영향: F1,F3

  **참고**:
  - 공지 기준 제출요건(깃허브 URL, 라이브 URL, AI 보고서 PDF, 서명 문서)

  **수용 기준(Acceptance Criteria)**:
  - 링크 및 증빙 매핑 테이블 포함 체크리스트 완성
  - 증빙: `.sisyphus/evidence/task-12-submission-checklist.md`

  **QA 시나리오**:
  - 정상 경로: 제출 필수 항목이 모두 존재하고 접근 가능
  - 실패 경로: 누락 항목 탐지 스크립트가 미완료 패키지를 플래그 처리

---

## 최종 검증 웨이브(필수)

- [ ] F1. **계획 준수 감사** — `oracle`
  출력: `Must Have [N/N] | Must NOT Have [N/N] | Tasks [N/N] | VERDICT`

- [ ] F2. **코드 품질 리뷰** — `unspecified-high`
  출력: `Build [PASS/FAIL] | Lint [PASS/FAIL] | Tests [N/N] | VERDICT`

- [ ] F3. **실 QA(에이전트 실행)** — `unspecified-high` (+playwright)
  출력: `Scenarios [N/N pass] | Integration [N/N] | Edge Cases [N] | VERDICT`

- [ ] F4. **범위 충실도 점검** — `deep`
  출력: `Tasks [N/N compliant] | Contamination [CLEAN/N issues] | VERDICT`

---

## 커밋 전략

- C1: `chore(setup): bootstrap nextjs wsl workflow and evidence structure`
- C2: `feat(auth-db): add supabase schema rls and auth shell`
- C3: `feat(coach): implement diagnosis and weekly plan generation with fallback`
- C4: `feat(progress): add weekly feedback and timeline`
- C5: `test(e2e): integrate playwright flows and accessibility checks`
- C6: `docs(contest): finalize submission artifact mapping`

---

## 성공 기준(Success Criteria)

### 검증 명령
```bash
npm run lint         # 기대값: 오류 0
npm run test         # 기대값: 모든 unit/integration 테스트 통과
npx playwright test  # 기대값: 모든 e2e 시나리오 통과
```

### 최종 체크리스트
- [ ] 모든 Must-Have 플로우 시연 완료
- [ ] Must-NOT-Have 경계 준수
- [ ] 접근성 베이스라인 증빙 존재
- [ ] RLS/사용자 격리 증빙 존재
- [ ] 공모전 제출 패키지 완성
