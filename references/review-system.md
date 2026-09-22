# Review and Retention System

The purpose of review is retrieval and reconnection, not rereading.

## Retrieval first

When revisiting a concept, ask for recall before giving the explanation again.

Examples:

- What problem did this solve?
- What was the core rule?
- Predict this tiny example.
- How is this different from X?
- Can you recreate the example from memory?

Then repair only what has decayed.

## Review checkpoints

Use adaptive checkpoints rather than rigid promises.

The concrete interval ladder is canonical in `long-term-state.md` (default `stage 0`–`stage 5`: ~1/3/7/14/30/60 days). Use only its earliest steps when no persistent review queue exists.

If the learner retrieves easily, expand the interval.
If retrieval is effortful or wrong, revisit sooner with a changed example.

## Interleaving

After individual concepts become stable, mix them.

Example:

```text
scope + closure + this
```

Give a problem where the learner must first decide which concept matters.

This is harder than blocked practice and should come later.

## Cumulative review

A new lesson should occasionally reuse an older concept without announcing it.

This tests whether knowledge can be selected, not just remembered when cued.

## Review summary

When the user wants progress tracking, summarize:

- stable concepts;
- concepts needing review;
- recurring error pattern;
- next review target;
- next application task.

Do not create fake numeric mastery percentages unless the user explicitly requests a scoring system.


## Persistent review queue

For ongoing learners, store active future reviews in `review-queue.json` using the schema and update rules in `long-term-state.md`.

Each queued review should name a capability and a retrieval action. Prefer:

```text
“Explain why this changed and predict a variation”
```

over:

```text
“Review chapter 4”
```

When a queued review is completed, update its result, the relevant knowledge-graph stability, and any linked error pattern. Remove or archive completed queue work rather than leaving an ever-growing active list.
