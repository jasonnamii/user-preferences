# 사용자 설정

**Claude Cowork와 Claude Code용 DSL 기반 행동 명령어 집합.**

> 🇺🇸 [English README](./README.md)

## 사전 요구사항

- **Claude Cowork 또는 Claude Code** 환경
- **[up-manager](https://github.com/jasonnamii/up-manager)** 스킬 — 편집 및 버전 관리용

## 목적

대규모 언어 모델은 기본적으로 일반적으로 동작합니다. 사용자 설정(UP)은 세션 수준에서 Claude의 행동을 형성하는 간결한 15줄 DSL 설정입니다 — 사각지대 탐지, 신뢰도 채점, 편집 프로토콜, 톤 규칙, 정보원 계층을 강제합니다. 범용 어시스턴트를 캘리브레이션된 운영 파트너로 변환합니다.

UP는 프롬프트 템플릿이 아닙니다. 사용을 통해 진화하는 살아있는 사양이며, 전용 스킬 ([up-manager](https://github.com/jasonnamii/up-manager))로 관리되고 버전 제어, 안정도 추적, 전파를 거칩니다.

## 사용 시점 및 방법

UP는 모든 Claude Cowork와 Claude Code 세션 시작 시 자동으로 로드됩니다. 호출할 필요 없음 — 지속적 행동 계약으로 배경에서 실행됩니다. 규칙을 수정하려면 `up-manager` 스킬을 사용하세요. GitHub로 변경 사항을 동기화하려면 `git-sync`를 사용하세요.

## 내부 구성

`UP_user-preferences_v29.5.md` — 활성 설정 (15개 규칙, 순수 DSL 문법):

| 규칙 | 기능 |
|---|---|
| **BLIND_SPOT** | 사용자가 고려하지 않은 격차, 위험, 가정을 사전에 표시 |
| **EXECUTOR** | 사용자 프레임 내에서 자율적 수정 권한 (추가, 재정렬, 문구 변경)으로 작업 |
| **CONFIDENCE** | 신뢰도 점수 (90/70/50/30) 첨부; ≥70에서 필수 검증 |
| **INFO_BRANCH** | 의사결정 중요 정보 부족 시 중단하고 질문 |
| **TONE** | 기본적으로 단호함; 신뢰도 ≥70일 때 회피 없음 |
| **NUM_VERIFY** | 화폐 금액에 대한 Python 검증 필수 |
| **SOURCE** | 정보원 계층: 공식 > 산업 > 일반; 3순위 단독 불허 |
| **EDIT4** | 5단계 편집 영향 평가 (L0–L4) 및 상향 게이트 |
| **OBSIDIAN** | Obsidian Vault에 대한 MCP 쓰기 금지 — 어떤 상황에서도 MCP를 통한 쓰기 불가 |
| **MCP_SPEED** | Desktop Commander, Obsidian, Cowork 도구 호출 기본값 |

## 설계 철학

Claude가 이미 기본적으로 따르는 규칙은 안전을 위해 유지하지 않고 삭제됩니다. v29.0은 345줄을 37줄로 축소했고, 상세 절차를 전용 스킬 (deliverable-engine, session-briefing, trigger-dictionary 등)로 추출하여 15줄로 더 압축했습니다. 남은 모든 규칙은 명시적 명령 없이는 Claude가 하지 않을 것이기 때문에 존재합니다.

## 핵심 기능

- **순수 DSL 문법** — 자연어 산문 없음; 명확 파싱을 위해 `::=`, `→`, `|`, `ENUM`, `GATE:`, `IF` 연산자 사용
- **버전 제어** — 변경 추적이 있는 시맨틱 버전 관리; up-manager 스킬로 관리
- **안정도 추적** — 각 규칙에 성숙 상태 (고정 → 안정 → 시험) 및 승격 기준
- **스킬 위임** — 복잡한 절차는 전용 스킬에 있으며 UP 파일 자체가 아님

## 연관 스킬

- **[up-manager](https://github.com/jasonnamii/up-manager)** — UP 변경을 편집, 버전 범프, 전파
- **[git-sync](https://github.com/jasonnamii/git-sync)** — 수정 후 이 GitHub 레포로 UP 동기화
- **[session-briefing](https://github.com/jasonnamii/session-briefing)** — 세션 맥락 생성 시 UP 규칙 참조

## 사용법

이 설정은 Claude Cowork 세션에서 자동으로 로드됩니다. Claude Code에서 사용하려면, 프로젝트의 `CLAUDE.md`에서 UP 파일을 참조하세요:

```markdown
# 사용자 설정
참조: [UP_user-preferences_v29.5.md](path/to/UP_user-preferences_v29.5.md)
```

## Cowork 스킬 생태계

25개 이상의 커스텀 스킬의 일부입니다. 전체 카탈로그: [github.com/jasonnamii/cowork-skills](https://github.com/jasonnamii/cowork-skills)

## 라이선스

MIT 라이선스 — 자유롭게 사용, 수정, 공유하세요.
