---
layout: textbook
title: "Recursion and the Call Stack"

info:
  chapter: 3
  subtitle: "How a function can be written in terms of itself, and what the machine does when it is."
  time: "35 minutes"
  level: "Week 4"

  prereqs:
    - title: "Chapter 2: Functions and Scope"
      link: "../ch02/"

  objectives:
    - Trace a recursive call by hand and draw the resulting call stack.
    - Identify the base case and the recursive case in an unfamiliar function.
    - Explain why a missing base case produces a stack overflow rather than an infinite loop.
    - Convert a simple recursive function into its iterative equivalent.

  keyterms:
    - term: "base case"
      definition: "The branch of a recursive function that returns without calling itself, terminating the recursion."
    - term: "call stack"
      definition: "The region of memory holding one activation record per in-progress function call."
    - term: "activation record"
      definition: "The bookkeeping a single call needs: its arguments, its local variables, and where to return to."

  prev: "../ch02/"
  prevtitle: "Functions and Scope"
  next: "../ch04/"
  nexttitle: "Sorting and Searching"
---

A function that calls itself sounds like a trick, or a mistake. It is neither. Recursion is simply
what happens when a problem contains a smaller copy of itself, and we are willing to let the machine
keep track of the bookkeeping on our behalf.
{: .tb-lede}

Most students meet recursion twice. The first time it feels like sleight of hand, because the
function appears to be finished before it has computed anything. The second time, usually after
drawing a call stack by hand, it becomes the most natural thing in the world.

> A recursive function is one that is defined in terms of itself, together with at least one
> **base case** that is defined without recursion.
{: .tb-definition}

## The shape of a recursive function

Every recursive function has the same two-part skeleton, and it is worth learning to see that
skeleton before worrying about what any particular function computes.

```python
def factorial(n):
    if n <= 1:          # base case: answerable without recursion
        return 1
    return n * factorial(n - 1)   # recursive case: a smaller copy of the same problem
```

The base case is what makes the whole thing terminate.<span class="tb-sn">Some functions need more
than one base case. Fibonacci needs two, at `n = 0` and `n = 1`, because its recursive case reaches
back two steps: with only `n == 1` defined, `fib(n - 2)` would step straight past it.</span> The
recursive case is what makes progress toward it.

> Do not ask "how does the whole recursion work?" Ask only two questions. Does the base case return
> the right answer? And *if* the recursive call returns the right answer for a smaller input, does
> this line combine it correctly? If both hold, the function is correct. This is induction, wearing
> a different hat.
{: .tb-intuition}

### Why the order of the two branches matters

Put the recursive case first and the base case never runs, because control never reaches it.

> [!WARNING]
> A recursive function with no reachable base case does **not** hang like an infinite loop. It
> consumes one stack frame per call until the stack is exhausted, then crashes. In Python you will
> see `RecursionError`; in C or Java, a segmentation fault or `StackOverflowError`.

> Writing `if n == 1` instead of `if n <= 1` looks harmless, and works perfectly for every positive
> input you are likely to test. Then someone calls `factorial(0)` and the function recurses through
> every negative integer until the stack dies.
{: .tb-pitfall data-title="Off-by-one in the base case"}

## What the machine is actually doing

Each call gets its own **activation record**: its arguments, its locals, and the address to return
to. These records stack up, which is why we call it the call stack.

<figure class="tb-fig">
  <img src="callstack.png" alt="Four stacked activation records for factorial(4) down to factorial(1)">
  <figcaption>Evaluating <code>factorial(4)</code>. Each call is suspended, holding its own value of
  <code>n</code>, until the base case returns and the stack unwinds from the bottom up.</figcaption>
</figure>

Nothing multiplies until the base case returns. The four pending multiplications happen on the way
back *out*, in reverse order.

| Call | `n` | Waiting to compute | Returns |
|---|---|---|---|
| `factorial(4)` | 4 | `4 * factorial(3)` | 24 |
| `factorial(3)` | 3 | `3 * factorial(2)` | 6 |
| `factorial(2)` | 2 | `2 * factorial(1)` | 2 |
| `factorial(1)` | 1 | nothing, base case | 1 |

> Every recursive function can be rewritten with an explicit loop and an explicit stack, because
> that is precisely what the runtime is doing for you. Recursion is not slower by magic; it is
> slower when the bookkeeping the runtime does is more than the bookkeeping you would have done.
{: .tb-key}

> CPython does not optimize tail calls, and caps recursion depth at 1000 by default. A recursive
> solution that is elegant on paper may need to be rewritten iteratively for production input sizes.
> Check `sys.getrecursionlimit()` before assuming depth is free.
{: .tb-practice}

> Ask a chatbot to "write a recursive solution" and you will usually get correct, idiomatic code.
> Ask it *why* the base case is `n <= 1` rather than `n == 1` and the answer is often confidently
> wrong. Generated code is a draft to be traced, not an answer to be trusted. Trace it by hand, on
> the edge cases, before you submit it.
{: .tb-ethics}

## Practice

> Trace `factorial(4)` by hand. Write down each activation record as it is created, and each value
> as the stack unwinds. How many multiplications happen, and in what order?
{: .tb-exercise}

<details class="tb-solution" markdown="1">
<summary>Show solution</summary>

Four records are created, for `n = 4, 3, 2, 1`. No multiplication happens on the way down. The base
case returns `1`, and then three multiplications happen on the way out: `2 * 1 = 2`, then
`3 * 2 = 6`, then `4 * 6 = 24`.

</details>

> Rewrite `factorial` iteratively. Then, without running either version, predict which one is faster
> and by roughly how much, and say what you would measure to check.
{: .tb-exercise data-title="From recursion to iteration"}

> Start from the base case and work outward, not from the top down. It is far easier to reason about
> `factorial(1)` than about `factorial(4)`.
{: .tb-tip}

> The word "stack" is doing double duty in this chapter: the call stack is a specific region of
> memory managed by the runtime, while a stack is also the abstract last-in-first-out data structure
> we will build ourselves in Chapter 6. They are related but not the same thing.
{: .tb-note}
