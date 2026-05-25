---
name: matter-close
description: Fecha um caso — captura desfecho, exposição final e lições, depois arquiva fora do portfólio ativo sem apagar o registro. Use quando o usuário quer fechar um caso, diz "[caso] está encerrado", ou precisa registrar acordo, extinção, sentença, desistência ou consolidação como desfecho.
argument-hint: "[slug]"
---

# /matter-close

1. Siga o workflow e a referência abaixo.
2. Confirme slug e status atual.
3. Capture desfecho: tipo de resolução (acordo, extinto, sentença favorável/desfavorável, desistência, consolidado), data, custo/exposição final, lições.
4. Atualize `_log.yaml`: `status: closed`, adicione campos `closed: YYYY-MM-DD` e `outcome:`.
5. Anexe entrada final a `~/.claude/plugins/config/claude-for-legal/litigation-legal/matters/[slug]/history.md`.
6. Caso permanece em `_log.yaml` e `~/.claude/plugins/config/claude-for-legal/litigation-legal/matters/[slug]/` — não apagado. `/portfolio-status` o filtra dos rollups ativos.

---

# Matter Close

## Propósito

Casos terminam. O desfecho é o dado isolado mais valioso que o portfólio gera — calibra o framework de risco para casos futuros. Fechar um caso captura o desfecho estruturalmente para que o registro seja útil, não só arquivado.

## Carregar contexto

- `~/.claude/plugins/config/claude-for-legal/litigation-legal/matters/_log.yaml` — encontrar a linha
- `~/.claude/plugins/config/claude-for-legal/litigation-legal/matters/[slug]/matter.md` — referência (contexto de intake)
- `~/.claude/plugins/config/claude-for-legal/litigation-legal/matters/[slug]/history.md` — alvo do append

**Gate de impedimentos — incontornável.** Antes de fechar, cheque `_log.yaml` para o slug. Se o caso não está em `_log.yaml`, recuse e route:

> "Não vejo [slug do caso] no log de casos. Nada para fechar — ou o slug está errado, ou o caso nunca foi intaken via `/litigation-legal:matter-intake`. Confira o slug primeiro; se genuinamente nunca foi intaken, não há linha para atualizar nem estrutura de arquivo para fechar."

## Input

Slug (obrigatório).

## O fechamento

### 1. Tipo de resolução

- `acordo` — com contraparte, valor, termos estruturais; observar prazo em dobro CPC art. 186 se Defensor; sucumbência reverte ao Fundo da DP (LC 80/94)
- `extinto` — com ou sem resolução de mérito (CPC arts. 485-487), por que mecanismo
- `sentença-favorável` — em que fase, exposição a recurso, trânsito em julgado?
- `sentença-desfavorável` — em que fase, status de recurso, exposição cristalizada; para Defensor, hipossuficiência presumida (Súmula 481 STJ) costuma suspender exigibilidade da sucumbência (CPC art. 98)
- `desistência` — pela contraparte ou pelo(a) assistido(a) com homologação, circunstâncias
- `consolidado` — conexão / continência (CPC arts. 55-57) ou reunião (fornecer slug do caso parente)
- `outro` — com explicação

### 2. Data da resolução

A data em que o caso efetivamente terminou (termo de acordo assinado, sentença/decisão proferida e transitada, homologação de desistência).

### 3. Exposição final

- Custo real para a empresa / assistido(a) (valor de acordo + honorários + custo injuntivo/estrutural)
- vs. faixa de exposição inicial no intake (acertamos?)
- Acurácia da provisão (se provisionado): contabilizado vs. real

### 4. Lições

Duas ou três frases. O que acertamos? O que erramos no julgamento? Algo que o intake deveria ter sinalizado antes?

Esta é a parte que o(a) próximo(a) advogado(a) ou Defensor(a) vai reler. Seja honesto. "Subestimei o agressivo do escritório do autor" vale mais que "resolvido favoravelmente."

### 5. Prompt de documento-semente

Termo de acordo, sentença, sentença de extinção — path se disponível. Não obrigatório.

## Gravando

**Antes de fechar o caso (o ato consequencial — o caso é arquivado e o tracking ativo cessa):** Leia `## Quem está usando` em `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md`. Se o Papel é Não-advogado:

> Fechar um caso tem consequências jurídicas — encerra o tracking ativo, pode afetar qualquer dever de guarda associado (rode `/legal-hold --release` separadamente se apropriado), e estabelece o registro final no qual a parte se apoia. Você revisou com advogado(a) ou Defensor(a) Público(a)? Se sim, prossiga. Se não, segue brief para levar:
>
> [Gere sumário de 1 página: o caso, tipo de resolução e termos, exposição final vs. inicial, acurácia de provisão, casos relacionados ou recursos ainda vivos, o que pode dar errado em fechamento prematuro, o que perguntar ao(à) advogado(a) ou Defensor(a).]
>
> Se precisa achar advogado(a) habilitado(a) ou Defensor(a) Público(a) na sua localidade: o serviço de referência da OAB Seccional do estado (ou da Defensoria Pública Estadual/União) é o ponto de partida mais rápido.

Não grave os campos de fechamento nem anexe a entrada de fechamento sem um sim explícito.

### Atualizar `~/.claude/plugins/config/claude-for-legal/litigation-legal/matters/_log.yaml`

```yaml
status: closed
closed: [YYYY-MM-DD]
outcome: [tipo-resolução]
final_cost: [valor em R$]
last_updated: [hoje]   # fechamento é o último toque; registre
```

Retenha todos os campos existentes. Não apague a linha.

### Anexar entrada final a `~/.claude/plugins/config/claude-for-legal/litigation-legal/matters/[slug]/history.md`

```markdown
## [YYYY-MM-DD] — Caso fechado: [tipo-resolução]

**Resolução:** [narrativa — o que aconteceu, em que termos]
**Custo final:** [valor + termos estruturais se houver]
**vs. exposição inicial:** [comparar com faixa de intake em matter.md]
**Acurácia de provisão:** [se aplicável]

**Lições:**
[2-3 frases — retrospectiva honesta]

**Documento relacionado:** [termo de acordo / sentença / etc., se fornecido]
```

### Toque `~/.claude/plugins/config/claude-for-legal/litigation-legal/matters/[slug]/matter.md`

Adicione um bloco de fechamento no fim (não modifique seções anteriores — são o intake histórico):

```markdown
---

## Fechado em [YYYY-MM-DD]

[Sumário de resolução em um parágrafo. Ponteiro para a entrada final de histórico para detalhe.]
```

## Confirme

Mostre ao usuário a entrada completa de fechamento e as mudanças do yaml antes de gravar.

## O que esta skill não faz

- Apaga casos. Casos fechados permanecem em `_log.yaml` e em disco — são o conjunto de treino para o julgamento do portfólio.
- Reabre. Se um caso fechado volta (recurso, litígio conexo), abra novo caso que referencia o fechado em `matter.md`.
- Inventa lições que o usuário não disse. Se o usuário pula a seção de lições, deixe em branco em vez de inventar.
