# <name>

<One paragraph: what the skill does and why it is useful.>

It is an [Agent Skill](https://agentskills.io) (a `SKILL.md` plus <templates / scripts>), so it works in Claude Code,
Codex, Cursor and any other agent that loads skills.

## How it works

<Steps or phases, a table if it helps.>

## Install

### Any agent: the `skills` CLI

```bash
npx skills add <owner>/<repo>
```

It asks which agents to install for and whether to install into the current project or globally. Non-interactive:
`npx skills add <owner>/<repo> -a claude-code -a codex -y`, plus `-g` for global.

### By hand

Copy the `skills/<name>` folder into your agent's skills directory.

| | Project (shared with your team) | All your projects |
|---|---|---|
| Claude Code | `.claude/skills/<name>/` | `~/.claude/skills/<name>/` |
| Codex | `.agents/skills/<name>/` | `~/.agents/skills/<name>/` |
| Cursor | `.cursor/skills/<name>/` | `~/.cursor/skills/<name>/` |

## Contributing

Issues and pull requests are welcome. The skill is deliberately short: if a change makes `SKILL.md` longer, it should
earn it.

## License

MIT, see [LICENSE](LICENSE).
