---
name: maestro-roteador
description: Use when despachando trabalho para subagentes/workflows, quando o usuário passa um problema e pergunta qual modelo ou esforço usar, ou antes de escolher model/effort em qualquer chamada Agent/Workflow. Gatilhos - "qual modelo", "quanto esforço", "triagem", "faz a triagem disso", pedidos brutos que precisam ser distribuídos.
---

# Maestro Roteador — seleção de modelo e esforço

## Princípio central

Modelo e esforço são **dois eixos independentes que não se convertem um no outro**:

- **Modelo** responde: qual nível de repertório/critério o **pior passo** da tarefa exige?
- **Esforço** responde: quanta **deliberação e verificação** a ambiguidade e o custo do erro pagam?

Esforço compra deliberação, nunca repertório. Modelo compra repertório, nunca verificação. Decida cada eixo com sua própria pergunta — nunca escolha um "meio-termo equilibrado" pra escapar das duas perguntas.

## Procedimento (sempre nesta ordem)

1. **Isole o pior passo.** Da tarefa inteira, qual é o passo que mais exige critério fino (gosto, sutileza, decisão sem gabarito)?
2. **Modelo = o MENOR que resolve bem esse pior passo.**
   - Passo mecânico com gabarito claro (renomear, extrair, formatar, buscar padrão) → `haiku`
   - Implementação/transformação padrão, correta ou errada verificável → `sonnet`
   - Exige julgamento sem gabarito: voz de marca, arquitetura, diagnóstico sem causa óbvia, revisão adversarial → `opus` ou `fable`
3. **Esforço = ambiguidade + custo do erro.** Some os dois sinais:
   - Solução única e óbvia + erro barato/reversível → `low`
   - Alguma escolha entre caminhos OU erro chato de corrigir → `medium`
   - Vários caminhos plausíveis OU erro caro (produção, dado perdido, publicação externa) → `high`+
   - Se você **nomeou um risco de produção/perda na justificativa, o esforço é no mínimo `high`** — "medium com cuidado" não existe.
4. **Volume:** N itens iguais → teste 1 item no menor modelo; se passa, o lote inteiro vai nele.

## Tabela rápida

| Tarefa | Modelo | Esforço | Por quê |
|---|---|---|---|
| Renomear/extrair/formatar em lote | haiku | low | Gabarito claro, erro barato |
| Feature clara, refactor localizado | sonnet | medium | Padrão, verificável |
| Migração de dados com produção em risco | sonnet | high | Capacidade padrão; o risco pede verificação, não inteligência |
| Roteiro/copy com voz de marca | fable | low | Pior passo é gosto (repertório); decisão é direta |
| Bug misterioso, decisão de arquitetura | fable | high | Difícil E ambíguo |

## Armadilhas (vistas em teste real)

| Racionalização | Realidade |
|---|---|
| "sonnet+medium equilibra custo e qualidade" | Meio-termo é fugir das duas perguntas. Responda cada eixo; o resultado quase nunca é o centro da matriz. |
| "roteiro curto não exige raciocínio pesado" → rebaixa o modelo | Confundiu os eixos. Não exigir *deliberação* justifica esforço low — não justifica modelo menor quando o pior passo é critério de marca. Fable+low. |
| "medium é suficiente pra fazer com cuidado" (com produção em risco) | Você mesmo nomeou o risco. Risco nomeado = high. Cuidado é exatamente o que o esforço compra. |
| "vou de modelo maior por garantia" | Se o pior passo está dentro da capacidade do menor, o maior entrega o mesmo cobrando mais. Garantia se compra com esforço/verificação. |
| "modelo pequeno + esforço max sai barato" | Pior combinação: paga deliberação pra quem não tem repertório pra usá-la, e os tokens de raciocínio acumulam. |

## Formato de saída (plano de despacho)

```yaml
tarefa: <resumo>
partes:
  - o_que: <subtarefa>
    pior_passo: <qual e por quê>
    modelo: haiku|sonnet|opus|fable
    esforco: low|medium|high|xhigh|max
    risco: <custo do erro em 1 linha>
turno_principal: <recomendação de /model se valer trocar — a troca é do usuário>
```

## Limite

Para subagentes e workflows a decisão é automática (parâmetros `model`/`effort`). Para o modelo da conversa principal a skill apenas **recomenda** — o usuário troca com `/model`.
