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

Every assessed item carries three fields, and the evidence record it produces repeats them unchanged.

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
