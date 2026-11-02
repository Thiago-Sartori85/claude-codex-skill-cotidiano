# Claude & Codex — Everyday Skills

[Português](README.md) | [English](README.en.md)

Public Agent Skills collection for OpenAI Codex and Claude Code, maintained by [@Thiago-Sartori85](https://github.com/Thiago-Sartori85).

## Available skill
- **[skill-juridica](skill-juridica/SKILL.md)** — preliminary Brazilian family, matrimonial property, prenuptial agreement, estate and inheritance law research. Does not replace a licensed lawyer.

## Layout
- `skill-juridica/SKILL.md`: canonical skill instructions.
- `skill-juridica/references/`: official source index.
- `skill-juridica/templates/`: lawyer meeting checklist.
- `.agents/skills/skill-juridica/SKILL.md`: Codex discovery entry.
- `.claude/skills/skill-juridica/SKILL.md`: Claude Code discovery entry.

## Usage

```bash
git clone https://github.com/Thiago-Sartori85/claude-codex-skill-cotidiano.git
cd claude-codex-skill-cotidiano
```

Open Codex or Claude Code at the repository root. For personal installations across other repositories, copy the entire `skill-juridica/` folder to `~/.agents/skills/skill-juridica/` for Codex or `~/.claude/skills/skill-juridica/` for Claude Code. Local skills do not sync automatically to cloud environments.

## Governance
Only the maintainer determines what enters `main`. Forks cannot change upstream. No private projects, identifying family data, customer documents or secrets may be published. Review changes and configure GitHub branch rules: `CODEOWNERS` alone is not enforcement. Read [SECURITY.md](SECURITY.md). No executable scripts are included.
