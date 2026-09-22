# Learning Tutor Skill

An adaptive tutoring skill designed for beginners through intermediate learners, with a strong focus on technical learning.

## What it adds beyond a normal explainer

- prerequisite detection;
- learner-model tracking;
- dependency-aware knowledge maps;
- progressive depth;
- intuition-first explanations;
- active recall and prediction;
- deliberate exercise progression;
- misconception diagnosis;
- teach-back;
- review planning;
- project-based learning;
- interview preparation;
- adaptive difficulty.

## Directory

```text
learning-tutor/
├── SKILL.md
├── README.md
├── references/
│   ├── learner-model.md
│   ├── lesson-design.md
│   ├── assessment.md
│   ├── review-system.md
│   ├── technical-teaching.md
│   └── modes.md
├── templates/
│   ├── knowledge-map.md
│   ├── study-plan.md
│   └── session-note.md
└── examples/
    └── javascript-this.md
```

## Suggested use

Place the whole `learning-tutor` directory in the Skills directory used by your Agent environment. The exact installation path varies by product, but `SKILL.md` is the entry point and the other files are supporting references.

Typical prompts that should activate the behavior:

- “从零给我讲一下闭包。”
- “我总是搞不懂 JS 的 this。”
- “帮我规划一套 Vue 学习路线。”
- “不要直接告诉我答案，用苏格拉底方式教我。”
- “考我一下 Promise。”
- “根据我刚才答错的地方继续练习。”
- “把这个知识点压缩成面试时能说的版本。”

## Design principle

The tutor should gradually make itself less necessary.
