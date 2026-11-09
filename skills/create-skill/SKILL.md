---
name: create-skill
description: Create a new Agent Skill as its own publishable GitHub repo (SKILL.md, templates, README with install instructions, MIT license), review it with the user, publish it, and install it globally. Use when asked to create, write or package a skill, turn a workflow or instructions into a skill, or publish a skill to GitHub.
---

# Create a skill

A skill is a folder with a `SKILL.md` that an agent loads when its description matches the task. Each skill gets its own
small repo so it can be installed with `npx skills add <owner>/<repo>`. Keep the skill **short**: every line is read on
every use, so a line has to earn its place.

## Layout

```
<name>-skill/            # or just <name>/ ; the repo
├── README.md            # what it is, how it works, install, contributing, license
├── LICENSE              # MIT
├── .gitignore
└── skills/
    └── <name>/
        ├── SKILL.md     # frontmatter + the instructions
        ├── templates/   # optional: files the skill copies or fills in
        ├── scripts/     # optional: small dependency-free helpers
        └── agents/      # optional: subagent definitions the skill relies on
```

Start from `templates/SKILL.md` and `templates/README.md`.

## 1. Write SKILL.md

- **Frontmatter**: `name` (lowercase, hyphens, equals the folder name) and `description`.
- **The description decides whether the skill ever loads.** Say what it does *and* when to use it, with the phrases a
  user would actually say ("create a release", "set up the memory bank"). One or two sentences.
- Body: numbered steps in the order the agent does them, concrete commands, and a short **Rules** list for what it must
  never do (publishing, deleting, spending money, writing secrets) or must ask about first.
- Put long reference material in `templates/` or a separate file the skill links to, not in `SKILL.md`.
- Don't repeat what the agent already knows. Write only what is specific to the user's way of working.
- Scripts must run with the standard library only.

## 2. Write the README

For people, not agents: one paragraph of what and why, how it works (phases or steps, a table if it helps), install
(`npx skills add`, then by hand), how it fits with related skills, Contributing, License. Match the tone of the user's
other skill repos (`sdd-feature-skill`, `memory-bank`).

## 3. Review before publishing

Show the user the `SKILL.md` and README text (or the paths) and **wait for their approval**. Apply their changes first.
A skill is instructions the agent will follow, so the user must read them before it goes public.

## 4. Publish and install

Only after approval, and with the user's go-ahead to create a public repo:

```bash
git init -b main && git add -A && git commit -m "Add <name> skill"
gh repo create <owner>/<repo> --public --source . --push
npx skills add <owner>/<repo> -a claude-code -a codex -g -y     # install globally for Claude Code and Codex
```

Without the `skills` CLI, copy `skills/<name>/` to `~/.claude/skills/<name>/` (Claude Code) and `~/.agents/skills/<name>/`
(Codex). Project-level installs use `.claude/skills/<name>/` and `.agents/skills/<name>/`. Restart the agent session so it loads.

## Rules

- No secrets, personal paths or private project names in a skill that will be public.
- Never create a public repo or push without the user's explicit approval of the content.
- One skill per repo, one job per skill. If it grows a second job, split it.
