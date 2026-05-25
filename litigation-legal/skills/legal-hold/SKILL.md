---
name: legal-hold
description: Emite, renova, libera ou reporta sobre dever de guarda documental — redige a comunicação como .docx, atualiza campos legal_hold em _log.yaml, e agenda a próxima renovação. Use quando o usuário diz "emitir dever de guarda", "renovar dever de guarda", "liberar dever de guarda", ou pede relatório de status de guarda no portfólio.
argument-hint: "[slug] [--issue | --refresh | --release | --status]"
---

# /legal-hold

1. Se `--status` (sem slug): leia `_log.yaml`, produza relatório de status de guarda no portfólio.
2. Caso contrário: carregue `~/.claude/plugins/config/claude-for-legal/litigation-legal/matters/[slug]/matter.md` + linha do log.
3. Carregue `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md` → marcação de sigilo, ponteiro do template de dever de guarda, normas de escalonamento.
4. Siga o workflow e a referência abaixo.
5. Route por flag:
   - `--issue`: capture escopo, custodiantes, faixa de data, sistemas. Redija `legal-hold-v1.docx`. Atualize campos `legal_hold`. Anexe entrada de histórico. Defina `next_refresh` (default +6m).
   - `--refresh`: capture mudanças de escopo/custodiante. Redija próxima versão. Atualize `last_refresh` + `next_refresh`. Sinalize custodiantes desligados.
   - `--release`: capture data de liberação, instrução de retenção. Redija comunicação de liberação. Defina campo `released:`.
6. Confirme antes de gravar. Mostre ao usuário a minuta da comunicação e o diff do log.

---

# Dever de Guarda Documental (Legal Hold)

## Propósito

Dever de guarda é o documento mais mecânico de alto risco que advogado(a) interno(a) escreve. A comunicação em si é templada. Os failure modes são operacionais: emitida tarde demais, escopo estreito demais, nunca renovada, nunca liberada. Esta skill é dona das quatro fases: **emitir → renovar → (liberar) → rastrear**.

O portfólio já sinaliza guardas faltantes; esta skill as escreve.

## Assunção jurisdicional

Deveres de preservação variam materialmente por foro. No Brasil, a base normativa do dever de guarda decorre de:

- **CPC arts. 396-404** — exibição de documento ou coisa em poder de parte; consequências para recusa injustificada (CPC art. 400 — admissão de fato; CPC art. 403 — busca e apreensão);
- **CPC arts. 380-389** — exibição contra terceiros;
- **LGPD (Lei 13.709/2018)** — retenção de dados pessoais em hold precisa de base legal art. 7º VI (exercício regular de direitos em processo); art. 16 trata de eliminação após cumprida a finalidade;
- **Setores regulados específicos** — CVM (RCVM 80 — companhias listadas), Bacen (manuais), ANS (Resolução Normativa), ANEEL, etc.;
- **Improbidade administrativa (Lei 8.429/92, com alterações Lei 14.230/21)** — dever específico de guarda documental em órgão público.

O gatilho (quando o dever se atrai), escopo e exposição a sanções citados na minuta são leitura ponto-de-partida para o foro nomeado no caso — confirme com advogado(a) ou Defensor(a) antes de emitir, renovar, ou liberar.

## Carregar contexto

- `~/.claude/plugins/config/claude-for-legal/litigation-legal/matters/_log.yaml` — linha do log (campos legal_hold + status)
- `~/.claude/plugins/config/claude-for-legal/litigation-legal/matters/[slug]/matter.md` — contexto do caso (contraparte, fatos, custodiantes-chave de internal_owners)
- `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md` — estilo da casa para ponteiro de template de dever de guarda, marcação de sigilo, normas de escalonamento

**Gate de impedimentos — incontornável.** Antes de emitir, renovar, ou liberar um dever de guarda, cheque `_log.yaml` para o slug. Se o caso não está em `_log.yaml`, recuse e route:

> "Não vejo [slug do caso] no log de casos. Rode `/litigation-legal:matter-intake` primeiro para a checagem de impedimentos rodar e o workspace ser montado. Não emito, renovo, ou libero dever de guarda em caso não-intaken — a checagem de impedimentos é o gate, e dever de guarda emitido contra caso não-gerido não tem linha de `_log.yaml` para rastrear `last_refresh` / `next_refresh` / `released`."

Não prossiga em caso não-intaken. Intake é o que roda impedimentos e grava a linha de `_log.yaml` contra a qual `--refresh` / `--release` / `--status` operam.

## Modos

O comando aceita uma flag: `--issue | --refresh | --release | --status`. Default (sem flag) → pergunta.

### `--issue` — primeira emissão

Exigido quando `legal_hold.issued == false` e o caso é ativo ou razoavelmente antecipado.

**Antes de emitir o dever a custodiantes (o ato consequencial):** Leia `## Quem está usando` em `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md`. Se o Papel é Não-advogado:

> Emitir dever de guarda tem consequências jurídicas — escopo, lista de custodiantes, e timing criam o registro de preservação contra o qual a empresa será julgada se houver alegação de espoliação depois. Você revisou com advogado(a) ou Defensor(a) Público(a)? Se sim, prossiga. Se não, segue brief para levar:
>
> [Gere sumário de 1 página: o caso e gatilho, escopo proposto e custodiantes, regra de preservação aplicável pesquisada, exposição a espoliação conhecida, o que pode dar errado (amplo demais / estreito demais), o que perguntar ao(à) advogado(a).]
>
> Se precisa achar advogado(a) habilitado(a) ou Defensor(a) Público(a) na sua localidade: o serviço de referência da OAB Seccional do estado (ou da Defensoria Pública Estadual/União) é o ponto de partida mais rápido.

Não envie a comunicação sem um sim explícito. Redigir e escopar não exigem o gate — emissão exige.

**Pesquise a regra de preservação aplicável antes de emitir.** Identifique a jurisdição e a fonte do dever de preservação (CPC 396-404, CPC 380-389, dever regulatório, contratual). Confirme o gatilho operativo atualmente (quando o dever se atrai), o padrão de escopo (o que deve ser preservado), e exposição a sanções (CPC art. 400 — admissão; CPC art. 403 — busca e apreensão; sanções regulatórias específicas). Cite fontes primárias. Note que regime federal e estadual podem diferir materialmente; para órgão público, Lei 8.429/92 traz dever específico. Em incerteza, diga e obtenha sign-off externo antes de emitir.

> **Entregável externo:** a comunicação abaixo é enviada a custodiantes. NÃO inclua cabeçalho `SIGILOSO — TRABALHO DE ADVOGADO — PREPARADO SOB DIREÇÃO DE ADVOGADO HABILITADO` na comunicação enviada; use a marcação advogado-cliente no template. Confirme a marcação correta para sua jurisdição e caso.

**Inputs:**
1. **Escopo** — categorias de documentos, dados, comunicações. Comece específico: contratos com contraparte, todas as comunicações referenciando [projeto/objeto], registros financeiros relacionados, entradas de calendário. `[SME VERIFICAR — escopo amplo demais = carga operacional; estreito demais = risco de espoliação]`
2. **Custodiantes** — indivíduos nomeados provavelmente com material responsivo. Puxe sugestões de internal_owners de matter.md e de cargos comuns (líder de negócio, parceiro de RH se trabalhista, CISO se dados). `[SME VERIFICAR — a lista de custodiantes é a diferença entre preservação defensável e argumento de lacuna]`
3. **Faixa de data** — quando começar a preservar (usualmente: evento desencadeador ou antes), até o presente + em andamento.
4. **Sistemas** — e-mail, Slack/Teams, file shares, dispositivos (incluindo BYOD se aplicável), Jira/Asana, CRM, sistemas legados.
5. **Urgência** — se ação já distribuída ou notificação recebida com ameaça de processo, vai hoje.
6. **Data efetiva** — data do dever de guarda.

**Redija a comunicação** a cada custodiante, usando o template da casa em `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md` se configurado; caso contrário, o template default abaixo.

**Template default de dever de guarda:**

```
[SIGILOSO — COMUNICAÇÃO ADVOGADO-CLIENTE — ART. 7º XIX LEI 8.906/94]

DATA: [data efetiva]
PARA: [nome do(a) custodiante]
DE: [signatário — per default de `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md`]
ASSUNTO: COMUNICAÇÃO DE DEVER DE GUARDA DOCUMENTAL — [nome curto do caso]

Você está recebendo esta comunicação porque [empresa / parte] determinou que [descrição de uma frase da disputa / investigação, evitando detalhe prejudicial]. A lei (CPC arts. 396-404; LGPD art. 7º VI; regulamentação setorial aplicável) exige preservação de documentos e comunicações potencialmente relevantes a esta matéria.

EFETIVO IMEDIATAMENTE, você deve preservar:

1. Todos os documentos, e-mails, mensagens de texto, mensagens Slack/Teams,
   e outras comunicações relacionadas a [bullet de escopo 1].
2. [bullet de escopo 2]
3. [bullet de escopo 3]
...

Este dever de preservação aplica a:
- E-mail (incluindo enviados, arquivados, pasta de excluídos)
- Slack/Teams/plataformas de mensageria
- Drives compartilhados e armazenamento em nuvem
- Dispositivos pessoais usados para negócio da empresa (BYOD)
- Documentos físicos
- Mensagens de voz
- Entradas de calendário e notas de reunião

NÃO:
- Apague, modifique, destrua ou descarte qualquer material potencialmente responsivo
- Auto-delete ou "Inbox Zero" qualquer e-mail ou mensagem

Coordene com [contato jurídico] antes de compartilhar esta comunicação com
seus subordinados diretos ou TI.

Direcione perguntas sobre esta comunicação ou suas obrigações de preservação
a [contato jurídico]. Você pode continuar a discutir o objeto comercial
subjacente com colegas conforme necessário para seu trabalho, mas não
discuta esta comunicação jurídica, o litígio, ou estratégia jurídica.

SE VOCÊ NÃO TEM CERTEZA se algo é coberto, ERRE PELO LADO DA PRESERVAÇÃO.

Por favor confirme recebimento desta comunicação por [resposta / link / form]
em até três dias úteis. Se tiver dúvidas, contate [e-mail do signatário].

Esta comunicação permanece em vigor até você receber comunicação escrita de
sua liberação. Você pode ser solicitado a reafirmar compliance em intervalos
periódicos.

[Bloco de assinatura do signatário]
```

**Gate de envio (nota de fechamento na minuta):** Anexe ao preview in-chat da comunicação — removido antes da comunicação ir a custodiantes:

> Esta é minuta de dever de guarda para revisão de advogado(a), não comunicação pronta para emitir. Emitir um dever dispara obrigações de preservação contra as quais a parte será julgada em qualquer argumento posterior de espoliação, e a comunicação em si pode ser objeto de exibição. Um(a) advogado(a) habilitado(a) revisa, aprova, e emite. Não distribua esta minuta sem revisão.

**Grava:**
- `~/.claude/plugins/config/claude-for-legal/litigation-legal/matters/[slug]/legal-hold-v1.docx` via a skill `docx`
- Anexa a `~/.claude/plugins/config/claude-for-legal/litigation-legal/matters/[slug]/history.md`:
  ```
  ## [YYYY-MM-DD] — Dever de guarda emitido

  Dever emitido a [N] custodiantes: [lista].
  Escopo: [sumário de uma linha].
  Próxima renovação: [YYYY-MM-DD (default emitido + 6 meses)].
  ```
- Atualiza linha de `_log.yaml`:
  ```yaml
  legal_hold:
    issued: true
    issued_date: [YYYY-MM-DD]
    scope: "[sumário de uma linha]"
    custodians: [lista]
    last_refresh: [YYYY-MM-DD]   # mesmo que issued_date na primeira emissão
    next_refresh: [YYYY-MM-DD]   # default: issued_date + 6 meses
    released: null
  ```

### `--refresh` — reafirmação periódica

Cadência de renovação: default 6 meses; ajustável por caso. Quando `next_refresh < hoje` (ou usuário invoca manualmente), a skill redige comunicação de renovação.

**Inputs:**
1. Quaisquer **mudanças de escopo** desde última renovação (novos tópicos afloraram em instrução, novos custodiantes, novos sistemas).
2. Quaisquer **custodiantes a adicionar ou remover** (desligamentos exigem tratamento especial — vide abaixo).
3. Linguagem de re-confirmação.

**Template de comunicação de renovação:** similar à emissão; abre com "Esta é reafirmação do dever de guarda originalmente emitido em [data]." Lista escopo atual (aditado se necessário). Pede re-confirmação.

**Custodiantes desligados:** se um(a) custodiante deixou a empresa desde a última renovação, a skill sinaliza isto como item de ação de preservação — os arquivos e arquivo de e-mail do(a) empregado(a) desligado(a) precisam ser preservados em nível de TI, não só via comunicação ao indivíduo. Registra em history.md como entrada separada exigindo ação.

**Grava:**
- `~/.claude/plugins/config/claude-for-legal/litigation-legal/matters/[slug]/legal-hold-v[N].docx` (próximo número de versão)
- Entrada em `history.md`
- `_log.yaml`: atualiza campos `last_refresh` e `next_refresh`; modifica lista `custodians` se mudou

### `--release` — fechar o dever

Usualmente no fechamento do caso. Confirme que o caso está verdadeiramente acabado (não em recurso, não provável de reabrir, prescrição/decadência passou sobre pretensões relacionadas).

**Antes de liberar o dever (o ato consequencial — obrigações de preservação retornam à retenção normal):** Leia `## Quem está usando` em `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md`. Se o Papel é Não-advogado:

> Liberar dever de guarda tem consequências jurídicas — uma vez liberado, custodiantes podem começar a apagar material. Liberar no momento errado cria exposição a espoliação. Você revisou com advogado(a) ou Defensor(a) Público(a)? Se sim, prossiga. Se não, segue brief para levar:
>
> [Gere sumário de 1 página: status do caso, por que liberar é proposto agora, exposição a pretensão relacionada / recurso / prescrição, impacto em custodiantes, o que pode dar errado, o que perguntar ao(à) advogado(a).]
>
> Se precisa achar advogado(a) habilitado(a) ou Defensor(a) Público(a) na sua localidade: o serviço de referência da OAB Seccional do estado (ou da Defensoria Pública Estadual/União) é o ponto de partida mais rápido.

Não envie a comunicação de liberação sem um sim explícito.

**Inputs:**
1. Confirmação de autoridade de liberação (usualmente o(a) signatário(a) ou Diretor(a) Jurídico(a)).
2. Data de liberação.
3. Instrução de retenção — o que acontece com o material que estava sob guarda? (Retorna a retenção normal? Continua preservando por período definido? Transfere para arquivo?)

**Template de comunicação de liberação:** um parágrafo, formal. "O dever de guarda emitido em [data] referente a [caso] é liberado efetivo em [data]. Retenção normal retomada."

**Grava:**
- `~/.claude/plugins/config/claude-for-legal/litigation-legal/matters/[slug]/legal-hold-release.docx`
- Entrada em `history.md`
- `_log.yaml`: define `released: [YYYY-MM-DD]`

### `--status` — relatório no portfólio

Leia `_log.yaml`. Produza relatório:

```markdown
# Status de Dever de Guarda — [hoje]

## Deveres ativos

| Caso | Emitido | Última renovação | Próxima renovação | Custodiantes | Status |
|---|---|---|---|---|---|
| [slug] | [data] | [data] | [data] | [N] | [ok / ⚠️ renovação devida / ❌ atrasada] |

## ⚠️ Atenção

- **Renovação atrasada:** [liste slugs onde next_refresh < hoje]
- **Renovação devida em 30 dias:** [liste]
- **Casos ativos sem dever emitido:** [liste — alto/crítico primeiro]
- **Casos fechados com dever ainda ativo:** [liste — considere liberar]

## Recentemente liberados

[últimos 5 deveres liberados com datas]
```

Esta é invocação separada de comando (`/legal-hold --status` sem slug) OU invocada por `/portfolio-status` como seção no rollup de portfólio.

## Integração com portfolio-status

A skill `portfolio-status` já sinaliza "Dever não emitido em litígio ativo". Esta skill é o que resolve essas flags. Vale referenciar no briefing quando um caso é aberto: se `legal_hold.issued == false`, `/matter-intake` fecha oferecendo rodar `/legal-hold --issue`.

## O que esta skill não faz

- **Impõe preservação.** Emite a comunicação; TI/custodiantes preservam. A skill sinaliza quando custodiante desliga (para TI preservar em nível de sistema) mas não alcança sistemas.
- **Faz chamadas de escopo sozinha.** A skill propõe escopo do contexto do caso; o usuário confirma. Escopo amplo demais = carga operacional. Escopo estreito demais = risco de espoliação. Juízo do usuário.
- **Auto-renova sem revisão.** Mesmo quando `next_refresh` chega, o usuário revisa mudanças de escopo antes da comunicação de renovação sair.
- **Envia a comunicação.** Redige .docx; usuário envia per convenção da casa. (Integração futura: MCP Gmail/O365 poderia enviar diretamente após revisão.)
