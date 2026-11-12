# edr — Claude Code skills

My Claude Code skills, packaged as one plugin. Every skill is namespaced under `edr:`, so `babysit-pr` is invoked as `/edr:babysit-pr`.

## Install

In Claude Code (2.1.275 or later):

```
/plugin install edr --marketplace ederchrono/skills
```

From a shell:

```
claude plugin install edr --marketplace ederchrono/skills
```

On older versions, add the marketplace first:

```
/plugin marketplace add ederchrono/skills
/plugin install edr@edr
```

## Updates

The plugin has no pinned version, so every commit pushed to `main` is a new version.

- **Auto-update (recommended):** run `/plugin`, open the **Marketplaces** tab, select `edr`, and choose **Enable auto-update**. Claude Code then pulls new commits in the background. Auto-update is off by default for third-party marketplaces.
- **Auto-update via settings:** add `"autoUpdate": true` next to `source` in the `edr` entry under `extraKnownMarketplaces` in `~/.claude/settings.json`. The install command adds that entry for you.

  ```json
  "extraKnownMarketplaces": {
    "edr": {
      "source": { "source": "github", "repo": "ederchrono/skills" },
      "autoUpdate": true
    }
  }
  ```
- **Manual:** `claude plugin marketplace update edr && claude plugin update edr@edr`

## Skills

| Skill | What it does |
| --- | --- |
| [`/edr:show-me`](skills/show-me/SKILL.md) | Explains the current topic visually: pseudocode, call trees, component and file trees, Mermaid, diffs, or a focused HTML page when a diagram isn't enough. |
| [`/edr:tidy [base-ref]`](skills/tidy/SKILL.md) | Tidies the current branch's changes for readability and concision, with the same behavior. It deletes redundant comments, prefers names over comments, and moves business rules into `CONTEXT.md`. Then it runs your checks and leaves the result uncommitted. Only runs when you invoke it. |
| [`/edr:new-session [name]`](skills/new-session/SKILL.md) | Starts one unit of parallel work: a detached tmux session, a rift worktree on `<github-login>/<feat\|fix\|chore>/<name>`, and a Claude session, all sharing one name. Needs `tmux`, `rift` and a signed-in `gh`. Only runs when you invoke it. |

## Adding a skill

Create `skills/<name>/SKILL.md`. The directory name becomes the skill name, so `skills/foo/` turns into `/edr:foo`. Don't put `edr` in the name, because the plugin adds the prefix. Refer to files bundled with the skill as `${CLAUDE_SKILL_DIR}/…` in `SKILL.md`, never by an absolute path.

Test it locally before pushing:

```
claude --plugin-dir .
claude plugin validate .
```
