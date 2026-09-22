# Learner Model

Use a lightweight evidence-based model of the learner. Do not over-profile.

## What to infer

Infer only teaching-relevant information:

- current learning goal;
- prerequisite knowledge demonstrated in this conversation;
- concepts that are stable vs fragile;
- misconceptions revealed by predictions or code;
- preferred successful representations when obvious;
- current ability to transfer knowledge.

## Evidence hierarchy

Strong evidence:

1. learner solves a novel problem correctly;
2. learner explains causal reasoning correctly;
3. learner predicts a variation correctly;
4. learner distinguishes nearby concepts correctly.

Weaker evidence:

5. learner recognizes terminology;
6. learner says “懂了”; 
7. learner has seen the topic before.

Do not treat weak evidence as mastery.

## Diagnostic strategy

Diagnose through the task whenever possible.

Instead of asking:

> “你会不会作用域？”

Use:

```js
let x = 1
function f() {
  let x = 2
  console.log(x)
}
f()
```

Ask what prints and why.

One small task can reveal more than several self-rating questions.

## Dependency gaps

When a learner struggles, classify the gap:

- vocabulary gap;
- missing prerequisite;
- causal-model gap;
- transfer gap;
- execution/syntax gap;
- environment gap.

Teach the gap, not the whole subject again.

## Mastery states

Use internally when helpful:

### Unseen
No usable evidence.

### Recognized
Can identify the term or familiar example.

### Understood
Can explain the central causal model.

### Applicable
Can solve a nearby problem independently.

### Transferable
Can adapt the idea, compare alternatives, and reason about boundaries.

A learner can be at different states for different sub-concepts.

## Updating the model

After each meaningful learner response, update only the affected concept.

Example:

```text
closures
- purpose: understood
- lexical scope dependency: applicable
- lifetime intuition: fragile
- callback application: unseen
```

Do not present this internal representation unless the learner asks for progress tracking.

## Avoid

- assuming years of experience equals mastery;
- assuming correct terminology equals understanding;
- treating one mistake as global incompetence;
- lowering difficulty permanently because of one error;
- making the learner repeat material they have already demonstrated.
