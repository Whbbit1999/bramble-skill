# Technical Teaching Rules

## Separate layers

Beginners frequently merge several systems into one model. Distinguish when relevant:

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
- `fetch`: host-provided Web API, not core JavaScript syntax.
- React state behavior: framework behavior.
- Vite HMR: tooling behavior.

## Minimal executable examples

The first example should be mentally executable.

Prefer:

```js
const add = (a, b) => a + b
console.log(add(1, 2))
```

before a framework component containing unrelated concepts.

## Execution traces

When behavior is temporal or stateful, show the sequence.

```text
1. create object
2. read property
3. call function
4. determine binding
5. evaluate body
6. produce result
```

## One-change comparisons

Keep examples almost identical when teaching a distinction.

Good:

```js
user.sayName()
```

vs

```js
const fn = user.sayName
fn()
```

The changed call site becomes visible.

## Causality over mnemonics

Avoid rules that only predict one common case.

Use a simple rule only if it is marked with its scope.

Example:

> “For a normal method call like `obj.fn()`, using the object before the dot is a useful first model. Later we will see cases where `call`, `bind`, constructors, or arrow functions change the picture.”

## Debugging as teaching

When debugging learner code:

1. reproduce the learner's expected model;
2. locate where actual behavior diverges;
3. identify the governing concept;
4. make the smallest fix;
5. explain why it works;
6. optionally show a nearby failure mode.

Do not rewrite the whole program unless necessary.

## Documentation bridge

After the learner has intuition, connect it to the vocabulary used in official documentation.

This helps them become independent searchers and readers.

Useful pattern:

> “刚才我们说的‘函数记住创建时外部变量’，文档里通常会看到 closure 和 lexical environment 这两个词。”

## Version-sensitive behavior

If behavior depends on language/runtime/framework versions, verify current facts when tools are available and the version materially matters.

Do not distract beginner explanations with compatibility matrices unless relevant.

## Framework learning

Teach framework features in this order when possible:

```text
problem the framework feature solves
→ minimal feature usage
→ data/control flow
→ lifecycle or reactivity implications
→ common mistake
→ composition with nearby features
```

Do not teach API memorization without the underlying data/control model.

## Architecture topics

For backend, systems, APIs, databases, and architecture:

start with request/data flow.

Example:

```text
client
  ↓ request
API handler
  ↓ validation
service logic
  ↓ query
database
  ↑ result
response
```

Then attach terms and layers.
