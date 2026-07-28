---
layout: post
kind: worklog
title: "초안이 경험을 대신 말하기 시작한 지점"
description: "자연스러운 문장 하나가 실제 기록에 없던 감정을 만들었다. 소재 선택과 집필 사이에 근거 묶음을 추가했다."
date: 2026-07-27 21:00:00 +0900
project: "content-agent"
tags: [evidence]
path_label: "projects / content-agent / evidence"
summary_label: "STATUS"
summary_body: "phase 0 complete"
evidence:
  - ref: "a94e9b2"
    note: "phase 0 설계 갱신"
  - ref: "EDITORIAL.md"
    note: "Evidence Pack 규칙"
---

생성된 초안에 "한참 고민했다"는 문장이 들어갔습니다. 읽을 때는 자연스러웠지만 입력으로 사용한 커밋과 설계 문서에는 고민 시간이나 감정이 없었습니다.

문체만 보면 문제가 없었습니다. 오히려 너무 자연스러워서 더 늦게 발견됐습니다. 자동 초안에서 먼저 고정해야 할 것은 말투가 아니라 무엇을 사실로 말할 수 있는지였습니다.

<figure class="decision-figure">
  <div class="decision-row" role="img" aria-label="최근 커밋 전체를 전달하던 방식에서 선택된 근거 묶음 하나만 전달하는 방식으로 변경">
    <div class="decision-cell"><span class="micro-label">BEFORE</span><p>최근 커밋과 문서를 한꺼번에 작가 프롬프트로 전달</p></div>
    <div class="decision-arrow" aria-hidden="true">→</div>
    <div class="decision-cell"><span class="micro-label">AFTER</span><p>선택된 Evidence Pack 하나만 집필 단계로 전달</p></div>
  </div>
  <figcaption>소재의 양을 줄인 것이 아니라, 초안이 사용할 수 있는 사실의 경계를 먼저 정했다.</figcaption>
</figure>

## 문장은 맞았지만 사실은 아니었다

커밋은 무엇이 바뀌었는지 알려주지만 작성자가 어떻게 느꼈는지는 알려주지 않습니다. 작업 기록처럼 보이게 만들수록 이 구분이 더 중요해졌습니다.

> 사람 메모가 없으면 감정과 기억을 만들지 않는다.
>
> <cite>EDITORIAL.md · Evidence Pack 규칙</cite>

"오래 고민했다", "마음이 기울었다" 같은 문장은 직접 남긴 메모가 있을 때만 사용할 수 있습니다. 답이 없으면 기술적 사실만으로 쓰거나 후보함에 남깁니다.

## 아직 남은 검수

근거 밖 숫자와 1인칭 경험을 잡는 검수는 아직 연결되지 않았습니다. 다음 dry-run에서는 문장마다 어떤 근거를 사용했는지 편집 기록에 남겨볼 생각입니다.

- 숫자와 고유명사가 근거에 연결되는지 확인
- 사람 메모가 없는 감정·기억·행동 문장 검출
- 최근 글과 같은 결론을 반복하는 후보 제외

이 검수가 통과하지 않으면 발행하지 않습니다. 매일 글을 만드는 것보다, 쓸 수 없는 날을 정확히 남기는 편이 이 프로젝트에는 더 맞습니다.
