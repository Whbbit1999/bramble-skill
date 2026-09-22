# Example: Long-Term Learning State Across Sessions

This example shows how state should evolve without pretending that “covered” means “mastered”.

## Session 1 — JavaScript `this`

The learner correctly predicts:

```js
user.sayName()
```

but incorrectly predicts:

```js
const fn = user.sayName
fn()
```

### Recorded assessment

[session-checkpoint.json](session-checkpoint.json) contains the complete question, verbatim attempts, and linked assessments for this call-site misconception. The first failure remains visible after an assisted retry succeeds. Use that structure rather than recording only a verdict.

```text
ordinary-function this / understanding: understood
ordinary-function this / transfer: recognized
stability: fragile
```

Create an error instance:

```text
Assumed the object where a function was defined permanently owns its this.
```

Create a review item for a detached-method variation in roughly one day or next session.

The next lesson should **not** immediately jump deep into `call`, `apply`, and `bind` if the call-site model is still fragile.

## Session 2 — retrieval first

Start with a fresh example rather than re-explaining the rule.

The learner correctly predicts method vs detached calls and explains that ordinary-function `this` depends on how the function is called.

Update:

```text
understanding: applicable
transfer: understood
stability: developing
```

Move the review interval later.

Now the graph frontier may expose:

```text
arrow-function lexical this
explicit binding
callback context loss
```

Choose only one coherent next capability.

## Session 3 — transfer failure

The learner understands ordinary calls but misreads an arrow function inside an object.

Do not downgrade the whole `this` topic.

Create/update a more specific node:

```text
js-arrow-this
```

and a possible error pattern:

```text
Treats arrow functions as if they acquire this from their call site.
```

The dynamic next lesson becomes a **contrast lesson** between ordinary and arrow functions.

## Several weeks later

A delayed mixed problem combines:

```text
method call
+ detached callback
+ arrow function
```

If the learner selects the right rule without being told which concept is under test, that is stronger evidence of durable transfer than recognizing definitions in isolation.

The graph can now mark relevant capabilities as stable/durable and reduce review frequency.

## Interruption and same-question retry

See [session-checkpoint.json](session-checkpoint.json) for a complete fictional session: a strict-mode question, an independent wrong answer, hint feedback, a second correct answer, and both assessments. Its checkpoint is after the second assessment, before deciding the next activity. No real learner data is involved. The first failure still controls that review occurrence and keeps the queue's `last_result` at `failure`; the assisted success shows repair and does not extend the interval. The final feedback reveals the solution, so the checkpoint records `answer_seen: true` for any later retry, while the second attempt correctly retains `answer_seen: false` at submission.

The same session progresses through these save boundaries:

1. **Question saved, no answer.** Save the item with empty `attempts` and `assessments` arrays and a checkpoint with `attempt_id: null` and both exposure flags false. On “继续”, show its exact strict-mode code and response requirements. Do not show either assessment or solution.
2. **Answer saved, no assessment.** Append the first attempt and point the checkpoint at it. If interrupted, evaluate “Both return user because who was defined inside user.” from that record; do not ask again or add another answer.
3. **Assessment saved.** Append the first failure assessment and set checkpoint `hint_used: true` before giving its hint feedback. Resume with the saved feedback/result. Open a retry by clearing only checkpoint `attempt_id`, then append attempt 2 when the learner submits. Do not change attempt 1.
4. **Retry assessed.** The populated JSON shows this position. Retain both results, restore saved feedback, and choose a fresh unassisted variation for evidence of independent learning.

For example, after reconciling both assessments, the graph may contain:

```json
{
  "applied_assessment_ids": ["20260922-this-e1", "20260922-this-e2"]
}
```

This is an excerpt of a derived file, not another evidence log. If the error library was saved only through `20260922-this-e1`, recovery selects `20260922-this-e2` for that file and neither ID for the graph. Repeated recovery selects neither once both are marked; a retry's ID still cannot be used as another independent mastery success. Reuse existing error/review IDs and keep summary arrays unique. Atomic writes remain the host's responsibility.

Without persistent tools, these positions remain usable in the current conversation. Export the complete linked session and relevant state when the learner needs to carry it elsewhere; resume after they load that snapshot. Do not promise automatic cross-session recovery or force JSON into routine teaching.
