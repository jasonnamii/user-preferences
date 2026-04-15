---
linked_to: UP_user-preferences_v35.3.md
created: 2026-04-08T00:00:00.000Z
last_reviewed: '2026-04-14T00:00:00.000Z'
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
| ~~FRAME~~ | — | v31.0~v34.1. v35.0에서 M1.FRAME으로 재지정 | Tier 재편 |
| ~~CONFIDENCE~~ | — | v35.0에서 M6.CONFIDENCE로 재지정(+CAREFUL_READ 흡수) | Tier 재편 |
| ~~INFO_BRANCH~~ | — | v35.0 M7.INTENT_PARSE에 흡수 | 병합 |
| ~~REPORT_FORMAT~~ | — | v29.3 삭제 (EDIT4에 Before/After 포함, 중복) | |
| ~~PLAN_GATE~~ | — | v29.1 삭제 (Cowork default) | |
| ~~TONE~~ | — | v30.9 삭제 → DENSITY에 흡수 | |
| ~~DENSITY~~ | — | v35.0에서 M3.DENSITY로 재지정 | Tier 재편 |
| ~~INTERNAL_VOCAB~~ | — | v29.3 삭제 (deliverable-engine에 병합 가능) | |
| ~~NUM_VERIFY~~ | — | v31.0 VERIFY에 흡수 | |
| ~~SOURCE~~ | — | v31.0 VERIFY에 흡수 | |
| ~~VERIFY~~ | — | v35.0에서 M9.VERIFY로 재지정 | Tier 재편 |
| ~~SAVE~~ | — | v29.3 삭제 (_archive 규칙→EDIT4 POST ⑤ 흡수) | |
| ~~TOOL_PRIORITY~~ | — | v29.3 삭제 (환경 의존적, 발동 빈도 낮음) | |
| ~~SKILL_ROUTER~~ | — | v29.3 삭제 (스킬 시스템 자체 트리거 매칭으로 충분) | |
| ~~EDIT4~~ | — | v35.0에서 M8.EDIT4로 재지정(+OVERWRITE_BAN 흡수) | Tier 재편 |
| ~~OBSIDIAN~~ | — | v30.6 삭제 (redundant: 시스템 기본동작으로 충분) | |
| ~~MCP_SPEED~~ | — | v30.6 삭제 (redundant: 프로토콜 기본원칙으로 충분) | |
| ~~CAREFUL_READ~~ | — | v35.0 M6.CONFIDENCE에 흡수 | 병합 |
| ~~LOGIC_TAG~~ | — | M6.CONFIDENCE 서브규칙으로 유지(독립행 불필요) | 병합 |
| ~~OVERWRITE_BAN~~ | — | v35.0 M8.EDIT4에 흡수 | 병합 |
| ~~BEDROCK~~ | — | v35.0에서 M5.BEDROCK으로 재지정 | Tier 재편 |
| ~~ERROR_CORRECTION~~ | — | v35.0에서 M10.ERROR_CORRECTION으로 재지정 | Tier 재편 |
| ~~INTENT_PARSE~~ | — | v35.0에서 M7.INTENT_PARSE로 재지정(+INFO_BRANCH 흡수) | Tier 재편 |
| ~~TURN_OPS~~ | — | v35.0에서 M11.TURN_OPS로 재지정 | Tier 재편 |
| ~~SKILL_DISPATCH~~ | — | v35.0 삭제(시스템 자동매칭 위임) | 삭제 |
| ~~FAST_LANE~~ | — | v35.0에서 M2.FAST_LANE으로 재지정 | Tier 재편 |
| ~~MODULE_PRECEDENCE~~ | — | v35.0 삭제(M8.EDIT4에 인라인) | 병합 |
| ~~CHAT_TITLE~~ | — | v35.0에서 M4.CHAT_TITLE로 재지정 | Tier 재편 |
| | | | |
| **v35.0 Tier Architecture** | | | |
| M1.FRAME | trial | v35.0 T1. 기존 FRAME 계승 | Tier 재편 trial 유지 |
| M2.FAST_LANE | trial | v35.3 +STEALTH 서브규칙(WEIGHT 노출 차단 강화) | 수정 trial 유지 |
| M3.DENSITY | trial | v35.1 TREE 체크리스트→생성원리 전환, FINAL_CHECK·DIAGNOSIS_RULE 정합 | 수정 trial 유지 |
| ~~M4.CHAT_TITLE~~ | — | v35.3 삭제(앱 UI 영역—실행 불가) | 삭제 |
| M4.BEDROCK | trial | v35.1 RULE에 CONFIDENCE 인라인 병기 추가. v35.3 M5→M4 재넘버링 | 수정 trial 유지 |
| M5.CONFIDENCE | trial | v35.0 T2. +CAREFUL_READ 흡수. v35.3 M6→M5 재넘버링 | 병합 trial 리셋 |
| M6.INTENT_PARSE | trial | v35.0 T2. +INFO_BRANCH 흡수. v35.3 M7→M6 재넘버링 | 병합 trial 리셋 |
| M7.EDIT4 | trial | v35.1 GATE L1 auto 승격+CONFIDENCE선행 삭제. v35.3 M8→M7 재넘버링 | 수정 trial 유지 |
| M8.VERIFY | trial | v35.0 T2. 기존 VERIFY 계승. v35.3 M9→M8 재넘버링 | Tier 재편 trial 유지 |
| M9.ERROR_CORRECTION | trial | v35.0 T3. 기존 ERROR_CORRECTION 계승. v35.3 M10→M9 재넘버링 | Tier 재편 trial 유지 |
| M10.TURN_OPS | trial | v35.0 T3. 기존 TURN_OPS 계승. v35.3 M11→M10 재넘버링 | Tier 재편 trial 유지 |

---

## 요약

| 상태 | 항목 수 | 비율 |
|------|---------|------|
| frozen | 0 | 0% |
| stable | 0 | 0% |
| trial | 10 | 100% |
| 삭제/병합 | 29 | (이력) |

> v35.3 M4.CHAT_TITLE 삭제. M2.FAST_LANE STEALTH 신설. 전모듈 재넘버링(M4~M10). 10모듈 trial 유지.
