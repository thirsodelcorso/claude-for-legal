---
name: oc-status
description: Gera minutas semanais de e-mail de status a escritórios externos / DPs colaboradoras / núcleos especializados no portfólio ativo — markdown por caso, mais drafts Gmail quando o MCP está disponível. Use quando o usuário pede status requests a externos, check-ins semanais, ou quer e-mails de status por caso a partir do log do portfólio.
argument-hint: "[--all | --slug=foo | --no-gmail]"
---

# /oc-status

Para rodar semanalmente, agende lembrete recorrente para invocar `/litigation-legal:oc-status`. Agendamento automatizado exige integração de tarefas agendadas, que não está bundled.

1. Carregue `~/.claude/plugins/config/claude-for-legal/litigation-legal/matters/_log.yaml`, filtre per regras default (ou per flags).
2. Carregue `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md` → estilo de diretrizes ao escritório externo / DP colaboradora, defaults de signatário, postura orçamentária.
3. Siga o workflow e a referência abaixo.
4. Para cada caso em escopo: leia `matter.md` + `history.md`, redija e-mail por caso.
5. Grave markdown em `~/.claude/plugins/config/claude-for-legal/litigation-legal/oc-status/[YYYY-MM-DD]/[slug].md`.
6. Se Gmail MCP autenticado: crie drafts no Gmail. Senão: só markdown, anote no sumário.
7. Grave `~/.claude/plugins/config/claude-for-legal/litigation-legal/oc-status/[YYYY-MM-DD]/_summary.md` — o que rodou, o que foi pulado e por quê.

---

# OC Status

## Propósito

Escrever o mesmo e-mail de status para escritório externo / núcleo / DP colaboradora toda semana em 5–15 casos é tributo cognitivo mecânico. O conteúdo é consistente por caso (status, decisões pendentes, checagem de orçamento). A audiência é consistente (responsável pelo caso). O tom é consistente (per estilo de diretrizes da casa). Tarefa agendada redige todos; o(a) advogado(a) revisa e envia.

## Carregar contexto

- `~/.claude/plugins/config/claude-for-legal/litigation-legal/matters/_log.yaml` — fonte de filtragem e campos
- `~/.claude/plugins/config/claude-for-legal/litigation-legal/matters/[slug]/matter.md` — contexto do caso (postura atual, perguntas em aberto)
- `~/.claude/plugins/config/claude-for-legal/litigation-legal/matters/[slug]/history.md` — eventos recentes para informar o que perguntar
- `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md` → estilo de diretrizes ao escritório externo / núcleo, nome/e-mail do signatário, postura orçamentária

## Filtragem — quais casos?

Filtro default:

- `status != closed`
- `outside_counsel.firm != null` AND `outside_counsel.lead != null` (escritório externo ou DP colaboradora / núcleo especializado)
- Ou: última atualização mais que 10 dias atrás (tempo para algo ter acontecido) OU tem `next_deadline` dentro de 21 dias

Pule casos que tiveram update de status nos últimos 10 dias (sem necessidade de re-ping) e casos onde `outside_counsel.email` é null (endereços de e-mail necessários para draft Gmail; ainda produz markdown).

Flags:
- `--all` → redige para todo caso ativo independente de recência
- `--slug=[slug]` → redige só para um caso (request ad-hoc)
- `--no-gmail` → pula criação de draft Gmail mesmo se MCP disponível

## Draft de e-mail por caso

Cada e-mail tem o mesmo esqueleto; conteúdo é específico do caso.

**Assunto:** per convenção da casa (de `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md` estilo de diretrizes; fallback: `[Caso: [nome do caso]] — Update semanal de status`)

**Esqueleto do corpo:**

```
[primeiro nome do(a) responsável],

[Uma frase de abertura — natural, condiz com tom da casa.]

Checando status sobre [nome do caso]. Alguns itens:

1. **Status desde [data do último update capturado em history.md]** — o que se moveu, o que está pendente? Alguma peça, audiência, correspondência ou ligação desde que falamos por último?

2. **Prazos próximos** — vejo [next_deadline do log + quaisquer prazos em matter.md]. Confirme plano de cobertura e qualquer data que devamos adicionar. (Lembrar prazo em dobro CPC art. 186 se Defensor.)

3. **Decisões pendentes** — [puxe perguntas em aberto de matter.md que exigem input externo; se nenhuma, omita este item numerado e renumere]

4. **Orçamento** — [mensal / trimestral / sob demanda per `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md` postura orçamentária]. Onde estamos contra [autorização de orçamento de matter.md]? Variância para sinalizar? (Para DP colaboradora / núcleo: orçamento não se aplica; substitua por "andamento e necessidade de apoio".)

[Se material e relevante: 5. Pedido específico — ex.: "Por favor envie a última minuta da contestação antes de [data]" — extraído de perguntas em aberto de matter.md.]

[Assinatura — nome, função, contato. De `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md` signatário default para diretrizes a externos.]
```

Adapte o tom per `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md` estilo de diretrizes — algumas casas são "Prezado(a) Doutor(a)" formal; outras são primeiro-nome-e-bullets. Combine.

## Output

### Drafts markdown

Grave em: `~/.claude/plugins/config/claude-for-legal/litigation-legal/oc-status/[YYYY-MM-DD]/[slug].md`

Cada arquivo é um e-mail, formatado como:

```markdown
[CABEÇALHO DE SIGILO — por config do plugin ## Outputs — varia por papel; vide `## Quem está usando`]

# [Nome do caso] — OC status request — [YYYY-MM-DD]

**Para:** [outside_counsel.email do log] ([outside_counsel.lead], [outside_counsel.firm])
**De:** [nome / e-mail do signatário de `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md`]
**Assunto:** [linha de assunto]

> O cabeçalho de sigilo acima se aplica a este registro interno. O corpo do e-mail abaixo vai para escritório externo / núcleo / DP colaboradora em caso patrocinado, o que é por si só comunicação sigilosa — aplique a marcação de sigilo da casa (`~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md` convenções de sigilo) no topo do e-mail enviado, tipicamente `Sigiloso — Art. 7º, XIX, Lei 8.906/94 — Comunicação Advogado-Cliente / Trabalho de Advogado` (ou referência à LC 80/94 art. 4º-A V para Defensor), não este cabeçalho interno.

---

[corpo per esqueleto]
```

### Send gate (nota de fechamento em todo draft)

Anexe o seguinte a cada draft markdown, imediatamente abaixo do corpo e acima dos metadados do run — remover antes de enviar:

> Esta é minuta de e-mail de status para revisão do(a) advogado(a) antes de enviar ao escritório externo / núcleo / DP colaboradora. Cheque conteúdo sigiloso que você não pretendia compartilhar fora do círculo, acurácia factual, tom, e postura orçamentária. Não envie sem revisão — até check-ins semanais rotineiros podem aflorar tese, estratégia ou concessões que o remetente não pretendia pôr por escrito.

### Drafts Gmail (se MCP disponível)

Se o MCP de criação de draft do Gmail está autenticado:

- Crie draft no Gmail do usuário por caso com `to`, `from`, `subject`, `body` populados
- O draft fica na pasta Drafts; usuário revisa e envia segunda-feira de manhã
- Se Gmail MCP NÃO está disponível ou falha: volte para só markdown e avise o usuário

### Sumário do run

Após processar todos os casos, grave `~/.claude/plugins/config/claude-for-legal/litigation-legal/oc-status/[YYYY-MM-DD]/_summary.md`:

```markdown
# OC Status Run — [YYYY-MM-DD]

**Casos processados:** [N]
**Drafts criados:** [N]
**Drafts Gmail:** [criados / pulados — razão]

## Redigido para

| Caso | Responsável externo | Última atualização | Razão da inclusão |
|---|---|---|---|
| [slug] | [responsável] | [data] | [defasado / prazo próximo / --all / --slug] |

## Pulado

| Caso | Razão |
|---|---|
| [slug] | update recente (último toque em [data]) |
| [slug] | sem e-mail externo no log — atualize com `/matter-update [slug]` |

## Anomalias

- Casos sem externo atribuído: [lista — se algum é de risco alto/crítico, sinalizado]
- Casos com externo mas sem e-mail no log: [lista]
```

## Agendamento

Esta skill é desenhada para rodar semanalmente. Agendamento automatizado exige integração de tarefas agendadas que não está bundled com o plugin. Para rodar semanalmente, agende lembrete recorrente para invocar `/litigation-legal:oc-status` — ex.: segunda de manhã no seu calendário.

Ad-hoc: `/oc-status` a qualquer hora. `/oc-status --slug=foo` para caso único.

## O que esta skill não faz

- **Envia os e-mails.** Só drafts. Advogado(a) revisa e envia.
- **Gera conteúdo que não tem.** Se `matter.md` é fino, o e-mail é curto e pergunta status amplo. A skill não inventa perguntas específicas do nada.
- **Tenta novamente em falhas.** Se criação de draft Gmail falha no meio do run, a skill loga a falha e continua com markdown. Usuário pode tentar de novo após corrigir auth.
- **Reescreve history.md.** Lê para contexto; não modifica. (Se a resposta do externo aflora eventos novos, use `/matter-update [slug]` para logá-los.)
- **Impõe template mínimo.** Se o tom da casa é "uma linha, primeiro nome, fim", o draft honra e pula a estrutura em bullets. Combine com `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md`.
