# create-skill

Turn a workflow into an [Agent Skill](https://agentskills.io) that lives in its own GitHub repo, can be installed with
one command, and is reviewed by you before it goes public. This skill tells your agent how to write a short, well-triggered
`SKILL.md`, lay out the repo, write the README, and publish and install the result.

It works in Claude Code, Cursor and any other agent that loads skills.

## How it works

| Step | What happens |
|---|---|
| **Write `SKILL.md`** | Frontmatter whose description says *what* and *when*; numbered steps; a Rules list. Kept short on purpose |
| **Write the README** | What it is, how it works, install (`npx skills add`, then by hand), contributing, license |
| **Review** | The agent shows you the content and waits for your approval |
| **Publish and install** | After your go-ahead: create the public repo, then install it globally with `npx skills add … -g` |

Resulting repo layout:

```
<name>-skill/
├── README.md
├── LICENSE
├── .gitignore
└── skills/<name>/
    ├── SKILL.md
    └── templates/ scripts/ agents/   (optional)
```

`templates/` holds a starter `SKILL.md` and `README.md`.

## Install

### Any agent: the `skills` CLI

```bash
npx skills add illescasDaniel/create-skill
```

It asks which agents to install for and whether to install into the current project or globally. Non-interactive:
`npx skills add illescasDaniel/create-skill -a claude-code -y`, plus `-g` for global.

### By hand

Copy the `skills/create-skill` folder into your agent's skills directory.

| | Project (shared with your team) | All your projects |
|---|---|---|
| Claude Code | `.claude/skills/create-skill/` | `~/.claude/skills/create-skill/` |
| Cursor | `.cursor/skills/create-skill/` | `~/.cursor/skills/create-skill/` |

## Contributing

Issues and pull requests are welcome. The skill is deliberately short: if a change makes `SKILL.md` longer, it should
earn it.

## License

MIT, see [LICENSE](LICENSE).
