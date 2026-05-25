---
name: demand-received
description: Triagem de notificação extrajudicial recebida — extrai campos, cross-check com portfólio, avalia mérito, apresenta opções de resposta com recomendação, e handoff para matter-intake ou demand-intake se escalonamento se justifica. Use quando o usuário diz "recebemos notificação", "triagem desta notificação", ou compartilha notificação recebida para avaliar.
argument-hint: "[path-to-incoming] [--slug=custom-slug]"
---

# /demand-received

1. Leia o documento recebido do path fornecido.
2. Carregue `~/.claude/plugins/config/claude-for-legal/litigation-legal/matters/_log.yaml` para cross-check de portfólio.
3. Carregue `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md` → calibração de risco, panorama, prática de notificação.
4. Siga o workflow e a referência abaixo.
5. Extraia campos; cross-check portfólio; avalie mérito; apresente opções com recomendação.
6. Grave `~/.claude/plugins/config/claude-for-legal/litigation-legal/inbound/[slug]/triage.md`. Copie ou linke o recebido para `~/.claude/plugins/config/claude-for-legal/litigation-legal/inbound/[slug]/incoming.[ext]`.
7. Handoff per escolha do usuário:
   - Criar caso → `matter-intake` pré-populado
   - Responder com contranotificação → `demand-intake` pré-populado
   - Linkar a caso existente → atualize `related_matters` no log
   - Standalone → sem ação adicional

---

# Notificação Recebida

## Propósito

Notificações extrajudiciais recebidas são o feijão-com-arroz de contencioso. Uma pequena fração precisa escalonamento; a maioria pode ser tratada com resposta estruturada ou notificação de retenção. O failure mode é tratar todas igual. Esta skill triagia, faz cross-check com portfólio, e produz opções.

## Carregar contexto

- O documento recebido (usuário fornece path ou dropa in-session)
- `~/.claude/plugins/config/claude-for-legal/litigation-legal/matters/_log.yaml` — escaneie casos relacionados (mesma contraparte, contrapartes sobrepostas via relações entitárias, ou tipo de caso + data recente)
- `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md` → calibração de risco (para avaliação de mérito), panorama (o remetente é adversário frequente?), prática de notificação (tom da casa e defaults de resposta)

## Fluxo de trabalho

### Passo 1: Leia a notificação

Extraia do recebido:

- **Remetente** — entidade, signatário, advogado(a) (se assinada por escritório externo) / DP (se assinada por Defensor(a))
- **Destinatário** — qual entidade/pessoa na nossa parte
- **Entrega** — cartorial, e-mail, AR (importa para cálculo de prazo)
- **Data de recebimento** vs. **data de assinatura**
- **Tipo de notificação** — pagamento, mora/purgação, cessação, preservação, acordo, outro
- **Pedidos específicos** — o que querem, até quando
- **Fatos alegados** — versão deles do que aconteceu
- **Base jurídica** — leis, cláusulas, teses que citam
- **Ameaças** — o que dizem que farão se não cumprirmos
- **Framing de comunicação negocial** — pesquise as proteções de comunicação negocial aplicáveis ao foro (Lei 13.140/2015 art. 30 — confidencialidade em mediação; CPC art. 166 §3º). Note se a notificação está marcada como comunicação negocial, mas lembre: proteção decorre da conduta e contexto, não meramente do rótulo. Capture tanto o rótulo (se houver) quanto leitura de primeira passagem se a substância é de fato discussão de composição.

### Passo 2: Cross-check de portfólio

Busque `_log.yaml` por:

- **Match direto** — caso com mesma contraparte (slug bate com remetente)
- **Match por tipo** — caso similar com esta contraparte no passado (casos fechados contam — informam padrão)
- **Sobreposição de objeto** — casos onde o objeto pode ser a mesma disputa (ex.: mesmo contrato, mesmo produto, mesmo projeto)

Apresente achados:

- Se **match direto + ativo:** sinalize como quase certamente o mesmo caso; recomende anexar recebido ao caso existente, não abrir novo. Atualize `related_matters` se for tangente.
- Se **match direto + fechado:** sinalize — contraparte voltou. Pode ser disputa nova (abrir caso novo) ou ressuscitada (reabrir ou aditar). Usuário decide.
- Se **match por tipo:** anote como precedente/contexto; provavelmente caso distinto mas informa estratégia de resposta.
- Se **sem match:** novo. Trate como fresco.

### Passo 3: Avaliação de mérito

Não é parecer jurídico — leitura estruturada:

- **Fatos** — os fatos alegados se alinham com o que sabemos? Onde está o desencontro?
- **Base jurídica** — as cláusulas/leis citadas são efetivamente aplicáveis? (Sinalize cites para verificação do usuário — não tente validar lei autonomamente.)
- **Força no lado deles** — se fossem a juízo amanhã, qual a história?
- **Força no nosso lado** — quais defesas prováveis?
- **Danos pedidos vs. danos prováveis** — o pedido é proporcional ao que o juízo concederia se ganhassem?
- **Leverage e pressão** — estão credivelmente preparados a litigar? Têm capacidade? São adversário litigante repetido per `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md`?

Output rating de triagem: **mérito substancial / debatível / fraco / temerário**. Seja direto. O usuário está triando, não escrevendo a peça.

### Passo 4: Opções de resposta

Apresente 3-4 opções com tradeoffs:

**Opção A — resposta substantiva**
- Quando: a notificação tem mérito ou é ao menos debatível; resposta fundamentada protege o registro
- Tradeoff: nos compromete a uma posição por escrito
- Próximo passo: `/demand-intake` com campos pré-populados para contranotificação

**Opção B — notificação de retenção / ciência sem mérito**
- Quando: precisamos de tempo para investigar; não queremos conceder nada nem disparar a matemática dos prazos deles
- Tradeoff: não resolve nada; ganha 2-4 semanas
- Próximo passo: minuta curta de acuse de recebimento

**Opção C — resposta de tentativa de composição**
- Quando: resolução antecipada é mais barata que litigar; disposto a discutir sem admitir
- Tradeoff: postura de comunicação negocial exigida — pesquise a regra aplicável (Lei 13.140/2015 art. 30 ou equivalente) e estruture a resposta para que a substância, não só o rótulo, qualifique como discussão de composição. Precisa cuidar para não renunciar pretensões.
- Próximo passo: `/demand-intake` com `type: settlement-response`

**Opção D — ignorar + preservar**
- Quando: notificação é temerária ou o prazo deles não cria prejuízo jurídico
- Tradeoff: silêncio pode ser usado contra nós em alguns contextos (ex.: presunção em conta-corrente); dever de guarda ainda exigido
- Próximo passo: emitir dever de guarda via `/legal-hold --issue` se não emitido; logar a notificação e tocar adiante

Recomende uma. Seja específico sobre o porquê.

### Passo 5: Triagem de prazos

- **Prazo declarado por eles** — anote, mas não nos vincula
- **Nosso prazo interno** — quando temos que decidir (frequente: prazo declarado menos 5 dias úteis para redigir + aprovar; se Defensor, computar prazo em dobro CPC art. 186)
- **Prazos legais** — prescrição (CC arts. 205-206), decadência (CC 178), janelas contratuais de purgação, exigências procedimentais

Sinalize quaisquer prazos legais apertados. Agende.

**Sem suplementação silenciosa.** Se a notificação cita regras, julgados ou leis que exigem verificação, e consulta ao MCP de pesquisa configurado (JusRatio, BNP, CJF, TJAM, DataJud) retorna poucos ou nenhum resultado para dada autoridade, reporte o que foi encontrado e pare. NÃO preencha a lacuna com busca web ou conhecimento do modelo sem perguntar. Diga: "A busca retornou [N] resultados de [ferramenta]. Cobertura parece fina para [cite / doutrina]. Opções: (1) ampliar a query, (2) tentar outra ferramenta de pesquisa, (3) buscar na web — resultados tagueados `[busca web — verificar]` e devem ser checados contra fonte primária antes de confiar, ou (4) deixar a flag `[SME VERIFICAR]` e parar aqui. Qual prefere?" Um(a) advogado(a) decide se aceita fontes de menor confiança; a skill não decide por ele.

**Atribuição de fonte.** Tagueie cada citação trazida à triagem — incluindo as autoridades citadas pelo remetente, nossos racionais de opção de resposta, e qualquer pesquisa puxada para avaliação de mérito — com de onde veio: `[JusRatio]`, `[BNP]`, `[CJF]`, `[TJAM]`, `[DataJud]`, ou o nome do MCP para citações recuperadas de conector; `[busca web — verificar]` para citações de busca web; `[conhecimento do modelo — verificar]` para citações de dados de treino; `[usuário forneceu]` para citações fornecidas na própria notificação. Citações tagueadas `verificar` carregam maior risco de fabricação e devem ser checadas primeiro. Nunca strip ou colapse as tags.

### Passo 6: Gravar triagem

Output: `~/.claude/plugins/config/claude-for-legal/litigation-legal/inbound/[slug]/triage.md`.

```markdown
[CABEÇALHO DE SIGILO — por config do plugin ## Outputs — varia por papel; vide `## Quem está usando`]

> **Herança de sigilo.** Esta triagem deriva da notificação recebida e do log de portfólio, e registra nossa leitura de mérito de primeira passagem e postura de resposta. Aquelas análises internas são comunicação advogado-cliente e/ou trabalho preparatório. Distribuí-la além do círculo de sigilo — inclusive encaminhar a líder de negócio sem marcação, compartilhar com a contraparte, ou anexar a aviso de sinistro sem sanitizar — pode quebrar proteção tanto deste documento quanto do raciocínio dentro dele. Armazene com material sigiloso do caso, marque consistente com convenções da casa, e tome decisões de distribuição deliberadamente.

# Notificação Recebida — Triagem

> **LER PARA TRIAGEM, NÃO COMO OPINIÃO.** Este documento é varredura de intake e análise de opções — não opinião sobre mérito jurídico. O `Rating de triagem` abaixo é leitura estruturada para apoiar a decisão de como rotear a notificação. Não é recomendação sobre o mérito e não substitui análise jurídica específica do caso. Toda lei, regra ou julgado citado é flagueado para verificação por SME; toda chamada de mérito é do(a) advogado(a), não desta skill.

**Slug:** [slug]
**Recebida:** [YYYY-MM-DD]
**Recebida por:** [entidade / pessoa]
**Arquivo recebido:** [path]

---

## A notificação

**Remetente:** [entidade, signatário, advogado(a) / DP]
**Tipo:** [tipo]
**Pedidos específicos:** [lista]
**Prazo declarado por eles:** [data]
**Framing de comunicação negocial:** [rotulada / substantivamente / nenhuma / ambígua] — *proteção decorre da conduta e contexto, não do rótulo; `[SME VERIFICAR]` contra a regra aplicável (Lei 13.140/2015 art. 30 ou equivalente)*

## Fatos alegados

[versão deles, em um parágrafo]

## Base jurídica citada

[citações — cada uma inline-flagged com `[SME VERIFICAR: aplicabilidade / atualidade / jurisdição]` — não confie em nenhuma citação aqui sem checagem independente]

## Ameaças / próximos passos que declaram

[lista]

---

## Cross-check de portfólio

**Match direto:** [slug se existe, ou "nenhum"]
**Match por tipo / precedente:** [lista ou "nenhum"]
**Sobreposição de objeto:** [lista ou "nenhuma"]
**Recomendação:** [caso novo / anexar ao existente / linkar via related_matters / standalone inbound]

---

## Avaliação de mérito

**Fatos:** [alinhamento com nossa versão; desencontros]
**Base jurídica:** [aplicabilidade, com flags]
**Caso deles se litigado:** [um parágrafo]
**Nossas defesas:** [um parágrafo]
**Proporcionalidade do dano:** [avaliação]
**Credibilidade da ameaça:** [vão processar? capacidade? litigante repetido?]

**Rating de triagem:** [substancial / debatível / fraco / temerário] — *leitura estruturada para roteamento, não opinião de mérito; `[SME VERIFICAR: advogado(a) a confirmar antes de confiar]`*

---

## Opções de resposta

### A. Resposta substantiva
[Racional, tradeoffs, próximo passo]

### B. Notificação de retenção / acuse de recebimento
[Racional, tradeoffs, próximo passo]

### C. Resposta de tentativa de composição
[Racional, tradeoffs, próximo passo]

### D. Ignorar + preservar
[Racional, tradeoffs, próximo passo]

**Recomendação:** [A/B/C/D] — [duas frases por que] — `[SME VERIFICAR: advogado(a) a confirmar antes de executar]`

---

## Prazos

- **Prazo declarado por eles:** [data]
- **Nosso prazo interno de decisão:** [data]
- **Prazos legais:** [prescrição, períodos de purgação, procedimentais — com datas; lembrar prazo em dobro CPC art. 186 se Defensor]

---

## Ações imediatas

- [ ] Dever de guarda emitido — [sim/não] — se não, rode `/legal-hold [slug] --issue`
- [ ] Caso criado no log — [sim/não/TBD]
- [ ] Advogado(a) atribuído(a) — [quem]
- [ ] Seguro tendido — [sim/não/N-A]
- [ ] Escalonamento interno (Diretor(a) Jurídico(a) / Diretor(a) Financeiro(a) / líder de negócio / Coordenador(a) DP) — [quem/quando]
```

### Passo 7: Handoff

Baseado na recomendação e confirmação do usuário:

- Criação de caso → handoff para `/matter-intake` com: contraparte, tipo, `source: demand-letter` (recebida), tese inicial enquadrada defensivamente, pré-populado.
- Contra-resposta como notificação enviada → handoff para `/demand-intake` com: contraparte, contexto da triagem, pretensão como a resposta.
- Linkar a caso existente → atualize `related_matters` daquele caso em `_log.yaml`; anexe evento ao seu `history.md`.
- Standalone → deixe em `~/.claude/plugins/config/claude-for-legal/litigation-legal/inbound/`; sem mudança de portfólio.

## Feche com a árvore de decisão de próximos passos

Feche com a árvore de decisão de próximos passos per CLAUDE.md `## Outputs`. Customize as opções para o que esta skill acabou de produzir — as cinco ramificações default (redigir o X, escalonar, pegar mais fatos, observar e esperar, outra coisa) são ponto de partida, não trava. A árvore É o output; o(a) advogado(a) escolhe.

## O que esta skill não faz

- **Valida lei citada.** Sinaliza cites para o usuário rodar contra ferramenta de pesquisa (verificar se ainda é bom direito) ou checar com escritório externo. Inventar análise jurídica sobre notificações recebidas é exposição a responsabilidade profissional.
- **Envia resposta.** Minutas são redigidas em `demand-draft`; esta skill para na decisão de triagem.
- **Decide mérito definitivamente.** O rating é leitura para triagem; opinião formal de mérito vive com escritório externo ou análise mais aprofundada.
- **Toma a decisão de criar caso.** Aflora a recomendação; usuário decide.
