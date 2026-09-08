---
description: Create a new user story with AI assistance
---

# Create User Story Workflow

Este workflow ajuda voce a criar user stories bem definidas com assistencia de IA.

## Quando usar

- Ao adicionar nova funcionalidade ao product backlog
- Durante refinement sessions
- Quando um epic precisa ser quebrado em stories

## Passos

### 1. Criar arquivo da User Story

```bash
cp templates/user_story.md ./docs/stories/[US-XXX]_[titulo-descritivo].md
```

### 2. Definir a Historia

Comece com o contexto basico e peca ajuda da IA:

**Prompt sugerido:**
```text
Ajude-me a criar uma user story para: [descreva a funcionalidade desejada]

Formato esperado:
- Como [tipo de usuario]
- Eu quero [acao]
- Para que [beneficio/valor]
```

### 2.5. Definir Paridade com Figma (quando aplicavel)

Se existir `examples/figma/*.zip` com a mesma tela ou fluxo ja exportado, registre isso na story antes de refina-la.

**Prompt sugerido:**
```text
Analise se a user story [US-XXX] corresponde a uma tela, rota ou componente ja existente no ZIP do Figma.

Responda:
- Existe correspondencia visual direta? [sim/nao]
- Qual arquivo ou tela do ZIP representa essa interface?
- A expectativa desta story e `paridade_exata`, `adaptacao_controlada` ou `estrutural`?
- O modo de implementacao esperado e `copia_integral_frontend`?
- A story evolui a interface ja copiada do ZIP ou apenas o comportamento sobre essa base?
- Se nao for `paridade_exata`, qual a justificativa explicita?
```

> Regra recomendada: quando o ZIP ja trouxer a tela pronta, o default da story deve ser `paridade_exata`.
> Regra obrigatoria: quando o ZIP ja trouxer uma pagina ou rota funcional dentro de um app navegavel completo, o modo da story deve ser `copia_integral_frontend`.

### 3. Criar Criterios de Aceitacao

**Prompt sugerido:**
```text
Para a user story:
"Como [usuario], eu quero [acao], para que [beneficio]"

Gere criterios de aceitacao especificos, testaveis e mensuraveis.
Considere:
- Casos de sucesso
- Casos de erro
- Edge cases
- Validacoes necessarias
- Quando aplicavel, inclua criterio explicito de fidelidade visual com a tela exportada do Figma
- Quando aplicavel, inclua criterio explicito de que a implementacao deve partir da pagina ou rota exportada do ZIP, e nao de recriacao manual
```

### 4. Identificar Consideracoes Tecnicas

**Prompt sugerido:**
```text
Para implementar esta user story, quais sao:
- Componentes ou modulos que serao afetados
- APIs ou integracoes necessarias
- Mudancas no banco de dados
- Consideracoes de performance
- Dependencias tecnicas
- Erros observaveis que precisarao de mensagens especificas
```

### 5. Decidir se a US Exige Spec Governada

Antes de decidir Spec e Contract, escolha o menor Nivel AgileAIDev suficiente usando `docs/maturity_model.md`.

Regra pratica:

- Nivel 0 para processo/templates sem comportamento de produto
- Nivel 1 para comportamento observavel sem boundary tecnico
- Nivel 2 para API, evento, webhook, erro observavel ou schema compartilhado
- Nivel 3 quando houver runner local de contrato/BDD
- Nivel 4 quando os gates ja estiverem automatizados no CI

Antes de seguir para implementacao, avalie:

- A story altera comportamento observavel de API, UI ou regra de negocio?
- Toca seguranca, autenticacao, autorizacao ou rollback sensivel?
- Afeta multiplas areas do codigo e precisa de escopo explicito?

**Prompt sugerido:**
```text
Analise a user story [US-XXX] e responda:
- Nivel AgileAIDev recomendado: [0 | 1 | 2 | 3 | 4]
- Esta US exige Spec governada? [sim/nao]
- Justificativa objetiva
- Se sim: qual seria o caminho da Spec em `docs/specs/<nome>.yaml`?
- Se a Spec for relevante para comportamento observavel: qual seria o BDD em `docs/bdd/<nome>.feature`?
- Se nao: qual texto devemos registrar como "US nao exige Spec"?
```

### 6. Decidir se a US Exige Contract Governado

Antes da implementacao de qualquer boundary, avalie:

- A story altera request, response, parametros, status code ou headers de API?
- Introduz ou altera evento, webhook, job assincrono, upload, download ou integracao?
- Define erros observaveis por clientes ou validacoes relevantes no boundary?

**Prompt sugerido:**
```text
Analise a user story [US-XXX] e responda:
- Esta US exige Contract governado? [sim/nao]
- Justificativa objetiva
- Se sim: qual seria o caminho do Contract em `docs/contracts/<nome>.yaml`?
- Quais schemas ou entidades do modelo de dados precisam ser refletidos no Contract?
- Quais erros observaveis devem ser padronizados com `docs/contracts/error_standard.yaml`?
- O Contract fica apenas documentado (Nivel 2) ou ja existe runner para Nivel 3/4?
- Se nao: qual texto devemos registrar como "US nao exige Contract"?
```

### 7. Criar Plano de Testes

**Prompt sugerido:**
```text
Crie um plano de testes para esta user story incluindo:
- Testes unitarios (cenarios principais)
- Testes de integracao
- Testes de contract quando houver boundary governado
- Testes manuais ou exploratorios
- Dados de teste necessarios
```

Se a US exigir Spec ou Contract relevantes, inclua tambem:

- cenarios BDD em Gherkin
- estrategia para executar o runner de cenarios
- estrategia para validar exemplos e erros do Contract

### 8. Estimar Complexidade

**Prompt sugerido:**
```text
Analise esta user story e sugira:
- Estimativa em story points (escala Fibonacci: 1,2,3,5,8,13)
- Justificativa da estimativa
- Riscos que podem aumentar a estimativa
- Oportunidades de simplificacao
```

### 9. Solicitar Review da Story

**Prompt sugerido:**
```text
Revise esta user story e verifique:
- Os criterios de aceitacao cobrem todos os cenarios?
- Falta alguma consideracao tecnica importante?
- A estimativa esta coerente com a complexidade?
- Ha riscos nao identificados?
- A decisao sobre Spec e Contract faz sentido?
```

### 10. Adicionar ao Product Backlog

Depois de revisar e refinar:

```bash
# Adicione a referencia no docs/product_backlog.md
# Se o backlog ja usar colunas de rastreabilidade, preencha Spec, Contract e BDD quando aplicavel
git add docs/stories/[US-XXX]_*.md docs/product_backlog.md
git commit -m "feat(story): add user story [US-XXX] - [titulo]"
```

> A publicacao de issues no GitHub nao acontece aqui. Depois do backlog e da sprint estarem consolidados, use `/publish-github-planning`.

## Outputs Esperados

- User story bem definida com formato "Como/Quero/Para"
- Criterios de aceitacao claros e testaveis
- Notas tecnicas documentadas
- Decisao registrada sobre "US exige Spec?"
- Decisao registrada sobre "US exige Contract?"
- Nivel AgileAIDev registrado
- Plano de testes definido
- Story estimada
- User story adicionada ao backlog
- Story pronta para ganhar issue no GitHub via `/publish-github-planning`

## Consumo de Contexto (Estimado)

- Modo: [LIGHT | NORMAL | HEAVY]
- Estimativa: ~[N]k tokens (ref: token_budget.md)
- Justificativa/Otimizacao: [Breve descricao]

## Arquivos Utilizados

- Arquivos lidos para contexto: [...]
- Arquivos alterados: [se houver]
- Arquivos criados: [se houver]
- Leitura parcial relevante: [se aplicavel]

## Template de Prompt Completo

Para criar uma user story completa em uma unica interacao:

```text
Crie uma user story completa para: [descricao da funcionalidade]

Inclua:
1. Narrativa no formato "Como/Quero/Para"
2. Contexto adicional
3. Decisao sobre paridade com Figma (`paridade_exata`, `adaptacao_controlada` ou `estrutural`)
4. Decisao sobre modo de adocao do frontend (`copia_integral_frontend`)
5. 5-7 criterios de aceitacao especificos e testaveis
6. Consideracoes tecnicas (componentes, APIs, database)
7. Decisao se a US exige Spec governada, com justificativa
8. Decisao se a US exige Contract governado, com justificativa
9. Caminho esperado de Spec, Contract e BDD quando aplicavel
10. Erros observaveis esperados e como devem ser descritos
11. Plano de testes (unitarios, integracao, contract e manuais)
12. Estimativa em story points com justificativa
13. Riscos e dependencias

Use o template de user_story.md como referencia.
```

## Dicas

- Mantenha user stories pequenas (< 8 pontos)
- Criterios de aceitacao devem ser verificaveis
- Nem toda US exige Spec; quando exigir, registre isso antes de desenvolver
- Nem toda US exige Contract; quando exigir, registre isso antes de alterar o boundary
- Inclua mockups ou wireframes quando relevante
- Revise com stakeholders antes de estimar
- Stories devem ser independentes quando possivel (INVEST principles)
