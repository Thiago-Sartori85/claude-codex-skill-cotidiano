# Guia de skills — Claude e Codex para features e aplicações

Versão 1.0 — edição pública — 17/11/2026

Objetivo: organizar o trabalho em três blocos: arquitetura; especificações e planos; implementação, validação e segurança.

Este documento apresenta um roteiro reutilizável de desenvolvimento. Os nomes das skills são referências do catálogo do projeto, não garantia de instalação ou comandos nativos de cada ferramenta. Antes de executar, localizar e ler o SKILL.md correspondente. Se uma skill estiver ausente, informar e propor uma equivalente pelo objetivo, sem afirmar que foi executada.

Os exemplos de features são ilustrativos. Adapte nomes, sequência e regras ao seu projeto e confirme as decisões nos documentos de referência antes de implementar.

## Bloco 1 — Desenhar e fechar a arquitetura

Executar antes do código de produção. Definir uma arquitetura base uma vez e revisitar apenas decisões afetadas por mudanças materiais. Protótipos exploratórios devem ser identificados como descartáveis.

1. **Recuperar contexto e memória.**
   - Skill: `memoria-do-app` — consultar.
   - Ler arquitetura existente, regras de negócio, decisões, limitações e fichas das etapas.
   - Delimitar problema, usuários, resultado esperado, escopo e exclusões.
   - Separar fatos verificados, decisões aprovadas e hipóteses.
   - Saída: contexto consolidado e perguntas bloqueantes.

2. **Conduzir o desenho da solução.**
   - Skill principal: `superpowers:brainstorming`.
   - Comparar alternativas por simplicidade, custo, manutenção, segurança e aderência ao produto.
   - Retomar as decisões pendentes do modelo de dados e dos fluxos, após consultar a documentação do projeto.
   - Saída: arquitetura proposta e registros de decisão arquitetural — ADRs.

3. **Definir as camadas de abstração.**
   - Interface: telas, navegação, formulários, estados e acessibilidade.
   - Entrada/API: contratos, autenticação, validação de entrada e respostas.
   - Aplicação: casos de uso, autorização contextual, transações e orquestração.
   - Domínio: regras de negócio, invariantes e transições de estado.
   - Persistência: consultas, schema, constraints, índices, migrations e RLS.
   - Integrações: adaptadores para e-mail, pagamentos, IA e serviços externos, quando necessários.
   - Processamento assíncrono: jobs, workers, retries e idempotência, quando necessários.
   - Operação: configuração, segredos, logs, métricas, alertas, deploy e recuperação.
   - Para cada camada, registrar entradas, saídas, dependências permitidas e responsabilidade por validações. Camadas lógicas não exigem serviços separados.
   - Saída: mapa de componentes e fluxos principais.

4. **Escolher e justificar a stack.**
   - Skill: `backend-design:boring-by-default`.
   - Verificar primeiro a stack e as versões realmente utilizadas no repositório.
   - Definir frontend, backend, banco, autenticação, armazenamento, testes, observabilidade e hospedagem.
   - Adicionar cache, fila e fornecedores de IA apenas quando houver necessidade demonstrada.
   - Registrar, para cada escolha: necessidade, alternativa, custo operacional, compatibilidade e motivo.
   - Resolver divergências com benchmarks sem adotar ferramentas por imitação.
   - Saída: ficha da stack, distinguindo confirmado de proposto.

5. **Projetar identidade e autorização.**
   - Skill: `backend-design:auth-and-authorization`, dentro do brainstorming.
   - Definir papéis, ações, recursos e escopos: usuário, turma, escola e organização.
   - Registrar onde as checagens ocorrem: API, aplicação, banco/RLS, arquivos e workers.
   - Derivar o escopo de identidade e vínculos verificados, sem confiar apenas no identificador enviado pelo cliente.
   - Definir negação por padrão, menor privilégio, revogação e operações administrativas.
   - Incluir isolamento em caches, exports, buscas, jobs e integrações.
   - Saída: matriz de permissões e cenários positivos e negativos.

6. **Modelar dados e garantias de integridade.**
   - Skill: `backend-design:data-modeling-discipline`.
   - Definir entidades, relações, propriedade dos dados, histórico e ciclo de vida.
   - Registrar unicidade, chaves estrangeiras, constraints e índices.
   - No exemplo Escolas, avaliar chaves compostas para vínculos da mesma escola e definir regras de unicidade e evolução dos identificadores antes da implementação.
   - Separar identificador técnico, matrícula e vínculo acadêmico quando tiverem funções distintas.
   - Saída: modelo de dados e catálogo de invariantes.

7. **Incluir privacidade e conformidade.**
   - Skill de referência: `legal:compliance-check`.
   - Levantar dados, finalidades, perfis de acesso, retenção, exclusão, fornecedores e transferências aplicáveis.
   - Encaminhar a avaliação do tratamento de dados de menores, quando aplicável, após consultar as fontes e os requisitos do projeto.
   - Reaproveitar um relatório de impacto à proteção de dados existente somente após verificar sua versão e aderência aos novos fluxos.
   - Saída: requisitos de privacidade e pendências de validação jurídica. Este roteiro não constitui conclusão jurídica.

8. **Revisar SQL, policies e operação.**
   - Skills: `backend-design:review-migration` e `backend-design:explain-this-query`.
   - Avaliar constraints, índices, joins, filtros, custo das policies e bloqueios durante migrations.
   - Definir objetivos mensuráveis de desempenho, disponibilidade e recuperação.
   - Registrar estratégia de deploy, reversão ou correção progressiva.
   - Saída: arquitetura revisada para orientar as specs.

**Critério de saída:** escopo, stack, camadas, dados, permissões, integrações e riscos documentados; decisões bloqueantes resolvidas e arquitetura validada pelo responsável. Não presumir aprovação de documento ainda não revisado.

## Bloco 2 — Estruturar specs e plans por feature

Repetir para cada feature. A spec define o comportamento e os critérios de aceite; o plan define como implementar e verificar. Evitar um único plano extenso para toda a aplicação.

1. **Recarregar o contexto da etapa.**
   - Skill: `memoria-do-app` — consultar fichas e decisões relevantes.
   - Mapear dependências e interfaces compartilhadas.
   - Saída: contexto da feature com referências às decisões aplicáveis.

2. **Resolver decisões abertas.**
   - Skill: `superpowers:brainstorming`, somente quando necessário.
   - Reabrir arquitetura apenas diante de mudança material, registrando o motivo.
   - Se a adoção de monorepo já estiver decidida e registrada, seguir direto para o plano.

3. **Escrever uma spec por feature.**
   - Identificador estável, problema, usuários, resultado esperado, escopo e exclusões.
   - Fluxo principal, erros, estados vazios, exceções e regras de negócio.
   - Permissões, dados afetados, contratos de API/eventos e efeitos externos.
   - Requisitos de segurança, privacidade, desempenho e observabilidade proporcionais ao risco.
   - Critérios de aceite verificáveis, incluindo casos negativos e concorrência quando pertinente.
   - Dependências, riscos, perguntas abertas e condições de ativação.
   - Saída: `docs/backlog/<iniciativa>/<feature>/spec.md`.

4. **Detalhar os componentes de servidor.**
   - Skill: `backend-design:design`.
   - Aplicar a convites, importação, worker de analytics e demais componentes necessários.
   - Definir contratos, transações, idempotência, erros, retries, limites e recuperação.
   - Saída: desenho técnico vinculado à spec, sem duplicar decisões globais.

5. **Segmentar o plano em tarefas pequenas.**
   - Skill: `superpowers:writing-plans`.
   - Organizar por dependência e entrega verificável.
   - Cada tarefa deve conter objetivo, arquivos/componentes, mudança esperada, testes, comando de verificação e critério de conclusão.
   - Distinguir migrations, contratos, domínio, interfaces, integrações e ativação.
   - Relacionar cada critério de aceite a uma tarefa e a uma evidência esperada.
   - Saída: `docs/backlog/<iniciativa>/<feature>/plan.md`.

6. **Planejar migrations e testes.**
   - Skill: `backend-design:migration-safety`.
   - Definir aplicação, compatibilidade entre versões, backfill, volume, locks e recuperação.
   - Prever banco novo e banco no estado anterior, com dados representativos.
   - Usar PGlite como checagem rápida quando adequado e validar também no PostgreSQL/Supabase compatível com o ambiente real quando houver RLS, papéis, extensões ou comportamento específico.
   - Aprovação no PGlite não comprova, sozinha, isolamento em produção.
   - Saída: plano de migration e matriz de testes.

7. **Organizar as features na sequência do roteiro.**
   - Ordem: Créditos e consumo → acesso diário → monorepo → base da escola → convites → analytics → relatórios → piloto.
   - Confirmar os escopos propostos abaixo nos documentos do projeto.

### F01 — Créditos e consumo

- Objetivo: controlar débito de créditos e consumo sem duplicidade ou corrida.
- Spec: definir momento do débito, saldo insuficiente, falha, repetição e limites por usuário.
- Extras: `idempotency-and-side-effects` e `performance-and-scaling`.
- Plano: regra transacional, deduplicação, rate limit e registro operacional conforme a stack aprovada.
- Aceite: requisições concorrentes respeitam a regra de saldo; retries não repetem o efeito de negócio; limites são verificáveis.

### F02 — acesso_diario: acesso diário

- Objetivo: formalizar o significado de acesso diário no produto.
- Spec: evento contado, identidade, fuso, virada do dia, repetição e efeito no produto.
- Skills: ciclo padrão; adicionar `data-modeling-discipline` e `idempotency-and-side-effects` se houver unicidade ou efeitos associados.
- Plano: registro, consulta e integração com regras de acesso ou indicadores, conforme definição confirmada.
- Aceite: fronteiras de data, concorrência e acesso indevido testados; não presumir que dia equivale a janela móvel de 24 horas.

### F03 — Monorepo: organização do código

- Objetivo: implementar a estrutura aprovada para o projeto, preservando funcionamento e deploy.
- Spec: aplicações, pacotes, dependências, configuração e pipelines afetados.
- Extra: agente `backend-design:incident-thinker`, se disponível, para analisar rollback de deploy.
- Plano: migração progressiva da estrutura, imports, builds, testes e pipeline.
- Aceite: builds e checks aprovados; deploy em validação funcionando; recuperação documentada.

### F04 — Base da escola: organizações, turmas e vínculos

- Objetivo: estabelecer estrutura escolar e isolamento entre escolas.
- Spec: escola, turma, aluno, professor, coordenação e vínculos; esclarecer múltiplas escolas e mudanças de vínculo.
- Extras: `auth-and-authorization`, `data-modeling-discipline`, `review-migration` e `design:design-system`.
- Plano: schema, constraints, policies, serviços, contratos e painel derivado do visual atual.
- Aceite: permissões por papel e vínculo verificadas; identificadores de outra escola não permitem leitura ou alteração.

### F05 — Convites e importação

- Objetivo: cadastrar ou vincular pessoas por convite e importação controlada.
- Spec: criação, expiração, aceite, revogação, reenvio, destinatário, papel e erros por registro.
- Extras: `idempotency-and-side-effects`, `error-handling-as-design` e `design:ux-copy`.
- Plano: contratos, proteção do token, processamento, e-mail e interface de acompanhamento.
- Aceite: reenvio, webhook duplicado e importação repetida não criam vínculos indevidos; convite não concede escola ou papel diferente do autorizado.

### F06 — Analytics e workers

- Objetivo: produzir indicadores consistentes e recuperáveis.
- Spec: fontes, definição de métricas, período, fuso, atualização, acesso e eventos atrasados.
- Extras: `observability-by-default`, `error-handling-as-design` e `query-discipline`.
- Plano: coleta, worker, agregação, retries, reprocessamento, consultas e alertas.
- Aceite: falhas e reprocessamentos não duplicam resultados; indicadores reconciliam com a fonte; isolamento por escola preservado.

### F07 — Relatórios e visualização

- Objetivo: oferecer informações compreensíveis e autorizadas para cada perfil.
- Spec: filtros, métricas, escopo, exportação, ausência de dados e acessibilidade.
- Extras: `hunt-n-plus-one`, `dataviz`, `design:accessibility-review` e `design:design-critique`.
- Plano: consultas, paginação, gráficos, tabelas, exports e revisão visual.
- Aceite: números conferem com a fonte; consultas atendem às metas definidas; exports e downloads respeitam autorização.

### F08 — Piloto e lançamento

- Objetivo: validar a jornada integrada com grupo controlado e critérios de avanço.
- Spec: participantes, duração, sucesso, suporte, monitoramento, interrupção e recuperação.
- Extras: `ateam-security-audit:security-audit` → `security-fix` → `security-verify`; `legal:compliance-check` final.
- Plano: homologação, auditoria autorizada, correções, reteste, ativação gradual e acompanhamento.
- Aceite: achados bloqueantes corrigidos e verificados; isolamento entre escolas demonstrado; pendências e responsáveis registrados.

**Critério de saída:** spec e plano segmentado, dependências resolvidas, critérios de aceite, estratégia de migration e validação; nenhuma decisão bloqueante escondida nas tarefas.

## Bloco 3 — Implementar, testar, revisar segurança e concluir

Repetir por feature planejada. A criação deste guia não executa o processo nem autoriza, por si só, mudanças no produto.

1. **Preparar a área de trabalho.**
   - Skill: `superpowers:using-git-worktrees`, quando aplicável.
   - Verificar branch, alterações existentes e instruções do repositório.
   - No fluxo solicitado, usar worktree com o OK do responsável; não pedir novamente se a ação já foi autorizada.
   - Saída: ambiente isolado e referência da base utilizada.

2. **Executar o plano por tarefas.**
   - Skill: `superpowers:subagent-driven-development`, quando disponível e autorizado.
   - Delegar tarefas com limites e contratos claros, evitando alterações concorrentes nos mesmos arquivos.
   - Se subagentes não estiverem disponíveis ou autorizados, executar sequencialmente mantendo revisões e evidências.
   - Integrar cada tarefa à spec e registrar desvios antes de ampliar escopo.

3. **Aplicar TDD às mudanças comportamentais.**
   - Skill: `superpowers:test-driven-development`.
   - Criar teste que demonstre o comportamento ou reproduza o defeito; confirmar falha pelo motivo esperado; implementar e refatorar com testes passando.
   - Priorizar regras de negócio, autorização, concorrência e integrações críticas.
   - Adequar a validação de alterações documentais ou puramente visuais; evitar testes que apenas espelham a implementação.
   - Saída: implementação e testes significativos.

4. **Aplicar e verificar migrations.**
   - Skill: `backend-design:migration-safety`.
   - Aplicar em teste, verificar constraints, dados existentes e compatibilidade da aplicação.
   - Testar RLS com papéis representativos e sessões sem autenticação quando pertinente.
   - Validar recuperação prevista, sem presumir que toda migration permite reversão sem perda.
   - Saída: evidências no ambiente compatível com produção.

5. **Validar funcionalidade e integração.**
   - Unitários: regras e transições relevantes.
   - Integração: banco, API, workers e efeitos externos simulados ou controlados.
   - Contratos: compatibilidade das interfaces alteradas.
   - Jornada: fluxo principal, erros, estados vazios e recuperação.
   - Concorrência: débitos, convites, importações e reprocessamento quando aplicáveis.
   - Interface: responsividade, teclado, foco, mensagens e apresentação dos dados.
   - Desempenho: volume representativo, consultas, N+1 e metas da spec.
   - Saída: critérios de aceite associados aos resultados observados.

6. **Revisar código e segurança do diff.**
   - Skills: `code-review` e `security-review`.
   - Revisar cada etapa; intensificar segurança quando houver acesso, RLS, integrações ou dados sensíveis.
   - Conferir autorização no servidor, exposição de dados, validação, segredos, logs, consultas, uploads e downloads conforme a superfície afetada.
   - Testar leitura e escrita entre escolas, escalada de privilégio, vínculo revogado e isolamento de caches, jobs e exports.
   - Registrar achado, severidade, reprodução, correção e reteste.
   - Saída: revisão do diff final e pendências classificadas; checklist preenchido não prova ausência de vulnerabilidades.

7. **Auditar antes do piloto.**
   - Fluxo: `ateam-security-audit:security-audit` → `security-fix` → `security-verify`.
   - Usar escopo autorizado e dados de teste; focar vazamento entre escolas.
   - Corrigir causas e repetir os cenários que demonstraram as falhas.
   - Revisar requisitos de privacidade com `legal:compliance-check`.
   - Saída: achados, correções verificadas e riscos residuais com responsável.

8. **Verificar antes de declarar conclusão.**
   - Skill: `superpowers:verification-before-completion`.
   - Executar checks pertinentes sobre a versão final: testes, análise estática, build e exigências do repositório.
   - Registrar comandos, resultados, data, versão/commit quando disponível e limitações do ambiente.
   - Não afirmar que um teste passou sem executá-lo; distinguir implementado de validado.
   - Saída: `docs/backlog/<iniciativa>/<feature>/validation.md`.

9. **Encerrar a branch e preparar a entrega.**
   - Skill: `superpowers:finishing-a-development-branch`.
   - Resumir problema resolvido, comportamento resultante, validação e riscos.
   - Neste fluxo, commit e PR somente quando solicitados. Merge e deploy seguem autorização do projeto.
   - Manter o diff revisável se a publicação não tiver sido solicitada.
   - Saída: trabalho pronto para revisão e, quando autorizado, commit e PR.

10. **Atualizar a memória do app.**
    - Skill: `memoria-do-app` — atualizar.
    - Registrar decisões finais, contratos, migrations, testes, limitações, pendências e links.
    - Distinguir proposto, implementado, validado e publicado.
    - Saída: contexto atualizado para a próxima feature.

**Critério de conclusão:** aceite demonstrado, checks pertinentes aprovados, achados bloqueantes resolvidos, limitações explícitas e memória atualizada. Publicação é um estado separado da conclusão técnica.

### Organização sugerida de documentos

- `docs/architecture/architecture.md`: camadas, componentes, fluxos e stack.
- `docs/architecture/decisions/ADR-001-<decisao>.md`: contexto, alternativas, decisão e consequências.
- `docs/architecture/access-matrix.md`: papéis, ações e escopos.
- `docs/architecture/data-model.md`: entidades e invariantes.
- `docs/backlog/<iniciativa>/<feature>/spec.md`: comportamento e aceite.
- `docs/backlog/<iniciativa>/<feature>/plan.md`: tarefas e dependências.
- `docs/backlog/<iniciativa>/<feature>/validation.md`: evidências e limitações.
- `docs/backlog/<iniciativa>/<feature>/security-review.md`: achados e retestes, quando aplicável.
- No exemplo Escolas, usar `docs/backlog/escolas` como raiz da iniciativa.

### Prompt reutilizável

```text
Use o Guia de Skills para Features e Aplicações neste projeto.
Feature: [ID e nome].
Bloco atual: [1 — arquitetura | 2 — specs e plans | 3 — implementação].
Documentos de referência: [caminhos].

Leia as instruções do repositório, a memória do app e os documentos relevantes.
Verifique as skills disponíveis e leia suas instruções antes de executá-las.
Informe ausências e proponha equivalências sem inventar execução.
Separe fatos, decisões aprovadas, hipóteses e perguntas bloqueantes.
Execute o bloco indicado com os artefatos e critérios de saída deste guia.
Preserve a stack aprovada e justifique qualquer mudança necessária.
No bloco 3, implemente por tarefas e registre evidências funcionais e de segurança.
Respeite autorizações existentes: worktree depende de OK; commit/PR, de solicitação.
Finalize informando o que foi produzido, validado e o que permanece pendente.
```