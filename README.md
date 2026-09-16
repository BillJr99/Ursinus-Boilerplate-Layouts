# Ursinus-Boilerplate-Layouts

Pull into individual website repository with:  
git submodule add <this url> _layouts  

Update with:  
git submodule update --init --recursive  
git submodule update --remote --merge            
(automate with a workflow; use https link in .gitmodules file)  
---

## The `textbook` layout

`textbook.html` is a long-form reading layout for tutorials, walkthroughs and textbook-style
chapters. It keeps the site theme (the grey Slate page, Lexend Deca, the existing link and code
colours) and adds a set of flow constructs you can use directly from Markdown.

It is deliberately standalone: it does **not** read any of the `assignment.html` fields. Write the
walkthrough as its own page and link to it from the assignment.

```yaml
---
layout: textbook
title: "Recursion and the Call Stack"
---
```

That is the whole minimum. Everything below is optional.

### Callout boxes

Write a blockquote, then one attribute list line. No HTML, no JavaScript required.

```markdown
> A recursive function must have a base case, or it will never terminate.
{: .tb-warning}
```

| Class | Label | Colour |
|---|---|---|
| `.tb-key` | Key Idea | green |
| `.tb-tip` | Tip | green |
| `.tb-note` | Note | slate |
| `.tb-definition` | Definition | teal |
| `.tb-example` | Example | blue |
| `.tb-intuition` | Intuition | purple |
| `.tb-practice` | In Practice | amber |
| `.tb-exercise` | Exercise *n* (auto-numbered) | indigo |
| `.tb-ethics` | Ethics and AI Literacy | plum |
| `.tb-warning` | Warning | red |
| `.tb-pitfall` | Common Pitfall | deep red |

Give a box its own heading with `data-title`:

```markdown
> Writing `if n == 1` instead of `if n <= 1` looks harmless until someone calls `factorial(0)`.
{: .tb-pitfall data-title="Off-by-one in the base case"}
```

GitHub's alert syntax also works, and maps onto the same boxes:

```markdown
> [!WARNING]
> This does not hang like an infinite loop. It exhausts the stack and crashes.
```

`[!NOTE]`, `[!TIP]`, `[!IMPORTANT]`, `[!WARNING]`, `[!CAUTION]`, plus `[!KEY]`, `[!DEFINITION]`,
`[!EXAMPLE]`, `[!INTUITION]`, `[!PRACTICE]`, `[!PITFALL]`, `[!EXERCISE]` and `[!ETHICS]`.

### Sidenotes

A sidenote sits in the margin beside the paragraph that introduces it, and is numbered
automatically, so notes renumber themselves when you reorder prose. On narrower screens they become
indented inset notes.

```markdown
The base case is what makes it terminate.<span class="tb-sn">Some functions need two base
cases.</span>
```

Use `class="tb-mn"` for an unnumbered margin note.

### Figures

```markdown
<figure class="tb-fig">
  <img src="callstack.png" alt="Four stacked activation records">
  <figcaption>Evaluating <code>factorial(4)</code>.</figcaption>
</figure>
```

The caption is prefixed `Figure 3.1 — ` automatically. Add `class="tb-fig tb-full"` to let a wide
figure or table break out past the prose column.

### Exercises and solutions

```markdown
> Trace `factorial(4)` by hand.
{: .tb-exercise}

<details class="tb-solution" markdown="1">
<summary>Show solution</summary>

Four records are created, and three multiplications happen on the way out.

</details>
```

`markdown="1"` is required for Markdown inside the `<details>`.

### Drop cap

```markdown
A function that calls itself sounds like a trick. It is neither.
{: .tb-lede}
```

### Optional front matter

```yaml
info:
  chapter: 3                     # ghosted numeral, and the 3.1 / 3.2 section prefix
  eyebrow: "Tutorial"            # overrides the "Chapter 3" label
  subtitle: "How a function can be written in terms of itself."
  time: "35 minutes"
  level: "CS 173, Week 4"
  numbering: false               # switch off automatic section numbers
  contents: false                # switch off the contents card

  prereqs:
    - title: "Chapter 2: Functions and Scope"
      link: "../ch02/"
      liapage: true              # prefixes the LiaScript viewer URL, as elsewhere in this repo

  objectives:                    # renders as the opening green box
    - Trace a recursive call by hand.

  keyterms:                      # renders as a Key Terms glossary at the end
    - term: "base case"
      definition: "The branch that returns without recursing."

  prev: "../ch02/"
  prevtitle: "Functions and Scope"
  next: "../ch04/"
  nexttitle: "Sorting and Searching"
```

### What you get for free

Automatic section numbering, a contents card that becomes a floating margin index on wide screens
with scrollspy, a reading-progress bar, hover anchor links on every heading, a copy button on every
code block, syntax colouring for fenced code, and a print stylesheet that drops the page furniture
and keeps the boxes intact.

### Tuning

Widths are layout-level front matter in `textbook.html`: `contentwidth` (the band, default
`1000px`), `textmeasure` (the prose column, `680px`), `railwidth`, `railgap`, `raildrop` (the width
below which the margin rail collapses) and `tocfloat` (the width above which the contents float).

`textbook-example.md` is a complete sample chapter exercising every construct above.
