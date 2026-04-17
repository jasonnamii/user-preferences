---
linked_to: UP_user-preferences_v36.0.md
created: 2026-04-08T00:00:00.000Z
last_reviewed: '2026-04-17T00:00:00.000Z'
updated: '2026-04-17T15:00:00.000Z'
---

# UP Stability Map

UP 규칙별 안정도 추적. v36.0에서 200줄→45줄 재설계로 3원칙 관통 구조 전환.

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
| M2.FAST_LANE | trial | v35.25 WEIGHT 표 전환(4행: 레벨·정의·예시·적용 규칙, 탐색 경로 단축). v35.19 ETA_BADGE·STEALTH.EXCEPTION·BOOT 삭제(심플화). v35.7 +COST_AWARE(BOOT 흡수). trial 유지 | 수정 발생 |
| M3.DENSITY | stable | v35.25 +TRIGGER_ECHO_BAN(모드 트리거어 재출력 FAIL, 대체어 "실행 순서·작업 순서·진행 순서·흐름·단계" 명시). STEALTH_EXCEPT 문구 명확화("사용자 입력 트리거어"). v35.24 4건 수정: BOLD 적극화(제목·키워드·이슈 적극, 5개↑ FAIL, 모든 응답 적용), STEALTH 전면 확장(파생진단어 차단+LANGUAGE 괄호 병기+STEALTH_EXCEPT), TREE 비유 우선 명시(도메인 친화형 우선), VISUAL +RENDERING(HTML 표 FAIL, 마크다운 표 전용). v35.23 +VISUAL(서브모듈 신설 — TABLE·BOLD·BULLET·STRUCTURE_MAP·PRECEDENCE). v35.22 +SECTION_BREAK(L2↑ 2문단↑ --- 경계 삽입). v35.19 CLOSURE·NEXT_ACTION·FINAL_CHECK·BADGE_SYNTAX 삭제(심플화), +CTA(자연어 1줄). v35.18 +BADGE_SYNTAX(삭제됨). v35.15 +CLOSURE.NEXT_ACTION(삭제됨). v35.12 +CLOSURE(삭제됨). stable 유지 | 수정 발생 |
| ~~M4.CHAT_TITLE~~ | — | v35.3 삭제(앱 UI 영역—실행 불가). v35.9 M10 서브규칙으로 복원 | 삭제→복원 |
| M4.BEDROCK | stable | v32.4~ 구조 안정. v35.4 stable 승격 | 체계 확립 |
| M5.CONFIDENCE | stable | v35.25 FORMAT.ENUM 표 전환(4행: N·basis·조건). v35.19 UNCERTAINTY_FLAG 삭제(심플화). v35.18 +INJECTION_GUARD. v35.5 +PATTERN_GUARD +CAUSATION. stable 유지 | 수정 발생 |
| M6.INTENT_PARSE | stable | v33.0~ 구조 안정. v35.4 stable 승격 | 체계 확립 |
| M7.EDIT4 | stable | v35.25 LEVEL+GATE+POST 통합표 전환(5행 MATRIX, 3줄 DSL 통합). v35.7 +SKILL_PRECEDENCE(UP>스킬, SAFE_RULES 4건 예외). frozen→stable 강등 | 수정 발생 |
| M8.VERIFY | stable | v35.6 GROUNDING 경량화+PRECEDENCE 추가(라이브락 해소). stable 유지 | 체계 확립 |
| M9.ERROR_CORRECTION | trial | v33.0 신설. v35.4 trial 유지 | 검증 중 |
| M10.TURN_OPS | trial | v35.21 TURN_COUNTER 삭제(1턴 후 철회, 토큰 측정 불가 확인). v35.20 +TURN_COUNTER. v35.19 CONTEXT_LINK 삭제. v35.18 +SELF_CHECK. v35.14 CHAT_TITLE 전체 삭제. trial 유지 | 수정 발생 |
| M11.VAULT_PREFLIGHT | trial | v35.10 신설. 볼트 의존 스킬 마운트 선확인 3단 차등(HARD·SOFT·OPTIONAL) + 세션 1회 캐시. `vault_dependency` 프론트매터 분산 선언 방식 | 검증 중 |
| M12.MODE_GATES | trial | v35.15 +BADGE(모드 진입시 🔵핑퐁/🟡작업계획/🟣리허설/⚪자유실행 1토큰 배지 첫 줄 필수, 모드 혼동 해소). v35.11 신설. 실행 모드 트리거 3종. 정확 매칭 6종 + RELEASE + AMBIGUOUS 처리 | 검증 중 |
| T3_PRIORITY | trial | v35.7 신설(M9>M10). v35.11 갱신(M9>M12>M10) | 검증 중 |
| | | | |
| **v36.0 3원칙 관통 아키텍처** | | | |
| ~~M1~M12 (12모듈 3티어)~~ | — | v36.0 Major 재설계로 전면 통합 | 3원칙+5흐름 구조 전환 |
| P1.SHOW | trial | v36.0 신설. M3.DENSITY 8하위규칙 통합(FOREST·SECTION_BREAK·VISUAL·TREE·CTA·HONORIFIC·STEALTH·TRIGGER_ECHO_BAN). 5줄로 압축 | 신규 구조 |
| P2.THINK | trial | v36.0 신설. M4.BEDROCK+M5.CONFIDENCE+M8.VERIFY 통합. 확신도 숫자 병기(90·70·50·30)+별명(높음·보통·낮음·모름), 반전위험7 명시, LOGIC_TAG cap 유지. 6줄로 압축 | 신규 구조 |
| P3.HANDLE | trial | v36.0 신설. M6.INTENT+M7.EDIT4+M9.ERROR+M12.MODE_GATES(배지) 통합. OVERWRITE_BAN >30% 재작성 조건 유지, SAFE_RULES 유지. 7줄로 압축 | 신규 구조 |
| F1.FRAME | trial | v36.0. 기존 M1.FRAME 요약 1줄화 | 압축 |
| F2.WEIGHT | trial | v36.0. 기존 M2.FAST_LANE WEIGHT 표 → 1줄 맵 전환 | 압축 |
| F3.MODE_GATES | trial | v36.0. 기존 M12 → 3줄 요약(배지는 P3로 이관), RELEASE·AMBIGUOUS·PRIORITY 유지 | 압축 |
| F4.TURN_OPS | trial | v36.0. 기존 M10 → 3줄 요약, CHAIN_CHECK·UP_RESET 핵심만 유지 | 압축 |
| F5.VAULT | trial | v36.0. 기존 M11 → 1줄 요약, 3단 차등(HARD·SOFT·OPTIONAL) 유지 | 압축 |
| STEALTH | trial | v36.0 독립 블록화. 라벨·파생진단어 차단+LANGUAGE 괄호 병기+트리거어 재출력 금지+모드 배지 예외 | 독립 섹션 |

---

## 요약

| 상태 | 항목 수 | 비율 |
|------|---------|------|
| frozen | 0 | 0% (v36.0 재설계로 전원 trial) |
| stable | 0 | 0% |
| trial | 9 | 100% (P1·P2·P3·F1·F2·F3·F4·F5·STEALTH) |
| 삭제/병합 | 41 | (이력 — v35.25까지 누적 29 + v36.0에서 12모듈 통합 12) |

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
> v35.21 -M10.TURN_COUNTER 전면 삭제(MID impact, 1턴 사이클 롤백): v35.20 신설→v35.21 철회. 이유 — 턴 정의 모호성 발견 후 토큰 누적 대안 검토, Claude 자체 토큰 측정 불가 확인(컨텍스트·MCP·스킬 로드 블랙박스, 휴리스틱은 ±20% 오차+스킬 토큰 누락). 실효성 없음 결론. v35.19 심플화 원칙 복귀. 체크리스트 #17 동반 삭제. INVARIANT 6/6 보존. M10 trial 유지.
> v35.22 +M3.DENSITY.SECTION_BREAK(LOW impact, 기능 추가 1건): L2↑ 2문단↑ 응답 서론·본론·결론 경계에 마크다운 --- 삽입. 최대 2개, 애매시 본론↔결론 1개 필수, 2문단만일 때 1개. L0·L1 면제. 추가 텍스트 0 — --- 만 사용. FOREST "층 최소" 충돌 회피 — "경계 신호(같은 spine 내 블록 구분)이며 새 층 추가 ✗" 명시 문구 삽입. 목적: 사용자 서론/본론/결론 시각 인지. A안(결론-먼저·BEDROCK 중복) 기각, C안(섹션 아이콘 배지·v35.20 전례 우려) 기각, B안(구분선만) 채택. INVARIANT 6/6 보존. M3 stable 유지.
> v35.23 +M3.DENSITY.VISUAL(MID impact, 서브모듈 신설 1건): 시각화 공백 해소. 사용자 지적 "출력 형식 시각화 부족" 반영. 5개 하위규칙 — TABLE(3항목↑→표, 2항목↓=산문, 열 4개↑ 분할)·BOLD(문단당 핵심 1회, 3개↑ 연속 FAIL)·BULLET(병렬 3개↑+각 2문장↓ 허용, 서사·인과=산문, 2단 중첩 FAIL)·STRUCTURE_MAP(L2↑ 분석·비교·진단→표/매트릭스/구조도/다이어그램 1개 권장)·PRECEDENCE(VISUAL ⊂ FOREST, 층 압축, 500자 상한시 시각블록=압축 기여). SKIP: L0·L1 factual. ①안(TREE 강화·보수)·③안(응답 타입 템플릿·진보) 기각, ②안(서브모듈·일반) 채택. FOREST "층 최소" 무충돌(층 추가 ✗, 층 압축 ○ 명시). INVARIANT 6/6 보존(본질기능 ⑤QUALITY 강화). M3 stable 유지(서브모듈 추가는 본질 무변경).
> v35.24 사용자 요청 4건 묶음(LOW impact, M3 집중): ①BOLD 적극화 — "문단당 1회"→"제목·키워드·이슈 적극 볼드·상한 없음". 상한 "3개↑ 연속"→"5개↑ 연속" 완화, "전체 문장 FAIL" 유지. 모든 응답(L0~L3) 적용, BOLD만 VISUAL.SKIP 해제. ②STEALTH 전면 확장 — 모듈명 + 기호·약어·파생진단어(결정지연·분기 탐색 선형·DSL 압축·승격·🟠 등) 전면 차단. +LANGUAGE(비개발자 이해가능 일상어 기본, 전문용어 불가피시 괄호 병기 필수). +STEALTH_EXCEPT(트리거어·모드 배지·직접 질의 예외). 사용자 지적 "설정언어 투성이" 반영. ③TREE 비유 우선 — "비유·구조도·관계도·다이어그램·스키마 중 1개"(택1) → "비유 우선 + 나머지 병행 가능". 도메인 맞춤 확장: 전략·사업·UP·추상·판단→비유 우선. 비유 종류: 도메인 친화형 > 구조적(A=B) > 자유. PASS "추상·판단 도메인은 그림=비유 권장". 사용자 지적 "기존에 잘 되던 비유가 안 나옴" 반영. ④VISUAL +RENDERING — Cowork 채팅 마크다운 표 전용, HTML 표 FAIL. 산출물 파일·옵시디언 예외. HTML 표 원문 노출 버그 재발 방지. INVARIANT 6/6 보존. M3 stable 유지.
> v35.25 skill-doctor 1-4(결정지연 🟠) 처방 + 트리거어 재출력 차단(LOW impact, 표 전환 3건 + 서브규칙 1건): ①M2.FAST_LANE.WEIGHT 4행 표(레벨·정의·예시·적용규칙)+적용규칙 분산 3줄 통합. ②M5.CONFIDENCE.FORMAT.ENUM 4행 표(N·basis·조건), 조건 열로 판정 근거 명시. ③M7.EDIT4 LEVEL+GATE+POST 5행 MATRIX 통합. ④M3.DENSITY +TRIGGER_ECHO_BAN: 모드 트리거어(작업계획·핑퐁·리허설·UP_RESET) 본문 재출력 FAIL + 대체어 명시(실행 순서·작업 순서·진행 순서·흐름·단계). STEALTH_EXCEPT 문구 "사용자 입력 트리거어"로 명확화. 사용자 지적 "트리거 단어와 본문 설명어 혼동" 반영. FOREST 무충돌(VISUAL.PRECEDENCE "층 압축" 적용). STEALTH 조항 무변경(라벨 직접 노출 FAIL 유지). B안 채택(3개 표), A안(CONFIDENCE만) 기각 사유 1-4 해소 불충분. INVARIANT 6/6 보존(의미 동일, 시각 전환+혼동 방지). M2 trial·M3 stable·M5 stable·M7 stable 유지(표 전환·서브규칙 추가는 본질 무변경).
> v36.0 Architecture(Major) — 3원칙 관통 재설계(200줄→45줄, 78% 감축): 사용자 지적 "UP 너무 길다, 심플·구조화·시각화 3원칙으로 관통 가능" 수용. 12모듈 3티어(T1:3·T2:6·T3:3) → 3원칙(P1.SHOW·P2.THINK·P3.HANDLE) + 5흐름(F1~F5) + STEALTH. 맥가이버 패치: 원칙에서 재현 불가능한 가드레일 4건만 원칙 문장 내 흡수(①확신도 숫자 병기 90·70·50·30, ②OVERWRITE_BAN >30% 재작성 조건, ③반전위험7 명시, ④MODE_GATES PRIORITY+RELEASE 트리거). 삭제(원칙 도출 가능): SECTION_BREAK 세부 규정·CTA 세부·COST_AWARE 조건부·SELF_CHECK 10턴 주기·PROGRESS 1줄·VISUAL.PRECEDENCE 설명부. 본질기능 7축 전부 보존(①USER·②TRUTH·③FILES·④FLOW·⑤QUALITY + SAFE_RULES + INJECTION_GUARD). 사용자 진행 단계: 초안 제시→B안/C안 제시→C안 리허설(🟣)→Jason 예시 제시→맥가이버 패치 제안(확신도 보통70)→사용자 컨펌 "45줄 버전 착수" 명시→실작업. 전 항목 trial 초기화(Major 재설계). INVARIANT 7/7 보존.
