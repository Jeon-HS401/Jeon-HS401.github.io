# CLAUDE.md — Jeon-HS401.github.io (개인 블로그)

## 이 레포는
콘텐츠에이전트 파이프라인의 **발행 표면**. 드래프팅 "두뇌"는 여기 없고 **javi behavior**로 산다(결정 B — `취미/content-agent/조망.md` §7·§10-①). 이 레포는 승인된 글을 받아 **게시만** 한다.

## 스택
- Jekyll + minima, GitHub Pages 네이티브 빌드(CI 없음). 로컬 프리뷰는 선택(`bundle exec jekyll serve`, ruby 필요).
- 글 = `_posts/YYYY-MM-DD-제목.md`. 파일명 날짜 필수.

## 파이프라인 규약
- 초안 = PR · 승인 = 머지 · 게시 = 자동 배포. **사람 승인 게이트 없이 자동 머지 금지.**
- 보이스: 자비 보이스 계열(javi `docs/VOICE.md`) — 블로그용 톤은 별도 확정 여지.

## 안 하는 것
- 승인 없는 자동 게시 · 드래프팅 로직을 여기 넣기(그건 javi) · 네이버/티스토리/인스타 자동 게시(별도 어댑터, 나중).
