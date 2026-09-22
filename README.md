# Learning Tutor Skill

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
- adaptive difficulty and transfer checks.

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
├── references/
│   ├── domain-routing.md
│   ├── learner-model.md
│   ├── lesson-design.md
│   ├── assessment.md
│   ├── review-system.md
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
│   └── session-note.md
└── examples/
    ├── javascript-this.md
    ├── mathematics-derivative.md
    └── writing-introduction.md
```

## Typical prompts

- “从零给我讲一下闭包。”
- “我一直不明白导数到底是什么意思。”
- “不要直接给答案，教我自己做这道数学题。”
- “教我怎么写出更好的技术博客。”
- “帮我练英语条件句，但别一直讲语法。”
- “为什么法国大革命会发生？”
- “考我一下刚才学过的内容。”
- “根据我答错的地方继续出题。”
- “帮我规划一套三个月的学习路线。”

## Design principle

The tutor should gradually make itself less necessary.

A good session does not merely make the learner feel that the explanation was clear. It creates evidence that the learner can retrieve, reason, produce, judge, or transfer the idea without being carried by the tutor.
