# WSL 운영 규칙

## 원칙

- 개발 실행 허브는 **WSL**로 통일한다.
- Windows는 문서 확인, 브라우저 데모, 발표 자료 작업에 사용한다.
- 테스트 로그와 증빙은 모두 프로젝트 내부에 남긴다.

---

## 권장 tmux 세션

- `uedu-app` : 개발 서버
- `uedu-test` : 테스트 실행
- `uedu-agent` : OpenCode/Codex 작업
- `uedu-notes` : 로그/메모 확인

---

## 기본 실행 순서

1. WSL 진입
2. 프로젝트 실행
3. 테스트 실행
4. 증빙 저장
5. 제출 전 체크

---

## 로그/증빙 규칙

- 텍스트 로그: `.sisyphus/evidence/*.txt`
- 리뷰/계획 문서: `.sisyphus/evidence/*.md`
- QA 산출물: `.sisyphus/evidence/qa-run/`
- 화면 캡처: `.sisyphus/evidence/screenshots/`

---

## 작업 단위 규칙

- 한 번에 하나의 기능 슬라이스만 진행
- 기능 완료 직후 테스트 실행
- 테스트 결과 없으면 완료로 간주하지 않음
- 증빙 없이 “됐다”고 보고하지 않음

---

## 추천 작업 루프

1. `/discovery`
2. `/plan-scope`
3. `/plan-arch`
4. 구현
5. `/review-impl`
6. `npm run test`
7. `/qa-run`
8. `/ship-check`
