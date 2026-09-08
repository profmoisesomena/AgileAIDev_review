# 🤝 Guia de Contribuição — AgileAIDev

Bem-vindo! Este guia explica como usar e contribuir com o framework.

Este processo é agnóstico de IDE, editor ou agente. Integrações específicas de ferramenta são opcionais e não substituem as regras canônicas do repositório.

---

## Usando o Framework em Seu Projeto

### 1. Instalar o Framework

```bash
# Clone o repositório
git clone https://github.com/seu-usuario/agileaidev.git meu-projeto
cd meu-projeto

# Ou copie os artefatos para um projeto existente
cp -r .agents/ templates/ examples/ /caminho/do/seu/projeto/
mkdir -p /caminho/do/seu/projeto/docs
```

### 2. Inicializar o Projeto

No chat do seu agente de IA, digite:
```
/init-project
```

O agente criará a estrutura de pastas e preparará o ambiente.

### 3. Executar o Primeiro Sprint Planning

Adicione exemplos de design em `examples/figma/` (ZIPs do Figma, imagens, código legado) e execute:
```
/sprint-planning
```

---

## Fluxo de Trabalho Básico

```
/create-user-story  →  /sprint-planning  →  /feature-development  →  /code-review  →  /deploy  →  /sprint-retrospective
```

Consulte `.agents/workflows/` para a lista versionada de workflows operacionais. O `README.md`, quando existir, é documentação humana e não substitui as fontes canônicas.

---

## Referências Canônicas

- `docs/engineering_playbook.md`: visão geral do processo.
- `docs/decisoes_governanca_us_spec_bdd.md`: regra canônica de US, Spec e BDD.
- `docs/git_workflow.md`: branch, commit, PR e merge.
- `docs/context.md`: comandos locais, paths e checks.
- `docs/definition_of_done.md`: critérios finais de conclusão.

---

## Configurar o Template de Commit

```bash
# Ativar o template de commit globalmente para este projeto
git config commit.template .gitmessage
```

---

## Contribuindo com Melhorias no Framework

### Como contribuir

1. Fork o repositório
2. Sincronize a `main`
3. Crie uma branch curta seguindo `docs/git_workflow.md`
4. Faça suas alterações seguindo os padrões do repositório
5. Rode os checks documentados em `docs/context.md`
6. Commit com Conventional Commits e scope
7. Abra um Pull Request usando o template do repositório
8. Aguarde CI verde, aprovação e conversas resolvidas
9. Faça merge via `Squash & Merge`

### O que aceita contribuição

| Área | O que melhorar |
|---|---|
| `templates/` | Novos templates de artefatos ágeis |
| `.agents/workflows/` | Novos workflows ou aprimoramentos dos existentes |
| `.agents/skills/` | Novas personas ou refinamento das existentes |
| `examples/` | Exemplos concretos e preenchidos |
| `docs/` | Documentação do processo |

### Boas práticas para contribuições

- **Templates:** mantenha genéricos, sem referências a projetos específicos
- **Workflows:** prefira instruções curtas, com links para a fonte canônica, evitando boilerplate repetido
- **Skills:** siga o padrão `SKILL.md` com frontmatter YAML + seções `Capability` e `Exemplo`
- **Commits:** use Conventional Commits com scope, por exemplo `feat(api): ...` e `docs(repo): ...`

---

## Dúvidas?

Abra uma issue ou consulte as referências canônicas em `docs/` e `.agents/workflows/`.
