# Next-Lesson Engine

Use this engine to decide what the learner should study next when they want an ongoing curriculum or ask “下一步学什么？”.

The result should be dynamic, evidence-based, and small enough to execute in one session.

Before running this engine for “continue”, restore any pending exercise using `long-term-state.md`. A saved unanswered question or unevaluated answer takes precedence over generating another lesson; an evaluated checkpoint retains its feedback while this engine helps decide the next step.

After an assessment correction or withdrawal, reconcile affected derived state before using this engine. Use only effective judgments; remove repair work supported solely by the mistaken assessment, retain independent evidence, and preserve the active checkpoint locator. See `long-term-state.md#correcting-derived-learning-state`.

## Inputs

Use only what is available:

- learner's explicit active goal;
- current knowledge graph;
- missing prerequisites;
- active error patterns;
- due review items;
- latest session evidence;
- current project/exam constraints;
- learner's explicit request in the present conversation.

## Priority order

Choose the next objective using this order unless the user's immediate request overrides it.

### 1. Honor the learner's explicit current goal

A focused question should not be hijacked by a generic curriculum.

### 2. Repair a blocking prerequisite

If the intended next topic depends on a capability that is not yet sufficiently demonstrated, teach the smallest blocking prerequisite first.

### 3. Address a recurring error pattern that will poison future learning

Prioritize active misconceptions when they are foundational or repeatedly causing failure.

### 4. Integrate due retrieval when relevant

If a due review is short and related to the current topic, start with or insert a 1–3 item retrieval check.

If unrelated, defer it rather than derailing the session.

### 5. Advance along the knowledge frontier

A frontier node is a useful next node whose prerequisites are sufficiently stable and which unlocks meaningful capability toward the active goal.

Prefer nodes that:

- unlock several useful downstream capabilities;
- connect directly to the learner's stated goal;
- are small enough for one coherent lesson;
- naturally reuse recently learned material.

### 6. Switch from acquisition to transfer when appropriate

If a concept is `applicable` but transfer is weak, the next lesson may be a changed context, mixed problem, project task, critique, or explanation-to-another-person rather than another explanation.

### 7. Consolidate before piling on

If several adjacent nodes are fragile, prefer a synthesis/retrieval lesson instead of adding another dependency.

## Readiness rule

A prerequisite does not need to be “perfect” before advancing.

Advance when the learner has enough evidence to use it without constant prompting and errors are unlikely to make the new topic unintelligible.

Use judgment rather than a rigid percentage threshold.

## Candidate selection

For each candidate, consider:

```text
Goal relevance
+ prerequisite readiness
+ error-repair value
+ retrieval value
+ unlock value
+ transfer value
- cognitive overload
- redundancy
```

Do not expose a fake numeric score unless the user explicitly wants scoring.

## Generate one observable objective

A next lesson should be phrased as a capability:

Bad:

> Learn closures.

Better:

> Explain why a returned function can still access a variable from its creation scope, then predict two small closure examples without running them.

Bad:

> Learn thesis statements.

Better:

> Given a topic and audience, write a debatable thesis and distinguish it from a factual topic sentence.

## Next-lesson packet

When planning the next session, generate internally or in `current-plan.json`:

```text
Objective:
Why now:
Prerequisites to reactivate:
One opening retrieval check:
Core learning activity:
One controlled variation:
Proof-of-learning task:
Likely misconception to watch:
What this unlocks:
```

Do not automatically show all fields to the learner. Use them to drive instruction.

## Dynamic replanning

Replan immediately when strong evidence contradicts the current plan.

Examples:

- learner solves the diagnostic transfer task easily → skip redundant lesson;
- learner fails because of a prerequisite → move backward one dependency;
- recurring misconception reappears → insert targeted repair;
- learner's goal changes → rebuild the frontier around the new destination;
- project exposes an authentic gap → prefer that gap over an abstract syllabus order.

## Review-aware lesson generation

A good next lesson can combine acquisition and retrieval:

```text
2-minute retrieval of older prerequisite
→ new concept
→ controlled practice
→ transfer task that reuses the older concept
```

This is usually better than creating a completely separate review block.

## Stop rule

Do not keep generating new lessons indefinitely.

If the learner has reached the stated destination capability, shift to authentic application, independent projects, mixed review, or let the learner choose a new goal.
