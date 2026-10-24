# Skill Packs

**🇧🇷 Português** · [🇺🇸 English](README.en.md)

Coleção pública, versionada e extensível de **Agent Skills** para **OpenAI Codex** e **Claude Code**. Cada skill encapsula um fluxo de trabalho especializado, com instruções, referências e checklists reutilizáveis.

> **Segurança:** o conteúdo publicado é genérico e não deve conter dados pessoais, credenciais, segredos comerciais ou documentos de clientes. Apenas o mantenedor controla as alterações aceitas na branch `main`. Forks não modificam o repositório original.

## Skills disponíveis

| Skill | Área | O que faz | Idioma |
| --- | --- | --- | --- |
| [`direito-familia-patrimonio-sucessoes`](skills/direito-familia-patrimonio-sucessoes/SKILL.md) | Direito brasileiro | Apoia análise de regimes de bens, pacto antenupcial, partilha, imóveis, participações societárias, testamentos, herança e usufruto, com checagem de normas e riscos. **Não substitui advogado.** | PT-BR |

O catálogo cresce conforme novas skills próprias forem revisadas e publicadas. Ideias futuras: revisão de contratos SaaS, adaptação de currículo para ATS, engenharia full-stack com IA e marketing educacional.

## Estrutura do repositório

```text
skill-packs/
├── README.md                       # Português
├── README.en.md                    # English
├── CONTRIBUTING.md                 # Política de contribuições
├── SECURITY.md                     # Segurança e publicação responsável
├── LICENSE
├── .gitignore
├── .github/
│   ├── CODEOWNERS
│   └── PULL_REQUEST_TEMPLATE.md
├── skills/                          # Fonte canônica das skills
│   ├── README.md                    # Como criar e revisar skills
│   └── direito-familia-patrimonio-sucessoes/
│       ├── SKILL.md
│       ├── references/
│       │   └── fontes-oficiais.md
│       └── templates/
│           └── checklist-advogado.md
├── .agents/skills/                  # Pontos de entrada para Codex
│   └── direito-familia-patrimonio-sucessoes/SKILL.md
└── .claude/skills/                  # Pontos de entrada para Claude Code
    └── direito-familia-patrimonio-sucessoes/SKILL.md
```

A pasta `skills/` contém a **fonte única** das instruções. As pastas `.agents/skills/` e `.claude/skills/` contêm pontos de entrada que encaminham o agente para a fonte principal. **Mantenha o repositório completo** ao usá-los.

## Como utilizar

### Codex

```bash
git clone https://github.com/Thiago-Sartori85/skill-packs.git
cd skill-packs
```

Abra o Codex **na raiz deste repositório**. Ele poderá descobrir os pontos de entrada em `.agents/skills/`. Para disponibilizar uma skill globalmente, copie a **pasta completa da fonte canônica** de `skills/<nome>/` para `~/.agents/skills/<nome>/`, em vez de copiar apenas o ponto de entrada.

### Claude Code

Abra o Claude Code na raiz do repositório clonado. As skills ficam visíveis em `.claude/skills/`. Para instalação pessoal, copie a pasta completa de `skills/<nome>/` para `~/.claude/skills/<nome>/`.

### GitHub e Codex Cloud

Quando o agente trabalhar com um **checkout deste repositório** que inclua as pastas de integração, poderá ler a skill correspondente. Isso não implica instalação global nem sincronização automática com outros repositórios.

## Modelo para novas skills

Crie `skills/<nome>/SKILL.md` com frontmatter YAML contendo `name` e `description`. Adicione referências e templates conforme a necessidade. Crie também um ponto de entrada em **cada** uma das pastas de integração, atualize `README.md` e `README.en.md`, e revise as diretrizes de [segurança](SECURITY.md).

## Governança

- **Forks:** permitidos; não concedem acesso de escrita à `main` original.
- **Pull requests:** propostas externas podem ser avaliadas; nenhum merge automático.
- **Aprovação e publicação:** somente o mantenedor decide o que integrar.
- **Execução de código:** nenhuma skill deve executar scripts ou instalar dependências sem revisão explícita.
- **Licença:** [MIT](LICENSE); permite reutilização e modificações em forks, preservando os avisos de copyright.

**Mantenedor:** [@Thiago-Sartori85](https://github.com/Thiago-Sartori85)
