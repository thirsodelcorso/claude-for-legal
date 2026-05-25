---
name: matter-update
description: Anexa evento datado ao arquivo de histórico do caso e atualiza a linha do log — captura novos desenvolvimentos, mudanças de status, reavaliações de risco, prazos deslocados e mudanças de alçada para transação. Use quando o usuário quer logar update de um caso, anotar desenvolvimento, ou registrar mudança de status no portfólio.
argument-hint: "[slug] [brief event description]"
---

# /matter-update

1. Siga o workflow e a referência abaixo.
2. Confirme que o slug existe em `~/.claude/plugins/config/claude-for-legal/litigation-legal/matters/` e `_log.yaml`.
3. Pergunte por tipo de evento, data (default hoje), sumário, e quaisquer atualizações de campo do log (mudança de risco, mudança de status, prazo seguinte deslocado, reclassificação de materialidade).
4. Anexe entrada datada a `~/.claude/plugins/config/claude-for-legal/litigation-legal/matters/[slug]/history.md`.
5. Atualize `_log.yaml` — defina `last_updated` como hoje, aplique quaisquer atualizações de campo.
6. Confirme.

---

# Matter Update

## Propósito

O portfólio só permanece útil se permanecer atual. Esta skill torna barato logar update — dois minutos de captura estruturada, sem drift em formato livre.

## Carregar contexto

- `~/.claude/plugins/config/claude-for-legal/litigation-legal/matters/_log.yaml` — encontrar a linha
- `~/.claude/plugins/config/claude-for-legal/litigation-legal/matters/[slug]/history.md` — alvo do append
- `~/.claude/plugins/config/claude-for-legal/litigation-legal/matters/[slug]/matter.md` — referência (não reescrever)
- `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md` — calibração de risco (se reavaliando risco)

**Gate de impedimentos — incontornável.** Antes de logar update, cheque `_log.yaml` para o slug do caso. Se o caso não está em `_log.yaml`, recuse e route:

> "Não vejo [slug do caso] no log de casos. Rode `/litigation-legal:matter-intake` primeiro para a checagem de impedimentos rodar e o workspace ser montado. Não anexo histórico em caso não-intaken — a checagem de impedimentos é o gate, e não existe `history.md` para anexar até o caso ser intaken."

## Input

Slug (obrigatório). Se não fornecido, pergunte — com lista curta de casos atualizados recentemente para escolher.

## O update

### 1. Tipo de evento

Ofereça categorias:

- **Procedimental** — petição protocolada/recebida, decisão proferida, audiência realizada, prazo fixado
- **Instrução probatória** — produção feita/recebida, oitivas/depoimentos tomados, ofício requisitório expedido
- **Substantivo** — fatos novos, documento-chave aflorou, decisão sobre o mérito
- **Estratégia** — mudança de postura, proposta de acordo feita/recebida, mudança de alçada
- **Reavaliação de risco** — severidade ou probabilidade mudaram
- **Stakeholder** — pessoa nova no loop, troca de escritório externo / DP colaboradora
- **Administrativo** — contrato de honorários assinado, orçamento ajustado, dever de guarda renovado

Ou formato livre se nenhum couber.

### 2. Data

Default hoje. Aceite override (ex.: capturando evento da semana passada).

### 3. Sumário

Narrativa em um parágrafo. O que aconteceu, o que significa, qualquer implicação imediata.

### 4. Mudanças de campo do log

Caminhe pelos campos potencialmente afetados:

- `status:` — fase mudou (ex.: petição inicial → instrução)?
- `stage:` — atualização de subfase
- `risk:` — reavaliação necessária?
- `materiality:` — alguma mudança (fatos novos podem disparar provisão ou divulgação, ou escalonamento ao(à) Defensor(a) Público(a)-Geral)?
- `exposure_range:` — revisar se informação nova
- `next_deadline:` — nova data próxima, se houver (lembrar prazo em dobro CPC art. 186 se Defensor)
- `outside_counsel:` — mudou (escritório externo ou DP colaboradora)?
- `internal_owners:` — alguém novo ou removido?
- `legal_hold:` — renovado, expandido, liberado?

Só pergunte por campos provavelmente afetados pelo tipo de evento. Atualizações procedimentais usualmente tocam só `stage` e `next_deadline`; uma proposta de acordo pode tocar `materiality`, `exposure_range`, `status`.

### 4pre. Gate de aceitação de acordo

Se a atualização de Estratégia é uma **aceitação de acordo** (a empresa / o(a) assistido(a) está aceitando proposta de acordo, executando termo de acordo, ou autorizando aceitação em princípio — não meramente logando proposta feita ou recebida): Leia `## Quem está usando` em `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md`. Se o Papel é Não-advogado:

> Aceitar acordo tem consequências jurídicas — resolve a pretensão, tipicamente exige quitação, e pode afetar seguro, tributação e matérias correlatas. Você revisou com advogado(a) ou Defensor(a) Público(a)? Se sim, prossiga. Se não, segue brief para levar:
>
> [Gere sumário de 1 página: o caso, termos propostos do acordo (valor, estruturais, escopo da quitação, confidencialidade, não-depreciação), exposição em jogo, status da escada de alçada (vide `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md` alçada de transação), o que pode dar errado, o que perguntar ao(à) advogado(a) antes de aceitar.]
>
> Se precisa achar advogado(a) habilitado(a) ou Defensor(a) Público(a) na sua localidade: o serviço de referência da OAB Seccional do estado (ou da Defensoria Pública Estadual/União) é o ponto de partida mais rápido.

Não logue a aceitação nem mude materialidade com base em aceitação sem um sim explícito. Logar propostas ou contrapropostas não exige o gate — aceitação exige.

### 4a. Gatilho de materialidade — prompt explícito

Certos tipos de evento forçam re-checagem de materialidade. Quando o tipo de evento está nesta lista, **sempre pergunte** — não deixe o usuário seguir sem resposta explícita:

| Tipo de evento | Prompt de gatilho de materialidade |
|---|---|
| Substantivo (fatos novos, documento-chave, decisão de mérito) | "Este evento é substantivo. Empurra `materiality`? Atual: `[atual]`. Opções: `provisionado / divulgado / monitorado / nenhum` (DJ) ou `escalado-DPG / escalado-coordenador / monitorado / nenhum` (Defensor). Mudar?" |
| Estratégia (mudança de postura, proposta de acordo feita ou recebida) | "Atividade de acordo frequentemente dispara reclassificação de materialidade. Atual: `[atual]`. Se a proposta, contraproposta, ou aceitação move exposição ou desloca de contestado para provável-e-estimável, reclassifique." |
| Reavaliação de risco (severidade ou probabilidade mudaram) | "Risco mexeu. Materialidade deveria acompanhar. Atual: `[atual]`. Reclassificar?" |
| Desenvolvimento regulatório / fiscalizatório | "Ação de regulador (ofício, intimação, notificação fiscalizatória de ANPD/CVM/ANS/Bacen/RFB) usualmente dispara análise de divulgação. Atual: `[atual]`. Mudar?" |

Respostas aceitáveis incluem `sem mudança` — mas `sem mudança` deve ser explícito, não implícito por silêncio. Capture na entrada de histórico:

```markdown
**Checagem de materialidade:** [sem mudança / mudou de X para Y]
**Razão:** [uma frase]
```

Se materialidade move para `provisionado` ou `divulgado` (DJ) ou `escalado-DPG` (Defensor), e o caso não carregava provisão / divulgação / escalonamento prévio, sinalize o evento como exigindo notificação financeira / comitê de auditoria (DJ) ou comunicação ao(à) Coordenador(a) / Defensor(a) Público(a)-Geral (Defensoria) per `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md` limiares de materialidade.

### 5. Prompt de documento-semente (opcional)

Se o update referencia documento (decisão, peça protocolada, correspondência), pergunte se há path para linkar. Sem insistir.

## Gravando

### Anexar a `~/.claude/plugins/config/claude-for-legal/litigation-legal/matters/[slug]/history.md`

Mais recente no topo, diretamente sob o `---` que segue o cabeçalho.

```markdown
## [YYYY-MM-DD] — [Tipo de evento]: [título curto]

[Parágrafo de sumário.]

**Campos alterados:**
- [campo]: [velho → novo]
- [campo]: [velho → novo]

**Documento relacionado:** [path, se fornecido]
```

Se nenhum campo mudou, omita o bloco "Campos alterados".

### Atualizar `~/.claude/plugins/config/claude-for-legal/litigation-legal/matters/_log.yaml`

- Aplique qualquer mudança de campo.
- Defina `last_updated: [hoje]` (ou a data do evento se o usuário sobrescreveu — o log rastreia quando o registro foi tocado pela última vez).

## Confirmar

Mostre ao usuário a entrada de histórico e o diff do yaml antes de gravar:

> Eis o que vou anexar e atualizar. Tudo certo para confirmar?

## O que esta skill não faz

- Edita entradas de histórico passadas. Correções são novas entradas que referenciam e corrigem as anteriores.
- Muda o log silenciosamente. Toda mudança de campo é mostrada ao usuário antes do write.
- Decide se um desenvolvimento novo merece provisão/divulgação. Aflora a pergunta ("isto pode empurrar materialidade — quer reclassificar?"), o usuário responde.
