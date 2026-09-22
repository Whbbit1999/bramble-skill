# Lesson and Roadmap Design

## Dependency-first planning

A learning roadmap is a graph, not a shopping list.

For each topic identify:

```text
prerequisites → concept → capabilities unlocked → proof of understanding
```

Example:

```text
values + variables
      ↓
functions
      ↓
scope
      ↓
closures
      ↓
callbacks / composition / encapsulation
```

## Build milestones, not topic piles

Prefer:

```text
Concepts A+B
→ learner can build X
→ Concept C
→ learner can debug Y
→ Concepts D+E
→ small project
```

Milestones create observable capability.

## Lesson objective

Each lesson should have one primary objective that can be demonstrated.

Weak objective:

> Learn promises.

Better:

> Predict the order of a simple Promise example and explain why `.then()` runs after the current synchronous work.

## Lesson structure

A robust unit can follow:

1. orientation;
2. motivating problem;
3. minimal phenomenon;
4. intuition/model;
5. rule;
6. terminology;
7. guided prediction;
8. independent practice;
9. boundary or misconception;
10. retrieval summary.

Do not include all steps mechanically when the lesson is simple.

## Difficulty ladder

Move through:

```text
worked example
→ one-variable variation
→ partially completed problem
→ independent small problem
→ unfamiliar transfer
→ integrated project
```

Do not skip directly from explanation to integration unless the learner is already advanced.

## Project selection

A learning project should have a high ratio of target learning to incidental work.

Good beginner project properties:

- quick visible feedback;
- simple setup;
- natural incremental milestones;
- few external dependencies;
- errors are understandable;
- extensions are obvious.

Avoid projects dominated by authentication, deployment, CSS setup, framework configuration, or third-party APIs when those are not the learning target.

## Roadmap output

When the learner asks for a roadmap, include:

- destination capability;
- prerequisite map;
- ordered stages;
- proof-of-understanding task per stage;
- one or two projects at useful checkpoints;
- optional deeper branches.

Use `../templates/knowledge-map.md` or `../templates/study-plan.md` when helpful.
