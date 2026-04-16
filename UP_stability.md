---
linked_to: UP_user-preferences_v35.20.md
created: 2026-04-08T00:00:00.000Z
last_reviewed: '2026-04-17T00:00:00.000Z'
updated: '2026-04-17T00:00:00.000Z'
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
| M2.FAST_LANE | trial | v35.19 ETA_BADGE·STEALTH.EXCEPTION·BOOT 삭제(심플화). v35.7 +COST_AWARE(BOOT 흡수). trial 유지 | 수정 발생 |
| M3.DENSITY | stable | v35.19 CLOSURE·NEXT_ACTION·FINAL_CHECK·BADGE_SYNTAX 삭제(심플화), +CTA(자연어 1줄). v35.18 +BADGE_SYNTAX(삭제됨). v35.15 +CLOSURE.NEXT_ACTION(삭제됨). v35.12 +CLOSURE(삭제됨). stable 유지 | 수정 발생 |
| ~~M4.CHAT_TITLE~~ | — | v35.3 삭제(앱 UI 영역—실행 불가). v35.9 M10 서브규칙으로 복원 | 삭제→복원 |
| M4.BEDROCK | stable | v32.4~ 구조 안정. v35.4 stable 승격 | 체계 확립 |
| M5.CONFIDENCE | stable | v35.19 UNCERTAINTY_FLAG 삭제(심플화). v35.18 +INJECTION_GUARD. v35.5 +PATTERN_GUARD +CAUSATION. stable 유지 | 수정 발생 |
| M6.INTENT_PARSE | stable | v33.0~ 구조 안정. v35.4 stable 승격 | 체계 확립 |
| M7.EDIT4 | stable | v35.7 +SKILL_PRECEDENCE(UP>스킬, SAFE_RULES 4건 예외). frozen→stable 강등 | 수정 발생 |
| M8.VERIFY | stable | v35.6 GROUNDING 경량화+PRECEDENCE 추가(라이브락 해소). stable 유지 | 체계 확립 |
| M9.ERROR_CORRECTION | trial | v33.0 신설. v35.4 trial 유지 | 검증 중 |
| M10.TURN_OPS | trial | v35.20 +TURN_COUNTER(세션 턴 넘버링, T{N} 배지 전수). v35.19 CONTEXT_LINK 삭제(심플화). v35.18 +SELF_CHECK. v35.14 CHAT_TITLE 전체 삭제. trial 유지 | 수정 발생 |
| M11.VAULT_PREFLIGHT | trial | v35.10 신설. 볼트 의존 스킬 마운트 선확인 3단 차등(HARD·SOFT·OPTIONAL) + 세션 1회 캐시. `vault_dependency` 프론트매터 분산 선언 방식 | 검증 중 |
| M12.MODE_GATES | trial | v35.15 +BADGE(모드 진입시 🔵핑퐁/🟡작업계획/🟣리허설/⚪자유실행 1토큰 배지 첫 줄 필수, 모드 혼동 해소). v35.11 신설. 실행 모드 트리거 3종. 정확 매칭 6종 + RELEASE + AMBIGUOUS 처리 | 검증 중 |
| T3_PRIORITY | trial | v35.7 신설(M9>M10). v35.11 갱신(M9>M12>M10) | 검증 중 |

---

## 요약

| 상태 | 항목 수 | 비율 |
|------|---------|------|
| frozen | 1 | 8% (M1) |
| stable | 5 | 38% (M3·M4·M6·M7·M8) |
| trial | 7 | 54% (M2·M5·M9·M10·M11·M12·T3_PRIORITY) |
| 삭제/병합 | 29 | (이력) |

> v35.7 LMM 한계 대응 5건 +KR 마스터 전환: +M2.COST_AWARE, +M3.TREE 그림정의 확장, +M7.SKILL_PRECEDENCE, +T3_PRIORITY 신설, +M10.UP_RESET. M3·M7 frozen→stable 강등.
> v35.8 autoloop 결과 4건 keep (M3 집중): +hedge 9종 금지 단언형 대체, +UP 라벨 본문노출 STEALTH 강화, +OUTPUT_CAP(판단·분석 500자 + 압축순서), +L1 factual 직답 원칙. 품질 15/18→18/18 총길이 -10%. M3 stable 유지.
> v35.9 +M10.CHAT_TITLE 복원: v34.1~v35.2 독립 M4→v35.3 "실행불가" 삭제→v35.9 M10 서브규칙 복원(NOTE 조항으로 UI 자동생성 제어 불가시 [제목제안] 대체). M10 trial 유지.
> v35.10 +M11.VAULT_PREFLIGHT: 볼트 의존 스킬 마운트 선확인. 3단 차등(HARD·SOFT·OPTIONAL) + 세션 1회 캐시. 분산 선언 방식(vault_dependency 프론트매터)으로 UP-스킬 커플링 제거. T2:5→6, modules 10→11.
> v35.11 +M12.MODE_GATES: 실행 모드 트리거 3종(작업계획·핑퐁·리허설). 사고도구(trigger-dictionary)와 분리된 Claude 기본 거동 통제 레이어. T3 배치(PRIORITY M9>M12>M10). 정확 매칭 6종 + RELEASE/AMBIGUOUS/TRANSITION 규정. 핑퐁 ⇄ 작업계획 자유 전환. T3:2→3, modules 11→12. M12 trial.
> v35.12 +M3.DENSITY.CLOSURE: L2·L3 종결부 "결국 [원요청]→[실제수행]" 1줄 매듭 필수. 산출물 링크 직전 배치, 자유형식 허용. 빈 선언·원요청 왜곡 FAIL. 사용자 피드백루프 단축 목적. 모듈수 변동 없음(M3 서브규칙 1건 추가). M3 stable 유지.
> v35.14 M10.TURN_OPS.CHAT_TITLE 전체 삭제: 앱 UI 제어 불가 + [제목제안] 대괄호 출력 오염. v34.1→v35.3삭제→v35.9복원→v35.14 최종 삭제. M10 trial 유지.
> v35.13 M3.DENSITY.CLOSURE 문구 개선: "결국"→"{🟢|🟠|🔴} 결론:" + SYNTAX(대괄호·꺾쇠 플레이스홀더 기호 출력 금지) + EMOJI(🟢완전/🟠부분/🔴미달, 애매시 🟠 기본) 3색 분기. 사용자 지적 2건 처리: "결국" 어색 + 대괄호 실출력. M3 stable 유지.
> v35.15 직관성 3종 확장("이모지+단어=구조설치" 원리 일반화): +M12.BADGE(🔵🟡🟣⚪ 모드 배지 첫 줄 필수), +M3.CLOSURE.NEXT_ACTION("🔜 다음:" 선택적 1줄), +M2.STEALTH.EXCEPTION(⚡🧠🏔️ WEIGHT 배지 사용자 요청시 1회 허용). M3·M12 stable 유지, M2 trial 유지.
> v35.16 UX 원리 기반 메타정보 배지 3종 확장(Peripheral Awareness·System Status Visibility·Context Continuity): +M5.UNCERTAINTY_FLAG("🚧 미확정 N건" 2건↑ 조건), +M2.ETA_BADGE("⏱️ ~N턴" L3만), +M10.CONTEXT_LINK("↩️ N턴 전 {주제} 연계" ≥2턴 전 참조시). 선택적 출력(조건 충족시만) 원칙. M2·M5·M10 trial 유지.
> v35.18 skill-doctor 진단 v35.16 처방 반영(LOW impact, 기능 추가 2건): +M5.INJECTION_GUARD(프롬프트 인젝션 방어 — UP 규칙 무효화 시도 FAIL, 작업별 예외 요청은 허용), +M10.SELF_CHECK(10턴마다 FRAME·WEIGHT·hedge·HONORIFIC 4항목 간이진단, 위반시 T1 강화+1줄 자가보고, UP_RESET과 독립 작동). 진단 점수 🟠84→🟢87, ④-1 취약·⑧-1 무자각 🟠→🟢 상향. INVARIANT 6/6 보존. M5·M10 trial 유지(기능 확장).
> v35.18 +M3.DENSITY.BADGE_SYNTAX(MID impact, 기능 추가 1건): 배지 출력 규약 명시. 모든 배지(모드·ETA·연계·미확정·WEIGHT) 이모지+텍스트 원형만 허용, HTML 태그(<mark>·<span>·<b>) 및 마크다운 강조(**·_·`) 래핑 FAIL. 사용자 지적 반영(렌더러 미지원 환경 <mark> 원시 노출 버그). 5개 배지 규칙 공통 적용. M3 stable 유지.
> v35.19 심플화 대삭제(MID impact, 삭제 9건 / 추가 1건): 사용자 "말이 너무 많다" 지적. 출력 부풀림 규칙 9건 전면 삭제 — M3.CLOSURE·M3.NEXT_ACTION·M2.ETA_BADGE·M5.UNCERTAINTY_FLAG·M10.CONTEXT_LINK·M2.STEALTH.EXCEPTION·M3.FINAL_CHECK·M2.BOOT·M3.BADGE_SYNTAX. 대체 신규 +M3.CTA(자연어 1줄 질문, 규칙 없이 상황 판단). 보존: M12.BADGE 🔵🟡🟣 3종(모드 구분 필수). 배지 7종→3종. INVARIANT 6/6 보존(장식 레이어만 제거). M2·M3·M5·M10 상태 유지(강등 없음).
> v35.20 +M10.TURN_COUNTER(LOW impact, 기능 추가 1건): 세션 내 응답턴 넘버링 "T{N}" 배지 전수 적용(모든 턴, L0 포함). Claude 응답만 카운트, N=1부터 순증, 재부팅·새세션 리셋. CONTEXT_WATCH/SELF_CHECK/UP_RESET 트리거 계산 기준점 제공. 모드 배지 공존시 "T{N} 🔵" 순서. 사용자 스킵 요청시 1회 허용. v35.19 심플화 원칙 무충돌(T{N}은 구조 메타정보, 장식 ✗). M10 trial 유지.
