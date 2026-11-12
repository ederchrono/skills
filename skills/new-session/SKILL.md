---
name: new-session
description: Open a new tmux session with a rift worktree on `<user>/<type>/<name>` and a Claude session named `<name>`.
disable-model-invocation: true
argument-hint: "[dashed-name]"
---

Spins up one unit of parallel work: a tmux session, a rift worktree, and a Claude session, all sharing one **name** so each can be found from the others later.

## 1. Collect the inputs

- **user**: the GitHub login, from `gh api user --jq .login`. Stop and report if `gh` is not signed in.
- **name**: take it from the arguments if given; otherwise ask for it in plain text. It must be lowercase kebab-case (`[a-z0-9]+(-[a-z0-9]+)*`); if it isn't, propose the kebab-cased version and confirm.
- **type** and **base**: ask both in one `AskUserQuestion` call.
  - type: `feat`, `fix`, `chore`.
  - base: the repo's default branch (resolve with `git symbolic-ref --short refs/remotes/origin/HEAD`, strip `origin/`) or the current branch (`git branch --show-current`). Label each option with the resolved branch name. If HEAD is detached, offer only the default branch.

Done when user, name, type, and a concrete base branch name are all known.

## 2. Preflight

Run from the current working directory. Stop and report to the user if either check fails:

- `git show-ref --verify --quiet refs/heads/<user>/<type>/<name>` must fail (the branch is free).
- `tmux has-session -t "=<name>"` must fail (the session name is free).

## 3. Launch

```bash
tmux new-session -d -s "<name>" -c "$PWD"
tmux send-keys -t "=<name>:" "rift open <user>/<type>/<name> --base <base> --skip-agent && claude -n <name>" Enter
```

The command goes through `send-keys` because `rift` is a shell function: it only `cd`s into the new worktree inside an interactive shell. `--skip-agent` keeps rift's own agent launch out of the way so `claude -n` can name the session; rift still `cd`s and runs its hooks.

The session stays detached; the user attaches to it from another window.

Done when `tmux has-session -t "=<name>"` succeeds. Report the session name, branch, and base in one line, plus `tmux attach -t <name>`.
