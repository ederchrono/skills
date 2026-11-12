---
name: tidy
description: Tidy this branch's changes for readability and concision, moving business-rule comments into CONTEXT.md.
disable-model-invocation: true
argument-hint: "[base-ref]"
---

Tidy the code this branch changed so it reads clearly in fewer lines, with identical behavior. Readability and concision outrank cleverness and the author's original structure.

## 1. Scope

- **Base:** `$ARGUMENTS` if given; otherwise the merge-base with the default branch (`git merge-base HEAD origin/HEAD`; when `origin/HEAD` is unset, use `origin/main` or `origin/master`).
- **Scope:** `git diff <base>` (committed and uncommitted) plus untracked files. Code outside it stays as it is, unless a caller or definition must change to stay consistent with a tidy.

Done when you have read every changed hunk in its surrounding code.

## 2. Tidy

Check every changed hunk for:

- **Comments a name could replace.** Rename, or extract a well-named function or constant, then delete the comment.
- **Redundant comments:** ones that restate the code, narrate history ("changed X to Y", ticket notes), or label obvious sections. Delete them.
- **Duplication**, within the diff or against helpers the codebase already has. Reuse the existing helper.
- **Indirection that earns nothing:** single-use wrappers, pass-through variables, checks for states that cannot happen, dead branches.
- **Nesting** a guard clause or early return would flatten.

A comment stays only when it states a business rule or external constraint the code cannot express: the _why_, in one line. When the rule needs more than a line or applies beyond this spot, record it in `CONTEXT.md` (step 3) and keep at most a one-line pointer in the code.

Every tidy preserves behavior: same API, outputs, errors and side effects. List a change that would alter behavior as a proposal in the report instead of making it.

Done when every changed hunk has been checked against every item above.

## 3. CONTEXT.md

Use the nearest `CONTEXT.md` above the tidied code; with none, create one at the package root (the repo root outside a monorepo). Give each rule a heading named for its domain concept, then the rule, why it holds, and the file that enforces it, in as few lines as it takes. Extend an existing entry rather than adding a second one for the same rule.

## 4. Verify

Run the project's own typecheck, lint and tests for the touched code. Find them in `package.json`, the `Makefile`, or `CLAUDE.md`/`AGENTS.md`. Fix whatever the tidy broke.

Done when they pass, or when you can name the checks that could not run and why.

## 5. Report

Leave the changes uncommitted for review, and report:

- Per file, what changed, in a line or two.
- Proposed behavior changes, if any.
- `CONTEXT.md` entries added or extended.
- Check results.
