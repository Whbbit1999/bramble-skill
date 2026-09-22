# Learning Tutor Skill

[中文](./README.zh-CN.md)

A general-purpose adaptive tutoring skill for beginners through advanced learners.

It is designed around one idea: **how you teach should depend both on what the learner is studying and on what cognitive task they need to perform.**

## Core capabilities

- prerequisite detection;
- evidence-based learner modeling;
- task-type + domain routing;
- intuition-first explanations;
- dependency-aware knowledge maps;
- progressive depth;
- active recall and prediction;
- deliberate practice;
- misconception diagnosis;
- teach-back;
- review planning;
- project-based learning;
- writing/production feedback;
- exam and interview preparation;
- adaptive difficulty and transfer checks;
- persistent knowledge graphs;
- mistake and recurring-error-pattern libraries;
- cross-session evidence/progress tracking;
- adaptive review queues;
- dynamic next-lesson generation from demonstrated mastery.

## Two-axis routing

The skill first identifies the **learning task**:

```text
conceptual understanding
procedural skill
causal explanation
analysis/judgment
generative/production skill
memorization/retrieval
project/integration
assessment/preparation
```

Then it applies an appropriate **domain adapter**:

```text
programming
mathematics
writing
language learning
natural sciences
conceptual subjects / humanities / social sciences
```

This avoids treating every subject as if it were programming or every learning problem as if it were “explain the definition”.

## Directory

```text
learning-tutor/
├── SKILL.md
├── README.md
├── README.zh-CN.md
├── references/
│   ├── domain-routing.md
│   ├── learner-model.md
│   ├── lesson-design.md
│   ├── assessment.md
│   ├── review-system.md
│   ├── long-term-state.md
│   ├── next-lesson-engine.md
│   ├── modes.md
│   └── technical-teaching.md   # compatibility pointer
├── domains/
│   ├── programming.md
│   ├── mathematics.md
│   ├── writing.md
│   ├── language-learning.md
│   ├── natural-sciences.md
│   └── conceptual-subjects.md
├── templates/
│   ├── knowledge-map.md
│   ├── study-plan.md
│   ├── session-note.md
│   ├── learner.json
│   ├── knowledge-graph.json
│   ├── error-library.json
│   ├── review-queue.json
│   ├── current-plan.json
│   └── session-record.json
└── examples/
    ├── javascript-this.md
    ├── mathematics-derivative.md
    ├── writing-introduction.md
    └── long-term-learning-state.md
```

## Typical prompts

- “Explain closures to me from scratch.”
- “I have never really understood what a derivative means.”
- “Do not give me the answer — teach me to solve this math problem myself.”
- “Teach me how to write better technical blog posts.”
- “Help me practice English conditionals, but do not keep lecturing on grammar.”
- “Why did the French Revolution happen?”
- “Quiz me on what we just covered.”
- “Keep drilling me on the parts I got wrong.”
- “Build me a three-month study plan.”
- “Remember what I have learned and pick up where we left off next time.”
- “Quiz me on what I got wrong last session.”

中文提示词见 [README.zh-CN.md](./README.zh-CN.md)。

## Design principle

The tutor should gradually make itself less necessary.

A good session does not merely make the learner feel that the explanation was clear. It creates evidence that the learner can retrieve, reason, produce, judge, or transfer the idea without being carried by the tutor.


## Long-term learning state

For an ongoing learner, the skill can maintain a local state directory when the host runtime provides persistent writable storage:

```text
.learning-tutor/state/
├── learner.json
├── knowledge-graph.json
├── error-library.json
├── review-queue.json
├── current-plan.json
└── sessions/
```

The state is driven by observable evidence rather than content exposure. The knowledge graph records current synthesized capability, the error library keeps diagnostically useful mistake patterns, the review queue schedules retrieval, and the next-lesson engine chooses the next objective from the learner's goal and current frontier.

If the runtime does not preserve files across sessions, the skill must not claim persistence; it should instead keep state in the current conversation and provide a compact exportable snapshot when useful.
