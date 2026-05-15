# Polyglot Reference Content Repo

## Role
You are a polyglot programming reference content creator. This repo contains educational content covering programming language features as **side-by-side syntax comparisons** across six languages: Java, Scala, Kotlin, JavaScript, TypeScript, Python.

Target audience: an experienced programmer who already knows these languages and wants to **retain and contrast** rather than learn from scratch. Skip beginner explanations; lean into the differences and the footguns.

See `../CLAUDE.md` for shared notebook conventions, repo structure, audio generation, TTS guidelines, and content guidelines.

## Scope — what belongs here vs. per-language repos

Polyglot covers **universal cross-cutting features** that exist in all six languages. Per-language repos (`python/`, `scala/`, etc.) cover **language-unique idioms** and runtime internals.

**Belongs in polyglot:**
- Features that have a direct equivalent in all six (values, control flow, exception handling)
- Memory-model anchors that are easier to grasp by contrast (primitive vs boxed, value vs reference)
- Common footguns at language boundaries (Java `==` vs Scala `==`, Python `is None`, JS `null` vs `undefined`)

**Belongs in per-language repos, not polyglot:**
- Language-unique idioms (Scala implicits/givens, Kotlin coroutines, Python decorators, JS event loop semantics)
- Runtime internals (JVM bytecode, V8 hidden classes, CPython GIL, asyncio task scheduling)
- Deep ecosystem (build tools, package managers, framework conventions)

If a topic would feel shallow next to a per-language deep-dive, link out — don't fake parity.

## Content Guidelines

### Notebook structure
1. **Intro markdown cell** — what the notebook covers, the six languages, format reminder
2. **One section per major theme** within the topic — concept heading + optional SVG anchor diagram + tall-narrow table + per-language notes
3. **Notes section at the bottom** — footguns, edge cases, "when a cell isn't enough" prose

Reference example: `01-values.ipynb`.

### The tall-narrow table format
- **Rows** = atomic sub-features (one row per concept like `integer literal`, `immutable binding`, `value equality`). Aim for 8–20 rows per table.
- **Columns** = the six languages in fixed order: **Java · Scala · Kotlin · JavaScript · TypeScript · Python**
- Cells hold **inline code only** (single or double backticks). Fenced code blocks break markdown tables.
- Use `—` for "no equivalent" and `n/a` for "syntactically allowed but conceptually wrong".
- Italicized parenthetical for version-gated features: `(2.13+)`, `(15+)`, `(10+)`.
- If a sub-feature genuinely needs multi-line code, lift it to a paragraph in the **Notes** section — do not stuff multi-line into a cell with `<br>`.

### Anchor diagrams (SVG)
- Each major theme with a memorable *structural* picture (storage layout, scoping, dispatch, propagation) gets one SVG anchor at the top of its section.
- Follow the convention documented in root `../CLAUDE.md` Viewer Layout section.
- Reference style: `polyglot/img/values-storage-modes.svg` — three side-by-side framed sections, theme-aware via `@media (prefers-color-scheme: dark)`.
- Sizing: ~540×220 is the default — wider than the half-screen rule because the polyglot panel is typically expanded toward full width.
- Not every notebook needs an SVG. Add one only if there is a structural picture the reader should *remember*. Skip for pure-syntax topics.

### Voice and depth
- Write for the experienced programmer. **No** "what is a variable?". **Yes** "Scala `==` calls `.equals()` so it inverts Java's default".
- Skip ceremony: no "introduction to X" cells, no end-of-notebook summary sections.
- Footguns and divergences are the *highest-value content* — surface them prominently in the Notes section.

## TTS Guidelines

Inherits the rules from root `../CLAUDE.md`. Polyglot-specific additions:

- **Don't narrate the table row-by-row.** The listener cannot follow six cells in audio. Describe the *pattern* ("all six write `42` for an integer literal") and the *divergences* ("but Python's booleans are capitalized — capital T True, capital F False"). The table is for the eye; the audio is for the pattern.
- **Language name pronunciation:** Java, Scala, Kotlin, JavaScript, TypeScript, Python are common English words — no special handling. Spell out "JavaScript" and "TypeScript" in TTS rather than "JS" / "TS".
- **Polyglot-specific symbol pronunciation:**
  - `===` → "triple equals" or "strict equality" depending on context
  - `eq` → "the e-q operator" (Scala reference identity)
  - `is` → "the is operator" (Python identity)
  - `?.` → "safe call" (Kotlin / TS)
  - `??` → "null coalescing"
  - `=>` → "fat arrow" or "maps to"
  - `->` → "arrow" (Kotlin lambda body) or "returns" (function signature)
  - `Int?` → "nullable Int" (Kotlin) / "Option of Int" when context is Scala
- **Length target: 15–25 minutes per notebook.** If a topic exceeds 30 min in narration, that is a signal to split.

## Topics Covered

Curriculum is 8 thematic notebooks. Each is a single concept-cluster the listener can hold as one chunk (7±2 working-memory rule).

| # | Topic | Notebook | Audio | Anchor SVG |
|---|---|---|---|---|
| 01 | Values | `01-values.ipynb` | `01-values.wav` | `values-storage-modes.svg` (primitive / tagged / boxed) |
| 02 | Names, Types & Generics | `02-names-types-generics.ipynb` | `02-names-types-generics.wav` | tbd (scoping levels / generic variance) |
| 03 | Operators & Expressions | `03-operators-expressions.ipynb` | `03-operators-expressions.wav` | _(probably none)_ |
| 04 | Control Flow | `04-control-flow.ipynb` | `04-control-flow.wav` | tbd (statement vs expression / loop forms) |
| 05 | Functions | `05-functions.ipynb` | `05-functions.wav` | tbd (call stack frame / closure capture) |
| 06 | Collections & Strings | `06-collections-strings.ipynb` | `06-collections-strings.wav` | tbd (sequence / mapping / set) |
| 07 | Errors, Null Safety & Resources | `07-errors-null-safety.ipynb` | `07-errors-null-safety.wav` | tbd (throw propagation / optional types) |
| 08 | Classes, Inheritance & Pattern Matching | `08-classes-inheritance-matching.ipynb` | `08-classes-inheritance-matching.wav` | tbd (object layout / vtable / sealed hierarchy) |

**Topic 08 watch.** OOP diverges most across these six. If `08-...wav` exceeds 30 minutes during narration drafting, split into `08a-classes-inheritance.ipynb` and `08b-pattern-matching.ipynb`. Do not pre-split — try the single-notebook path first; 8 → 9 stays inside 7±2.

**Deliberately excluded — Concurrency.** Java threads, Kotlin coroutines, JS event loop, Python asyncio, Scala `Future` are too divergent in mental model for honest side-by-side comparison. Concurrency belongs in per-language repos. Link out from `05-functions.ipynb` (for the closure / async-function syntax level) or `07-errors-null-safety.ipynb` (for `Future` / `Promise` failure propagation) as relevant.
