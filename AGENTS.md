# AGENTS.md — Contrato Operacional do Agente

## Ordem de Leitura e Precedência Operacional

Leia nesta ordem, pare quando tiver contexto suficiente:

1. `docs/project_manifest.md` → estado atual + o que ler a seguir
2. `AGENTS.md` (este arquivo) → regras de operação
3. `docs/token_budget.md` (para decidir o nível de leitura)
4. Workflow ativo
5. Skill utilizada
6. Artefatos específicos da tarefa (story, sprint, template, ADR, etc.)

Leia tambem `docs/maturity_model.md` quando a tarefa envolver comportamento observavel, Spec, Contract, BDD, mocks, CI ou escolha de stack.

Se o contexto for suficiente, PARE de ler arquivos adicionais.
Evite reler, a menos que o contexto se perca ou seja explicitamente necessário.

**Não leia `README.md` para operar.** É documentação para humanos.

---

## Regras Globais

1. Verifique `docs/sprints/` antes de criar arquivo de sprint — nunca pule número.
2. Nunca commite ou faça push direto em `main` — use branches curtas `feat/<id>-<slug>`, `fix/<id>-<slug>` ou `refactor/<id>-<slug>`. Para mudanças apenas de documentação/processo, `docs/<id>-<slug>` e `chore/<id>-<slug>` são aceitáveis.
   - Antes de qualquer alteração de código, verifique a branch atual e o estado do worktree.
   - Se a branch atual for `main` ou `develop`, crie uma branch temática antes da primeira edição.
   - Se o worktree já estiver sujo com mudanças não relacionadas, não force branch/commit automaticamente; registre o bloqueio e alinhe o próximo passo com o usuário.
   - Se a task for apenas documental/processual, a mesma checagem continua valendo, com branch `docs/` ou `chore/` quando apropriado.
3. Nunca commite `.env` ou secret files.
4. Nunca modifique `templates/` sem confirmação explícita.
5. Nunca assuma que um arquivo existe — verifique antes.
6. Nunca adicione dependências externas sem propor e obter aprovação.
7. Nunca crie arquivos fora das pastas convencionadas sem confirmação explícita.
    - Estrutura de código padrão:
        - `frontend/` → Interface e lógica de cliente.
        - `backend/` → Servidores e lógica de servidor customizada.
        - `database/` → Migrations, schemas e configurações de banco.
    - Estrutura de artefatos operacionais:
        - `docs/sprints/` → planejamentos e acompanhamento de sprint.
        - `docs/stories/` → user stories refinadas.
        - `docs/tasks/` → task breakdowns e rastreabilidade técnica por US.
        - `docs/retrospectives/` → retrospectivas e melhorias de processo por sprint.
        - `docs/specs/`, `docs/contracts/` e `docs/bdd/` → governança técnica, contratos e comportamento.
8. Nunca pule etapas de um workflow (todos os passos têm propósito).
9. Nunca crie `sprint_planning_N+2.md` pulando uma sprint.
10. SEMPRE inclua uma seção de "Consumo de Contexto (Estimado)" ao final de cada relatório de saída, baseando-se no `docs/token_budget.md`.
11. Em saídas com contexto `NORMAL` ou `HEAVY`, incluir também um bloco de rastreabilidade de arquivos para auditoria da estimativa de tokens.
    - Nome sugerido da seção: `Arquivos Utilizados`
    - Incluir, quando aplicável:
        - `Arquivos lidos para contexto`
        - `Arquivos alterados`
        - `Arquivos criados`
        - `Leitura parcial relevante` (ex: trechos, linhas ou leitura seletiva)
    - Em saídas `LIGHT`, esse bloco é opcional.
12. Quando existir `examples/figma/*.zip` com código exportado e a task envolver UI já presente nesse ZIP, o padrão é buscar **paridade visual máxima** com o artefato do Figma.
    - Se o ZIP trouxer um frontend funcional (ex: `package.json`, `src/`, `routes`, páginas navegáveis), ele deve ser tratado como **fonte primária da UI**.
    - O default deixa de ser “recriar a interface” e passa a ser **`copia_integral_frontend`**.
    - Não use estratégia alternativa de adoção quando o ZIP trouxer um app navegável completo.
    - Placeholders, simplificações visuais, versões “shell only”, redesigns ou qualquer alteração da interface copiada do ZIP só podem acontecer em etapas posteriores com anuência explícita do usuário registrada na sprint, na story ou no PR.
13. Para rastreabilidade tecnica, use o menor Nivel AgileAIDev suficiente:
    - Nivel 0-2 pertencem ao framework base.
    - Nivel 3-4 so entram quando o projeto derivado tiver runner/stack real documentado em `docs/context.md`.
    - Nao crie mocks, fixtures, tipos gerados, Docker ou CI acoplado a stack sem decisao explicita do projeto derivado.

---

## Política de Leitura Mínima

| Tarefa | Leia |
|---|---|
| Qualquer tarefa | `docs/project_manifest.md` |
| Gerar código | `+ .agents/rules/coding-standards.md` |
| Sprint ativa | `+ docs/sprints/sprint_planning_NN.md` |
| Story específica | `+ docs/stories/US-XXX.md` |
| Breakdown técnico específico | `+ docs/tasks/breakdown_US-XXX.md` |
| Spec/Contract/BDD ou gates opcionais | `+ docs/maturity_model.md` |
| Planejamento / decisão transversal | `+ docs/token_budget.md` (modo HEAVY) |

**Não releia o que já foi lido na sessão atual.**

---

## Expansão de Contexto

Expanda o contexto apenas quando:

- a tarefa referenciar artefatos ainda não lidos
- houver conflito entre instruções ou documentos
- o contexto atual não permitir decisão segura
- houver impacto transversal (arquitetura, backlog, sprint)
- o usuário solicitar análise mais profunda

Caso contrário, mantenha o contexto atual.

---

## Fallback

Se o contexto for insuficiente:

1. Verifique `docs/project_manifest.md`
2. Leia apenas o workflow ou story relevante
3. Consulte `docs/token_budget.md` para decidir escalonamento
4. Expanda gradualmente apenas se necessário

Nunca leia o repositório completo sem necessidade.

---

## Skills → ative apenas quando necessário

`architect` · `backend-python` · `frontend-vue` · `react-expert`  
`qa-engineer` · `security-expert` · `story-refiner` · `technical-writer`

Referência completa: `.agents/skills/[nome]/SKILL.md`

---

## Workflows → ativados por slash command

`/init-project` · `/sprint-planning` · `/create-user-story` · `/feature-development`  
`/publish-github-planning` · `/code-review` · `/e2e-test` · `/sprint-retrospective` · `/deploy` · `/integrate-backend`

Referência completa: `.agents/workflows/[nome].md`
