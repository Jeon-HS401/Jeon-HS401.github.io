# CLAUDE.md — Jeon-HS401.github.io

이 저장소는 **공개**다. 여기 적는 것은 전부 공개된다고 보고 쓴다.
운영 맥락(파이프라인 구조·다른 프로젝트·로컬 경로)은 여기 두지 않는다 — 비공개 저장소 쪽에 있다.

## 이 레포는
개인 블로그의 **발행 표면**. 승인된 글을 받아 게시한다. 글을 만드는 로직은 여기 없다.

## 스택
- Jekyll + minima, GitHub Pages 네이티브 빌드(CI 없음).
- 로컬 프리뷰는 선택 — `bundle exec jekyll serve` (ruby 필요).
- 글 = `_posts/YYYY-MM-DD-제목.md`. 파일명 날짜 필수.

## 게시 규약
초안 = PR · 승인 = 머지 · 게시 = 자동 배포. **사람 승인 없이 머지하지 않는다.**

## 발행 파일 front matter 규약

레이아웃이 아래 필드를 읽는다. **필수는 반드시 채운다** — 비면 목록과 글 화면의 밀도가 글마다 달라진다.

| 필드 | 필수 | 값 | 어디에 쓰이나 |
|---|:-:|---|---|
| `layout` | ● | `post` (`_config.yml` defaults가 자동) | — |
| `title` | ● | 한 문장 제목 | 목록 · 글 머리 · `<title>` |
| `date` | ● | `YYYY-MM-DD HH:MM:SS +0900` | 목록 정렬·표시 |
| `description` | ● | **한 문장.** 첫 문단을 줄이되 본문에 없는 사실을 넣지 않는다 | 목록 설명 · 글 머리 deck · `<meta name="description">` |
| `kind` | ● | `worklog` / `technical` | 글 머리 좌측 라벨. 없으면 `NOTE` |
| `project` | ● | `_data/projects.yml`의 `key`와 **정확히 일치** | 목록 meta · 글 좌측 레일 · 프로젝트 페이지 집계 |
| `summary_label` + `summary_body` | ○ | 예: `STATUS` + 한두 줄 | 글 머리 **오른쪽 열**. 있을 때만 3열이 된다 |
| `evidence[]` (`ref`, `note`) | ○ | 독자가 확인할 수 있는 근거 | 본문 하단 "근거와 재현" |

**규칙**
- `project` 값이 `_data/projects.yml`에 없으면 프로젝트 페이지에서 그 글은 **어디에도 안 잡힌다.** 새 프로젝트면 데이터 파일에 먼저 추가한다.
- `evidence`는 **본문 하단에만** 나온다.
- `path_label`은 폐기했다. 실재하지 않는 라우트를 가리켰고, 그 자리는 `project`가 대신한다.
- 발행 단계에서 새로 쓰는 텍스트(`description`·`summary_body`·`evidence[].note`·이미지 alt)는 **초안 검토를 거치지 않은 신규 산출**이다. PR 전에 한 번 더 검토한다.

## 안 하는 것
- 승인 없는 자동 게시
- 운영 맥락·다른 프로젝트 정보를 이 저장소에 적기
