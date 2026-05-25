---
name: matter-briefing
description: Briefing aprofundado de um caso — postura atual, o que mudou, próximo prazo, perguntas em aberto, e checagem de reavaliação de risco, pronto antes de update ao(à) Diretor(a) Jurídico(a) ou ligação com escritório externo / núcleo da DP. Use quando o usuário diz "me dê briefing de [caso]", "onde estamos em [caso]", ou precisa de leitura sobre caso específico.
argument-hint: "[slug]"
---

# /matter-briefing

1. Carregue `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md` → calibração de risco + stakeholders relevantes.
2. Siga o workflow e a referência abaixo.
3. Leia `~/.claude/plugins/config/claude-for-legal/litigation-legal/matters/[slug]/matter.md` + `~/.claude/plugins/config/claude-for-legal/litigation-legal/matters/[slug]/history.md` + linha do log de `_log.yaml`.
4. Produza briefing: postura atual, o que mudou desde o último update, próximo prazo, perguntas em aberto, checagem de reavaliação de risco ("o campo `risk:` ainda reflete a realidade?").
5. Sinalize defasagem: se `last_updated` > 30 dias, diga.

---

# Matter Briefing

## Propósito

Dar ao(à) advogado(a) ou Defensor(a) uma leitura limpa sobre um caso no tempo que se leva para caminhar até uma sala de reunião. Postura atual, o que mudou, o que vem, o que vale reconsiderar.

## Carregar contexto

- `~/.claude/plugins/config/claude-for-legal/litigation-legal/matters/_log.yaml` — linha estruturada
- `~/.claude/plugins/config/claude-for-legal/litigation-legal/matters/[slug]/matter.md` — intake narrativo
- `~/.claude/plugins/config/claude-for-legal/litigation-legal/matters/[slug]/history.md` — log de eventos
- `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md` — calibração de risco (para que "risk: high" signifique algo específico, não genérico)

**Gate de impedimentos — incontornável.** Antes de produzir briefing, cheque `_log.yaml` para o slug. Se o caso não está em `_log.yaml`, recuse e route:

> "Não vejo [slug do caso] no log de casos. Rode `/litigation-legal:matter-intake` primeiro para a checagem de impedimentos rodar e o workspace ser montado. Não produzo briefing em caso não-intaken — a checagem de impedimentos é o gate."

## Input

Slug (obrigatório). Se ambíguo ou ausente, peça ao usuário para escolher de uma lista de casos ativos.

## O briefing

```markdown
[CABEÇALHO DE SIGILO — por config do plugin ## Outputs — varia por papel; vide `## Quem está usando`]

# [Nome do caso] — Briefing em [hoje]

**Status:** [status / fase]
**Risco:** [rating] ([severidade] × [probabilidade])
**Materialidade:** [categoria]
**Escritório externo / DP colaboradora:** [escritório / núcleo — responsável]
**Última atualização:** [data] [sinalizar ⚠️ DEFASADO se >30d]
**Impedimentos:** [status — sinalizar ⚠️ se `pendente` ou `não-rodado`]

---

## Sumário em um parágrafo

[Postura atual. O que estamos fazendo e por quê. Nomeie o fato pivô se um foi capturado.]

## O que mudou recentemente

[Últimas 3-5 entradas de history.md, mais recente primeiro. Se o histórico está fino, diga.]

## O que vem

- **Prazo imediato:** [next_deadline + o que é — lembrar prazo em dobro CPC art. 186 se Defensor]
- **Marcos próximos:** [qualquer coisa datada em matter.md ou histórico recente]
- **Decisões pendentes:** [perguntas em aberto sinalizadas em matter.md]

## Exposição

[Faixa + qualquer mudança desde intake. Se provisionado, provisão atual + se recalibração está atrasada. Se Defensor: não há provisão CPC 25 pessoal; lembre que sucumbência reverte ao Fundo da DP.]

## Stakeholders internos

[Quem está no loop; se alguém deveria estar no loop e não está — para Defensor, isso inclui o(a) Coordenador(a) da área e, em caso atípico, o(a) Defensor(a) Público(a)-Geral conforme regulamento interno]

## Checagem de reavaliação de risco

*Um prompt, não uma resposta.*

- O `risk: [rating]` ainda parece certo, ou o caso se moveu?
- O `materiality: [categoria]` ainda combina? (Fatos novos podem empurrar para provisão ou divulgação no DJ, ou escalonamento ao(à) DPG na Defensoria.)
- Algum stakeholder novo de que o caso precisa (ex.: CISO se torna relevante após desenvolvimento na instrução; Núcleo Especializado da DP se a tese transbordou a unidade)?

## Perguntas em aberto

[De matter.md e qualquer coisa não resolvida no histórico]

## Para a conversa

[Se o usuário especificou propósito — "me prepare para a ligação com escritório externo" — customize a seção final: perguntas a fazer, decisões a extrair, atualizações a extrair. Se nenhum propósito dado, omita esta seção.]
```

## Defasagem

Se `last_updated > 30 dias atrás`: sinalize no topo E sugira rodar `/litigation-legal:matter-update [slug]` após a reunião para capturar o que for discutido.

## Tom

Isto não é marketing. Diga o que se sabe; sinalize o que não. Se um caso tem histórico fino e acabou de ser aberto, o briefing é curto — e está correto. Não infle.

## Feche com a árvore de decisão de próximos passos

Feche com a árvore de decisão de próximos passos per CLAUDE.md `## Outputs`. Customize as opções para o que esta skill acabou de produzir — as cinco ramificações default (redigir o X, escalonar, pegar mais fatos, observar e esperar, outra coisa) são ponto de partida, não trava. A árvore É o output; o(a) advogado(a) escolhe.

## O que esta skill não faz

- Prediz desfechos. O rating de risco é um juízo capturado, não previsão.
- Recomenda estratégia. Aflora perguntas; o(a) advogado(a) ou Defensor(a) responde.
- Re-triagem. Se o usuário quer re-triagem, é um `/matter-update` com mudanças de campo — esta skill lê, não escreve.
