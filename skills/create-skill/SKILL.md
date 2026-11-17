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

## 2. Document, test and evaluate

Write a compact, useful README from `templates/README.md`: icon and honest badges, title and description, how it works, quick start, requirements, evaluation, tests and license. Use a small diagram or infographic when it clarifies the skill; put long examples and manual install in `<details>` or linked docs. Never claim popularity, compatibility, test coverage or results that were not verified. Artwork must match the actual behavior. Limit README infographic height to 350 pixels with an HTML img height attribute, preserving its aspect ratio and meaningful alt text.

If the skill has scripts, add meaningful offline tests using temporary fixtures under the repository's top-level `tests/`, outside the installable skill folder, and a CI workflow where appropriate. Run them and check a scratch installation to verify tests and repo-only assets are not installed. Do not invent tests for instructions alone; explain what remains untested.

When NVIDIA SkillEvaluator is available, run its applicable keyless checks on the actual skill revision. Record tool versions, date, revision, score, findings and limits in `docs/evaluation.md`, including security findings, checks not run and any prior-revision results. Summarize faithfully in the README; use a score graphic only for a measured result and name the evaluated artifact, revision and date; show meaningful submetrics when actually reported. Do not imply the quality score proves agent performance. Live model evaluations may incur costs: obtain permission before paid runs. If a tool is unavailable or a check cannot run, say so instead of inventing a score. The SVG at `templates/evaluation.svg` is an unscored starting point; adapt it only to verified results, with accessible text and separate quality and security results. Place the grade in a distinct colored circle with spacing before the numeric score, and make each filled bar proportional to its measured submetric; leave the unscored template unfilled. Never reuse a component score as a score for the enclosing product.

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
