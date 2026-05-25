---
name: portfolio-status
description: Rollup do portfólio a partir de _log.yaml — distribuição de risco, prazos próximos (em dias úteis CPC 219 + dobro Defensor CPC 186 quando aplicável; dias corridos no JEC pela Lei 9.099), casos parados, totais de materialidade ou escalonamentos institucionais, distribuição de fase, e anomalias sinalizadas. Para Defensor, agrega por vara da atribuição. Use quando perguntar "onde estamos", "quantos casos abertos", "audiências da semana", ou quiser rollup de status em todos os casos ativos.
argument-hint: "[--all | --risco=alto | --parado | --vara=<num>]"
---

# /portfolio-status

1. Carregue `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md` → calibração de risco (define como ler o campo `risk:`), atribuição (para Defensor — varas).
2. Siga o workflow e referência abaixo.
3. Parse `~/.claude/plugins/config/claude-for-legal/litigation-legal/matters/_log.yaml`. Filtre casos encerrados por default (inclua com `--all`).
4. Produza rollup: distribuição de risco, prazos nos próximos 14/30/60 dias úteis (com nota de suspensão CPC 220 20/12-20/1), casos sem atualização > 30 dias, totais de materialidade ou escalonamentos institucionais, distribuição de fase, audiências marcadas.
5. Flag anomalias — tudo marcado crítico, prazo vencido, casos sem escritório externo / núcleo especializado atribuído quando risco médio ou alto, casos de Defensor sem prazo em dobro computado (CPC 186).

---

# Status do Portfólio

## Propósito

Uma leitura que responde: o que eu tenho agora, o que precisa de atenção, e o que está escapando? Output é escaneável — desenhado para Defensor(a) / advogado(a) que tem 3 minutos antes da próxima audiência ou atendimento.

**Para Defensor:** agrega por vara da atribuição (ex.: 1ª JEC, 12ª JEC, 19ª Cível, 20ª Cível) — torna visível distribuição de carga, prazos vencendo numa vara específica, audiências concentradas no mesmo dia.

## Carregar contexto

- `~/.claude/plugins/config/claude-for-legal/litigation-legal/matters/_log.yaml` — fonte da verdade
- `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md` — calibração de risco (para interpretar campos risk/materiality corretamente), atribuição da unidade (Defensor)

## Flags e filtros

Default: casos ativos só (excluir `status: encerrado`).

Flags:
- `--all` — inclui encerrados
- `--risco=alto` (ou `crítico` / `médio` / `baixo`) — filtra por banda de risco
- `--parado` — só casos com `last_updated` > 30 dias
- `--tipo=civel-saude` — filtra por tipo de caso
- `--vara=19` — filtra por vara específica (Defensor)
- `--humanitario` — só casos com `risco_humanitario` não-nulo (Defensor)
- `--owner=[nome]` — filtra por owner

## O rollup

```markdown
[CABEÇALHO DE SIGILO — per plugin config ## Outputs — difere por papel; vide `## Quem está usando`]

# Status do Portfólio — [hoje]

**Casos ativos:** [N]
**Encerrados (ano corrente):** [N] *(mostrado só com --all)*

---

## Por risco

| Risco | Contagem | Casos |
|---|---|---|
| Crítico | [N] | [slugs] |
| Alto | [N] | [slugs] |
| Médio | [N] | [contagem só — expanda com `--risco=médio`] |
| Baixo | [N] | [contagem só] |

## Por vara (Defensor) — distribuição da atribuição

| Vara | Contagem ativos | Audiências próximas 14 dias | Prazos próximos 7 dias úteis |
|---|---|---|---|
| 1ª JEC | [N] | [N] | [N] |
| 12ª JEC | [N] | [N] | [N] |
| 19ª Cível Comum | [N] | [N] | [N] |
| 20ª Cível Comum | [N] | [N] | [N] |

## Prazos próximos

*Cálculo: CPC art. 219 (dias úteis) para casos sob rito CPC; Lei 9.099/95 (dias corridos) para JEC; CPC art. 186 (em dobro) para Defensor. Suspensão CPC art. 220 (20/12 a 20/1) aplicada quando o período cruza.*

| Dentro de | Casos |
|---|---|
| 7 dias úteis | [slug — prazo — descrição — base CPC/Lei] |
| 8-14 dias úteis | [...] |
| 15-30 dias úteis | [...] |

*Prazos vencidos sinalizados separadamente abaixo.*

## Audiências marcadas

| Data | Vara | Caso | Tipo de audiência | Defensor(a) presente |
|---|---|---|---|---|
| [AAAA-MM-DD HH:MM] | [vara] | [slug] | conciliação CPC 334 / instrução / JEC 9.099 / outro | [nome ou titular/suplente] |

*Conflito de agenda destacado em vermelho.*

## Materialidade / escalonamento institucional

**Para DJ corporativo:**

| Categoria | Contagem | Exposição total (midpoint) |
|---|---|---|
| Provisionado | [N] | [R$ X] |
| Divulgado | [N] | [R$ X] |
| Monitorado | [N] | — |
| Nenhum | [N] | — |

**Para Defensor Público:**

| Categoria | Contagem | Notas |
|---|---|---|
| Escalado ao DPG | [N] | [tese inédita / acordo extraordinário / TAC / ACP] |
| Escalado ao Coordenador | [N] | |
| Enviado a Núcleo Especializado | [N] | [Saúde / Idoso / Mulher / Consumidor / etc.] |
| Monitorado | [N] | — |
| Nenhum | [N] | — |

## Por fase

[tabela: postulatória / instrução / decisória / cumprimento de sentença / recurso / arquivamento]

## Por urgência humanitária (Defensor)

[lista de casos com `risco_humanitario` não-nulo, ordenados por urgência percebida]

---

## ⚠️ Anomalias e flags

- **Prazos vencidos:** [lista de slugs onde next_deadline passou]
- **Parado (>30d sem update):** [lista]
- **Conflitos / impedimentos não resolvidos:** [lista de slugs com `conflicts.status in [pending, not-run]`]
- **Conflitos com bypass (override ativo):** [lista de slugs onde `conflicts.override.by` está populado — flag permanente até clearance manual]
- **Risco alto/crítico sem escritório externo / núcleo:** [lista]
- **Provisionado sem last_updated em >60d:** [lista] — recalibração de provisão provavelmente atrasada (DJ)
- **Defensor sem prazo em dobro computado (CPC 186):** [lista] — casos onde `prazo_em_dobro_defensor` é null ou false mas o papel é defensor-publico
- **Dever de guarda não emitido em litígio ativo:** [lista]
- **Audiência conflito de agenda:** [pares de casos com audiência no mesmo dia/hora]
- **Hipossuficiência sem registro (Defensor):** [lista] — assistido(a) sem declaração de hipossuficiência ou comprovação documental no `matter.md`
- **Campos faltantes:** [slug → campo]

---

## Recomendação de fechamento

[Uma ou duas frases sobre o que olhar primeiro, se algo realmente se destaca. Não boilerplate — só se realmente algo se destaca. Para Defensor: priorizar urgência humanitária + prazos vencendo.]
```

## Regras de anomalia

Estas são as checagens que tornam a skill útil em vez de decorativa:

1. **Prazo vencido:** `next_deadline < hoje` e `status != encerrado`
2. **Parado:** `last_updated < hoje - 30d` e `status != encerrado`
3. **Conflitos não resolvidos:** `conflicts.status in [pending, not-run]` e `status != encerrado`
3b. **Conflitos override ativo:** `conflicts.override.by != null` (nunca auto-clear)
4. **Alto risco sem cobertura:** `risk in [alto, crítico]` e `outside_counsel.firm == null` (ou para Defensor: sem núcleo especializado quando a tese tipicamente exige)
5. **Provisão parada:** `materiality == provisionado` e `last_updated < hoje - 60d`
6. **Lacuna de dever de guarda:** `status in [ameaçado, ativo, instrução, decisão, recurso]` e `legal_hold.issued == false` — dever de preservação atrasa a partir da expectativa razoável de litígio
7. **Defensor sem dobro CPC 186:** `papel == defensor-publico` e `prazo_em_dobro_defensor != true`
8. **Audiência em conflito:** dois ou mais casos com audiência no mesmo dia/hora e vara distinta sem cobertura por substituto(a)
9. **Hipossuficiência sem registro (Defensor):** `papel == defensor-publico` e ausência de campo declaração no `matter.md`
10. **Campos faltantes:** qualquer campo obrigatório null — `risk`, `materiality` (ou `escalonamento_institucional` para Defensor), `status`, `opened`, `conflicts.status`

## Feche com a árvore de decisão de próximos passos

Termine com a árvore per CLAUDE.md `## Outputs`. Customize as opções ao que esta skill acabou de produzir — as cinco branches default (redigir o X, escalonar, pegar mais fatos, observar e esperar, outra coisa) são ponto de partida.

Se o portfólio tem mais de ~10 casos, ou se a pessoa pedir: ofereça o dashboard (vide CLAUDE.md `## Outputs → Oferta de dashboard para outputs com muitos dados`). Forme a oferta para este output — contagens por nível de risco, timeline de prazos próximos, agenda de audiências da semana, e ledger ordenável de casos com status, checagem de conflitos, e data da última movimentação.

## O que esta skill NÃO faz

- Toma decisões. Surface o que precisa de atenção; o(a) Defensor(a)/advogado(a) decide prioridade.
- Finge precisão que não tem. Midpoints de exposição são rudes e devem ser rotulados como tal.
- Substitui sistema oficial de gestão (Sapiens-DPGU, LegalDesk, Themis, Projuris, etc.). É rollup de memória de trabalho, não sistema de registro.
- Calcula prazo a partir do termo inicial. Lê `next_deadline` do `_log.yaml`. Quem coloca o prazo lá (cold-start, matter-intake, docket-watcher agent, ou edição manual) é responsável pela contagem correta.
