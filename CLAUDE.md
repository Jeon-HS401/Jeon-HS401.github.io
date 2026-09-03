# CLAUDE.md — Jeon-HS401.github.io (개인 블로그)

## 이 레포는
콘텐츠에이전트 파이프라인의 **발행 표면**. 이 레포는 승인된 글을 받아 **게시만** 한다.

드래프팅 "두뇌"는 **content-agent가 자립으로 갖는다**(결정 B' — `취미/content-agent/조망.md` §7·§10-①).
javi는 아침에 열린 PR을 CD에게 상신하는 창구일 뿐 드래프팅을 하지 않는다.
*(2026-09-03 정정: 이 문단은 B'로 대체된 옛 결정 B를 말하고 있었다.)*

## 스택
- Jekyll + minima, GitHub Pages 네이티브 빌드(CI 없음). 로컬 프리뷰는 선택(`bundle exec jekyll serve`, ruby 필요).
- 글 = `_posts/YYYY-MM-DD-제목.md`. 파일명 날짜 필수.

## 파이프라인 규약
- 초안 = PR · 승인 = 머지 · 게시 = 자동 배포. **사람 승인 게이트 없이 자동 머지 금지.**
- 보이스: 자비 보이스 계열(javi `docs/VOICE.md`) — 블로그용 톤은 별도 확정 여지.

## 발행 파일 front matter 규약 (2026-09-03 확정)

레이아웃은 아래 필드를 읽는다. **필수는 반드시 채운다** — 비면 목록과 글 화면의 밀도가 글마다 달라진다.
(2026-09-03 이전에는 규약이 없어 3편 중 1편만 확장 필드를 갖고 있었고, 나머지 둘은 오른쪽 보조 열이 빈 채로 렌더됐다.)

| 필드 | 필수 | 값 | 어디에 쓰이나 |
|---|:-:|---|---|
| `layout` | ● | `post` (`_config.yml` defaults가 자동) | — |
| `title` | ● | 한 문장 제목 | 목록·글 머리·`<title>` |
| `date` | ● | `YYYY-MM-DD HH:MM:SS +0900` | 목록 정렬·표시 |
| `description` | ● | **한 문장.** 본문의 첫 문단을 요약하되 본문에 없는 사실을 넣지 않는다 | 목록 설명·글 머리 deck·`<meta name="description">` |
| `kind` | ● | `worklog` \| `technical` | 글 머리 좌측 라벨(WORKLOG / TECHNICAL NOTE). 없으면 `NOTE` |
| `project` | ● | `_data/projects.yml`의 `key`와 **정확히 일치** | 목록 meta · 글 좌측 레일 · 프로젝트 페이지 집계 |
| `summary_label` + `summary_body` | ○ | 예: `STATUS` + 한두 줄 | 글 머리 **오른쪽 열**. 있을 때만 3열이 된다 |
| `evidence[]` (`ref`, `note`) | ○ | 독자가 확인할 수 있는 근거 | 본문 하단 "근거와 재현" |

**규칙**
- `project` 값이 `_data/projects.yml`에 없으면 프로젝트 페이지에서 그 글은 **어디에도 안 잡힌다.** 새 프로젝트면 데이터 파일에 먼저 추가한다.
- `evidence`는 **본문 하단에만** 나온다. 2026-09-03 이전에는 좌측 레일에도 같은 항목이 중복 렌더됐다.
- `path_label`은 **폐기**했다. 실재하지 않는 라우트를 가리키고 있었고, 그 자리는 `project`가 대신한다.
- **발행 단계에서 새로 쓰는 텍스트(`description`·`summary_body`·`evidence[].note`·이미지 alt)는 초안 게이트를 안 거친 신규 산출이다.** PR 전에 factcheck를 한 번 더 돌린다 (`취미/content-agent/PROCESS.md` 관문 6).

## 안 하는 것
- 승인 없는 자동 게시 · 드래프팅 로직을 여기 넣기(그건 content-agent) · 네이버/티스토리/인스타 자동 게시(별도 어댑터, 나중).
