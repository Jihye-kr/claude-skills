# gitflow

이슈 → 브랜치 → 커밋 → PR → 릴리스를 Git Flow 규칙(타입 + 이슈 번호)대로 처리하는 [Claude Code](https://docs.claude.com/en/docs/claude-code) 스킬입니다. 커밋·PR 전에 lint / typecheck / build를 직접 실행합니다.

## 설치

```bash
git clone https://github.com/Jihye-kr/gitflow.git ~/gitflow-skill

# 개인 설치 — 내 모든 프로젝트에 적용
cp -r ~/gitflow-skill/gitflow ~/.claude/skills/

# 프로젝트 설치 — 이 저장소를 쓰는 팀원 모두에게 적용 (설치 후 커밋)
cp -r ~/gitflow-skill/gitflow <내 프로젝트>/.claude/skills/
```

설치 후 Claude Code를 새로 열면 적용됩니다.

### 업데이트

```bash
cd ~/gitflow-skill && git pull && cp -r gitflow ~/.claude/skills/
```

## 사용법

이렇게 말하거나, `/gitflow`로 직접 실행하세요.

- "브랜치 만들어줘"
- "커밋해줘"
- "PR 올려줘"
- "릴리스 PR 만들어줘"
- "핫픽스 해줘"

## 브랜치 구조

```
main      ← 배포
develop   ← 통합
{타입}/#{이슈}  ← 작업 (develop에서 시작, hotfix만 main에서 시작)
```

## 타입

`feat` `fix` `hotfix` `refactor` `design` `docs` `test` `chore` `revert` `release`

## 이름 규칙

| 대상 | 형식 | 예 |
|---|---|---|
| 브랜치 | `{타입}/#{이슈}` | `feat/#12` |
| 커밋 | `{타입}: {설명} (#{이슈})` | `feat: 로그인 폼 유효성 검사 추가 (#12)` |
| PR 제목 | 커밋과 같은 형식 | `feat: 이메일 로그인 기능 추가 (#12)` |
| PR 본문 | `Closes #{이슈}` | |
| 릴리스 PR | `release: YYYY-MM-DD` | `release: 2026-10-19` |

## 자동으로 해 주는 것

- 이슈가 없으면 이슈부터 만들자고 제안
- PR base가 규칙에 맞는지 확인 (`hotfix/*` 외에는 main 직행 금지)
- 커밋·PR 전에 `lint` → `typecheck` → `build` 실행. 실패하면 커밋·PR을 하지 않음
- 저장소의 PR 템플릿을 채우고, 확인 안 된 항목이 있으면 Draft로 생성
- 커밋·PR에 Claude가 작업했다는 표시를 넣지 않음

이슈 생성, push, PR 생성, 머지, 태그는 항상 실행 전에 확인을 받습니다.

## 내 팀에 맞게 바꾸기

[gitflow/SKILL.md](gitflow/SKILL.md)를 직접 수정하세요. 바꾸기 쉬운 부분은 이렇습니다.

- **타입 목록**: `## 타입` 표
- **릴리스 주기 / 태그 형식**: `## 4. 릴리스 PR`
- **PR 본문 구조**: `## 3. PR 열기`의 템플릿 (저장소에 `.github/PULL_REQUEST_TEMPLATE.md`가 있으면 그걸 우선 사용)
- **작성 언어**: `## 규칙`의 마지막 줄
