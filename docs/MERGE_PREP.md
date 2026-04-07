# Merge Prep

## 현재 기준으로 남겨야 할 핵심

이 레포는 아직 앱 코드가 아니라 **아이디어/리서치 중심 저장소**다.
합치기 전 기준 문서는 아래 3축으로 본다.

1. `README.md`
   - 현재 프로젝트의 가장 짧은 정체성

2. `docs/ideas.md`
   - 초기 아이디어 탐색과 문제정의 기록

3. `docs/research_docs/office_hour_result.md`
   - gstack `/office-hours` 결과 중 가장 살아 있는 방향
   - 특히 현재는 `confusing financial messages`와 `pre-action education agent` 방향이 가장 구체적이다.

---

## archive로 옮긴 이유

아래 문서들은 틀린 문서는 아니지만, **아이디어 확정 전에 너무 구체적이거나 방향이 이미 바뀐 문서**라서 전면에 두지 않았다.

- gstack 차용 가이드
- 프로젝트 전용 명령 템플릿
- WSL 운영 규칙
- 제출 체크리스트
- 초기 UI 프롬프트
- 중장년 AI 코치형 앱 계획서
- discovery 초안

이 문서들은 버린 것이 아니라 `archive`로 이동했다.
필요할 때 재사용하면 된다.

---

## 지금 합칠 때 기준 문서

WSL에서 작업을 이어갈 때는 아래 순서로 문서를 참고한다.

1. `docs/research_docs/office_hour_result.md`
2. `docs/ideas.md`
3. `README.md`

즉, 현재 기준 메인 질문은 이것이다.

> 우리는 `AI 첫걸음`을 유지하되,
> **막연한 생활형 교육 앱**이 아니라
> **실제 생활 이벤트 전에 행동을 준비시켜 주는 pre-action education agent**로 좁힐 것인가?

---

## WSL에서 합칠 때 실제 의미

`gstack` 레포를 이 프로젝트에 합치는 것이 아니다.

정확한 구조는 아래다.

- `~/gstack` = 글로벌 도구
- `~/projects/universal_edu` = 실제 작업 repo

즉, WSL에서 `universal_edu`를 clone 한 뒤,
그 프로젝트 안에서 `/gstack-office-hours`, `/gstack-plan-ceo-review`, `/gstack-review` 등을 실행해 산출물만 이 repo에 남긴다.

---

## 다음 단계 추천

1. WSL에 `universal_edu` clone
2. `docs/research_docs/office_hour_result.md` 기반으로 방향 확정
3. `/gstack-plan-ceo-review` 실행
4. 최종 문제정의 1개 확정
5. 그 뒤에만 Stitch/구현 문서 재생성
