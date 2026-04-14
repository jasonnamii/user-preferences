---
linked_to: UP_user-preferences_v31.1.md
created: 2026-04-08T00:00:00.000Z
last_reviewed: 2026-04-14T00:00:00.000Z
---

# UP Stability Map

UP 규칙별 안정도 추적. v29.0에서 345줄→37줄 재설계로 전면 리셋.

---

## 상태 정의

| 상태 | 의미 | 전환 조건 |
|------|------|-----------| 
| **frozen** | 핵심 원칙. 변경 가능성 극히 낮음 | → stable: 해당 규칙 수정 발생 시 자동 강등 |
| **stable** | 체계 확립. 미세 조정 가능 | → frozen: 형 명시 요청만. → trial: 해당 없음(신규 규칙만 trial) |
| **trial** | 실험 중. 언제든 변경·폐기 가능 | → stable: 5세션 연속 무수정 시 자동 승격. → 폐기: 형 판단 |

---
## 운영 규칙

```
REVIEW_CYCLE ::= up-manager 파이프라인 내 자동 갱신
  TIMING: QC 후, 보고 전
  ACTION:
    수정된 규칙이 frozen → stable 자동 강등 + 보고
    신규 규칙 추가 → trial 태깅
  PROMOTION: trial → stable: 5세션 연속 무수정 확인 시 자동 승격
  DEMOTION: frozen → stable: 수정 발생 시 자동
  SESSION_COUNT: 세션브리핑 생성 = 1세션
```
---

## 규칙별 상태

| 규칙 | 상태 | 근거 | 비고 |
|------|------|------|------|
| ~~PRIORITY~~ | — | v29.1 삭제 (Claude default) | |
| ~~BLIND_SPOT~~ | — | v31.0 FRAME에 흡수 | |
| ~~EXECUTOR~~ | — | v31.0 FRAME에 흡수 | |
| FRAME | trial | v31.0 신규. BLIND_SPOT+EXECUTOR 통합. 상위원리 계층화 | 신규 trial |
| CONFIDENCE | stable | v30.7: fabrication=FAIL+"모름"+LOGIC_TAG cap 추가 | v30.0~연속 수정 |
| INFO_BRANCH | stable | 실용 규칙 | |
| ~~REPORT_FORMAT~~ | — | v29.3 삭제 (EDIT4에 Before/After 포함, 중복) | |
| ~~PLAN_GATE~~ | — | v29.1 삭제 (Cowork default) | |
| ~~TONE~~ | — | v30.9 삭제 → DENSITY에 흡수 | |
| DENSITY | trial | v30.9 신규. 상위원리(구조설치)+하위8규칙. TONE 흡수 | 신규 trial |
| ~~INTERNAL_VOCAB~~ | — | v29.3 삭제 (deliverable-engine에 병합 가능) | |
| ~~NUM_VERIFY~~ | — | v31.0 VERIFY에 흡수 | |
| ~~SOURCE~~ | — | v31.0 VERIFY에 흡수 | |
| VERIFY | trial | v31.0 신규. SOURCE+NUM_VERIFY 통합. 상위원리 계층화 | 신규 trial |
| ~~SAVE~~ | — | v29.3 삭제 (_archive 규칙→EDIT4 POST ⑤ 흡수) | |
| ~~TOOL_PRIORITY~~ | — | v29.3 삭제 (환경 의존적, 발동 빈도 낮음) | |
| ~~SKILL_ROUTER~~ | — | v29.3 삭제 (스킬 시스템 자체 트리거 매칭으로 충분) | |
| EDIT4 | stable | v29.3: POST ⑤ _archive/ move 흡수 | |
| ~~OBSIDIAN~~ | — | v30.6 삭제 (redundant: 시스템 기본동작으로 충분) | |
| ~~MCP_SPEED~~ | — | v30.6 삭제 (redundant: 프로토콜 기본원칙으로 충분) | |
| CAREFUL_READ | trial | v30.7 신규 추가. 반전위험7 정밀읽기 | 신규 trial |
| LOGIC_TAG | trial | v30.7 신규 추가. CONFIDENCE 서브규칙(추론유형 캡) | 신규 trial |
| OVERWRITE_BAN | trial | v30.8 신규 추가. 기존파일 Write 절대금지 글로벌 승격 | 신규 trial |

---

## 요약

| 상태 | 항목 수 | 비율 |
|------|---------|------|
| frozen | 0 | 0% |
| stable | 3 | 14% |
| trial | 6 | 27% |
| 삭제 | 13 | 59% |

> 잔류 8개 규칙(+서브규칙). v31.0 전면 계층화로 frozen 0개. FRAME·DENSITY·VERIFY 신규 trial. CONFIDENCE·INFO_BRANCH·EDIT4 stable 유지. CAREFUL_READ·LOGIC_TAG·OVERWRITE_BAN trial 유지.
