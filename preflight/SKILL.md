---
name: preflight
description: Pre-execution check — surfaces edge cases, roles, and lifecycle states before writing any code. Use this skill whenever a user's first message in a session describes work that feels mid to high effort: building a feature, fixing a bug, refactoring code, adding functionality, investigating an issue, wiring up an integration, changing how something works, or anything that would take more than a few minutes. If the task sounds like real engineering work rather than a quick one-liner, run this. When in doubt, run it.
---

# Preflight

A senior engineer's pre-execution check. Intercepts the developer's request before any code gets written, classifies the work, surfaces the full problem space, and produces tests upfront so the implementation is grounded in what actually needs to be true.

## Opening

Acknowledge what the developer asked for in one sentence, then naturally ask: do they have a ticket link, or want to describe it quickly? Keep it conversational — no process announcements, no "this usually takes X minutes". If they want to skip ahead, just do it.

If the developer wants to skip, proceed with their original request as-is.

## Step 1 — Get context

If a ticket key or URL is provided, fetch it via the Atlassian MCP tool and summarize what you found in 2-3 bullets so the developer can confirm. If not, ask them to describe the goal briefly.

## Step 2 — Classify the work

Based on the ticket or description, identify which type this is:

**Bug** → go to Bug path  
**New feature or UI change** → go to Feature path  
**Refactor or cleanup** → go to Refactor path  
**Code exploration or investigation** → go to Exploration path

---

### Bug path

Ask: reproduction steps, expected vs actual, and whether it's state/role-specific. Then state the root cause hypothesis, list the repro + 2 edge cases, write tests first, fix to make them pass.

---

### Feature path

Ask: what does "done" look like, which roles are involved, and what lifecycle states matter (loading, error, empty, full, recovery). Then produce a state/role matrix, identify which permutations need tests, write those first, then implement.

---

### Refactor path

Ask: what's the motivation (performance, readability, structure, tech debt), what must not change behavior, and what's the blast radius (how many callers / files are affected). Then list invariants to preserve, write characterization tests for any untested paths, refactor to make them pass, and verify behavior is unchanged.

---

### Exploration path

Ask: what decision is this exploration for, and what would a useful answer look like. Investigate with that goal in mind and report findings around the decision — not a code dump.

---

## Output format

After gathering context, present:

1. **Understanding** — 2-3 bullets on what you think the task is
2. **Permutations** — states, roles, or edge cases to cover
3. **Tests to write first** — brief list of test descriptions
4. Ask "Want me to write these tests and then implement?" — wait for a yes before proceeding
