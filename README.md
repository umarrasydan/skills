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
| [`deslop`](skills/deslop/SKILL.md) | Removes AI-generated slop from the branch diff. From Cursor, unchanged. |
| [`thermo-nuclear-code-quality-review`](skills/thermo-nuclear-code-quality-review/SKILL.md) | A strict maintainability review of the branch diff. From Cursor, with one change. |

### ship-pr

Start it yourself with `/ship-pr` or `$ship-pr`. Agents do not start it on their own, since it pushes code and replies on GitHub. It needs the GitHub CLI (`gh`), signed in, and the two skills below. Installing the whole repo includes them.

### deslop and thermo

Both come from Cursor's [cursor-team-kit](https://github.com/cursor/plugins/tree/main/cursor-team-kit) under the MIT license, copied at [`679fdaf`](https://github.com/cursor/plugins/tree/679fdaf2c180df94d99b6e3dd862460082b73af6/cursor-team-kit/skills). Each folder keeps Cursor's `LICENSE`.

Cursor's thermo sets `disable-model-invocation: true`, so only you can start it. Claude Code then refuses to start it from inside `ship-pr`. This copy drops that line. As a side effect, agents can also start thermo on their own when you ask for a harsh code-quality review, and its description loads in every session. If you already have Cursor's version installed, `ship-pr` asks you to run thermo yourself and continues once the review is in.

## License

[MIT](LICENSE). The `deslop` and `thermo-nuclear-code-quality-review` folders keep Cursor's MIT license.
