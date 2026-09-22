# Worked Example: Teaching JavaScript `this`

This demonstrates the method; do not mechanically copy the wording.

## Orientation

`this` is a value available inside some JavaScript functions. The important beginner idea is that for a normal function, its value is usually determined by **how the function is called**, not simply where the function was written.

## First phenomenon

```js
const user = {
  name: 'Tom',
  sayName() {
    console.log(this.name)
  }
}

user.sayName()
```

Trace:

```text
user.sayName()
↑ function is called through user

for this normal method call:
this → user

therefore:
this.name → user.name → 'Tom'
```

## Extract rule

For this ordinary method-call form, the call site gives `this` the value `user`.

Do not yet dump all binding categories.

## One-variable variation

```js
const fn = user.sayName
fn()
```

Ask before explaining:

> “这里只改变了调用方式。你觉得 `this` 还会是 `user` 吗？为什么？”

The key repair, if needed:

> Assigning the function to `fn` did not permanently carry `user` with it. The later call is now `fn()`, not `user.sayName()`.

## Attach terminology

Once this intuition is stable, introduce “call site” and later “implicit binding”.

## Next dependency

Only then compare with an arrow function, explaining that an arrow function does not create its own `this` binding.

## Mastery evidence

A learner is ready to move on when they can correctly reason about both:

```js
user.sayName()
```

and

```js
const fn = user.sayName
fn()
```

and explain the difference without relying on a memorized slogan.
