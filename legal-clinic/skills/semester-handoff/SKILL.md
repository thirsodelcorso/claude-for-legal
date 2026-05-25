---
name: semester-handoff
description: >
  Memos de handoff de caso de fim de termo/semestre — o espelho do /ramp.
  Produz memos de transição por caso e sumário de turma para que a turma que
  sai entregue o trabalho à turma entrante de forma limpa. Lê prazos, comms
  com assistidos(as), e histórico de caso. Use quando supervisor(a) ou
  estagiários(as) que saem precisam fechar o termo/semestre, construir memos
  de transição, ou offboarding de estagiário(a) que se desliga.
argument-hint: "[--semester=YYYY-term (default: current)] [--case=[case_id] (for a single case)]"
---

# /semester-handoff

1. Carregue `~/.claude/plugins/config/claude-for-legal/legal-clinic/CLAUDE.md` → perfil da unidade, datas do termo, estilo de supervisão.
2. Carregue `~/.claude/plugins/config/claude-for-legal/legal-clinic/deadlines.yaml` e `~/.claude/plugins/config/claude-for-legal/legal-clinic/client-comms/[case-id]/log.md` por caso.
3. Use o workflow abaixo.
4. Pegue lista de casos ativos como input (pergunte se a unidade não tem lista central). Mapeie owner que sai → owner que entra.
5. Gere memo de handoff por caso → `~/.claude/plugins/config/claude-for-legal/legal-clinic/handoffs/[termo]/[case_id].md`.
6. Gere sumário de turma → `~/.claude/plugins/config/claude-for-legal/legal-clinic/handoffs/[termo]/_summary.md`.
7. Rote conforme modelo de supervisão — fila formal / flags configuráveis / toque mais leve.

---

# Handoff de Termo/Semestre

## Propósito

Todo termo/semestre, unidades e NPJs perdem sua força de trabalho inteira e reconstroem. `/ramp` resolve metade do problema — faz onboarding da turma nova. Esta skill resolve a outra metade: faz offboarding da turma que sai, produzindo memos de handoff que capturam o que o(a) próximo(a) estagiário(a) precisa saber sobre todo caso ativo.

Sem isto, conhecimento do caso sai pela porta junto com o(a) estagiário(a). Estagiário(a) novo(a) começa da pasta do caso e sumário de intake, o que nunca é suficiente. Duas semanas são desperdiçadas re-aprendendo o caso antes que o(a) estagiário(a) novo(a) consiga fazer algo útil. O(a) assistido(a) experimenta o re-aprendizado como regressão — ligações ficam sem resposta enquanto o(a) novo(a) estagiário(a) corre atrás, perguntas já respondidas são re-perguntadas.

## Público

Supervisor(a) ou estagiários(as) que saem. Supervisor(a) roda para orquestrar o offboarding inteiro da turma; estagiários(as) individuais podem rodar nos seus próprios casos se estão transicionando no meio do termo (formatura, desligamento).

## Carregue contexto

- `~/.claude/plugins/config/claude-for-legal/legal-clinic/CLAUDE.md` → perfil da unidade, termo, áreas de atuação, estilo de supervisão
- `~/.claude/plugins/config/claude-for-legal/legal-clinic/deadlines.yaml` → todos os prazos ativos, agrupados por caso
- `~/.claude/plugins/config/claude-for-legal/legal-clinic/client-comms/[case-id]/log.md` (por caso) → histórico de comunicações
- Pastas de caso / sumários de intake que a unidade mantém
- Lista de estagiários(as) — quem é owner do quê indo para o handoff

## Workflow

### Passo 1: Identifique casos e owners

- Puxe todos os casos ativos (de registros de intake + case_ids em `~/.claude/plugins/config/claude-for-legal/legal-clinic/deadlines.yaml` + pastas de client-comms)
- Para cada caso: quem é o(a) estagiário(a) owner atual? Está ficando ou saindo?
- Mapeie: owner que sai → owner que entra (se conhecido(a); senão marque "TBD — supervisor(a) atribui")

Se a unidade não mantém lista central de casos ativos, a skill precisa de um input: lista dos casos ativos. Pergunte. Não chute.

### Passo 2: Memo de handoff por caso

Para cada caso:

```markdown
# Handoff de Caso — [nome do caso] — [termo encerrando]

**Case ID:** [case_id]
**Área de atuação:** [área]
**Estagiário(a) que sai:** [nome]
**Estagiário(a) entrante:** [nome ou "TBD"]
**Defensor(a)-Supervisor(a) / Professor(a)-Orientador(a):** [supervisor(a)]
**Assistido(a):** [nome ou ID do(a) assistido(a)]

---

## Onde estamos

[Um parágrafo: posição atual. O que foi feito, o que está pendente, para onde o caso está indo. Se está em ponto de pausa natural ou entre peças, diga.]

## Prazos pendentes

*Puxados de `~/.claude/plugins/config/claude-for-legal/legal-clinic/deadlines.yaml`. Primeiro trabalho do(a) estagiário(a) entrante é confirmar que estão corretos e atribuídos.*

| Devido | Tipo | Descrição | Notas |
|---|---|---|---|
| [data] | [tipo] | [uma linha] | [se apertado: "URGENTE — vence em [N] dias após início do termo"] |

## O que foi feito

- [Ações-chave neste termo: intake, protocolizações, audiências, correspondência principal]
- [Documentos produzidos — com pointers de onde vivem]

## O que está aberto

- [Decisões pendentes: ex.: "assistido(a) ainda não decidiu se aceita acordo proposto"]
- [Lacunas de pesquisa: ex.: "precisa confirmar se [tribunal] admite [remédio]"]
- [Comunicações abertas: ex.: "aguardando resposta de advogado(a) contrário(a)"]

## Relação com o(a) assistido(a)

- [Com que frequência o(a) estagiário(a) tem estado em contato? Telefone, e-mail, presencial?]
- [Qualquer contexto de relação que o(a) próximo(a) estagiário(a) deve saber: idioma preferido, notas de construção de confiança, circunstâncias que afetam agendamento]
- [Contato ou atendimentos planejados próximos]

## Documentos redigidos / protocolados

*Pointers, não conteúdo.*

- [Data] [Tipo de documento] — [caminho ou referência ao arquivo] — [status: protocolado / minutado / em fila de revisão]

## Sumário de histórico de comunicações

*De `~/.claude/plugins/config/claude-for-legal/legal-clinic/client-comms/[case-id]/log.md`. Sumário de três linhas aqui; estagiário(a) entrante lê o log completo.*

[Sumário curto dos padrões recentes de contato — ex.: "3 ligações desde intake, todas em português, assistido(a) prefere noites. Último contato: 2026-04-15, confirmou endereço para intimação de audiência."]

## Flags do(a) supervisor(a) para estagiário(a) entrante

*Adicionadas por revisão do(a) supervisor(a) antes do memo de handoff ir para o(a) estagiário(a) entrante. Pode incluir: "este caso tem dinâmica familiar sensível — leia o intake com cuidado antes de ligar para assistido(a)"; "assistido(a) pediu que toda correspondência vá para caixa postal e não residência"; "há questão de escopo aqui que não resolvemos — cheque comigo na semana 1."*

[flags, ou "nenhuma"]

## Prioridades de primeira semana para estagiário(a) entrante

1. [Específico — ex.: "Ligue para [assistido(a)] dentro de 48h de pegar o caso. Apresente-se. Confirme que recebeu a pasta."]
2. [Movido por prazo — ex.: "Contestação à ação de despejo vence [data]. Revise minuta do(a) estagiário(a) que saiu, revise, protocole."]
3. [Lacuna de conhecimento — ex.: "Leia memo do(a) estagiário(a) que saiu sobre a exceção de habitabilidade antes da audiência de conciliação em 28/04."]

---

**Handoff preparado por:** [estagiário(a) que sai]
**Data:** [AAAA-MM-DD]
**Revisado por:** [Defensor(a)-Supervisor(a) ou Professor(a)-Orientador(a), se aplicável conforme modelo de supervisão]
```

### Passo 3: Sumário de turma

Depois de todos os memos por caso, produza `~/.claude/plugins/config/claude-for-legal/legal-clinic/handoffs/[termo]/_summary.md`:

```markdown
# Sumário de Handoff de Turma — [termo encerrando]

**Estagiários(as) que saem:** [N]
**Estagiários(as) entrantes:** [N]
**Casos ativos transicionando:** [N]
**Casos encerrando no fim do termo (sem transição):** [N]

---

## Transições

| Caso | Sai | Entra | Área de atuação | Urgência |
|---|---|---|---|---|
| [case_id] | [nome] | [nome ou TBD] | [área] | [padrão / prazo em 2 semanas / urgente] |

## Não-atribuídos

[casos cujo(a) estagiário(a) entrante é "TBD" — supervisor(a) atribui antes do próximo termo]

## Prazos em 30 dias do início do termo

[puxados de deadlines.yaml — esses são os casos em que a turma nova entra correndo]

## Notas para supervisor(a)

- [Qualquer caso que levantou preocupação sobre performance do(a) estagiário(a), sinalizado para supervisão mais próxima]
- [Qualquer caso em que o(a) estagiário(a) que sai está disposto(a) a ficar de consulta — ex.: aluno(a) do último período que quer mentorar o(a) que assume]
- [Padrões entre handoffs — ex.: "três de seis casos têm prazos ativos nos primeiros 14 dias; considere antecipar exercícios de ramp nessas áreas de atuação"]
```

### Passo 4: Revisão do(a) supervisor(a) (se modelo de supervisão pede)

Encerrar caso ou transicionar para novo(a) estagiário(a) é ação consequente. O gate é o workflow de supervisão em `## Estilo de supervisão` em `~/.claude/plugins/config/claude-for-legal/legal-clinic/CLAUDE.md`, reforçado pela checagem de papel da Parte 0 confirmando que Defensor(a)-Supervisor(a) ou Professor(a)-Orientador(a) habilitado(a) é dono(a) do setup. Memos de encerramento de caso sempre pegam sign-off do(a) supervisor(a) antes do caso ser marcado encerrado no documento de handoff, independentemente da escolha de estilo de supervisão.

Conforme estilo de supervisão em `~/.claude/plugins/config/claude-for-legal/legal-clinic/CLAUDE.md`:

- **Fila de revisão formal:** todo memo de handoff entra na fila de revisão antes de release ao(à) estagiário(a) entrante. Supervisor(a) aprova, edita ou devolve.
- **Flags configuráveis:** memos carregam "CHECAR COM [SUPERVISOR(A)] ANTES DE CONFIAR" — supervisor(a) revisa informalmente, estagiário(a) responsável por procurar.
- **Toque mais leve:** memos carregam rótulo padrão de IA-assistida; supervisor(a) revisa via estrutura existente. Memos de encerramento de caso ainda roteiam para supervisor(a) antes do fechamento.

### Passo 5: Handoff

Uma vez revisados, memos de handoff vivem em `~/.claude/plugins/config/claude-for-legal/legal-clinic/handoffs/[termo]/[case_id].md`. O(a) estagiário(a) entrante lê durante seu `/ramp` no início do próximo termo — `/ramp` deve surface os memos dos casos para os quais o(a) estagiário(a) novo(a) é atribuído(a).

## Integração

- **`/ramp`:** no início do próximo termo, lê `~/.claude/plugins/config/claude-for-legal/legal-clinic/handoffs/[termo-mais-recente]/` e surface memos por caso dos casos que cada estagiário(a) novo(a) está pegando.
- **`/deadlines`:** alimenta a seção de prazos pendentes de cada memo.
- **`/client-comms-log`:** alimenta o sumário de histórico de comunicações.
- **`/supervisor-review-queue` (se revisão formal habilitada):** memos de handoff roteiam aqui para aprovação do(a) supervisor(a).

## O que esta skill não faz

- **Encerrar casos.** Handoff é para casos transicionando para a próxima turma. Casos encerrando no fim do termo devem ganhar memo de status interno final (`/legal-clinic:status internal`) para a pasta e ser marcados encerrados no documento de handoff; a skill status suporta audiências `client | internal | court`.
- **Atribuir estagiários(as) entrantes.** Supervisor(a) atribui. Skill registra qual é a atribuição; não escolhe.
- **Gerar handoffs do zero sem dados da unidade.** Precisa da lista de casos ativos como input. Se a unidade não mantém uma, a skill surface essa lacuna como bloqueador em vez de inventar.
- **Substituir uma conversa.** O memo escrito é o registro. O(a) estagiário(a) que sai também deve ter uma conversa com o(a) que entra quando viável — o memo captura fatos; uma conversa captura juízo e contexto de relação que o memo não captura.
