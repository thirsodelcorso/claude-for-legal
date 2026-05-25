---
name: status
description: >
  Sumário de status do caso por audiência — voltado ao(à) assistido(a)
  (linguagem simples), interno (para o(a) supervisor(a)), ou court-ready (em
  formato de petição com cabeçalho próprio conforme regimento local). Mesmos
  fatos, enquadramento e profundidade diferentes. Use quando estagiário(a)
  precisa atualizar assistido(a), informar supervisor(a), ou preparar petição
  de informação de andamento ao juízo.
argument-hint: "[client | internal | court]"
---

# /status

1. Carregue `~/.claude/plugins/config/claude-for-legal/legal-clinic/CLAUDE.md` → estilo de supervisão, padrões de linguagem simples, jurisdição.
2. Use o workflow abaixo. Leia notas do caso.
3. Gere para a audiência especificada:
   - `client` — linguagem simples, o que aconteceu/próximos passos/o que você faz/como contatar
   - `internal` — posição processual, feito desde último check-in, próximos passos, precisa de input do(a) supervisor(a), avaliação do(a) estagiário(a)
   - `court` — peça formal de informação ao juízo em formato com cabeçalho próprio conforme regimento local
4. Roteamento de supervisão por audiência (voltado ao(à) assistido(a) e ao juízo geralmente sinalizam).

```
/legal-clinic:status client
```

```
/legal-clinic:status internal
```

```
/legal-clinic:status court
```

---

# Status: Sumários de Caso por Audiência

## Propósito

Unidades de DP e NPJs geram quantidades enormes de atualizações de status — ao(à) assistido(a), ao(à) supervisor(a), ao(à) co-advogado(a), ao juízo. Mesmo caso, mesmos fatos, documentos completamente diferentes. Esta skill pega as notas do caso e produz o sumário certo para o(a) leitor(a) certo(a).

## Carregue contexto

`~/.claude/plugins/config/claude-for-legal/legal-clinic/CLAUDE.md` → estilo de supervisão, padrões de linguagem simples (para voltado ao(à) assistido(a)), jurisdição.
Notas do caso para fatos.

## Modos de audiência

### Voltado ao(à) assistido(a)

**Leitor(a):** O(a) assistido(a). Provavelmente estressado(a). Possivelmente não-familiarizado(a) com processo judicial. Nível de leitura conforme padrões de linguagem simples em `~/.claude/plugins/config/claude-for-legal/legal-clinic/CLAUDE.md` (default ensino fundamental II — 6º-9º ano), em cumprimento ao dever de informar com clareza (LC 80/94 art. 4º-A III).

**Inclua:**
- O que aconteceu desde a última vez que ouviu da unidade
- O que vem em seguida e quando
- O que (se algo) precisa fazer
- Como contatar a unidade

**Não inclua:**
- Análise jurídica (não precisam saber o FIRAC)
- Pontos fracos do caso (a não ser que seja hora dessa conversa — e isso é decisão do(a) supervisor(a), não de atualização de status)
- Jargão

*Rótulo de revisão para o(a) estagiário(a) (não para o(a) assistido(a) — retirar antes de enviar):*
`[MINUTA ASSISTIDA POR IA — exige análise do(a) estagiário(a) e revisão do(a) supervisor(a)]`

Cheque a norma de regência do estágio (LC 80/94 art. 4º §6º para DP; Resolução CNE/CES 5/2018 + regimento da IES + convênio para NPJ acadêmico) para o texto de identificação de estagiário(a) exigido.

```markdown
Prezado(a) [Assistido(a)],

Queria te dar uma atualização sobre seu caso.

**O que aconteceu:** [Português simples. "Protocolamos sua contestação no juízo
em [data]" não "Foi apresentada a peça contestatória."]

**Próximos passos:** [O quê e quando. "O juízo marcou audiência para [data] às
[hora]. Você precisa estar presente." Ou: "Estamos aguardando a resposta do(a)
advogado(a) do(a) locador(a). Isso pode levar algumas semanas."]

**O que você precisa fazer:** [Específico e claro. Ou: "Nada agora — a gente
te avisa quando precisar de algo."]

**Como nos contatar:** [Telefone da unidade, horário, nome do(a) estagiário(a)]

[Nome do(a) estagiário(a)]
Estagiário(a) de Direito, inscrito(a) na OAB [seccional/nº] sob LC 80/94 art. 4º §6º
Sob a supervisão de [Defensor(a)-Supervisor(a) ou Professor(a)-Orientador(a)]
[Nome da unidade / NPJ]
```

**Antes de enviar:** enviar atualização de status ao(à) assistido(a) é ação consequente. O gate é o workflow de supervisão em `## Estilo de supervisão` em `~/.claude/plugins/config/claude-for-legal/legal-clinic/CLAUDE.md`, reforçado pela checagem de papel da Parte 0 confirmando que Defensor(a)-Supervisor(a) ou Professor(a)-Orientador(a) habilitado(a) é dono(a) do setup. Confirme que a minuta foi revisada conforme o protocolo de supervisão (fila / flag / toque mais leve) e todos os rótulos internos de revisão (`[MINUTA ASSISTIDA POR IA]`, `[VERIFICAR]`, etc.) foram removidos da cópia voltada ao(à) assistido(a).

### Interno (para o(a) supervisor(a))

**Leitor(a):** O(a) Defensor(a)-Supervisor(a) ou Professor(a)-Orientador(a). Sabe a lei. Quer saber onde o caso está e o que o(a) estagiário(a) precisa.

**Inclua:**
- Posição processual (onde na vida do caso)
- O que foi feito desde último check-in
- O que vem em seguida (prazos, audiências)
- Questões precisando de input do(a) supervisor(a)
- Avaliação do(a) estagiário(a) (como está indo, preocupações)

```markdown
# Status: [Assistido(a)] — [Matéria] — [data]

**Estagiário(a):** [nome] | **Posição processual:** [pré-protocolo /
contestação apresentada / instrução / impugnação pendente / sentença / etc.]

## Desde último check-in

- [O que foi feito]

## Próximos passos

| Data | O quê | Ação necessária até |
|---|---|---|
| [data] | [prazo/audiência] | [data] |

## Precisa de input do(a) supervisor(a)

- [Questão ou ponto de decisão — específico]

## Avaliação do(a) estagiário(a)

[Como está indo. Pontos fortes, preocupações, questões estratégicas. Aqui é
onde o pensamento do(a) estagiário(a) aparece.]

---
[MINUTA ASSISTIDA POR IA — estagiário(a) deve revisar especialmente a seção de
avaliação; é seu pensamento, não sumário de notas]
```

### Court-ready

**Leitor(a):** Magistrado(a) ou serventuário(a). Formal. Específica para o que o juízo precisa (frequentemente uma petição de informação de andamento determinada pelo juízo, ou manifestação prévia à audiência).

**Inclua:**
- Histórico processual (brevemente)
- Status atual de instrução/impugnações/conciliação
- O que está pendente
- Próximos passos propostos ou agendamento

**Formato:** Conforme regimento local. Cabeçalho com endereçamento, qualificação, bloco de assinatura, certidão se aplicável.

```markdown
═══════════════════════════════════════════════════════════════════════
  MINUTA ASSISTIDA POR IA — exige análise do(a) estagiário(a) e revisão
  do(a) supervisor(a)
  Peças a protocolar SEMPRE exigem revisão do(a) supervisor(a) antes do
  protocolo
═══════════════════════════════════════════════════════════════════════

[Cabeçalho conforme jurisdição — VERIFICAR contra regimento local atual]

PETIÇÃO DE INFORMAÇÃO DE ANDAMENTO

[Parte], por sua Defensoria Pública / Núcleo de Prática Jurídica, vem
respeitosamente apresentar a presente informação de andamento [em
cumprimento ao despacho de [data] / nos termos do art. [X] do regimento /
em vista da audiência marcada para [data]].

1. Histórico processual: [breve]

2. Status atual: [status de instrução / status de impugnações / status de
   conciliação]

3. Matérias pendentes: [o que está pendente]

4. Próximos passos propostos: [agendamento, se o juízo quer input]

[Bloco de assinatura — estagiário(a) de Direito sob supervisão de
[Defensor(a)-Supervisor(a) ou Professor(a)-Orientador(a)], indicando
inscrição OAB sob LC 80/94 art. 4º §6º se na DP, ou estágio acadêmico se
em NPJ]

[Certidão se aplicável]

---

[VERIFICAR: formato de cabeçalho, requisitos locais de petição de
informação, requisitos de intimação — conforme regimento [Tribunal] atual]
```

## Roteamento de supervisão

Conforme `~/.claude/plugins/config/claude-for-legal/legal-clinic/CLAUDE.md`:
- Voltado ao(à) assistido(a) → geralmente gatilho de flag (comunicação com assistido(a))
- Interno → sem flag (vai para o(a) supervisor(a) de qualquer forma)
- Court-ready → sempre sinalizado se fila formal habilitada (peças a juízo)

## O que esta skill NÃO faz

- **Decidir o que dizer ao(à) assistido(a).** Especialmente sobre más notícias ou pontos fracos do caso — isso é conversa para o(a) estagiário(a) e supervisor(a) terem, depois o(a) estagiário(a) ter com o(a) assistido(a). Atualizações de status são status, não parecer estratégico.
- **Protocolar qualquer coisa no juízo.** Redige o documento; supervisor(a) revisa; protocolo conforme procedimento da unidade.
- **Substituir a avaliação do(a) estagiário(a) no status interno.** A seção "avaliação do(a) estagiário(a)" é o pensamento do(a) estagiário(a) — a minuta pode escafoldar mas não pode escrever.

## Encerre com a árvore de próximos passos

Encerre com a árvore de próximos passos conforme CLAUDE.md `## Outputs`. Customize as opções para o que esta skill acabou de produzir — os cinco branches default (redigir o X, escalonar, pegar mais fatos, observar e esperar, outra coisa) são ponto de partida, não lock-in. A árvore é o output; o(a) supervisor(a) escolhe.

