---
name: stem-cheat-sheet
description: Use when creating exam cheat sheets for college STEM courses from slides, textbooks, notes, homework, quizzes, past exams, practice exams, or review guides, especially when student direction, source grounding, and a strict page limit matter.
---

# STEM Cheat Sheet

## Overview

Build the densest legal reference a student can bring to a STEM exam, calibrated to the specific exam rather than to a generic topic.

**Core thesis:** Past exams outrank slides, slides outrank textbook. Optimize for what is actually tested, not what is merely present in the materials.

**Second thesis:** The student is a source. Their weak topics, confidence, exam strategy, and allowed format determine what deserves space and how it should be presented.

## When to Use

- Producing a cheat sheet for a specific upcoming STEM exam.
- A page limit, paper size, or other format constraint applies.
- The user has provided course materials and (ideally) past or practice exams.
- Output should be a standalone HTML file and, when tools allow, a PDF.

**Do NOT use for:** general topic summaries with no exam, tutoring/teaching content, OCR of handwritten notes, or "study tips" without targeted reference material.

## The Iron Law — Source Grounding

**Every formula, theorem, definition, constant, and procedure on the final sheet MUST be traceable to a source the user provided.**

Content recalled from training data without a provided source goes into the dedicated `⚠ Unverified — confirm before exam` section. Never mix it into the main reference. Never silently fill gaps.

A wrong formula on a cheat sheet is uniquely bad — the student trusts it precisely when verification is impossible.

## The Iron Law — Direction Before Design

**Do not design, generate, or export the cheat sheet until you have either captured the user's Direction Brief or the user explicitly told you to infer and generate.**

Feedback after the final PDF is too late. The mandatory feedback point is before layout and content allocation are locked.

Acceptable direction bypasses: "infer," "just do it," "best guess," "skip feedback," or "generate without asking." If direction is bypassed, add a `direction-inferred` note to the plan or generation assumptions and `#title-bar`.

"Skip plan" bypasses only the plan checkpoint. It does not bypass the Direction Brief unless paired with a direction bypass.

## Source Priority

Rank materials in this order. Always.

1. Exam rules and allowed format
2. Past exams from the same instructor
3. Official practice exams and answer keys
4. Quizzes, graded homework, recitation worksheets, review problems
5. Instructor review guides
6. Lecture slides and notes
7. Instructor formula sheets
8. Textbook chapters
9. Supplemental materials

## Workflow

### Phase 1 — Bounded Intake + Direction Brief (one turn maximum)

If minimum inputs or user direction are missing, ask in ONE consolidated message. If the user bypasses intake, proceed with an assumptions block at the top of the plan.

Do not ask "any preferences?" Ask for decisions that change the sheet:

```text
Before I build this, I need the direction that determines what earns space:
1. What are your weakest / highest-anxiety topics?
2. What should this optimize for? Pick up to 2: formulas, problem-recognition rules, worked mini-examples, conceptual distinctions, derivations/proofs, traps, diagrams, calculator/code patterns.
3. What must be included or excluded?
4. Allowed format: pages/sides/paper size/color/examples?
5. Density: ultra-dense, dense-readable, or moderate?

If you want me to infer all of this from the materials, say "infer and generate."
```

**Minimum viable inputs:**

- `COURSE` — name + level
- `EXAM` — type, date, topic scope
- `ALLOWED_FORMAT` — page count, paper size, sides, color allowed, examples allowed
- `MATERIALS` — files / pastes the user has
- `PAST_EXAMS` — yes / no / proxies-only (quizzes, HW, review packets)
- `DENSITY` — ultra-dense | dense-readable | moderate
- `MATH_RENDERING` — katex-cdn | katex-inline | unicode-only | mathml
- `USER_DIRECTION` — weak topics, priority content types, must-include / must-exclude, example policy, notation preferences

Do NOT ask more than once. Do NOT exceed one intake turn. If the reply is incomplete, continue with explicit assumptions rather than entering an interview loop.

### Phase 2 — Exam Pattern + Direction Analysis

If past exams or proxies exist, extract BEFORE planning:

- Repeated problem types and stem wording cues
- Frequently tested formulas
- Concepts tested together
- Recurring diagrams, circuits, proofs, tables, code patterns
- Common traps and distractors
- Solution-format expectations (units, sig figs, notation)
- Point-heavy vs low-yield topics

Convert into Exam Playbook rules:

- "If problem says X → use Y"
- "If these variables given → solve in this order"
- "Watch for Z confusion"
- "This formula only applies when ..."

**If no past exams or proxies exist:** replace the playbook with a single confidence-limitation note. Do not invent patterns.

Then create a priority ledger before planning:

| Field | Required contents |
|---|---|
| Exam evidence | High / medium / low likelihood from past exams or proxies |
| User direction | Weak topic, must-include, desired format, or no signal |
| Planned treatment | Formula table, trigger rule, mini-example, concept contrast, trap, diagram, or omit |
| Space budget | Large / medium / small |

Resolve conflicts this way:

- Exam evidence controls inclusion and omission.
- User direction controls emphasis, ordering, examples, and rescue notes.
- If the user is weak on a low-evidence topic, include a compact rescue note only if space permits.
- If the user asks for something disallowed by exam rules, exclude it and say why in the plan.

### Phase 3 — Plan, Direction Checkpoint, then STOP

Output a plan and wait for user approval unless the user explicitly bypassed the plan checkpoint.

The plan contains:

- Direction Brief recap: weak topics, top 1-2 optimization goals, must-include / must-exclude, density, example policy
- Page count target and layout (columns, page sections)
- Section list with size estimates and priority labels
- What is included; what is deliberately excluded
- Diagrams to include (one line each)
- `MATH_RENDERING` choice with ONE rendered sample formula
- Assumptions (if intake was skipped)
- Confidence note: "exam-targeted" if past exams provided, else "topic-summary-leaning"

End the plan with a specific approval prompt:

```text
Reply "approve" to generate, or adjust any of these before I build:
1. Add/drop topics
2. Rebalance formulas vs procedures vs examples vs concepts
3. Change weak-topic emphasis or traps
4. Change density/layout/math rendering
```

Do not accept your own plan as approval. Do not treat silence as approval.

### Phase 4 — Generate HTML

Use `templates/skeleton.html` (in this skill directory). Fill every required section ID:

| Section ID | Purpose | Required? |
|---|---|---|
| `#title-bar` | Course, exam, scope, page count, confidence note, direction status | Always |
| `#command-center` | Topic index + problem-type → method lookup | Only for 3+ page sheets |
| `#exam-playbook` | Rules from past-exam analysis (or single note if none) | Always |
| `#formulas` | Formula table with variables, when-to-use, trap | Always |
| `#concepts` | Comparison tables, A vs B, recognition cues | Always |
| `#procedures` | Trigger → minimal steps → final check | Always |
| `#diagrams` | Inline SVG with one-line captions | If useful |
| `#traps` | Common mistakes and distractors | Always |
| `#unverified` | Training-data content with no provided source | Omit if truly empty |

**Page-count layout guidance:**

- **1 page:** formulas, decision rules, compact tables, exam traps. No long prose.
- **2 pages:** formulas, procedures, compact concept summaries, common-mistake boxes, weak-topic rescue notes.
- **3+ pages:** page 1 is the command center (topic index, problem-type → method lookup, top traps, weak-topic rescue, key constants). Later pages cluster by topic.

Content rules:

- Every table row should answer at least one exam-day question: "when do I use this?", "how do I recognize this?", "what mistake will cost points?", or "what is the next step?"
- Prefer trigger rules, compact examples, contrasts, and traps over prose summaries.
- Worked examples must be tiny: setup cue → method → final form/check. No full teaching walkthroughs.
- Put the user's weak topics early unless exam evidence says they are low yield.

### Phase 5 — PDF Export (detection ladder)

Try in order. Stop at first success.

1. `npx --no-install playwright` (if cached locally)
2. `puppeteer` (npm package, headless Chrome)
3. `chromium --headless --print-to-pdf` (or `google-chrome` / `chrome`)
4. `wkhtmltopdf`

**On all-failed:** save the HTML, emit exact Chrome print-dialog settings (paper size, margins, scale, "Background graphics" ON), and state PDF export unavailable. Do NOT claim success without a generated file.

### Phase 6 — Verify (every check is a command, not a vibe)

| Check | Verification |
|---|---|
| Page count ≤ allowed | `pdfinfo cheatsheet.pdf \| grep Pages` |
| All required section IDs present | `grep -c 'id="<id>"' cheatsheet.html` for each |
| Direction captured or explicitly bypassed | Plan or generation assumptions include `Direction Brief` or `direction-inferred`; `#title-bar` contains matching status |
| No raw LaTeX in PDF (unless `mathml` debug) | `pdftotext cheatsheet.pdf - \| grep -E '\\\\frac\|\\\\sum'` returns empty |
| Unverified content quarantined | `⚠` markers appear only inside `#unverified` |
| Exam playbook non-empty (if past exams provided) | `#exam-playbook` contains at least 3 `If X → Y` rules |
| No unresolved placeholders | `grep -E '\\{\\{|TODO|TBD|FIXME' cheatsheet.html` returns empty |

**If actual pages > allowed: REGENERATE denser.** Do not ship an overflowing sheet.

## Math Rendering Decision

| Setting | Use when | Caveats |
|---|---|---|
| `katex-cdn` | External assets allowed; sheet viewed or printed from a browser | Needs internet at print time |
| `katex-inline` | External assets not allowed but full math needed | Adds ~200KB to HTML; slower load |
| `unicode-only` | Simple math only; strict size limits | Σ √ ∫ ∂ ∇ ≤ ≥ ≈ ∞ are fine; `\frac{a}{b}` becomes `a/b` |
| `mathml` | Modern browsers only (Safari, Chrome 109+) | Renders natively but visually plain |

**Default:** `katex-cdn` if external assets allowed, else `unicode-only`.

## Common Mistakes

| Mistake | Fix |
|---|---|
| Treating uploaded files as enough direction | Capture a Direction Brief or mark `direction-inferred` after explicit bypass |
| Asking "any preferences?" | Ask forced decisions that affect allocation: weak topics, top content types, must-include / exclude, density |
| Waiting for feedback until after final PDF | Stop at the plan checkpoint before HTML generation |
| Generating from training data when materials are sparse | Add content to `#unverified`, never to main reference |
| Producing a topic summary despite past exams existing | Build Exam Playbook FIRST; let it drive section sizing |
| Letting weak-topic requests override exam evidence | Include compact rescue notes, but do not crowd out high-yield exam patterns |
| Claiming "clean PDF" without checking page count | Run `pdfinfo`; if over, regenerate |
| Emitting raw `\frac{}{}` in HTML body | Match `MATH_RENDERING`; fall back to `unicode-only` on failure |
| Starting an interview loop | Ask one consolidated direction prompt; then proceed with explicit assumptions |
| Decorative borders, large titles, padded margins | Boxed dark headers only; 8mm page margins; 9pt body |
| Inventing past-exam patterns when none provided | Replace `#exam-playbook` with a confidence-limitation note |

## Red Flags — STOP

- About to plan without Direction Brief or explicit `direction-inferred` bypass
- About to generate HTML before user approval when feedback was not bypassed
- Asking vague preferences instead of forcing allocation decisions
- Saying "tell me what to change" only after final PDF
- About to write a formula you "remember" but the user didn't provide → goes in `#unverified`
- Past exams exist but `#exam-playbook` has no `If X → Y` rules → analysis was skipped
- PDF page count not verified before reporting success
- Intake has gone past one turn without a direction bypass from user
- Decorative element added "to look nicer" → delete; every item earns its space
- Confidence note missing from `#title-bar` when no past exams were provided

## Output Contract

Final deliverable: **one HTML file** plus, when tools allow, **one PDF file**. Nothing else. No separate notes file, no markdown export, no explanation document.
