---
name: deposition-prep
description: Construa outline de oitiva/depoimento para testemunha — puxe documentos da pessoa da plataforma de gestão documental, organize tópicos em torno da tese do caso, e aflore material de impeachment. Use quando o usuário diz "preparar oitiva de [testemunha]", "construir outline de oitiva", ou "preparar depoimento pessoal de [nome]".
argument-hint: "[witness name]"
---

# /deposition-prep

1. Carregue `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md` → tese do caso, fatos-chave.
2. Siga o workflow e a referência abaixo.
3. Puxe docs de autoria de / mencionando a testemunha da plataforma de gestão documental.
4. Construa outline: background, docs-chave, tópicos amarrados à tese, material de impeachment.

---

# Preparação de Oitiva

## Audiência de instrução e julgamento (AIJ) — CPC 2015

No Brasil, a coleta de prova oral ocorre majoritariamente em audiência de instrução e julgamento concentrada (CPC art. 358 e ss.), e não em "deposition" pré-julgamento à americana. O CPC não admite descoberta probatória ampla pré-julgamento — vigora o princípio dispositivo (CPC art. 2º) e o requerimento específico de prova (CPC arts. 369-373). As modalidades de prova oral são: depoimento pessoal da parte (CPC arts. 385-388), oitiva de testemunha (CPC arts. 442-463), e acareação (CPC art. 461). Em sede de produção antecipada de prova (CPC arts. 381-383), a oitiva pode ser pré-processual.

**No JEC (Lei 9.099/95):** a oitiva é em audiência una de conciliação, instrução e julgamento (Lei 9.099 art. 27); rito mais informal; testemunhas até 3 por parte; partes podem ser ouvidas em depoimento pessoal a qualquer momento. Princípio do *ius postulandi* até 20 SM (Lei 9.099 art. 9º).

**Para Defensor Público:** prazo em dobro para preparação (CPC art. 186) aplica a quaisquer manifestações sobre a prova; intimação pessoal do(a) Defensor(a) é necessária (LC 80/94 art. 44 I).

**O que esta skill faz e o que NÃO faz:** prepara prompts de pergunta para extrair a recolecção real da testemunha; captura e organiza o que a testemunha diz (palavras dela, não suas); roda checklist de pertinência (CPC art. 370 — relevância) contra rol que você redigiu; redige o tópico da carta de preposição (se for depoimento de preposto de pessoa jurídica). Não escreve evidência na voz da testemunha. Depoimento na voz da pessoa que ela não escreveu é problema de credibilidade na melhor hipótese — e em hipóteses piores, atrai a aplicação do CPC art. 80 (litigância de má-fé por alterar a verdade dos fatos).

## Checagem de destino

Antes de produzir output, cheque para onde vai. Se o usuário nomeou destino (canal, lista de distribuição, contraparte, "todos"), pergunte se está dentro do círculo de sigilo. Canais públicos, listas company-wide, contraparte/advogado(a) contrário(a), fornecedores e clientes (para trabalho-produto) quebram a proteção. Quando o destino parece fora do círculo, sinalize e ofereça (a) a versão sigilosa só para o Jurídico, (b) versão sanitizada para o canal mais amplo, ou (c) ambas — não aplique silenciosamente cabeçalho sigiloso e depois ajude a colar onde o cabeçalho não vai proteger. Vide o canônico `## Guardrails compartilhados → Checagem de destino` no CLAUDE.md deste plugin.

## Propósito

Outline de oitiva é um mapa: background → fixar os fatos bons → confrontar com os ruins → encurralar sobre a tese. Esta skill constrói o mapa dos documentos e da tese do caso.

## Fidelidade do registro — citações literais e pinpoints

Duas regras governam toda citação e toda transcrição puxada do registro para este outline. Declaração canônica vive nos guardrails compartilhados do CLAUDE.md do plugin; repetidas aqui porque uma confrontação de impeachment construída sobre declaração anterior mal-citada ou cite mal-fundamentado colapsa a confrontação.

**Citações literais do registro devem ser literais.** Nunca coloque aspas em palavras atribuídas a advogado(a) contrário(a), à testemunha, a outro(a) depoente, ao juízo, ou a qualquer documento dos autos a menos que tenha a passagem exata diante de você e possa pinpointar. Quando você quer caracterizar o que alguém disse mas não acha as palavras exatas:

- **Parafraseie sem aspas**, atribuindo claramente: "A testemunha disse anteriormente que X `[verificar contra registro — Ata fl. __]`."
- **Marque o placeholder:** `[verificar citação literal — pinpoint pendente]`
- **Nunca preencha a lacuna.** Declaração anterior inventada destrói o impeachment no momento em que a testemunha desafirma e a ata não bate. Toda `[verificar citação literal]` deve ser flagueada na nota do revisor.

**Pinpoints devem sustentar a proposição inteira.** Se o ponto de impeachment é "a testemunha disse X, Y e Z em [data]", verifique que o pinpoint sustenta X E Y E Z. Se sustenta só Z, divida o cite — "disse X (Ata fl. 10), Y (Ata fl. 12), Z (Ata fl. 15)" — ou estreite a proposição. Cite que sustenta parte de um impeachment é o failure mode em que o(a) advogado(a) contrário(a) pede para a testemunha ler mais do entorno da ata e sua confrontação desmonta.

## Calibração oral

Outline de oitiva é lido em voz alta em tempo real. Isso é advocacia oral, não escrita. Significa:

- Escolha 3-4 tópicos que efetivamente importam. Não tente cobrir tudo — outline de 200 perguntas em AIJ de 4 horas faz o(a) advogado(a) escanear, e escanear é como linhas de questionamento se perdem no meio da sequência.
- Lidere com sua confrontação mais forte. A testemunha está mais fresca no início, e as primeiras páginas da ata são as que o(a) juiz(a) ou tribunal de apelação tem mais chance de ver.
- Para testemunhas adversas: as perguntas mais fechadas vão nas sequências mais fechadas. Tudo o mais é andaime.
- Se você está preparando memorial final após a AIJ, a calibração é ainda mais estrita — o tribunal lembra os primeiros dois minutos e os últimos dois.

"Detalhado demais" para trabalho oral lê como desfocado. Se o outline é longo porque o registro é profundo, diga e sinalize onde o(a) advogado(a) deve colapsar.

## Carregar contexto

`~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md` → tese do caso (tese, fato pivô, fatos-chave a favor/contra), plataforma de gestão documental.

**Gate de impedimentos — incontornável.** Antes de construir outline, cheque `~/.claude/plugins/config/claude-for-legal/litigation-legal/matters/_log.yaml` para o slug. Se o caso não está em `_log.yaml`, recuse e route:

> "Não vejo [slug do caso] no log de casos. Rode `/litigation-legal:matter-intake` primeiro para a checagem de impedimentos rodar e o workspace ser montado. Não construo outline de oitiva em caso não-intaken — a checagem de impedimentos é o gate."

Não prossiga em caso não-intaken. Intake é o que roda impedimentos e grava a linha de `_log.yaml` que esta skill lê.

## Fluxo de trabalho

### Passo 1: Quem é esta testemunha?

- Nome, papel, relação com o caso
- Por que estamos ouvindo — o que precisamos desta testemunha?

O "por que" se conecta à tese. Se a testemunha pode estabelecer o fato pivô, isso é o centro do outline.

### Passo 1a: Postura da testemunha — ramifique antes de redigir perguntas

Estrutura de preparação difere por postura. Identifique a postura antes de escrever uma única pergunta:

- **Adversa / hostil** — estilo de reperguntas: perguntas fechadas, dirigidas, um fato por vez. Construa a caixa. (No Brasil, CPC art. 459 — perguntas formuladas diretamente pela parte que arrolou; o(a) juiz(a) controla impertinência.)
- **Amiga / própria** — estilo de extração direta: perguntas abertas que deixam a testemunha contar a história. Perguntas fechadas dirigidas com testemunha própria são usualmente impróprias e minam credibilidade.
- **Terceiro neutro** — misto; frequentemente aberto para pegar a história, fechado para fixar específicos.
- **Preposto(a) de pessoa jurídica (CPC art. 386 / Lei 9.099 art. 9º §4º)** — carta de preposição define poderes; o(a) preposto(a) é ouvido(a) pelo conhecimento que tem do fato (não personalíssimo). Confirme: que poderes a carta delegou, quem foi indicado, escopo do conhecimento vinculante.

**Pesquise as regras aplicáveis ao depoimento para o foro e tipo de testemunha** (CPC arts. 385-388 e 442-463, regulamento interno do tribunal, ordens permanentes do(a) juiz(a) sobre instrução). Cite fontes primárias. Não aplique estrutura única de preparação — a forma de pergunta, a abordagem dos documentos, e o uso de material de impeachment dependem da postura.

**Sem suplementação silenciosa.** Se consulta ao MCP de pesquisa configurado (JusRatio, BNP, CJF, TJAM, DataJud) retorna poucos ou nenhum resultado para as regras de depoimento do foro ou um cite que você precisa para impeachment, reporte o que foi encontrado e pare. NÃO preencha a lacuna com busca web ou conhecimento do modelo sem perguntar. Diga: "A busca retornou [N] resultados de [ferramenta]. Cobertura parece fina para [regra / autoridade]. Opções: (1) ampliar a query, (2) tentar outra ferramenta de pesquisa, (3) buscar na web — resultados serão tagueados `[busca web — verificar]` e devem ser checados contra fonte primária antes de confiar, ou (4) deixar o marcador `[INCERTO]` e parar aqui. Qual prefere?" Um(a) advogado(a) decide se aceita fontes de menor confiança; a skill não decide por ele.

**Atribuição de fonte.** Tagueie cada referência a regra, citação de julgado, e autoridade no outline com de onde veio: `[JusRatio]`, `[BNP]`, `[CJF]`, `[TJAM]`, `[DataJud]`, ou o nome do MCP para citações recuperadas de conector de pesquisa; `[busca web — verificar]` para citações de busca web; `[conhecimento do modelo — verificar]` para citações de dados de treino; `[usuário forneceu]` para citações que o(a) sócio(a) ou advogado(a)-supervisor(a) forneceu. Citações de documento (movimentações CNJ, números de produção) retêm sua fonte nativa. Citações tagueadas `verificar` carregam maior risco de fabricação e devem ser checadas antes da audiência. Nunca strip ou colapse as tags.

### Passo 2: Puxe os documentos dela

Da plataforma de gestão documental (se conectada):

- Documentos de autoria da testemunha
- Documentos enviados para ou de
- Documentos mencionando a testemunha por nome
- Entradas de calendário e notas de reunião com a testemunha presente

Organize por data. Sinalize os docs quentes — os que mais importam para a tese.

### Passo 3: Construa tópicos

Cada tópico é coisa que você quer estabelecer ou explorar. Organize em torno da tese:

**Background (sempre primeiro — fixe fatos não-controversos antes de a testemunha ficar defensiva):**
- Cargo, tempo, responsabilidades
- Estrutura hierárquica
- Como interagiu com os atores-chave

**Fatos bons (fixe antes de confrontar):**
- Fatos de `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md` → fatos-chave a favor, que esta testemunha pode estabelecer
- Documentos que sustentam nossa tese, de autoria ou recebidos por esta testemunha

**Fatos ruins (confronte com documentos):**
- Fatos contra nós sobre os quais esta testemunha será perguntada de qualquer jeito — pegue sua versão primeiro
- Documentos que prejudicam — saiba como a testemunha vai explicá-los

**Impeachment (se hostil ou se contradiz):**
- Declarações anteriores inconsistentes (de docs, depoimentos anteriores, declarações)
- Documentos que contradizem o que se espera que diga

**O fato pivô:**
- A sequência de perguntas que estabelece (ou mina) o fato em que o caso gira
- Esta é a seção mais cuidadosamente construída. Forma de pergunta segue postura do Passo 1a: fechada dirigida em adversa, aberta controlada em amiga, mista em neutra. Não default para um padrão único.

### Passo 4: Escreva o outline

```markdown
[CABEÇALHO DE SIGILO — por config do plugin ## Outputs — varia por papel; vide `## Quem está usando`]

# Outline de Oitiva: [Nome da testemunha]

**Data:** [data da audiência]
**Cargo da testemunha:** [título, relação com o caso]
**Postura:** [adversa / amiga / neutra / preposto(a) CPC 386] — orienta forma de pergunta
**Regras aplicáveis:** [CPC arts. 442-463 / Lei 9.099 art. 27 / regimento interno / ordem permanente — com pinpoints] `[INCERTO — verificar atualidade]`
**Por que estamos ouvindo:** [uma frase — o objetivo]
**Conexão com a tese:** [como esta testemunha se encaixa na tese do caso]

---

## I. Background

[Perguntas — fechadas, um fato cada. Fixe o não-controverso.]

## II. [Tópico de fato bom]

**Objetivo:** Estabelecer [fato] para uso em manifestação sobre prova / memorial / julgamento.

**Documentos:**
- [movimentação CNJ / fl.] — [descrição] — [por que importa]

**Perguntas:**
[A sequência. Cada pergunta fechada. Construa para a admissão.]

## III. [Tópico de fato ruim]

**Objetivo:** Obter a explicação da testemunha sobre [fato ruim] nos nossos termos antes que ela seja preparada para a sustentação oral.

[Mesma estrutura]

## IV. Material de impeachment (use se necessário)

[Declarações anteriores / documentos para confrontar, se a testemunha contradiz]

## V. [Sequência do fato pivô]

**Objetivo:** [A coisa em que o caso gira]

[Esta é a seção mais fechada. Cada pergunta é sim/não. Cada pergunta estabelece um fato. Construa a caixa.]

---

## Lista de exibidos

| # | Movimentação CNJ / fl. | Descrição | Usado na seção |
|---|---|---|---|

## Disciplina de marcador

Use inline ao construir e revisar:
- `[VERIFICAR: alegação factual]` — qualquer fato não confirmado contra o registro
- `[INCERTO: proposição jurídica]` — qualquer ponto jurídico (regra, prazo, limite de escopo de perguntas) não confirmado contra autoridade atual
- `[CITE FALTANDO: cite específico]` — cite de registro ou autoridade pendente

## Notas para o(a) advogado(a) / Defensor(a)

- [Qualquer coisa que o outline não captura — notas de postura da testemunha, chamadas estratégicas a fazer no momento]

---

**Material sigiloso / preparatório.** Este outline é construído de materiais do caso e trabalho preparatório e herda seu status de proteção. Mantenha na pasta de materiais sigilosos, marque apropriadamente, e tome qualquer decisão de distribuição (co-patrocinador, cliente/assistido(a), peritos) deliberadamente — distribuição fora do círculo de sigilo pode quebrar a proteção.

**Cite-check qualquer autoridade.** Citações de regra (CPC arts. 442-463, Lei 9.099 art. 27, regimento interno, ordens permanentes) e qualquer jurisprudência puxada para o outline foram geradas por modelo de IA. Verifique cada uma contra JusRatio, BNP, ou sua plataforma de pesquisa — confirme atualidade e escopo antes de usar na audiência. Tags de fonte em cada citação (ex.: `[JusRatio]`, `[busca web — verificar]`) mostram de onde veio; tags `verificar` carregam maior risco de fabricação e devem ser checadas primeiro.
```

## O que esta skill não faz

- Toma a oitiva. O outline é mapa; o(a) advogado(a) ou Defensor(a) conduz.
- Prevê o que a testemunha vai dizer. Prepara para respostas prováveis, mas testemunhas surpreendem.
- Decide o que perguntar de improviso. Reperguntas são juízo do(a) advogado(a) na sala.
