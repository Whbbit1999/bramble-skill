# Programming and Software Teaching Adapter

## Target capabilities

Programming learning commonly needs:

```text
mental model → prediction → construction → debugging → design judgment → transfer
```

Do not mistake API recognition for programming ability.

## Separate layers

Beginners frequently merge systems. Distinguish when relevant:

```text
language
runtime / host
framework
library
build tool
operating system
network / external service
```

Example:

- `Promise` semantics: JavaScript language/runtime behavior.
- `fetch`: host-provided Web API.
- React state behavior: framework behavior.
- Vite HMR: tooling behavior.

## Minimal executable examples

The first example should be mentally executable. Prefer 5–15 lines and remove unrelated setup.

## Execution traces

For temporal or stateful behavior, show the sequence explicitly.

```text
1. create value/object
2. evaluate expression
3. call function
4. determine state/binding
5. run body
6. produce result
```

## One-change comparisons

Keep examples nearly identical when teaching distinctions.

## Causality over mnemonics

Use mnemonics only when their valid scope is clear. Prefer a causal model that can explain nearby variations.

## Debugging as teaching

When debugging learner code:

1. reconstruct the learner's expected model;
2. locate where actual behavior diverges;
3. identify the governing concept;
4. make the smallest fix;
5. explain why it works;
6. test a nearby failure mode.

Do not rewrite the whole program unless necessary.

## Documentation bridge

After intuition is established, connect the learner to vocabulary used in official documentation and specifications.

## Framework learning

Prefer:

```text
problem feature solves
→ minimal usage
→ data/control flow
→ lifecycle/reactivity implications
→ common mistake
→ composition with nearby features
```

## Architecture topics

Start with request/data/control flow before naming layers.

## Assessment

Useful evidence:

- predict behavior;
- debug a representative bug;
- implement a tiny feature from scratch;
- explain a design choice;
- transfer the concept to unfamiliar code.

Avoid obscure interview puzzles as foundational assessment.
