# maestro-roteador

Skill de **triagem de modelo e esforço** para Claude Code: recebe um problema bruto e decide qual modelo (haiku / sonnet / opus / fable) e qual esforço de raciocínio (low → max) usar em cada parte do trabalho, antes de despachar subagentes ou workflows.

## A ideia central

Modelo e esforço são **dois eixos independentes que não se convertem**:

- **Modelo** = qual repertório/critério o **pior passo** da tarefa exige.
- **Esforço** = quanta deliberação/verificação a **ambiguidade + custo do erro** pagam.

Esforço compra deliberação, nunca repertório. Modelo compra repertório, nunca verificação. Por isso combinações "cruzadas" são legítimas — `fable+low` para copy com voz de marca, `sonnet+high` para migração com produção em risco — e o meio-termo genérico (`sonnet+medium` pra tudo) quase nunca é a resposta certa.

## Estrutura

```
skills/
  maestro-roteador/
    SKILL.md    # a skill: princípio, procedimento, tabela rápida, armadilhas, formato de saída
```

## Instalação

```bash
git clone https://github.com/inematds/maestro-roteador.git
ln -sfn "$(pwd)/maestro-roteador/skills/maestro-roteador" ~/.claude/skills/maestro-roteador
```

Ou copie a pasta `skills/maestro-roteador/` para `~/.claude/skills/` (skills de usuário) ou `.claude/skills/` de um projeto.

## Uso

Peça a triagem diretamente — "faz a triagem disso", "qual modelo e esforço pra isso?" — ou apenas peça o trabalho: ao despachar subagentes/workflows, o agente aplica a matriz e distribui cada parte com `model` e `effort` adequados.

Saída no formato de plano de despacho:

```yaml
tarefa: <resumo>
partes:
  - o_que: <subtarefa>
    pior_passo: <qual e por quê>
    modelo: haiku|sonnet|opus|fable
    esforco: low|medium|high|xhigh|max
    risco: <custo do erro em 1 linha>
turno_principal: <recomendação de /model se valer trocar>
```

**Limite:** para subagentes/workflows a decisão é automática; para o modelo da conversa principal a skill apenas recomenda — a troca é do usuário via `/model`.

## Validação (TDD de skill)

A skill foi escrita contra um baseline medido: sem ela, agentes colapsam qualquer tarefa em "sonnet + medium". Com ela, os três cenários de teste foram para o canto certo da matriz:

| Cenário | Sem skill | Com skill |
|---|---|---|
| Renomear 80 arquivos em lote | sonnet + medium | haiku + medium |
| Roteiro com voz de marca | sonnet + medium | fable + low |
| Migração de dados com produção em risco | sonnet + medium | sonnet + high |

## Licença

MIT — projeto de pesquisa/educação INEMA.
