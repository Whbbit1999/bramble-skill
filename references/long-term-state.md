# Long-Term Learning State

Use this system when the learner wants ongoing study across multiple sessions, progress tracking, adaptive review, or a curriculum that changes based on demonstrated mastery.

The state is **evidence-based**. Do not convert exposure into mastery and do not invent precise percentages.

## Persistence capability check

At the start of an ongoing-learning workflow, determine whether the runtime has a persistent writable workspace.

Preferred state location:

```text
.bramble/state/
```

If the user specifies another location, use it instead.

When the working directory is version-controlled, do not silently commit learning history. Add `.bramble/` to the workspace ignore file, or place state in a user-scoped path outside the repository, before writing the first state file.

If persistent storage is unavailable, do not pretend that cross-session state will survive. Keep state in the current conversation and, when useful, produce a compact state snapshot the learner can save and restore later.

## Initialize state

Create state only for an ongoing-learning workflow or when the learner asks for progress tracking. Do not create persistent files for every one-off explanation.

If `.bramble/state/` does not exist and persistence is appropriate:

1. create the directory and `sessions/`;
2. copy or instantiate the JSON skeletons from `templates/`;
3. keep `schema_version` in every state file;
4. create only minimal initial graph nodes needed for the learner's active goal;
5. do not backfill invented history.

## State layout

```text
.bramble/state/
├── learner.json
├── knowledge-graph.json
├── error-library.json
├── review-queue.json
├── current-plan.json
└── sessions/
    └── <session-id>.json
```

Keep state machine-editable and focused on teaching decisions. Minimize unnecessary content; this is not a fixed size limit. The deduplication ID lists grow with the assessment history as described below.

## 1. learner.json — durable learning context

Store only stable, learning-relevant information:

- active goals;
- subject/domain;
- desired capability or destination;
- constraints the learner explicitly gave;
- demonstrated strengths that materially affect instruction;
- teaching representations that repeatedly worked;
- active course/roadmap identifiers.

Do **not** turn this into a general personal profile.

Example shape:

```json
{
  "schema_version": 1,
  "active_goals": [
    {
      "id": "js-foundations",
      "goal": "Build a reliable mental model of core JavaScript",
      "status": "active"
    }
  ],
  "preferences": {
    "effective_representations": ["small executable examples", "execution traces"]
  }
}
```

## 2. knowledge-graph.json — dependency and mastery graph

Represent the curriculum as a graph, not a flat checklist.

Each node should describe a learnable capability or concept.

Recommended node fields:

```json
{
  "id": "js-this-call-site",
  "title": "Determine ordinary-function this from call site",
  "domain": "programming",
  "prerequisites": ["js-function-call"],
  "relations": [
    {"type": "contrasts_with", "target": "js-arrow-this"}
  ],
  "capabilities": {
    "understanding": "applicable",
    "procedure": "understood",
    "transfer": "recognized"
  },
  "stability": "developing",
  "last_evidence_at": "2026-09-22",
  "last_review_at": "2026-09-22",
  "tags": ["javascript", "this"]
}
```

Use the mastery states from `references/learner-model.md`:

```text
unseen → recognized → understood → applicable → transferable
```

Use a separate stability label when useful:

- `fragile` — recently learned or repeatedly fails retrieval;
- `developing` — works in familiar tasks but is inconsistent;
- `stable` — reliably retrieved and applied in nearby contexts;
- `durable` — survives delayed retrieval and transfer.

Mastery and stability answer different questions. A learner can understand a concept deeply today while its memory is still fragile.

### Graph relation types

Use only relations that improve teaching decisions, such as:

- `prerequisite_of`;
- `part_of`;
- `contrasts_with`;
- `supports`;
- `applies_to`;
- `often_confused_with`;
- `generalizes_to`.

Avoid creating an enormous ontology. Add nodes and relations as they become relevant.

## 3. error-library.json — mistakes and recurring error patterns

Record meaningful mistakes because they reveal what to teach next.

Do not store every typo.

Separate **instances** from **patterns**.

### Error instance

An instance captures one diagnostic event. Keep the learner's own words first, then the diagnosis:

```json
{
  "id": "err-20260922-001",
  "topic_id": "js-this-call-site",
  "category": "incorrect_conceptual_model",
  "summary": "Assumed this is determined by where the function was defined",
  "task_summary": "Predict the receiver of a detached method call",
  "learner_answer": "It's still obj, because the function was defined inside obj",
  "learner_response_summary": "Expected the original object to remain this",
  "corrective_principle": "For ordinary functions, infer this from the call form",
  "retest_prompt": "Predict a different detached method example and explain the call site",
  "session_id": "20260922-js-this",
  "created_at": "2026-09-22"
}
```

### Error pattern

A pattern is created only when the same underlying cause recurs or is important enough to track deliberately:

```json
{
  "id": "pat-this-definition-site",
  "category": "incorrect_conceptual_model",
  "signature": "Uses function definition location to infer ordinary-function this",
  "topics": ["js-this-call-site"],
  "status": "active",
  "occurrences": 2,
  "repair_strategy": "Use call-site traces and detached-method contrasts",
  "last_test_result": "partial",
  "last_seen_at": "2026-09-22"
}
```

Useful categories include:

- missing prerequisite;
- terminology/notation confusion;
- incorrect conceptual model;
- overgeneralized rule;
- procedure-selection error;
- execution slip;
- evidence-selection problem;
- structure/organization problem;
- judgment/calibration problem;
- context/environment confusion;
- edge-case gap;
- retrieval failure.

A pattern can move through:

```text
active → improving → resolved → watch
```

Resolved patterns may remain in the library so future regressions can be recognized.

## 4. review-queue.json — adaptive retrieval queue

Each review item should test a specific capability, not merely say “review chapter 3”.

Recommended shape:

```json
{
  "id": "rev-js-this-001",
  "topic_id": "js-this-call-site",
  "capability": "transfer",
  "kind": "explanation",
  "prompt": "Predict this for a detached method call and explain why",
  "due_at": "2026-09-25",
  "stage": 1,
  "last_result": "effortful_success",
  "source": "err-20260922-001"
}
```

`last_result` is `null | "easy_success" | "effortful_success" | "partial" | "failure"`. It stores the `review_result` of the first attempt's assessment from the most recent review occurrence used for scheduling; use `null` before any such assessment exists. Same-item retries never overwrite it or update the interval again. A fresh review occurrence can replace it when its first attempt is assessed. For example, first-attempt failure followed by an assisted success leaves `last_result: "failure"`; both assessments remain in the session.

Use retrieval prompts that can reveal whether the model survived:

- explain without notes;
- solve a small problem;
- predict a changed case;
- reconstruct a causal chain;
- revise a short passage;
- produce a sentence;
- compare two nearby concepts;
- apply the idea in a novel context.

### Default adaptive interval ladder

This is the canonical ladder; other references point here. Use it only as a practical default, not as a scientific guarantee:

```text
stage 0 → next session or ~1 day
stage 1 → ~3 days
stage 2 → ~7 days
stage 3 → ~14 days
stage 4 → ~30 days
stage 5 → ~60 days
```

Update by independent retrieval evidence; apply the `result` / `review_result` mapping and retry restrictions in `assessment.md`:

- `easy_success` → move one stage later;
- `effortful_success` → usually keep the stage or move one stage later when transfer was strong;
- `partial` → shorten the next interval and target the broken link;
- `failure` → return to an early stage, repair first, then retest;
- successful novel transfer is stronger evidence than recognition.

Do not make review scheduling dominate a user's focused question. If the learner asks something specific, answer that first; fold in a short due review only when it is relevant and non-disruptive.

## 5. sessions/<session-id>.json — cross-session evidence log

Create one concise record per meaningful learning session using `templates/session-record.json`. The session log is the source of evidence; the graph, errors, and review queue are derived teaching state.

Session files use `schema_version: 1`. Required fields:

| Field | Type / meaning |
| --- | --- |
| `schema_version` | integer `1` |
| `session_id` | nonempty string, stable and unique in this learner's state; filename is `<session_id>.json` |
| `started_at` | ISO 8601 date-time string with timezone |
| `goal` | nonempty string describing this session's learning goal |
| `items`, `attempts`, `assessments` | arrays using the canonical [assessment contract](assessment.md#stored-records-session-schema-version-1) |
| `checkpoint` | `null` when no exercise is active, otherwise the object below |

The optional `topics_touched`, `errors_created`, `reviews_completed`, and `next_candidates` are arrays of unique nonempty strings. They are summaries, not additional evidence. Instantiate the template's null session metadata before saving a real session. `session_id` must match `[A-Za-z0-9][A-Za-z0-9._-]*`: a safe single filename component, not a path.

Keep IDs and immutable records together in their original session. An assessment's topic, capability, kind, and original answer are obtained by following `attempt_id → item_id`, not by making another evidence copy. `review_id` remains a historical reference even if completed queue work is removed.

## Learning checkpoints — save and resume

A checkpoint belongs inside the session file so the active question, submitted answer, evaluation, and resume position can be saved together. No separate checkpoint file or UI is required.

| Checkpoint field | Required type / meaning |
| --- | --- |
| `item_id` | nonempty string referring to a saved item in this session |
| `attempt_id` | `null` while awaiting a submission, otherwise the latest saved attempt ID for this item |
| `hint_used`, `answer_seen` | booleans recording cumulative exposure for the next submission; never lower than any saved attempt for this item |

Do not also store a phase flag that can disagree with the records. Derive the resume position:

| Saved position | Resume action |
| --- | --- |
| Checkpoint has no `attempt_id` | Present the **exact saved prompt and response requirements** and wait; do not expose a solution, grading note, or previous answer. This also covers an explicitly opened retry after earlier attempts were evaluated. |
| Checkpoint attempt has no assessment | Evaluate its saved `learner_answer` with the original requirements and assistance flags. Do not ask the learner to repeat it or create another attempt. |
| Checkpoint attempt has an assessment | Retain and, when useful, re-show its saved feedback and result. Reconcile only unapplied assessments below, then decide retry, consolidation, or a fresh next item. Do not grade the same attempt again. |

Only one exercise is active in a session. Before moving away from a submitted answer, finish its evaluation. A null checkpoint requires all stored attempts to be evaluated. While waiting on a retry, all older attempts must already be evaluated. To open the retry, retain the item and exposure flags and set checkpoint `attempt_id` to null; create the next numbered attempt only on submission. Clear the checkpoint only after the exercise has been deliberately closed, or replace it when presenting the next item. Do not clear it just because the session is ending.

### Save boundaries

For ongoing learning in a host with persistent file tools, execute these saves, not merely describe them:

1. Before presenting an exercise, save its complete item and checkpoint. Then set `current-plan.json.resume_session_id` to its session ID (defined in the current-plan section below).
2. Before giving a hint or revealing a solution, save the updated checkpoint exposure flags. Do not change flags on already submitted attempts.
3. On receiving an answer, append the verbatim attempt and point the checkpoint at it; save **before** evaluating.
4. Append its assessment, retain the checkpoint, and save **before** showing feedback or updating derived state. If that feedback provides hints/answers for a retry, save the corresponding checkpoint exposure flags in this same write.
5. Reconcile affected derived files using the assessment IDs below. Save the next checkpoint when a retry or next question is actually chosen. Replanning must preserve `resume_session_id` while the exercise is active.

Validate the candidate session before replacing the saved file. On failed validation or a failed write, keep the last valid file and the unsaved work in the conversation; report that the checkpoint was not saved. Do not claim recovery beyond the last successful save. These are tutor/host execution rules, not background autosave.

### Locate a resume point

Honor a specific current request first. For “continue”, load the relevant `current-plan.json.resume_session_id` and inspect that session's checkpoint **before generating a next lesson**. Resolve only a safe session filename inside `sessions/`. If the pointer is absent, stale, or references a closed session, inspect relevant session files for non-null checkpoints; repair the pointer when exactly one valid candidate matches the active goal. Do not rely on filename ordering or modification times to choose between conflicting checkpoints. If several candidates remain ambiguous, ask which to resume while preserving them. If none exists, use the latest relevant evidence and next-lesson engine.

A broken reference or partial/corrupt snapshot is not a license to fabricate a missing question or answer. Preserve the records, explain the missing piece, and use an intact checkpoint or request the missing snapshot.

Without persistent storage, apply the same teaching sequence within the current conversation. For cross-session continuation, export/load the session with its complete referenced items, attempts, assessments, and checkpoint plus the relevant goal/derived state and their applied-ID markers. A pointer alone is not a usable snapshot. Validate imported records, preserve IDs, and never blindly append an imported session to itself. On a conflicting existing ID with different content, preserve both sources and resolve the conflict before replacing records. JSON is an internal/export format; conversational learners need only the question, feedback, and a short save-status message when relevant.

### Reconciliation without duplicate evidence

`assessment_id` is the deduplication key. `knowledge-graph.json`, `error-library.json`, and `review-queue.json` each have an optional `applied_assessment_ids: string[]` (unique IDs; templates start empty). Track it separately per destination because a save may succeed for one file and fail for another.

Accept linear growth of these lists with the number of assessments, with an ID stored in up to three destinations. Retain the IDs even when an assessment makes no teaching-state change; do not trim them independently of history, which could allow replay. No compaction or archival mechanism is defined at this stage.

- For each assessment not yet applied to a destination, follow its links, synthesize only relevant changes, and save those changes **together with** its ID in that destination's list. Record the ID even when no change is warranted. Already listed means skip, including counters, error occurrences, mastery promotion, and review rescheduling.
- A retry's new ID preserves repair evidence but does not make it independent evidence or another completion of the same scheduled review. Use the first attempt's assessment for that item's review interval and `last_result`, as specified in `assessment.md`; do not increment recurring-error occurrences for repeats of the same mistake on that same item.
- Resume after an interrupted reconciliation checks each destination separately. Do not reapply already recorded IDs.
- Keep summary arrays such as `reviews_completed` unique. Recomputing `current-plan.json` from synthesized state is allowed; it must not itself add evidence or erase an active checkpoint locator.

Prefer validated temporary-file replacement for each destination when supported. IDs plus same-file markers specify idempotent processing semantics; they do **not** provide database transactions, atomic multi-file commits, concurrency control, browser refresh handling, or guaranteed delivery of feedback. If a host cannot safely save changes and markers together, inspect/reconcile an ambiguous write before continuing instead of asserting exactly-once behavior. Those runtime guarantees belong to the future host application.

## 6. current-plan.json — active roadmap and frontier

Keep the current goal and the next few candidate capabilities, not a rigid semester plan.

`resume_session_id` is a required `string | null` locator for the active session in `sessions/`, not a second checkpoint. A non-null value follows the session ID filename rule above. Initialize it to `null` when no exercise is active. When regenerating the plan, carry forward the existing locator instead of resetting it from the template; ending a conversation does not close an exercise. Clear it only after the exercise is explicitly closed and the locator still points to that session. Opening another exercise sets it to that exercise's session ID.

Example with an active exercise:

```json
{
  "schema_version": 1,
  "goal_id": "js-foundations",
  "current_focus": "js-this-call-site",
  "frontier": [
    "js-arrow-this",
    "js-explicit-binding"
  ],
  "blocked_by": [],
  "resume_session_id": "20260922-js-this-retry",
  "last_updated_at": "2026-09-22"
}
```

The frontier should change as evidence changes.

`current-plan.json` may also store the immediately generated next lesson (excerpt; preserve the other plan fields):

```json
{
  "schema_version": 1,
  "resume_session_id": "20260922-js-this-retry",
  "next_lesson": {
    "objective": "Distinguish ordinary-function this from arrow-function lexical this",
    "why_now": "Ordinary call-site reasoning is usable; the next contrast is ready",
    "reactivate": ["js-this-call-site"],
    "opening_retrieval": "Predict one detached ordinary-function call",
    "core_activity": "Compare equivalent ordinary and arrow-function examples",
    "proof_of_learning": "Explain and predict a mixed example without hints",
    "watch_for": ["Treating arrows as call-site bound"],
    "unlocks": ["callback-context-reasoning"]
  }
}
```

Regenerate this object when new evidence materially changes readiness.

## Session start protocol

For an ongoing learner:

1. load `learner.json`;
2. inspect the active goal and only the relevant part of `knowledge-graph.json`;
3. check due items in `review-queue.json`;
4. inspect active error patterns relevant to the current topic;
5. locate and restore a checkpoint using the rules above;
6. only when no exercise is pending, choose today's objective using `references/next-lesson-engine.md`.

Do not dump this state to the learner unless they ask for a progress view.

## During-session update protocol

After saving an assessment, reconcile it once per destination using its ID:

1. classify the evidence type;
2. update only the affected capability dimension;
3. update stability only when retrieval evidence justifies it;
4. create an error instance for a diagnostic mistake;
5. promote recurring mistakes into an error pattern;
6. create/update review items for fragile or important learning;
7. update the graph frontier if prerequisites or unlocks changed.

Save the session at each checkpoint boundary above. Derived-state changes may be batched after a learning unit; never defer saving a pending question or submitted answer until session end.

## Session end protocol

When a meaningful session ends:

1. save the session record, preserving any active checkpoint;
2. synthesize the latest capability states into the knowledge graph;
3. reconcile active error patterns;
4. update the review queue;
5. regenerate `current-plan.json` using the next-lesson engine, preserving `resume_session_id` while the exercise remains active;
6. optionally give the learner a short human summary: what became stable, what remains fragile, and the best next step.

## State minimization and privacy

Persist only what is useful for learning.

Do not store secrets, unrelated personal details, or sensitive information merely because it appeared in a study conversation.

For sensitive learning topics, prefer minimal topic labels and capability evidence over unnecessary content reproduction.

If the learner asks to delete or reset learning state, remove or reset the relevant local state files according to the host environment's file capabilities.

## Consistency rules

- Prefer atomic state-file updates when the host supports them: write a temporary file, validate JSON, then replace the old file.
- Evidence log is append-oriented; avoid rewriting history.
- Knowledge graph is synthesized current state and may be updated.
- Error instances always keep the learner's actual response (`learner_answer`, verbatim or near-verbatim); never store only the diagnosis. If the response was long, keep the relevant excerpt in the instance and the full answer in the session log.
- Error patterns summarize repeated causes, not every wrong answer.
- Review queue contains active future retrieval work, not completed history.
- Current plan is provisional and can change after any strong new evidence.
- Never infer mastery simply because a session covered the topic.
