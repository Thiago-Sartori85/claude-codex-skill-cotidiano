# Claude & Codex — Skills do Cotidiano

[Português](README.md) | [English](README.en.md)

Coleção pública de Agent Skills para OpenAI Codex e Claude Code, mantida por [@Thiago-Sartori85](https://github.com/Thiago-Sartori85).

## Skill disponível
- **[skill-juridica](skill-juridica/SKILL.md)** — análise preparatória de Direito de Família, patrimônio, pacto antenupcial, partilha, herança, testamento, usufruto e sucessões no Brasil. Não substitui advogado habilitado.

## Estrutura

```text
claude-codex-skill-cotidiano/
├── README.md
├── README.en.md
├── SECURITY.md
├── .github/CODEOWNERS
├── skill-juridica/
│   ├── SKILL.md
│   ├── references/fontes-oficiais.md
│   └── templates/checklist-advogado.md
├── .agents/skills/skill-juridica/SKILL.md
└── .claude/skills/skill-juridica/SKILL.md
```

A pasta `skill-juridica` é a fonte principal. As pastas ocultas contêm pontos de entrada de descoberta para Codex e Claude Code, que direcionam à fonte principal.

## Usar

```bash
git clone https://github.com/Thiago-Sartori85/claude-codex-skill-cotidiano.git
cd claude-codex-skill-cotidiano
```

Abra o Codex ou Claude Code na raiz do repositório clonado. Para uso global em outros projetos, copie **a pasta inteira** `skill-juridica/` para `~/.agents/skills/skill-juridica/` (Codex) ou `~/.claude/skills/skill-juridica/` (Claude Code). Instalações locais não sincronizam automaticamente com a nuvem.

## Política
- Somente o mantenedor escolhe o que integrar na `main`; forks são permitidos e não alteram a origem.
- Não inclua segredos, dados pessoais, patrimônios individuais ou projetos privados.
- Revise qualquer PR antes de integração. `CODEOWNERS` não substitui proteção de branch.
- Configure regras de proteção de `main` em **Settings → Rules → Rulesets**. [Detalhes](SECURITY.md).
- Esta versão contém apenas instruções Markdown, sem scripts executáveis.

**Nota:** nenhum material ou referência a projetos privados foi publicado.
