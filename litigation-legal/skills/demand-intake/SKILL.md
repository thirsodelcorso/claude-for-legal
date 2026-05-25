---
name: demand-intake
description: Captura de contexto pré-redação de notificação extrajudicial — partes, fatos, base, leverage, BATNA e filtros de sigilo — gravados em intake.md estruturado que a skill demand-draft lê. Use quando o usuário quer preparar notificação extrajudicial, rodar intake antes de redigir, ou capturar contexto para notificação de pagamento, mora/purgação, cessação, separação trabalhista ou preservação.
argument-hint: "[title] [--full]"
---

# /demand-intake

1. Carregue `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md` → prática de notificação extrajudicial, panorama, calibração de risco.
2. Siga o workflow e a referência abaixo.
3. Rode o intake adaptativo (core 8 sempre; bloco estratégico se material ou `--full`).
4. Gere slug do título + contraparte + ano-mês.
5. Grave `~/.claude/plugins/config/claude-for-legal/litigation-legal/demand-letters/[slug]/intake.md`.
6. Confirme com o usuário: "Intake salvo. Rode `/litigation-legal:demand-draft [slug]` quando pronto."

---

# Notificação Extrajudicial — Intake

## Propósito

A redação é downstream. O valor está na pré-redação — forçar as perguntas que uma notificação descuidada pula. Leverage, BATNA, tolerância a downside, filtros de sigilo, a audiência real. Notificação enviada sem pensar nisso é pior que nenhuma notificação.

## Carregar contexto

- `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md` → prática de notificação (timing de aviso de sinistro ao seguro, limiar de materialidade para criação de caso, quaisquer seed-doc templates), panorama (tipo de contraparte, padrões de adversário repetido), calibração de risco (para pré-estimar materialidade), estilo da casa. **Tom, prazo de compliance, marcação, signatário NÃO são defaults nível-prática — são setados por caso no passo `## Postura para este caso` abaixo.**

## Flags

- `--full` → rode o intake completo independente das heurísticas de materialidade (para advogado(a) que quer profundidade sempre)

## O intake

### Postura para este caso (pergunte PRIMEIRO, antes do core)

> **Postura para este caso.** Tom e termos da notificação são caso-a-caso, não default de prática. Pergunte:
> - **Tom:** moderado / assertivo / agressivo? (depende da relação, do valor, e se litígio é provável)
> - **Janela de resposta:** o que é razoável dada a pretensão? (10 dias é comum para cobrança; 30 dias para purgação de mora; 48h para cessação imediata — mas o contrato ou protocolo pode setar; locação em ação de despejo: 15 dias para purgação Lei 8.245/91 art. 62 II)
> - **Marcação:** isto precisa de "sem prejuízo" ou "sem prejuízo das medidas judiciais cabíveis"? (comunicações de tentativa de mediação têm confidencialidade negocial Lei 13.140/2015 art. 30; afirmações de direito frequentemente não; jurisdição importa — pergunte se incerto)
> - **Signatário:** você, o(a) cliente, o(a) Diretor(a) Jurídico(a), advogado(a) externo instruído? Para Defensor: o(a) próprio(a) Defensor(a) (Ofício DPEAM)?
> Não assuma. Leia a correspondência prévia da notificação no arquivo do caso se houver — ela estabelece o registro.

Registre as respostas no intake sob uma seção `## Postura` antes de `## Partes`. Estas respostas governam o resto do intake e a minuta downstream — não default para nível-prática se o usuário deixou alguma em branco; pergunte de novo.

### Core — sempre perguntadas (8 questões)

**1. Tipo de notificação**
`pagamento | mora-purgacao | cessacao | separacao-trabalhista | preservacao | outro`

**2. Partes**
- **Remetente:** nossa pessoa jurídica / unidade da Defensoria (e qualquer entidade específica se multi-entidade) / o(a) assistido(a) por intermédio da DP
- **Destinatário:** contraparte — nome, entidade, endereço
- **Audiência do destinatário:** quem efetivamente lê (Diretor(a) Jurídico(a)? CEO? indivíduo? jurídico interno?)
- **Relação:** `cliente | fornecedor | ex-empregado(a) | concorrente | terceiro | outro`

**3. Evento desencadeador**
- O que aconteceu e quando (datas importam — prescrição CC arts. 205-206, decadência CC 178, prazos de notificação)
- Evidência disponível (contratos, e-mails, registros, testemunhas)

*Oportunidade de doc-semente: "Se você puder compartilhar o contrato subjacente, correspondência, ou evidência, a minuta será materialmente mais afiada. Paths funcionam."*

**4. Base jurídica / contratual**
- Quais cláusulas — seções específicas do contrato se aplicável
- Lei de regência (jurisdição, cláusula de eleição de foro — atentar Súmula 335 STJ e CDC art. 6º se relação de consumo)
- Leis ou regras invocadas (placeholders OK — a minuta vai sinalizar `[CITE:___]` de qualquer jeito)

**5. Pretensão**
- Pedidos específicos. Não "resolução" — pagamento de R$ X até dia Y; cessação da atividade específica Z; purgação dentro de N dias; devolução de bem específico.
- Se múltiplos pedidos, ordene (primário vs. fallback)

**6. Prazos**
- Prazo externo que dirige isto (prescrição/decadência, janela de dano contínuo, evento de negócio)
- Prazo de compliance da notificação — quanto damos ao destinatário. Use a janela de resposta capturada em `## Postura para este caso` acima; não default para nível-prática.

**7. Outreach anterior**
- Isto foi levantado informalmente? Quando, por quem, em que forma?
- Alguma resposta até agora?
- Por que o escalonamento para notificação acontece agora?

**8. Distribuição**
- Método de entrega (pergunte; sem default nível-prática). Opções: notificação cartorial (Tabelionato de Notas), notificação postal com AR, e-mail com confirmação, protocolo presencial, Ofício DPEAM com AR.
- Signatário — capturado em `## Postura para este caso` acima
- Cópias — stakeholders internos, seguradora (se tendendo pré-notificação per regra nível-prática de timing de aviso de sinistro), advogado(a)

### Estratégico — perguntado se material, ou se `--full`

Heurística de materialidade: pergunte o bloco estratégico se qualquer das seguintes é verdade.

- Tipo é `cessacao`, `mora-purgacao`, `separacao-trabalhista`, ou `preservacao`
- Valor dos pedidos ≥ a faixa de severidade média de `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md` calibração de risco
- Contraparte é cliente, concorrente, ou adversário frequente per `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md` panorama
- Usuário rodou com `--full`

**Opção explícita de pular.** Quando o bloco estratégico é disparado, o usuário pode declinar de responder. Pergunte clara:

> Esta é notificação material pela heurística. O bloco estratégico (leverage, BATNA, tom, filtros de sigilo) é onde a maior parte do valor da pré-redação vive. Pular produz minuta mais fina.
> - **Responder agora** — caminhar pelo bloco estratégico (5-7 min)
> - **Responder parcial** — caminhar pelo subset para o qual se sente preparado
> - **Pular** — prosseguir para a minuta só com o bloco core; vou sinalizar `strategic_block: skipped` no intake

Se o usuário escolhe Pular, o arquivo de intake registra:

```yaml
strategic_block: skipped        # answered | partial | skipped
skipped_reason: string | null   # capturado se o usuário forneceu
```

A skill de minuta honra o skip — o gate pré-minuta roda independentemente, mas seções que dependem das respostas do bloco estratégico recebem marcadores `[SME VERIFICAR: leverage/tom/sigilo não capturados no intake]`. O comando `/demand-draft` também pergunta uma segunda vez, indagando se o usuário quer completar o bloco estratégico antes de redigir.

**9. Leverage e BATNA**
- O que nos dá poder de negociação (direitos contratuais, leverage factual, reputacional, comercial)
- E se recusam — estamos preparados a litigar? Ir a público? Aceitar resultado menor?
- BATNA provável deles — qual a melhor alternativa deles? (Se não acham que vamos processar, a notificação é fraca.)

**10. Tolerância a downside**
- Exposição reputacional se isto se tornar público
- Risco de precedente — esta notificação seta padrão que afeta outros casos?
- Implicações regulatórias / de divulgação (este é o tipo de disputa que vira item de Formulário de Referência CVM?)
- Implicações de seguro — enviar sem tender quebra cobertura?

**11. Postura de tom**
- Já capturada em `## Postura para este caso` acima. Aqui, sonde o trade-off se o usuário escolheu tom mais forte que os fatos parecem justificar, ou mais fraco.
- Vale nomear explicitamente: tom agressivo queima a relação. Se quer manter a relação de negócio mas precisa proteger a posição jurídica, `moderado` é usualmente a chamada certa.

**12. Postura de comunicação negocial**
- Pesquise as proteções de comunicação negocial aplicáveis ao foro (Lei 13.140/2015 art. 30 — confidencialidade em mediação; CPC art. 166 §3º — princípios da conciliação). Esta notificação é comunicação negocial que deveria ser protegida? Ou afirmação de direitos que não deveria?
- Se protegida: a minuta vai incluir o marcador de confidencialidade negocial e será estruturada para que a substância (uma discussão de composição) — não só o rótulo — sustente a postura.
- Proteção decorre da conduta e contexto, não meramente do rotular. O marcador é escolha cinto-e-suspensórios.

**13. Filtros de sigilo**
- O que está em nossa análise interna que NÃO deve aparecer na notificação? (Fatos que não verificamos, nossas dúvidas sobre nossa causa, raciocínio estratégico, discussões de acordo anteriores)
- Uma única frase mal-redigida pode quebrar sigilo sobre análise correlata. Seja explícito sobre o que fica fora.

**14. Risco de admissão e quitação tácita (CC art. 320 / accord and satisfaction)**
- Algo na notificação que a contraparte poderia depois caracterizar como admissão de fato ou de responsabilidade?
- Esta notificação corre risco de inadvertidamente satisfazer (ou parecer aceitar) outra pretensão? (Aceitar pagamento "como quitação total" sem ressalva pode encerrar débito disputado — CC art. 320 sobre quitação.)

## Escrevendo o intake

### Slug

`[tipo]-[contraparte-curta]-[aaaa-mm]`. Confirme unicidade em `~/.claude/plugins/config/claude-for-legal/litigation-legal/demand-letters/`.

### `~/.claude/plugins/config/claude-for-legal/litigation-legal/demand-letters/[slug]/intake.md`

```markdown
[CABEÇALHO DE SIGILO — por config do plugin ## Outputs — varia por papel; vide `## Quem está usando`]

# Intake de Notificação Extrajudicial: [título]

**Slug:** [slug]
**Tipo de notificação:** [tipo]
**Redigida por:** [advogado(a) / Defensor(a)]
**Aberta:** [YYYY-MM-DD]
**Status:** intake | ready-to-draft | drafted | sent | closed
**Bloco estratégico:** answered | partial | skipped
**Razão do skip:** [se aplicável]

---

## Postura

- **Tom:** [moderado / assertivo / agressivo — com racional de uma linha amarrado à relação e ao valor]
- **Janela de resposta:** [N dias — amarrada à pretensão / contrato / protocolo]
- **Marcação:** [nenhuma / sem prejuízo / sem prejuízo das medidas judiciais / outra — com racional]
- **Signatário:** [nome / função — você / cliente / Diretor(a) Jurídico(a) / advogado(a) instruído(a) / Defensor(a) Público(a)]

*Esta é a postura por-caso capturada no intake. A skill de minuta lê daqui.*

---

## Partes

- **Remetente:** [nossa entidade / unidade DP]
- **Destinatário:** [contraparte, entidade, endereço]
- **Audiência do destinatário:** [quem lê]
- **Relação:** [tipo]

## Evento desencadeador

[O que aconteceu, quando, evidência]

## Base jurídica / contratual

[Cláusulas, lei de regência, leis aplicáveis]

## Pretensão

[Pedidos específicos em ordem de prioridade]

## Prazos

- **Externo:** [prescrição/decadência, janela de dano contínuo]
- **Compliance:** [quanto damos]

## Outreach anterior

[Histórico, mais recente primeiro]

## Distribuição

- **Entrega:** [método]
- **Signatário:** [nome/função]
- **Cópias:** [lista]

---

## Estratégico (se aplicável)

### Leverage & BATNA

[Nosso poder, resposta provável deles]

### Tolerância a downside

[Reputacional, precedente, regulatório, seguro]

### Postura de tom

[preservadora da relação / moderada / terra arrasada — com racional]

### Postura de comunicação negocial

[Protegida ou não no foro — com raciocínio. Cite fonte primária per a regra aplicável (Lei 13.140/2015 art. 30 ou equivalente).]

### Filtros de sigilo

[O que NÃO PODE aparecer na minuta]

### Risco de admissão / quitação tácita

[Riscos específicos sinalizados]

---

## Documentos-semente

| Doc | Path |
|---|---|
| [contrato subjacente] | [path ou "não compartilhado"] |
| [correspondência prévia] | [path ou "não compartilhada"] |
| [evidência] | [path ou "não compartilhada"] |

---

## Avaliação de materialidade

**Heurística automática diz:** [material / imaterial — com razão]
**Chamada do usuário:** [material / imaterial / TBD pós-envio]
```

## Confirme antes de gravar

Mostre ao usuário a minuta do intake. Sinalize qualquer coisa fina:

> Eis o intake. Notei [pontos finos]. Antes de eu gravar, algo a adicionar?

## Handoff para redação

Termine com:
> Intake salvo. Quando pronto: `/litigation-legal:demand-draft [slug]`

## Feche com a árvore de decisão de próximos passos

Feche com a árvore de decisão de próximos passos per CLAUDE.md `## Outputs`. Customize as opções para o que esta skill acabou de produzir — as cinco ramificações default (redigir o X, escalonar, pegar mais fatos, observar e esperar, outra coisa) são ponto de partida, não trava. A árvore É o output; o(a) advogado(a) escolhe.

## O que esta skill não faz

- Redige a notificação. Isso é `demand-draft` — os dois passos são intencionalmente separados para que o(a) advogado(a) possa pausar para input de negócio, consulta a externo, ou aviso de sinistro antes de redigir.
- Decide se enviar. Algumas sessões de intake terminam com "na verdade, não envie — vamos negociar direto". É desfecho válido; o registro de intake ainda tem valor.
- Roda a checagem de impedimentos. Se a contraparte é cliente ou entidade conhecida, sinalize que isto deve passar por impedimentos (per `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md`) antes de enviar — mas a checagem em si vive no workflow de matter-intake ou fora desta skill.
