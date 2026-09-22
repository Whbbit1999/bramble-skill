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

When a future skill version changes a schema, preserve existing evidence and migrate fields conservatively. Unknown fields should not be silently discarded unless they are known to be obsolete.

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

The state files should stay small and machine-editable. Store only evidence that changes teaching decisions.

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

Update by evidence:

- `easy_success` → move one stage later;
- `effortful_success` → usually keep the stage or move one stage later when transfer was strong;
- `partial` → shorten the next interval and target the broken link;
- `failure` → return to an early stage, repair first, then retest;
- successful novel transfer is stronger evidence than recognition.

Do not make review scheduling dominate a user's focused question. If the learner asks something specific, answer that first; fold in a short due review only when it is relevant and non-disruptive.

## 5. sessions/<session-id>.json — cross-session evidence log

Create one concise record per meaningful learning session.

Recommended fields:

```json
{
  "session_id": "20260922-js-this",
  "started_at": "2026-09-22T10:00:00+08:00",
  "goal": "Understand ordinary function this",
  "topics_touched": ["js-this-call-site"],
  "evidence": [
    {
      "topic_id": "js-this-call-site",
      "capability": "understanding",
      "result": "success",
      "kind": "prediction",
      "learner_answer": "user.sayName() calls sayName with user as this; but fn() just calls the function, so this is undefined",
      "note": "Correctly predicted method vs detached call"
    }
  ],
  "errors_created": ["err-20260922-001"],
  "reviews_completed": [],
  "next_candidates": ["js-arrow-this"]
}
```

The session log is an audit trail. The knowledge graph is the current synthesized state.

Every quiz, prediction, explanation, or production item keeps the learner's **verbatim answer** in `learner_answer`, alongside the `result` and diagnostic `note`. This is what a later review session reads to see what was actually said — not a paraphrase of it. Keep the full response even when it is long; trim only unrelated chatter and note the trim.

`topic_id`, `capability`, and `kind` follow the item contract in `references/assessment.md`. `kind` must be the value the item had when it was presented, so an item and its evidence round-trip.

## 6. current-plan.json — active roadmap and frontier

Keep the current goal and the next few candidate capabilities, not a rigid semester plan.

Example:

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
  "last_updated_at": "2026-09-22"
}
```

The frontier should change as evidence changes.

`current-plan.json` may also store the immediately generated next lesson:

```json
{
  "schema_version": 1,
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
5. read the latest relevant session record only when needed;
6. choose today's objective using `references/next-lesson-engine.md`.

Do not dump this state to the learner unless they ask for a progress view.

## During-session update protocol

After meaningful evidence:

1. classify the evidence type;
2. update only the affected capability dimension;
3. update stability only when retrieval evidence justifies it;
4. create an error instance for a diagnostic mistake;
5. promote recurring mistakes into an error pattern;
6. create/update review items for fragile or important learning;
7. update the graph frontier if prerequisites or unlocks changed.

Avoid rewriting every state file after every sentence. Batch updates after a learning unit or at natural checkpoints.

## Session end protocol

When a meaningful session ends:

1. write/update the session record;
2. synthesize the latest capability states into the knowledge graph;
3. reconcile active error patterns;
4. update the review queue;
5. regenerate `current-plan.json` using the next-lesson engine;
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
