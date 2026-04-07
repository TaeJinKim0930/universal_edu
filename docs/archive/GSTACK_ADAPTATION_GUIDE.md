# gstack 패턴 차용 가이드

## 목적

이 프로젝트는 `garrytan/gstack`를 **포크하지 않고**, 그 안의 **운영 방식**만 차용한다.

핵심은 다음 4가지다.

1. 명령 기반 작업 흐름
2. 계획 → 리뷰 → QA → 제출 점검 루프
3. 증빙 중심 문서화
4. WSL 기반 실행 표준화

---

## 가져올 것 / 바꿔서 가져올 것 / 가져오지 않을 것

### Borrow
- 역할별 명령 흐름
  - `office-hours`
  - `plan-*`
  - `review`
  - `qa`
  - `ship`
- 문서가 다음 작업의 입력이 되는 구조
- 작업 결과를 증빙 파일로 남기는 습관
- 제출 직전 체크리스트 기반 운영

### Adapt
- Claude/OpenClaw 중심 명령을 **OpenCode + Codex + WSL** 기준으로 재정의
- gstack의 광범위한 제품/디자인/배포 기능을 **공모전 MVP 범위**로 축소
- 범용 slash command를 우리 프로젝트용 내부 명령 문서/프롬프트 템플릿으로 치환

### Exclude
- 전체 레포 구조 복제
- gstack 전용 도구/호스트 의존성
- 과도한 브라우저/배포/분석 인프라
- 공모전 MVP와 무관한 역할 세분화

---

## 우리 프로젝트용 명령 매핑

| gstack 계열 | 우리 명령 | 목적 | 입력 | 출력 | 증빙 |
|---|---|---|---|---|---|
| office-hours | `/discovery` | 문제정의, 페르소나, 범위 고정 | 아이디어, 대상 사용자, 제약 | 문제정의 문서 | `.sisyphus/evidence/discovery.md` |
| plan-ceo-review | `/plan-scope` | MVP 범위 압축, 차별화 포인트 명확화 | discovery 결과 | 범위/우선순위 결정 | `.sisyphus/evidence/plan-scope.md` |
| plan-eng-review | `/plan-arch` | 기술 구조, 데이터 흐름, 리스크 정리 | 요구사항, 스택 | 실행 플랜, 아키텍처 초안 | `.sisyphus/evidence/plan-arch.md` |
| review | `/review-impl` | 구현 품질, 누락, 범위 이탈 점검 | 변경사항, 플랜 | 수정 포인트 목록 | `.sisyphus/evidence/review-impl.md` |
| qa / qa-only | `/qa-run` | 핵심 사용자 플로우 검증 | 앱 URL/로컬 앱 | QA 결과, 실패 목록 | `.sisyphus/evidence/qa-run/` |
| ship | `/ship-check` | 제출 직전 최종 점검 | repo, live URL, 산출물 | 제출 준비 상태 | `.sisyphus/evidence/ship-check.md` |

---

## 추천 운영 순서

1. `/discovery`
2. `/plan-scope`
3. `/plan-arch`
4. 구현
5. `/review-impl`
6. `/qa-run`
7. `/ship-check`

이 순서를 벗어나는 경우는 아래 둘 뿐이다.

- 긴급 버그 수정
- 제출 마감 직전 증빙 누락 보완

---

## 공모전 맥락에서 왜 이 구조가 유효한가

### 기술적 완성도
- 구현 전에 범위와 구조를 고정하므로 완성도가 올라간다.

### AI 활용 능력 및 효율성
- AI를 단순 코드 생성기가 아니라, 기획/검토/QA 파트너로 사용한다.

### 기획력 및 실무 접합성
- discovery → scope → architecture 흐름이 실제 제품 개발 프로세스와 맞닿아 있다.

### 창의성
- 흔한 “AI 튜터”가 아니라, 운영 자체를 AI 협업형으로 설계한 점을 설명할 수 있다.

---

## 프로젝트에 바로 적용할 최소 단위

우선 아래 4개만 도입하면 충분하다.

1. `docs/PROJECT_COMMANDS.md`
2. `docs/WSL_OPS.md`
3. `docs/SUBMISSION_CHECKLIST.md`
4. `.sisyphus/evidence/` 증빙 폴더 규칙

이후 필요하면 아래를 추가한다.

- `AGENTS.md`
- `DESIGN.md`
- `AI_REPORT.md`
