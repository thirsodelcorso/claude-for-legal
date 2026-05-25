---
name: client-letter
description: >
  Correspondência de rotina com o(a) assistido(a) a partir de templates —
  confirmações de atendimento, pedidos de documento, atualizações breves do
  tipo "protocolamos". Linguagem simples, elementos obrigatórios, roteamento de
  supervisão. NÃO é parecer substantivo. Use quando um(a) estagiário(a) precisa
  enviar correspondência de rotina, confirmação de atendimento, carta de pedido
  de documentos, ou nota breve de status ao(à) assistido(a).
argument-hint: "[appointment | doc-request | update]"
---

# /client-letter

1. Carregue `~/.claude/plugins/config/claude-for-legal/legal-clinic/CLAUDE.md` → padrões de linguagem simples, estilo de supervisão, dados de contato da unidade.
2. Use os templates e o workflow abaixo.
3. Case tipo com template. Checagem de linguagem simples.
4. Output com rótulo IA-assistida, roteamento de supervisão.

Escopo: rotina apenas. Parecer substantivo → `/status client` ou conversa com o(a) supervisor(a).

```
/legal-clinic:client-letter appointment
```

```
/legal-clinic:client-letter doc-request
```

---

# Carta ao(à) Assistido(a): Correspondência de Rotina

## Propósito

Unidades de DP e NPJs mandam muita correspondência de rotina: "seu atendimento é terça às 14h," "favor trazer seu contrato de locação," "protocolamos sua contestação." Esta skill cuida dessas a partir de templates para que estagiários(as) não fiquem digitando a mesma carta toda semana.

**Escopo: rotina apenas.** Parecer substantivo, má notícia, estratégia de caso — essas são `/status client` ou conversa, não carta-template.

## Carregue contexto

`~/.claude/plugins/config/claude-for-legal/legal-clinic/CLAUDE.md` → padrões de linguagem simples, estilo de supervisão, dados de contato da unidade.

## Checagem pedagógica

Leia o guia do(a) supervisor(a) para esta área de atuação em `~/.claude/plugins/config/claude-for-legal/legal-clinic/guides/<area-de-atuacao>.md`. Cheque o setting `pedagogy_posture`:

- **`guide` (default):** Produza a estrutura e o checklist (elementos obrigatórios, alvos de linguagem simples, sign-off conforme norma de regência do estágio). Peça ao(à) estagiário(a) para redigir cada seção. Dê feedback no que escreveu (registro, nível de leitura, elementos obrigatórios, o que faltou). Ofereça preencher uma seção só depois que o(a) estagiário(a) tentou uma vez.
- **`assist`:** Produza a carta. Sinalize itens para revisão do(a) estagiário(a). Estagiário(a) edita e aprende revisando.
- **`teach`:** Não produza a carta. Peça ao(à) estagiário(a) para redigir. Dê feedback. Faça perguntas leading quando travam. Só mostre um parágrafo-modelo depois de duas tentativas, e só na seção em que está travado(a). Acompanhe o que acertou e errou para que o(a) supervisor(a) veja progresso.

Se nenhum guia existe, use `guide`. Se o guia existe mas não seta postura, use `guide`.

Qualquer que seja a postura, o output sempre inclui: "**Modo pedagógico: [assist/guide/teach]** — setado pelo guia do(a) seu(sua) supervisor(a). Isso significa que eu [descrição do que o(a) estagiário(a) fez vs. o que a skill fez]."

## Sign-off e identificação como estagiário(a)

Cheque a norma de regência do estágio (LC 80/94 art. 4º §6º para estágio na DP; Resolução CNE/CES 5/2018 + regimento da IES + convênio para NPJ acadêmico) para o texto de identificação exigido em cartas assinadas por estagiário(a). Estagiários(as) na DP devem se identificar como estagiários(as) inscritos(as) na OAB sob LC 80/94 art. 4º §6º e identificar o(a) Defensor(a)-Supervisor(a); em NPJ, identificar como estagiário(a) acadêmico(a) sob orientação do(a) Professor(a)-Orientador(a) e a IES. Os templates abaixo usam forma genérica — conforme o sign-off à sua norma antes de enviar.

## Tipos de carta

> **Rótulo de revisão fica FORA da carta.** A tag `[MINUTA ASSISTIDA POR IA — exige análise do(a) estagiário(a) e revisão do(a) supervisor(a)]` é nota ao(à) estagiário(a), não parte do corpo da carta. Coloque acima do template renderizado (ou em cabeçalho que o(a) estagiário(a) deleta antes de enviar), nunca dentro do conteúdo da carta. Se acabar na cópia que vai ao(à) assistido(a), a skill falhou.

### Confirmação de atendimento

*Rótulo de revisão para o(a) estagiário(a) (não para o(a) assistido(a) — retirar antes de enviar):*
`[MINUTA ASSISTIDA POR IA — exige análise do(a) estagiário(a) e revisão do(a) supervisor(a)]`

```markdown
Prezado(a) [Assistido(a)],

Confirmamos seu atendimento com [Nome da unidade / NPJ]:

**Data:** [data]
**Horário:** [hora]
**Onde:** [endereço / sala / ou "por telefone no [número]"]
**Com:** [nome do(a) estagiário(a)]

**Por favor traga:** [documentos necessários — das notas do caso ou deixar
como prompt para o(a) estagiário(a) preencher]

Se precisar remarcar, ligue para nós em [telefone da unidade] com no mínimo 24
horas de antecedência.

[Nome do(a) estagiário(a)]
Estagiário(a) de Direito, inscrito(a) na OAB [seccional/nº] sob LC 80/94 art. 4º §6º
Sob a supervisão de [Defensor(a)-Supervisor(a) ou Professor(a)-Orientador(a)]
[Nome da unidade / NPJ] | [telefone] | [horário de atendimento]
```

### Pedido de documentos

*Rótulo de revisão para o(a) estagiário(a) (não para o(a) assistido(a) — retirar antes de enviar):*
`[MINUTA ASSISTIDA POR IA — exige análise do(a) estagiário(a) e revisão do(a) supervisor(a)]`

```markdown
Prezado(a) [Assistido(a)],

Para dar andamento ao seu caso, precisamos dos seguintes documentos:

- [Documento 1 — ex.: "Seu contrato de locação"]
- [Documento 2 — ex.: "A notificação que você recebeu do(a) locador(a)"]
- [Documento 3]

**Como nos entregar:** [trazer pessoalmente à unidade / enviar por e-mail para
[endereço] / trazer no próximo atendimento]

**Por favor envie até:** [data — se houver prazo, explique por quê: "Precisamos
desses documentos até [data] para protocolar sua contestação antes do prazo do
juízo."]

Se você não tem algum desses ou não sabe a que estamos nos referindo, ligue
para nós em [telefone da unidade] e a gente ajuda.

[Nome do(a) estagiário(a)]
Estagiário(a) de Direito, inscrito(a) na OAB [seccional/nº] sob LC 80/94 art. 4º §6º
Sob a supervisão de [Defensor(a)-Supervisor(a) ou Professor(a)-Orientador(a)]
[Nome da unidade / NPJ] | [telefone] | [horário de atendimento]
```

### Atualização breve de status

Para atualizações de rotina do tipo "protocolamos" / "estamos aguardando". (Atualizações mais cheias → `/status client`.)

*Rótulo de revisão para o(a) estagiário(a) (não para o(a) assistido(a) — retirar antes de enviar):*
`[MINUTA ASSISTIDA POR IA — exige análise do(a) estagiário(a) e revisão do(a) supervisor(a)]`

```markdown
Prezado(a) [Assistido(a)],

Atualização rápida: [uma linha do que aconteceu — "Protocolamos sua contestação
no juízo em [data]" / "Enviamos a notificação extrajudicial ao(à) locador(a)
em [data]"].

**Próximos passos:** [uma linha — "Estamos aguardando a resposta deles(as)" /
"O juízo vai marcar a audiência e nos informar a data"].

Você não precisa fazer nada agora. A gente te avisa quando precisar.

[Nome do(a) estagiário(a)]
Estagiário(a) de Direito, inscrito(a) na OAB [seccional/nº] sob LC 80/94 art. 4º §6º
Sob a supervisão de [Defensor(a)-Supervisor(a) ou Professor(a)-Orientador(a)]
[Nome da unidade / NPJ] | [telefone] | [horário de atendimento]
```

## Antes de enviar

Enviar carta ao(à) assistido(a) é ação consequente. O gate deste plugin é o workflow de supervisão descrito em `## Estilo de supervisão` em `~/.claude/plugins/config/claude-for-legal/legal-clinic/CLAUDE.md`, reforçado pela checagem de papel da Parte 0 que confirma que um(a) Defensor(a)-Supervisor(a) ou Professor(a)-Orientador(a) habilitado(a) é dono(a) do setup da unidade. Esse gate continua valendo: toda carta passa pela revisão antes de sair da unidade.

Antes de enviar qualquer carta acima, confirme:

1. A minuta foi revisada conforme o protocolo de supervisão em `~/.claude/plugins/config/claude-for-legal/legal-clinic/CLAUDE.md` (fila / flag / toque mais leve).
2. Todos os rótulos internos de revisão (`[MINUTA ASSISTIDA POR IA]`, qualquer tag `[VERIFICAR]` ou `[FATO NECESSÁRIO]`) foram removidos da cópia que vai ao(à) assistido(a).
3. O sign-off conforma à norma de regência do estágio para correspondência assinada por estagiário(a) de Direito.

**Isto é minuta de estagiário(a) para revisão do(a) supervisor(a), não carta final.** Enviá-la tem consequências jurídicas para o(a) assistido(a) e pode constituir parecer ou comunicação em nome do(a) assistido(a). Um(a) Defensor(a)-Supervisor(a) ou Professor(a)-Orientador(a) habilitado(a) revisa, edita e dá sign-off antes da carta sair da unidade. Não envie sem aprovação do(a) supervisor(a).

## Checagem de linguagem simples

Conforme padrões de `~/.claude/plugins/config/claude-for-legal/legal-clinic/CLAUDE.md`. Frases curtas. Sem jargão. Nível de leitura alvo (ensino fundamental II — 6º-9º ano) forçado, em cumprimento ao dever de informar com clareza (LC 80/94 art. 4º-A III). Se um template acima inclui termo jurídico que o(a) assistido(a) pode não conhecer, explique na primeira vez: "Protocolamos sua 'contestação' — esse é o documento que conta sua versão da história ao juízo."

## Roteamento de supervisão

Conforme `~/.claude/plugins/config/claude-for-legal/legal-clinic/CLAUDE.md`. Correspondência de rotina pode ou não ser gatilho de flag dependendo do estilo de supervisão que o(a) supervisor(a) escolheu. Se toque mais leve: essas saem depois da revisão do(a) estagiário(a) sem passar por fila. Se fila formal: mesmo cartas de rotina vão para a fila.

## O que esta skill NÃO faz

- **Parecer substantivo.** Se a carta diria "aqui está o que eu acho do seu caso" ou "aqui está o que você deve fazer," isso não é rotina — é `/status client` ou conversa com o(a) supervisor(a) primeiro.
- **Má notícia.** Encerramento de caso, decisão desfavorável, não-é-caso-da-unidade — esses precisam de pensamento, não template. Sinalize para o(a) supervisor(a).
- **Qualquer coisa para advogado(a) contrário(a) ou juízo.** Audiência diferente, skill diferente (`/draft` ou `/status court`).
