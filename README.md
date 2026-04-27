# Skills

A collection of Claude Code skills. Install any skill using the [skills CLI](https://skills.sh):

```
npx skills add ederchrono/skills -s <skill-name>
```

## Table of Contents

- [preflight](#preflight)

---

## preflight

A pre-execution check that runs before writing any code. It surfaces edge cases, user roles, and lifecycle states upfront — so implementation is grounded in what actually needs to be true.

Covers three work types:
- **Bug** — reproduction steps, root cause hypothesis, edge cases, tests first
- **Feature** — state/role matrix, permutations, tests first
- **Exploration** — goal-driven investigation, findings structured around the decision

**Install:**
```
npx skills add ederchrono/skills -s preflight
```
