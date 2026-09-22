# Worked Example: Teaching the Derivative to a Beginner

This example demonstrates the mathematics adapter. It is not a script to repeat verbatim.

## Goal

The learner should understand derivative as **local rate of change / local slope** before relying on differentiation rules.

## 1. Start with a concrete change

Suppose a car's distance after `t` seconds is:

```text
s(t) = t²
```

Ask:

> From 2s to 3s, how much does distance change? What is the average change per second?

The learner can compute:

```text
(9 - 4) / (3 - 2) = 5
```

## 2. Shrink the interval

Compare average rate from `t=2` to:

```text
3
2.5
2.1
2.01
```

Ask what value the rate seems to approach.

## 3. Attach the geometric picture

Average rate corresponds to a secant slope. As the second point approaches the first, the secant approaches the tangent.

Now the learner has a reason for the “instantaneous” idea.

## 4. State the reusable idea

> A derivative tells us how fast the output is changing *right around a point*.

Only now attach formal notation such as `f'(x)` and, if appropriate, the limit definition.

## 5. Test a controlled variation

Ask:

> If a graph is increasing but getting flatter, should its derivative be positive or negative? Should its magnitude be growing or shrinking?

This checks the model without requiring symbolic differentiation.

## 6. Later add procedure

Once the concept is stable, teach rules such as:

```text
(x²)' = 2x
```

Then connect the rule back to the meaning: at `x=2`, slope/rate is `4`.

## 7. Mastery check

Use at least two kinds of evidence:

- estimate derivative sign/magnitude from a graph;
- differentiate a simple function and interpret the result;
- explain why “derivative = formula manipulation” is incomplete.
