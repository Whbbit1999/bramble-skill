# Assessment objections — fictional walkthrough

These examples illustrate [the review protocol](../references/assessment.md#assessment-objections). They use fictional records only. Learners can make every request in ordinary conversation without knowing record IDs.

## A valid alternative was marked wrong

- Item `q1`: “给出一个满足 x² = 4 的实数 x，并代入验证。”
- Original attempt `a1`: “-2，因为 (-2)² = 4。” First submission, no hint or answer exposure.
- Original assessment `e1`: failure, because the tutor incorrectly required the positive root.
- Objection: “我的答案也成立，题目只要求一个实数解。”

Re-read the wording: the original answer already meets the requirement. Append this assessment; do not change `a1` or `e1`:

```json
{
  "assessment_id": "e1-review",
  "attempt_id": "a1",
  "result": "success",
  "assessed_at": "2026-09-24T10:00:00+08:00",
  "supersedes_assessment_id": "e1",
  "objection": "我的答案也成立，题目只要求一个实数解。",
  "review_outcome": "corrected",
  "note": "原回答已给出负根并正确验证；原评估擅自增加了必须为正根的限制。这是对原回答的澄清，没有新增解题步骤。",
  "feedback": "你的异议成立。原题只要求一个实数解，-2 及你的验证都正确；我把它误判了。",
  "review_result": "easy_success"
}
```

Here `q1` is a scheduled review item with a saved `review_state_before`; otherwise `review_result` would be `null`. `easy_success` is justified only by the original independent fluent response, never by fluency during the objection.

Suppose `e2` is later independent evidence on the same capability: for x² = 9, the learner answered “9，因为 9² = 9。” That failure remains valid. The effective evidence is now **one success (`e1-review`) and one failure (`e2`)**; it is not two successes or zero failures.

| State | Corrected effect |
| --- | --- |
| Error instances | Remove only the instance from `a1`; retain the instance from `a2` |
| Shared error pattern | If both previously supported it, recompute from surviving distinct items: 2 → 1; do not delete the whole pattern |
| Capability/stability | Reconsider the affected dimension with both effective judgments; do not declare mastery from this correction alone |
| Review queue | Reconstruct `q1`'s occurrence from its saved pre-state and original retrieval date, then retain later valid review occurrences; no interval step on the objection date |
| Next lesson | Remove the “positive roots only” repair if it was based only on `e1`; retain justified work on substitution/squaring from `e2` |

Keep the active checkpoint, including a different pending question, while saving the corrected plan.

## The original judgment stands

For the second item above, “我觉得 9 也可以” does not make 9² equal 9. Append an `upheld` review of `e2`, preserving `failure` and its `review_result`, and explain the failed substitution. Save its chain IDs as reconciled without adding a second mistake, shortening the interval again, or lowering mastery twice.

If the objection is instead “我原来想写 3，现在改成 3”, preserve the original “9” submission. Evaluate “3，因为 3² = 9” as attempt 2 with the actual cumulative hint/answer exposure. It can show repair; it cannot make attempt 1 successful or independent. Even when both a criticism and a new solution arrive in one message, review and retry remain separate records.

## The item cannot support a fair judgment

- Item: “匀速行驶两小时，求路程的数值。” No speed is supplied.
- Original answer: “还需要速度。”
- Objection after a failure verdict: “题目没有给速度。”

Withdraw the numerical-performance judgment: `review_outcome: "voided"`, `result: "void"`, `review_result: null`. Acknowledge the missing condition; do not record missing speed as the learner's error. If every attempt on this item shares that defect, withdraw each affected chain. Close only this invalid exercise's checkpoint after any saved unevaluated answer is accounted for. A corrected problem with speed supplied gets a new item ID and retains relevant exposure. A sound conditional answer to a question that actually permits symbolic reasoning could instead merit a corrected success; do not withdraw automatically just because the learner says “条件不足”.

## Interrupted correction and repeated requests

After saving `e1-review`, imagine only the graph was saved:

```text
graph.applied_assessment_ids: [e1, e2, e1-review]
errors.applied_assessment_ids: [e1, e2]
queue.applied_assessment_ids: [e1, e2]
```

Recovery skips the graph and reconstructs the affected error/queue values, saving `[e1, e2, e1-review]` with each destination. If original `e1` was never applied, consume its chain using `e1-review` directly; never temporarily create its mistaken failure. If rebuilding also incorporates other pending chains, mark those consumed IDs in that same write.

After all three saves, repeated handling finds no pending chain. Refresh a stale plan/summary without adding evidence and preserve its resume locator. The same objection without new grounds reuses the saved review, not a freshly allocated ID. A failed destination write stays pending; an ambiguous write must be inspected before replay.

Without persistent tools, say: “这次对话中已纠正判断；我无法确认跨会话记录已保存。” If exporting a snapshot, include the original item/attempt, the full assessment chain, checkpoints, and relevant state/markers, not just the new verdict.
