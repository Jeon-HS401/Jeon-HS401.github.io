---
layout: post
kind: technical
title: "WITH 절로 긴 SQL을 세 단계로 나누는 법"
description: "중첩 쿼리를 읽을 수 있는 중간 결과로 바꾸고, 각 단계의 실행 계획을 따로 확인한다."
date: 2026-07-28 09:00:00 +0900
project: "query-notes"
tags: [SQL, query-design]
path_label: "notes / sql / query-design"
summary_label: "TESTED WITH"
summary_body: "PostgreSQL 17"
evidence:
  - ref: "query.sql"
    note: "비교한 두 쿼리"
  - ref: "explain.txt"
    note: "실제 실행 계획"
---

집계와 필터가 한 쿼리에 겹치면 최종 결과만 남고 중간 상태를 확인하기 어렵습니다. 문제가 생겼을 때 어느 조건에서 행이 줄었는지도 바로 보이지 않습니다.

먼저 주문별 합계를 별도 단계로 꺼냈습니다. 여기서 목적은 쿼리를 무조건 빠르게 만드는 게 아니라, 확인할 수 있는 경계를 만드는 것입니다.

## 중간 결과에 이름을 붙인다

주문 원본에서 고객별 합계를 만들고, 그 결과에서 다시 고액 고객을 고릅니다. 하나의 문장에 있던 두 판단이 각각 확인 가능한 단계가 됩니다.

<div class="code-wrap">
  <div class="code-head"><span>query.sql</span><span>PostgreSQL 17</span></div>
  <pre><code>WITH order_totals AS (
  SELECT customer_id, SUM(amount) AS total
  FROM orders
  GROUP BY customer_id
)
SELECT customer_id, total
FROM order_totals
WHERE total &gt; 100000
ORDER BY total DESC;</code></pre>
</div>

<div class="result-panel">
  <div class="result-head"><span>RESULT</span><span>18 rows · 42 ms</span></div>
  <table class="result-table">
    <thead><tr><th>customer_id</th><th>total</th></tr></thead>
    <tbody>
      <tr><td>c_1042</td><td>184,500</td></tr>
      <tr><td>c_0861</td><td>163,200</td></tr>
      <tr><td>c_1517</td><td>147,800</td></tr>
    </tbody>
  </table>
</div>

## 읽기 쉬움과 성능은 같은 말이 아니다

WITH 절 자체가 쿼리를 빠르게 만들지는 않습니다. PostgreSQL 버전과 쿼리 형태에 따라 CTE가 인라인될 수도, 별도로 계산될 수도 있습니다. 실행 계획을 확인하지 않고 성능 개선이라고 부르면 안 됩니다.

이번 변경에서 먼저 얻은 것은 속도가 아니라 검증 가능한 구조였습니다. 중간 결과의 행 수를 따로 보고, 필터 조건을 한 단계씩 바꿀 수 있게 됐습니다.

## 다음 확인

같은 조건을 서브쿼리로 작성하고 데이터 양을 늘린 뒤 실행 계획과 실제 시간을 비교할 예정입니다. 결과가 같다면 그때는 팀이 더 쉽게 읽는 쪽을 선택하면 됩니다.
