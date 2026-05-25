---
name: ramp
description: >
  Onboarding de termo/semestre do(a) estagiário(a) — procedimentos da unidade,
  walkthrough das ferramentas, exercícios práticos antes de casos reais. Lê o
  manual que o(a) supervisor(a) subiu no setup e ensina interativamente. Use
  quando estagiário(a) novo(a) diz "me onboarda", "sou novo(a) na unidade",
  "começando", ou no início de cada termo/semestre; passe --card para a
  referência de uma página.
argument-hint: "[--card for the one-page reference]"
---

# /ramp

1. Cheque que `~/.claude/plugins/config/claude-for-legal/legal-clinic/CLAUDE.md` está setado. Se placeholders: "Peça a [supervisor(a)] para rodar `/legal-clinic:cold-start-interview` primeiro."
2. Use o walkthrough abaixo.
3. Percorra: contexto da unidade (do manual) → comandos → exercícios práticos (intake fake, draft prático, roadmap de pesquisa) → hábitos de verificação.
4. `--card`: gere o cartão de referência de uma página.

```
/legal-clinic:ramp
```

```
/legal-clinic:ramp --card
```

---

# Ramp: Onboarding de Termo/Semestre

## Propósito

Todo termo/semestre, a unidade ou NPJ perde sua força de trabalho inteira e reconstrói do zero. Estagiários(as) novos(as) precisam aprender procedimentos, gestão de casos, convenções de protocolo, e básico das áreas de atuação antes de serem úteis. Tradicionalmente isso leva semanas de ler PDFs e fazer ao(à) supervisor(a) as mesmas perguntas todo termo.

Esta skill é o walkthrough guiado. Lê o que o(a) supervisor(a) subiu no cold-start — o manual, os guias de protocolo, as resoluções e regimentos locais — e ensina interativamente, com exercícios práticos para que estagiários(as) experimentem as ferramentas em ambiente de baixo risco antes de um(a) assistido(a) real estar na linha.

**Público: estagiários(as).** Supervisores(as) não rodam isto (rodam `/cold-start-interview`).

## Carregue contexto

`~/.claude/plugins/config/claude-for-legal/legal-clinic/CLAUDE.md` → perfil da unidade, áreas de atuação, jurisdição, caminho do manual, estilo de supervisão, templates por área de atuação.

Se esse arquivo está faltando ou ainda tem placeholders: "A unidade ainda não foi setada. Peça a [Defensor(a)-Supervisor(a) ou Professor(a)-Orientador(a)] para rodar `/cold-start-interview` primeiro."

## O walkthrough

### Abertura

> Bem-vindo(a) à [nome da unidade ou NPJ]. Vou te guiar por como esta unidade funciona e como usar essas ferramentas — uns vinte minutos, e você pode pausar a qualquer momento. Ao final, você vai ter rodado um intake prático, redigido um documento prático, e vai saber o que fazer quando pegar seu primeiro caso real.
>
> Uma coisa de cara: tudo que eu gero é ponto de partida, não resposta final. Você faz a análise. [Supervisor(a)] revisa seu trabalho [conforme estilo de supervisão]. Eu cuido da formatação e da primeira minuta para que você gaste seu tempo em advocacia, não em escrever "Excelentíssimo Senhor" pela vigésima vez.

### Parte 1: Esta unidade (5 min)

Leia de `~/.claude/plugins/config/claude-for-legal/legal-clinic/CLAUDE.md` e do manual ingerido. Cubra, interativamente:

- **Áreas de atuação** — o que a unidade atende, o que não (e para onde encaminhar se alguém aparece com matéria fora de escopo, ex.: matéria federal para DPU, ou núcleo especializado)
- **Assistidos(as)** — quem são, o que estão enfrentando, línguas (incluindo comunidades indígenas se aplicável)
- **Jurisdição** — quais juízos (TJAM, JEC, Juízos Cíveis), quais varas, quais quirks locais
- **Gestão de casos** — como casos são rastreados, onde pastas vivem (Sapiens-DPGU / sistema próprio AM), como se parece um caso bem-documentado
- **Supervisão** — como a revisão funciona nesta unidade (conforme estilo de supervisão no CLAUDE.md). Seja específico: "Antes de algo ir ao(à) assistido(a) ou ao juízo, [vai para a fila de revisão / você checa com [Defensor(a)-Supervisor(a)] / etc.]"

Não palestre — cheque entendimento. "Então se um(a) assistido(a) vem com notificação de despejo mas também menciona que é vítima de violência doméstica, o que você faz?" (Resposta: ambas as questões ficam anotadas no intake; a Lei Maria da Penha pode acionar encaminhamento para núcleo especializado ou flag para supervisor(a), dependendo do escopo da unidade.)

### Parte 2: Os comandos (5 min)

Percorra cada comando que o(a) estagiário(a) vai efetivamente usar:

| Comando | Quando você usa | O que você ganha |
|---|---|---|
| `/client-intake` | Atendimento do(a) assistido(a) | Sumário de caso formatado com questões identificadas, flags de conflito, triagem |
| `/draft [tipo de doc]` | Precisa de primeira minuta de documento comum | Template da área de atuação preenchido das notas — *ponto de partida, não final* |
| `/memo` | Precisa analisar um caso internamente | Memo formato FIRAC com lacunas de pesquisa sinalizadas |
| `/research-start [questão]` | Começando pesquisa jurídica | Roadmap: leis a checar, áreas de jurisprudência, termos de busca — *pistas, não citações autoritativas* |
| `/status [audiência]` | Atualizando alguém num caso | Sumário calibrado ao(à) assistido(a) / supervisor(a) / juízo |
| `/client-letter [tipo]` | Correspondência de rotina | Confirmação de atendimento, pedido de documento, atualização de status a partir de templates |

Para cada: o que faz, o que explicitamente não faz, o que o(a) estagiário(a) verifica antes de confiar.

### Parte 3: Exercícios práticos (8-10 min)

**Baixo risco. Assistido(a) fictício(a). Ferramentas reais.**

**Exercício 1 — Intake prático:**
> Aqui um cenário fictício de assistida: [hipo apropriado à área — ex.: para unidade que atende locação, "Maria recebeu notificação para desocupar em 30 dias terça passada. Está três meses atrasada com o aluguel após perder o emprego. O apartamento tem sistema hidráulico vazando desde novembro. Tem dois filhos."]
>
> Rode `/client-intake` e me entreviste como se eu fosse a Maria. Vou responder como Maria responderia. Ao final, olhe o sumário do caso que produziu — quais questões identificou? Pegou a exceção de habitabilidade prejudicada?

Debrief: o que o intake pegou, em que o(a) *estagiário(a)* devia ter sondado mais fundo, o que fica sinalizado para o(a) supervisor(a).

**Exercício 2 — Draft prático:**
> Usando o intake da Maria, rode `/draft contestacao-despejo`. Você vai pegar uma primeira minuta.
>
> Leia. O que está certo? O que está errado? O que você mudaria antes de mostrar a [Supervisor(a)]?

O ponto: a minuta é competente mas não final. O(a) estagiário(a) aprende a ler criticamente, não aceitar.

**Exercício 3 — Roadmap de pesquisa:**
> Rode `/research-start "exceção de habitabilidade em ação de despejo no AM"`. Você vai pegar um roadmap — leis, áreas de jurisprudência, termos de busca.
>
> Nenhuma dessas citações está verificada. É de propósito. Pegue uma lei do roadmap e me diga como você verificaria se está atual e se aplica aqui.

O ponto: `/research-start` é ponto de partida, não citação. O(a) estagiário(a) ainda faz a pesquisa.

### Parte 4: Hábitos de verificação (2 min)

Os hábitos que importam:

- **Todo output é ponto de partida.** Se foi ao(à) assistido(a) ou ao juízo sem você ter lido criticamente, algo deu errado.
- **Verifique toda citação** antes de ir em qualquer coisa. `/research-start` dá pistas, não autoridades.
- **Cheque detalhes específicos da jurisdição.** O plugin sabe sua UF do setup, mas quirks de regimento local mudam — duplo-cheque contra resoluções TJAM/CSDPGE atuais.
- **Quando incerto, ele diz.** Se um output tem flag `[INCERTO: ...]`, é prompt para pesquisar ou perguntar ao(à) supervisor(a), não para deletar a flag e seguir.
- **[Lembrete de supervisão conforme estilo no CLAUDE.md]** — o que é revisado antes de sair, e como.

### Encerramento

> Pronto. Você rodou um intake, redigiu um documento, e construiu um roadmap de pesquisa. Seu primeiro caso real vai parecer similar, exceto que o(a) assistido(a) é real e o(a) supervisor(a) está lendo seu trabalho.
>
> O cartão de referência de uma página: `/ramp --card`

## `/ramp --card`

Gere o cartão de referência de uma página do(a) estagiário(a) conforme spec. Conteúdo:

- Os comandos (tabela da Parte 2, condensada)
- Com o que o Claude pode ajudar / com o que não pode (pontos de partida sim, trabalho-produto final não, citações autoritativas não)
- Hábitos de verificação (bullets da Parte 4)
- Quem perguntar quando travar (nome do(a) supervisor(a) do CLAUDE.md)

Imprimível. Uma página. Entregue no primeiro dia.

## O que esta skill NÃO faz

- Substituir o(a) supervisor(a). Cobre procedimentos e ferramentas; supervisor(a) cobre juízo, estratégia, e as coisas que você só aprende vendo alguém bom(boa) fazer.
- Ensinar direito substantivo. *Orientação* à área de atuação, não curso doutrinário.
- Certificar que o(a) estagiário(a) está pronto(a). Supervisor(a) decide quando estagiário(a) pega caso real.
