# skills

Agent skills I use day to day. They follow the [Agent Skills](https://agentskills.io) format, so they work in Claude Code, Codex, Cursor, and other agents that read `SKILL.md`.

## Install

```bash
npx skills add umarrasydan/skills
```

The [`skills` CLI](https://github.com/vercel-labs/skills) asks which skills and agents to install to. Add `-g` to install for every project instead of the current one. To install by hand, copy a folder from [`skills/`](skills) into your agent's skills directory, such as `~/.claude/skills`, `~/.codex/skills`, or `~/.agents/skills`.

## Skills

| Skill | What it does |
| --- | --- |
| [`ship-pr`](skills/ship-pr/SKILL.md) | Takes a finished branch through deslop and a thermo-nuclear code quality review, opens the PR, then fixes or answers review comments until none are actionable. It never merges. |

### ship-pr

Start it yourself with `/ship-pr` or `$ship-pr`. Agents do not start it on their own, since it pushes code and replies on GitHub.

It needs the GitHub CLI (`gh`), signed in, and two skills from Cursor's [cursor-team-kit](https://github.com/cursor/plugins/tree/main/cursor-team-kit):

```bash
npx skills add https://github.com/cursor/plugins/tree/main/cursor-team-kit/skills/deslop
npx skills add https://github.com/cursor/plugins/tree/main/cursor-team-kit/skills/thermo-nuclear-code-quality-review
```

Upstream marks `thermo-nuclear-code-quality-review` as user-invoked only. Claude Code therefore won't let `ship-pr` start it, so the agent asks you to run `/thermo-nuclear-code-quality-review` and continues once the review is in. To skip that pause, delete the `disable-model-invocation: true` line from your installed copy of thermo.

## License

[MIT](LICENSE)
