# Jeon-HS401.github.io — 개인 블로그

콘텐츠에이전트 파이프라인의 **발행 표면**(외부 실체). 에이전트가 초안 → CD 승인(머지) → GitHub Pages 자동 배포.

- **스택**: Jekyll(minima) — GitHub Pages 네이티브 빌드(별도 CI 없음).
- **글**: `_posts/YYYY-MM-DD-제목.md` (마크다운 + 프론트매터).
- **설정**: `_config.yml` (title·description 확정 필요).
- **컨셉·설계**: `취미/content-agent/조망.md` 참조 — 드래프팅 "두뇌"는 javi behavior, 이 레포는 발행만.

## 배포
`main`에 머지 = 자동 게시. 첫 활성화는 GitHub → Settings → Pages.

## 로컬 프리뷰(선택)
`bundle exec jekyll serve` (ruby·bundler 필요). 안 해도 GitHub이 빌드함.
