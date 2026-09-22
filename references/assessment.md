# Assessment, Exercises, and Feedback

Assessment exists to reveal the learner's model and capability, not to manufacture difficulty.

## General exercise ladder

### 1. Recognition
Identify the relevant idea, feature, pattern, or tool.

### 2. Prediction / inference
Predict what will happen, what follows, or which change matters.

### 3. Completion
Fill in one missing step, line, sentence, derivation, or component.

### 4. Explanation
Explain why the result, claim, or choice makes sense.

### 5. Diagnosis / critique
Find the broken assumption, weak evidence, unclear structure, or inappropriate method.

### 6. Construction / production
Create a minimal solution, proof, paragraph, program, explanation, or response.

### 7. Transfer
Use the capability in a changed context where the relevant method is not explicitly named.

### 8. Integration
Combine multiple capabilities in an authentic task.

## Item contract

Every assessed item carries the three teaching fields below. Attempts and assessments refer back to that item; do not copy fields that could drift apart. This section is the canonical contract for both conversation and future UI hosts.

| Field | Values | Answers |
| --- | --- | --- |
| `topic_id` | a knowledge-graph node id | which capability is being probed |
| `capability` | `knowledge`, `understanding`, `procedure`, `reasoning`, `production`, `judgment`, `transfer` | which mastery dimension the evidence updates |
| `kind` | one of the eight rungs above | what the learner must actually do |

`kind` is a closed set. Use exactly these eight values:

```text
recognition | prediction | completion | explanation | diagnosis | construction | transfer | integration
```

Read `kind` off the action the learner must perform, not off the subject's vocabulary. Domain forms — “recall”, “debugging”, “calculation”, “causal explanation” — are instances of a rung:

```text
recall, classification                    → recognition
unit checking, graph reading, forecasting → prediction
fill-in, representation change            → completion
model explanation, causal account         → explanation
debugging, critique, evidence comparison  → diagnosis
calculation, derivation, drafting, proof  → construction
unfamiliar problem, design choice         → transfer
authentic multi-capability task           → integration
```

Explaining a given result, model, or choice is `explanation`; producing the artifact itself — the proof, calculation, derivation, or written explanation — is `construction`.

`kind` and `capability` answer different questions and are independent: a `recognition` item can test `knowledge`, and a `diagnosis` item can test `reasoning`. Never use `kind` to record the mastery dimension.

If one item genuinely requires more than one rung, use the highest — it governs both the response form and the strength of the evidence.

Which control renders each rung is a host decision, not a skill decision. But a host that renders items depends on this set staying closed, so do not invent `kind` values.

### Stored records (session schema version 1)

All fields below are required unless marked optional. `string` means nonempty text, except `learner_answer`, which may be empty for an explicitly submitted blank answer. IDs are opaque nonempty strings, unique within the learner's state for their record type, assigned once and reused on resume. Do not generate a new ID merely because a record is loaded again. Arrays retain creation order. Extra fields may be preserved; they must not override this contract.

| Record | Fields and types |
| --- | --- |
| Item (`items[]`) | `item_id: string`; `topic_id: string` (graph node ID when a graph exists); `capability: enum` and `kind: enum` above; `prompt: string` (complete question, including code, choices, data, context, and assumptions needed to answer); `response_requirements: string` (what to submit and how it will be judged); optional `review_id: string` (queue entry being tested), paired with `review_state_before: object` (the complete queue entry before this occurrence; preserve it for schedule correction) |
| Attempt (`attempts[]`) | `attempt_id: string`; `item_id: string`; `attempt_number: integer >= 1`; `learner_answer: string` (verbatim); `hint_used: boolean`; `answer_seen: boolean`; optional `answer_trim_note: string` (why unrelated chatter was removed) |
| Assessment (`assessments[]`) | `assessment_id: string`; `attempt_id: string`; `result: "success" \| "partial" \| "failure" \| "void"`; `assessed_at: string` (ISO 8601 date-time with timezone); `supersedes_assessment_id: string \| null`; `objection: string \| null` (verbatim review request); `review_outcome: "upheld" \| "corrected" \| "voided" \| null`; `note: string` (judgment reason grounded in this answer and the requirements); `feedback: string` (learner-facing feedback); `review_result: null \| "easy_success" \| "effortful_success" \| "partial" \| "failure"` |

`result` describes correctness/completeness against the requirements, not independence or mastery. `partial` means some required reasoning or work is valid but incomplete; `failure` means the target capability was not demonstrated. Explain subjective judgments as such. Keep `note` and `feedback` separate: the first explains the evaluation; the second tells the learner what worked and what to repair.

### Links, retries, and assistance

- Each attempt references an item in the same session; each assessment references exactly one saved attempt there. An attempt has one original assessment and zero or more linked reviews, with exactly one effective assessment (the chain tip). An unanswered item has no attempt; an unevaluated answer has an attempt but no assessment. Do not fabricate empty answers for unanswered items.
- Number attempts for each item consecutively from 1. A new answer or revision is a new attempt; never overwrite the earlier answer or its assessment. Reopening a pending exercise continues its original session record even on a later day.
- Preserve the item's wording, requirements, topic, capability, and kind. A changed question is a new item with its own ID. The same item retried after feedback is not an independent test.
- Assistance flags describe exposure **before that submission**. They are cumulative for retries of the same item: once a hint or solution has been given, later attempts cannot reset the corresponding flag. Persist exposure immediately in the checkpoint even if no answer has arrived yet. Feedback that reveals the answer sets `answer_seen` for the next submission, without changing the previous attempt's flags.
- Only a first attempt with both flags false can be candidate independent evidence. Success on a retry or after assistance can show repair, but cannot itself overturn the first judgment, increase stability as independent retrieval, or count as a second independent mastery demonstration. Use a fresh variation without assistance to test independence.
- Preserve the full learner response, including code or relevant artifact text, rather than only a mutable file link or a paraphrase. If unrelated chatter is removed, explain the removal in that attempt's optional `answer_trim_note`. Do not expose solutions, private grading notes, or previous assessed answers when presenting an unanswered question.

### Assessment and review results

Keep the existing correctness vocabulary in `result`. For `result: "void"`, `review_result` is always `null`: the assessment is withdrawn, not a learner failure. Otherwise, for an item without `review_id`, `review_result` is `null`; for a review item it is required to be non-null:

| `result` | `review_result` |
| --- | --- |
| `failure` | `failure` |
| `partial` | `partial` |
| `success` | `easy_success` for independent fluent retrieval; otherwise `effortful_success` |

An assisted success or same-item retry must use `effortful_success` and must not advance the review interval. Independent effortful success may advance it only under the rules in `long-term-state.md`. A scheduled review occurrence uses the first attempt's effective assessment to update the interval and the queue's `last_result` once; retries are repair evidence, not additional review completions. Keep the original failed/partial retrieval visible in history; only an evidence-based review below can replace its effective judgment. Schedule a fresh test after repair.

## Assessment objections

### Locate and recheck

1. Resolve the target `session_id → assessment_id → attempt_id → item_id`. In conversation, “the last question” or a quotation can identify it; learners need not know IDs. If multiple attempts or sessions match, ask which one before changing state. Reuse IDs for a repeated request or interrupted save; an explicit review of a later decision targets that chain's current tip.
2. Read the complete saved prompt, requirements, verbatim answer, pre-submission assistance flags, all prior judgments in this chain, and relevant feedback/exposure chronology. Do not substitute an answer summary or today's edited question. If a required source is missing or contradictory, explain what cannot be verified and request that source; do not manufacture a verdict or change derived state.
3. Distinguish **clarification of existing words** from **new reasoning, a repaired solution, or a changed answer after feedback**. Record the objection verbatim; use clarification only when the original response supports that reading. New material is a new numbered attempt with cumulative exposure flags, never a replacement `learner_answer` or proof of earlier independent success. A message may contain both a valid criticism and a new answer: review the old evidence separately and save the new attempt separately.
4. Recheck the domain reasoning and stated requirements, including alternative valid solutions, subjective criteria, and missing assumptions. Do not silently add a constraint to justify the old verdict. An under-specified item may still admit a valid conditional answer; judge that answer fairly. If the target capability cannot fairly be evaluated, use `void`, not `failure`. A corrected question is a new item; exposure to its solution still prevents a claim of independence.
5. Explain which original words/requirements support the outcome. Save the review before presenting it or repairing persistent state. A pending review does not preempt a different active exercise: keep that checkpoint and its exposure flags intact.

### Append-only assessment chain

Use the existing `assessments[]`, not a separate appeal system. Original assessments have `supersedes_assessment_id`, `objection`, and `review_outcome` all `null`. A review is another full assessment with a new stable ID, the **same `attempt_id`**, and:

- `supersedes_assessment_id`: the current tip's ID, already present earlier in the array;
- `objection`: the learner's exact review request;
- `review_outcome`: `upheld`, `corrected`, or `voided`;
- `note`: the recheck rationale, including whether the statement clarifies original words or introduces new work, and what was wrong with the previous judgment if changed;
- `feedback`: the learner-facing decision and explanation; preserve any hint/answer exposure it creates for future attempts;
- `assessed_at`: when this review was made, not a new retrieval time.

One linear chain per attempt: no second original, cross-attempt link, cycle, fork, missing parent, or duplicate ID. Loading/retrying a save never appends another copy. If the same ID has conflicting content, stop and resolve the conflict. A repeated objection without new grounds returns the saved decision and finishes any pending reconciliation; it is not another assessment or retrieval event.

| Outcome | Effective judgment | Consequence |
| --- | --- | --- |
| `upheld` | Copy previous `result` and `review_result`; give a fresh explanation | No new success, error occurrence, interval, or mastery change |
| `corrected` | A justified `success`, `partial`, or `failure`, with normal review-result mapping | Replace the previous interpretation of this original attempt; do not count both |
| `voided` | `result: "void"`, `review_result: null` | No positive or negative capability, error, or retrieval evidence from this attempt |

An original assessment may also be `void` when an invalid item is discovered before any verdict. A later review can correct a withdrawn assessment if the original evidence actually supports a fair evaluation. Preserve all previous notes and feedback; only the tip supplies current teaching evidence. An upheld withdrawal remains `void`.

If a defect invalidates the **whole item**, examine every assessed attempt on that item and append a withdrawal for each affected chain in the same validated session save; do not withdraw other items on the same topic. Assess a saved unevaluated answer as `void` without fabricating another submission. Stop offering retries of that invalid prompt; explicitly close its checkpoint after all submissions are accounted for, or present a corrected new item. Do not silently close an unrelated checkpoint.

For scheduling, use only the first attempt's effective judgment. A void first attempt does not promote attempt 2 into independent evidence or a review completion. Root assessment time and the saved pre-occurrence queue entry anchor schedule reconstruction; a review of a verdict is not retrieval today.

Follow [derived-state correction](long-term-state.md#correcting-derived-learning-state). Without persistent storage, explain the corrected conclusion and update the current conversation's learner model; offer a complete snapshot when useful, without claiming files were changed.

## Domain-specific evidence

Choose assessment forms that match what “knowing” means in the domain.

### Programming
Prediction, debugging, construction, design choice, transfer.

### Mathematics
Calculation, representation change, derivation, proof/explanation, unfamiliar problem.

### Writing
Revision, comparison, outlining, drafting, justification of choices, critique.

### Language learning
Comprehension, retrieval, controlled production, free production, repair after feedback.

### Natural sciences
Prediction, model explanation, units, graph/data interpretation, assumption/evidence reasoning.

### Conceptual subjects
Recall, chronology/structure, causal explanation, argument mapping, evidence comparison, counterexample.

## One-variable principle

When testing a newly learned relationship, change one important variable at a time when possible.

This makes errors diagnostically useful.

For open-ended tasks, hold the goal and audience constant while changing one feature such as organization, evidence, or wording.

## Worked examples and fading

For procedural skills:

```text
fully worked example
→ example with one missing step
→ partial scaffolding
→ independent problem
→ variation
```

Do not leave scaffolding in place forever.

## Good distractors

If multiple choice is appropriate, wrong options should correspond to plausible misconceptions, not random nonsense.

After answering, explain the misconception represented by relevant distractors.

## Hints

Use progressive hints:

1. point to the relevant feature, line, sentence, datum, or relationship;
2. restate the governing principle;
3. narrow the search space;
4. show one intermediate step or example;
5. reveal the answer with explanation.

Do not jump straight to the full solution unless requested.

## Feedback format

Useful feedback answers:

- What part of the reasoning or artifact was effective?
- What exact assumption, step, or choice failed?
- Why did it feel plausible?
- What principle repairs it?
- Can the learner now handle a nearby case?

For creative/open work, distinguish objective constraints from stylistic preferences.

## Quiz construction

For a short quiz, mix forms rather than asking the same type repeatedly.

A good conceptual quiz may include:

- direct retrieval;
- prediction/inference;
- explanation;
- one transfer item.

A good skill quiz may include:

- one guided performance;
- one independent performance;
- one critique/diagnosis;
- one novel variation.

If the learner has not answered, do not reveal solutions inline unless explicitly requested.

## Recording responses

For every quiz, prediction, explanation, or production item, record the learner's answer as they said it — their wording, not a normalization of it. A later review session needs the original phrasing to tell a wrong model from a wrong word. Keep the full response; trim only unrelated chatter and note the trim.

## Mastery check

A concept or skill is not stable merely because one item was correct.

Prefer at least two different forms of evidence, for example:

```text
correct prediction + correct explanation
```

```text
independent procedure + successful transfer
```

```text
strong draft + accurate self-critique/revision
```

## Error classification

Classify meaningful errors as one of:

- missing prerequisite;
- wrong causal/conceptual model;
- overgeneralization;
- terminology/notation confusion;
- procedure-selection error;
- execution slip;
- evidence-selection error;
- organization/structure error;
- judgment/calibration error;
- context/environment confusion;
- retrieval failure;
- edge-case gap.

Use the category to choose the next exercise.
