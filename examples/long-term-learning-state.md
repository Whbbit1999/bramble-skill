# Example: Long-Term Learning State Across Sessions

This example shows how state should evolve without pretending that “covered” means “mastered”.

## Session 1 — JavaScript `this`

The learner correctly predicts:

```js
user.sayName()
```

but incorrectly predicts:

```js
const fn = user.sayName
fn()
```

### Resulting evidence

Record the answers as they were given, not just the verdict:

```json
{
  "topic_id": "js-this-call-site",
  "capability": "transfer",
  "kind": "prediction",
  "result": "failure",
  "learner_answer": "fn() should still be user — the function was made inside user",
  "note": "Infers this from definition site, not call site"
}
```

```text
ordinary-function this / understanding: understood
ordinary-function this / transfer: recognized
stability: fragile
```

Create an error instance:

```text
Assumed the object where a function was defined permanently owns its this.
```

Create a review item for a detached-method variation in roughly one day or next session.

The next lesson should **not** immediately jump deep into `call`, `apply`, and `bind` if the call-site model is still fragile.

## Session 2 — retrieval first

Start with a fresh example rather than re-explaining the rule.

The learner correctly predicts method vs detached calls and explains that ordinary-function `this` depends on how the function is called.

Update:

```text
understanding: applicable
transfer: understood
stability: developing
```

Move the review interval later.

Now the graph frontier may expose:

```text
arrow-function lexical this
explicit binding
callback context loss
```

Choose only one coherent next capability.

## Session 3 — transfer failure

The learner understands ordinary calls but misreads an arrow function inside an object.

Do not downgrade the whole `this` topic.

Create/update a more specific node:

```text
js-arrow-this
```

and a possible error pattern:

```text
Treats arrow functions as if they acquire this from their call site.
```

The dynamic next lesson becomes a **contrast lesson** between ordinary and arrow functions.

## Several weeks later

A delayed mixed problem combines:

```text
method call
+ detached callback
+ arrow function
```

If the learner selects the right rule without being told which concept is under test, that is stronger evidence of durable transfer than recognizing definitions in isolation.

The graph can now mark relevant capabilities as stable/durable and reduce review frequency.
