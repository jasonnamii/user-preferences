---
linked_to: UP_user-preferences_v29.5.md
created: 2026-04-08
last_reviewed: 2026-04-09
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
| BLIND_SPOT | frozen | 근본 원칙. 변경 이력 없음 | |
| EXECUTOR | stable | v29.2 압축: insufficient info 문구 삭제(EXECUTOR 역할 자명) | frozen→stable 강등 |
| CONFIDENCE | frozen | 4단계 체계 확립 | |
| INFO_BRANCH | stable | 실용 규칙 | |
| ~~REPORT_FORMAT~~ | — | v29.3 삭제 (EDIT4에 Before/After 포함, 중복) | |
| ~~PLAN_GATE~~ | — | v29.1 삭제 (Cowork default) | |
| TONE | stable | v29.2 압축: ceremonial intro/outro 삭제(TONE 맥락상 자명) → 1줄화 | frozen→stable 강등 |
| ~~INTERNAL_VOCAB~~ | — | v29.3 삭제 (deliverable-engine에 병합 가능) | |
| NUM_VERIFY | stable | v29.2 압축: SIMPLE_CITE_EXEMPT 삭제(NUM_VERIFY 본문에서 추론 가능) | frozen→stable 강등 |
| SOURCE | stable | 소스 우선순위 확립 | |
| ~~SAVE~~ | — | v29.3 삭제 (_archive 규칙→EDIT4 POST ⑤ 흡수) | |
| ~~TOOL_PRIORITY~~ | — | v29.3 삭제 (환경 의존적, 발동 빈도 낮음) | |
| ~~SKILL_ROUTER~~ | — | v29.3 삭제 (스킬 시스템 자체 트리거 매칭으로 충분) | |
| EDIT4 | stable | v29.3: POST ⑤ _archive/ move 흡수 | |
| OBSIDIAN | stable | v29.4: MCP write/edit/create/delete BLOCK 강화. read-only HARD | |
| MCP_SPEED | trial | v29.5: 신규 추가. DC·Obs·Cowork 병목 파라미터 기본값+병렬/직렬 규칙 | 5세션 무수정 시 stable 승격 |

---

## 요약

| 상태 | 항목 수 | 비율 |
|------|---------|------|
| frozen | 2 | 12% |
| stable | 6 | 35% |
| trial | 1 | 6% |
| 삭제 | 8 | 47% (PRIORITY, PLAN_GATE, REPORT_FORMAT, INTERNAL_VOCAB, SAVE, TOOL_PRIORITY, SKILL_ROUTER, OBSIDIAN_LAYER→OBSIDIAN 축소잔류) |

> 잔류 10개 규칙 중 MCP_SPEED(trial) 1건 신규. 나머지 frozen 또는 stable.
