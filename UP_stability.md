---
linked_to: UP_user-preferences_v35.8.md
created: 2026-04-08T00:00:00.000Z
last_reviewed: '2026-04-16T00:00:00.000Z'
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
| M1.FRAME | frozen | v31.0~ 본질 불변. v35.4 frozen 승격 | 핵심 원칙 |
| M2.FAST_LANE | trial | v35.7 +COST_AWARE(L0 BOOT 완전스킵). trial 유지 | 수정 발생 |
| M3.DENSITY | stable | v35.8 hedge 9종 금지+STEALTH 강화+OUTPUT_CAP 압축순서(부연→층→어미보존)+L1 factual 직답 원칙 추가 (autoloop 4건 keep). stable 유지 | 수정 발생 |
| ~~M4.CHAT_TITLE~~ | — | v35.3 삭제(앱 UI 영역—실행 불가) | 삭제 |
| M4.BEDROCK | stable | v32.4~ 구조 안정. v35.4 stable 승격 | 체계 확립 |
| M5.CONFIDENCE | stable | v35.5 +PATTERN_GUARD +CAUSATION 추가로 frozen→stable 강등 | 수정 발생 |
| M6.INTENT_PARSE | stable | v33.0~ 구조 안정. v35.4 stable 승격 | 체계 확립 |
| M7.EDIT4 | stable | v35.7 +SKILL_PRECEDENCE(UP>스킬, SAFE_RULES 4건 예외). frozen→stable 강등 | 수정 발생 |
| M8.VERIFY | stable | v35.6 GROUNDING 경량화+PRECEDENCE 추가(라이브락 해소). stable 유지 | 체계 확립 |
| M9.ERROR_CORRECTION | trial | v33.0 신설. v35.4 trial 유지 | 검증 중 |
| M10.TURN_OPS | trial | v35.7 +UP_RESET(자기실패 재부팅, 세션1회+2회째 개입). trial 유지 | 검증 중 |
| T3_PRIORITY | trial | v35.7 신설(M9>M10 동시 발동시 오류 정정 선행) | 검증 중 |

---

## 요약

| 상태 | 항목 수 | 비율 |
|------|---------|------|
| frozen | 1 | 9% (M1) |
| stable | 5 | 45% (M3·M4·M6·M7·M8) |
| trial | 5 | 45% (M2·M5·M9·M10·T3_PRIORITY) |
| 삭제/병합 | 29 | (이력) |

> v35.7 LMM 한계 대응 5건 +KR 마스터 전환: +M2.COST_AWARE, +M3.TREE 그림정의 확장, +M7.SKILL_PRECEDENCE, +T3_PRIORITY 신설, +M10.UP_RESET. M3·M7 frozen→stable 강등.
> v35.8 autoloop 결과 4건 keep (M3 집중): +hedge 9종 금지 단언형 대체, +UP 라벨 본문노출 STEALTH 강화, +OUTPUT_CAP(판단·분석 500자 + 압축순서), +L1 factual 직답 원칙. 품질 15/18→18/18 총길이 -10%. M3 stable 유지.
