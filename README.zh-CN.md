# Bramble（学习辅导技能）

[English](./README.md)

一个面向初学者到进阶学习者的通用自适应辅导技能。

它围绕一个核心想法设计：**怎么教，既取决于学习者正在学什么，也取决于他们当下需要完成什么认知任务。**

## 核心能力

- 前置知识检测；
- 基于证据的学习者建模；
- 学习任务类型 + 学科领域的双轴路由；
- 直觉优先的讲解；
- 依赖关系感知的知识地图；
- 渐进式深入；
- 主动回忆与预测；
- 刻意练习；
- 错误概念诊断；
- 回讲（teach-back）；
- 复习规划；
- 项目式学习；
- 写作/产出反馈；
- 考试与面试准备；
- 自适应难度与迁移检验；
- 持久化知识图谱；
- 错误实例与反复出现错误模式库；
- 跨会话的证据与进度追踪；
- 自适应复习队列；
- 根据已展示的掌握程度动态生成下一课。

## 双轴路由

技能首先识别**学习任务类型**：

```text
conceptual understanding     概念理解
procedural skill             程序性技能
causal explanation           因果解释
analysis/judgment            分析/判断
generative/production skill  生成/产出技能
memorization/retrieval       记忆/提取
project/integration          项目/整合
assessment/preparation       测评/备考
```

然后套用合适的**领域适配器**：

```text
programming                                            编程
mathematics                                            数学
writing                                                写作
language learning                                      语言学习
natural sciences                                       自然科学
conceptual subjects / humanities / social sciences      概念类学科 / 人文学科 / 社会科学
```

这样可以避免把每门学科都当成编程来教，也避免把每个学习问题都当成“解释一下定义”。

## 目录结构

```text
bramble-skill/
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
│   └── technical-teaching.md   # 兼容性指针
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
    ├── long-term-learning-state.md
    └── session-checkpoint.json
```

## 典型提示词

- “从零给我讲一下闭包。”
- “我一直不明白导数到底是什么意思。”
- “不要直接给答案，教我自己做这道数学题。”
- “教我怎么写出更好的技术博客。”
- “帮我练英语条件句，但别一直讲语法。”
- “为什么法国大革命会发生？”
- “考我一下刚才学过的内容。”
- “根据我答错的地方继续出题。”
- “帮我规划一套三个月的学习路线。”
- “记住我学过什么，下次接着上次的进度继续。”

English prompts: see [README.md](./README.md).

## 设计原则

辅导者应当逐步让自己变得不再必要。

一次好的学习过程，不只是让学习者觉得“讲得很清楚”。它要留下证据：证明学习者能够不靠辅导者搀扶，自己提取、推理、产出、判断或迁移这个知识。

## 长期学习状态

对于持续学习的学习者，当宿主运行时提供可持久写入的存储时，本技能可以维护一个本地状态目录：

```text
.bramble/state/
├── learner.json
├── knowledge-graph.json
├── error-library.json
├── review-queue.json
├── current-plan.json
└── sessions/
```

状态由可观察的证据驱动，而不是由“讲过什么内容”驱动。知识图谱记录当前综合出的能力状态，错误库保留有诊断价值的错误模式，复习队列安排提取练习，下一课引擎则根据学习者的目标与当前前沿选择下一个学习目标。

如果运行时无法跨会话保留文件，技能不得声称具备持久化能力；此时应将状态保留在当前对话中，并在有用时提供一个紧凑、可导出、可恢复的状态快照。

题目、原话作答与评估共用[最小数据契约](references/assessment.md#item-contract)，同题重试保留首次结果与提示/答案暴露记录。[学习检查点](references/long-term-state.md#learning-checkpoints--save-and-resume)可定位未回答的原题、已回答未评估的作答，或已保存的反馈，再继续教学。纯对话无需展示 JSON。

实际保存和恢复由具备文件工具的宿主按 skill 执行。浏览器刷新、后台自动保存、事务、并发、UI 控件和服务 API 不在本仓库实现。
