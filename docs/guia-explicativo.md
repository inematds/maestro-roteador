# Entendendo modelo e esforço — tira-dúvidas e passo a passo

Este texto é para quem leu sobre o maestro-roteador (ou sobre "níveis de esforço" em geral) e ficou com dúvidas. Sem pressa, com analogias, e terminando num passo a passo que você pode seguir hoje.

---

## O problema, em uma frase

Toda vez que você pede algo a uma IA como o Claude Code, duas escolhas acontecem — com você percebendo ou não:

1. **Qual modelo** vai trabalhar (haiku, sonnet, opus, fable — do mais simples ao mais sofisticado);
2. **Quanto esforço de raciocínio** ele vai gastar (low, medium, high, extra high, max).

A maioria das pessoas trata isso como UM botão só de "qualidade": gira tudo pro máximo quando a tarefa parece importante, deixa no meio quando não sabe. **Isso é o erro.** São dois botões diferentes, que compram coisas diferentes.

## Os dois botões, explicados com uma analogia

Imagine que você precisa resolver um problema na sua casa e vai contratar alguém.

- **Modelo = QUEM você contrata.** Trocar uma lâmpada? Qualquer pessoa resolve. Reformar a fachada de um prédio tombado? Você precisa de um arquiteto com repertório — não adianta pedir pro eletricista "se esforçar mais". **Modelo compra repertório: gosto, critério, capacidade de julgar sem gabarito.**
- **Esforço = QUANTO TEMPO de verificação você paga.** Trocar a lâmpada: faz e pronto, ninguém confere três vezes. Mexer no registro geral de água do prédio: você quer que a pessoa desligue, teste, confira, teste de novo — porque o erro alaga todo mundo. **Esforço compra deliberação: pensar antes, comparar caminhos, revisar o próprio trabalho.**

Agora o ponto central: **um botão não substitui o outro.**

- Mandar o eletricista "pensar muito" não o transforma em arquiteto. → *Esforço não compra repertório.*
- Contratar o arquiteto famoso não garante que ele vai conferir o registro de água três vezes. → *Modelo não compra verificação.*

Por isso existem combinações "estranhas" que estão certíssimas:

- **fable + low** → escrever um texto com a voz da sua marca. O difícil é o *gosto* (quem escreve), não a decisão (o caminho é direto). Modelo grande, esforço pequeno.
- **sonnet + high** → migrar dados com produção em risco. A tarefa é *padrão* (qualquer bom executor faz), mas o erro é caro — então você paga *verificação*, não inteligência. Modelo médio, esforço alto.

---

## Dúvidas frequentes

### "Esforço mais alto não é sempre melhor? No pior caso, gasto mais e pronto."

Não — e essa é a dúvida mais importante de todas. **Esforço demais pode PIORAR o resultado, não só encarecer.**

Pense num aluno fazendo prova de múltipla escolha. Ele sabe a resposta: é a B. Mas sobra tempo, ele relê, começa a desconfiar, inventa interpretações... e troca pra D. Errou uma questão que já tinha acertado. Isso é **overthinking**.

Com a IA acontece igual: se você dá um orçamento enorme de "pensamento" pra uma tarefa simples, o modelo já achou a resposta, mas continua re-explorando caminhos já decididos, inventa alternativas, superdimensiona a solução. Num teste real — a mesma tarefa executada em 12 níveis de esforço, em 2 plataformas — os resultados funcionais foram quase idênticos; os níveis altos adicionaram um favicon e umas sombras, por 2 a 5 vezes mais tokens. E em alguns níveis o resultado ficou *pior* que no nível de baixo.

### "Então overthinking é um problema do esforço mesmo, não do modelo?"

Exatamente — **overthinking é o excesso do próprio eixo esforço**, não um defeito do modelo. O eixo esforço tem dois erros simétricos:

- **Esforço de MENOS** numa tarefa ambígua ou arriscada → erra por falta de verificação (ninguém conferiu o registro de água).
- **Esforço de MAIS** numa tarefa simples → erra por ruído (o aluno que trocou a B pela D).

A regra que sai disso: **o esforço certo é o MENOR que cobre o risco.** Acima disso você não está comprando segurança — está comprando ruído.

### "Por que não usar sempre o modelo maior, por garantia?"

Porque se o passo mais difícil da tarefa está dentro da capacidade do modelo menor, o maior entrega **o mesmo resultado cobrando mais**. Renomear 80 arquivos seguindo um padrão é a mesma coisa feita pelo haiku ou pelo fable — só muda o preço. "Garantia" se compra com verificação (esforço), não com modelo.

O contrário também vale: quando o passo difícil é de *gosto* (voz de marca, decisão de arquitetura), o modelo menor não alcança **por mais que se esforce** — aí não é desperdício usar o grande, é necessidade.

### "E qual é a pior combinação possível?"

**Modelo pequeno + esforço máximo.** Você paga horas de deliberação pra quem não tem repertório pra usá-las — e os tokens de raciocínio acumulam. É pagar hora extra pro estagiário refazer dez vezes o que ele não sabe fazer.

### "Quando eu começo em low e vou subindo? Ouvi dizer que é sempre assim."

Quase — a regra da escada ("comece baixo, suba só com evidência") vale **apenas quando o erro é barato e reversível**. Se você pode olhar o resultado, testar e desfazer, então sim: comece no menor esforço plausível e só suba se o resultado vier insuficiente. Isso mata o "vou de high por garantia".

Mas a escada **não vale quando o erro é caro ou irreversível** (produção, dado perdido, publicação externa). Nesses casos, "esperar a evidência de falha" significa esperar o prejuízo acontecer. Aí o esforço é decidido ANTES, pelo risco: risco nomeado = high, no mínimo.

Resumindo: **erro barato → escada. Erro caro → esforço alto de saída, sem escada.**

### "O que é o tal 'empate de gosto'?"

É quando a *estrutura* do trabalho já está garantida (por um template, uma skill detalhada, um formato fixo) e a única diferença entre o modelo médio e o grande é o **acabamento** — a qualidade da escrita, o polish. Nesse caso não existe resposta "certa": existe uma escolha de orçamento, e orçamento é seu, não da máquina. A triagem apresenta o par — `sonnet+low` (correto, funcional) vs. `fable+low` (mesma estrutura, texto superior) — e **você** decide.

Atenção: isso só vale quando o risco é de acabamento. Se o risco é de *correção* (conteúdo errado, dado perdido), não há opção barata — o mínimo que garante a correção é obrigatório.

### "O que é cache, e por que não posso trocar de modelo no meio do trabalho?"

Quando você conversa com a IA, todo o histórico da conversa (contexto) precisa ser reprocessado a cada mensagem. Pra não pagar isso de novo toda vez, existe o **cache**: o começo da conversa fica "guardado" e reler custa ~10% do preço. Só que o cache é **por modelo** — o cache do sonnet não serve pro opus.

Consequência prática: **trocar o modelo da conversa principal no meio de um bloco de trabalho joga fora o cache acumulado.** Num contexto grande, a próxima mensagem pode sair ~12,5× mais cara. Por isso:

- Troque de modelo só em **fronteira de trabalho** — terminou uma fase, fez um resumo (handoff), aí troca.
- **Subagentes não têm esse problema**: cada um tem contexto próprio. Despachar uma parte do trabalho pra um modelo menor via subagente não toca o cache da conversa principal. É o jeito certo de usar modelos diferentes sem pagar a multa.
- **Não mande mensagens vazias** ("ainda está aí?") pra "segurar o cache" — o Claude Code gerencia isso sozinho, e o ping custa mais do que salva.

### "O que é 'harness' e por que dizem que importa mais que o esforço?"

O modelo sozinho é um cérebro num pote: só entra texto e sai texto. Quem dá braços a ele — ler seus arquivos, rodar código, testar, buscar — são as ferramentas, skills e instruções ao redor: o **harness**. A lição prática: antes de girar o botão do esforço pra cima, melhore o pedido. Uma especificação clara do que "pronto" significa, em esforço low, entrega o que o esforço max tenta *adivinhar*. Esforço não compensa pedido vago.

### "Não é mais fácil deixar tudo num meio-termo, tipo sonnet+medium?"

É mais fácil — e erra nas duas direções ao mesmo tempo: paga demais nas tarefas mecânicas e paga de menos nas tarefas com risco real ou com exigência de gosto. O meio-termo não é uma resposta às duas perguntas; é uma forma de não respondê-las. Medido em teste: sem a skill, agentes colapsam *tudo* em sonnet+medium; com ela, cada cenário foi pro canto certo da matriz.

---

## Passo a passo: como decidir na prática

Antes de tudo, um filtro: **a tarefa é mais barata que a própria decisão?** "Corrige esse typo" não merece triagem — vai direto, no default. Pra todo o resto:

### Passo 1 — Escreva o que "pronto" significa

Uma frase: como você reconhece que o resultado ficou bom? Isso melhora qualquer combinação de modelo/esforço que você escolher depois (harness antes de esforço).

### Passo 2 — Ache o pior passo

Da tarefa inteira, qual é o pedaço que mais exige critério fino? Não o mais demorado — o mais *difícil de julgar*.

- Tem gabarito claro (renomear, extrair, formatar, buscar padrão)? → passo mecânico.
- É implementação padrão, dá pra verificar se está certa? → passo padrão.
- Exige julgamento sem gabarito (voz de marca, arquitetura, diagnóstico obscuro, gosto)? → passo de repertório.

### Passo 3 — Modelo = o MENOR que resolve bem esse pior passo

- Mecânico → **haiku**
- Padrão verificável → **sonnet**
- Repertório/julgamento → **opus** ou **fable**

Nunca suba "por garantia" (o maior entrega o mesmo cobrando mais) e nunca desça porque "a tarefa é curta" (curta ≠ fácil de julgar).

### Passo 4 — Esforço = ambiguidade + custo do erro

Duas perguntas, some as respostas:

- **Quantos caminhos plausíveis existem?** Um só e óbvio → aponta pra low. Vários → aponta pra cima.
- **Quanto custa errar?** Reversível em segundos → low/medium. Chato de corrigir → medium. Produção, dado perdido, publicação externa → **high no mínimo** (se você nomeou o risco, "medium com cuidado" não existe).

E a regra da escada: **erro barato e reversível → comece no menor esforço plausível e só suba com evidência** de que o resultado veio insuficiente. Erro irreversível → esforço alto de saída, sem escada.

### Passo 5 — Cheque as três situações especiais

- **Lote (N itens iguais):** teste 1 item no menor modelo. Passou? O lote inteiro vai nele.
- **Empate de gosto:** estrutura garantida por template e só falta acabamento? Não decida sozinho — apresente o par custo (`sonnet+low`) × qualidade (`fable+low`) e deixe o usuário escolher.
- **Cache:** a decisão pede um modelo diferente do da conversa principal? Prefira **subagente** (não paga multa de cache). Só troque o `/model` principal em fronteira de trabalho.

### Exemplos completos, do começo ao fim

**"Renomear 80 arquivos seguindo o padrão X."**
Pronto = todos renomeados no padrão. Pior passo: aplicar um padrão (mecânico, com gabarito). Modelo: haiku. Ambiguidade: nenhuma; erro: reversível. Esforço: low. Lote: testa 1, depois manda os 80. → **haiku + low**

**"Escreve o roteiro do vídeo com a voz do INEMA."**
Pronto = roteiro que soa como a marca. Pior passo: a voz (gosto, sem gabarito) → repertório. Modelo: fable. Ambiguidade da decisão: baixa (formato conhecido); erro: reversível (é só reescrever). Esforço: low. → **fable + low** — e se houver template garantindo a estrutura, vira empate de gosto: ofereça sonnet+low × fable+low.

**"Migra os dados de produção pro schema novo."**
Pronto = dados migrados, nada perdido. Pior passo: transformação de schema (padrão, verificável). Modelo: sonnet. Custo do erro: **produção — risco nomeado**. Esforço: high, sem escada (a "evidência de falha" seria o prejuízo). → **sonnet + high**

**"Esse bug intermitente que ninguém acha a causa."**
Pronto = causa identificada e corrigida. Pior passo: diagnóstico sem causa óbvia (julgamento). Modelo: fable. Ambiguidade: alta (várias hipóteses plausíveis). Esforço: high. → **fable + high**

---

## Cola rápida

```
Modelo   → responde: QUEM? (repertório do pior passo)
Esforço  → responde: QUANTA VERIFICAÇÃO? (ambiguidade + custo do erro)

haiku   = mecânico com gabarito
sonnet  = padrão, verificável
opus/fable = julgamento sem gabarito, gosto, arquitetura

low     = caminho único, erro barato
medium  = alguma escolha OU erro chato
high+   = vários caminhos OU erro caro (risco nomeado = high, mínimo)

Escada: erro barato → começa low, sobe só com evidência.
        erro irreversível → esforço alto de saída, sem escada.
Overthinking: excesso do eixo esforço — esforço demais em tarefa
        simples PIORA o resultado. O certo é o MENOR que cobre o risco.
Cache:  /model só em fronteira; subagente é grátis; sem keepalive.
Antes de subir esforço: melhore o pedido (spec clara vence esforço alto).
```
