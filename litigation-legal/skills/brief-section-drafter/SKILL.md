---
name: brief-section-drafter
description: Redige seção de peça em estilo da casa, consistente com a tese — todo fato citado, todo julgado conferido, todo argumento amarrado à tese. Use quando o(a) usuário(a) disser "redija a [seção]", "escreva os fatos", "argumento II sobre [questão]", "petição inicial JEC", "petição inicial comum", "contestação", "recurso inominado", "apelação", ou precisar de primeira minuta de seção de peça.
argument-hint: "[seção — ex.: 'fatos', 'fundamentos jurídicos', 'preliminares', 'petição inicial JEC vício de produto']"
---

# /brief-section-drafter

1. Carregue `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md` → tese do caso, estilo da casa.
2. Siga o workflow e a referência abaixo.
3. Redija no formato/tom/padrão de citação da casa. Consistente com a tese.
4. Output: seção em minuta. Flag todo lugar onde fato ou citação precisa de verificação.

---

# Redator de Seção de Peça

## Quatro formatos brasileiros principais

**Petição inicial JEC (Lei 9.099/95).** Procedimento sumaríssimo. Princípios: oralidade, simplicidade, informalidade, economia processual, celeridade. Estrutura típica:
1. Endereçamento (juízo competente — JEC da comarca)
2. Qualificação das partes (autor[a], réu[ré]; CPF/RG se possível, mas omissão admitida no JEC)
3. Fatos (objetivos, sem ornamentação — "no dia X o(a) requerido(a) fez Y")
4. Direito (suscinto — base legal + súmula/Tema se aplicável; doutrina opcional)
5. Pedidos (claros e específicos — tutela de urgência se cabível CPC 300, dano moral quantificado em valores, obrigação de fazer com prazo, etc.)
6. Valor da causa (atenção: valor de alçada do JEC é 40 SM — Lei 9.099/95 art. 3º I)
7. Provas (rol — sem juntada obrigatória de documentos pré-constituídos, salvo essenciais à inicial)
8. Requerimento de citação + indicação de horário para audiência de conciliação
9. Assinatura do(a) advogado(a)/Defensor(a) ou da própria parte (ius postulandi até 20 SM — Lei 9.099 art. 9º)

Tom: direto, objetivo, sem latim, sem floreios. Doutrina e jurisprudência usadas com parcimônia — JEC valoriza a tese clara mais que o aparato técnico.

**Petição inicial Comum (CPC 319).** Procedimento ordinário. Requisitos obrigatórios:
1. Endereçamento (juízo competente)
2. Qualificação completa (CPC 319 II — nome, prenome, estado civil, profissão, número de inscrição CPF, endereço eletrônico, domicílio e residência)
3. Fato e fundamentos jurídicos do pedido (causa de pedir próxima e remota)
4. Pedido com especificações (CPC 319 IV)
5. Valor da causa
6. Provas com que o autor pretende demonstrar a verdade dos fatos
7. Opção pela realização ou não da audiência de conciliação (CPC 319 VII + 334 §5º — só dispensável se ambas as partes manifestarem)
8. Documentos indispensáveis à propositura da ação (CPC 320)
9. Requerimento de tutela de urgência (CPC 300) ou tutela da evidência (CPC 311), se cabível
10. Requerimento de gratuidade da justiça CPC 98 (para assistido(a) da DP: hipossuficiência presumida — Súmula 481 STJ)
11. Assinatura

Tom: técnico, mas claro. Doutrina e jurisprudência citadas com pinpoint. Padrão CNJ de citação + ABNT NBR 6023/10520 onde couber.

**Contestação (CPC 335-342).** Prazo de 15 dias úteis (em dobro para Defensor — CPC 186; 30 dias úteis). Estrutura:
1. Endereçamento
2. Preliminares (CPC 337 — incompetência, perempção, litispendência, coisa julgada, conexão, falta de legitimidade ou interesse, etc.) — exaustivas; ônus de alegação concentrada (CPC 342)
3. Mérito (impugnação especificada dos fatos — CPC 341; impugnação ao valor da causa, se cabível; teses de defesa)
4. Pedido contraposto / reconvenção (se cabível)
5. Provas
6. Pedidos finais
7. Assinatura

Defensor em defesa: tipicamente em ação de cobrança contra hipossuficiente, despejo, embargos à execução. Frequentemente acompanhada de pedido de gratuidade + suspensão da exigibilidade (CPC 98 §3º).

**Recurso inominado (Lei 9.099/95 art. 41-46), apelação (CPC 1009), agravo de instrumento (CPC 1015) ou apelação cível.** Prazo de 10 dias corridos no JEC (Lei 9.099/95 art. 42) ou 15 dias úteis em rito CPC (em dobro para Defensor). Estrutura:
1. Endereçamento (ao juízo a quo, com pedido de remessa ao ad quem)
2. Razões: relatório sucinto + tese da impugnação (nulidades, errores in judicando, errores in procedendo)
3. Pedidos (reforma total/parcial, anulação, etc.)
4. Assinatura

**Outras peças relevantes para Defensor:** ofício institucional (timbre da DP, ao órgão administrativo — concessionária, secretaria, hospital, fornecedor), notificação extrajudicial (assistido(a) → contraparte privada), embargos de declaração (CPC 1022), agravo interno (CPC 1021), embargos à execução (CPC 914-920).

## Propósito

Uma boa seção de peça é consistente com a tese, citada aos autos, escrita em estilo da casa, e conferível. Esta skill produz a primeira minuta — ênfase em *minuta*. Sócio(a)/Defensor(a)/Defensor(a)-Supervisor(a) edita.

## Escrita ou sustentação oral?

Pergunte antes de redigir: "É para peça escrita ou sustentação oral?" São ofícios distintos:

- **Escrita:** detalhada. Cubra os pontos, desenvolva a autoridade, antecipe as respostas.
- **Sustentação oral (rebuttal, alegações finais, sustentação no tribunal):** estratégica. Escolha os 3-4 pontos mais importantes. Conceda ou ignore os fracos. Lidere com seu mais forte. Um tribunal lembra dos dois primeiros minutos e dos dois últimos. "Detalhada demais" para sustentação soa desfocado. Se está respondendo a peça multi-questão, diga ao(à) usuário(a) quais questões pressionaria e quais deixaria — isso é a minuta da estratégia, não só das palavras.

## Fidelidade aos autos — citações e pinpoints

Duas regras que governam toda citação e toda transcrição em peça. A declaração canônica vive no `CLAUDE.md` do plugin (guardrails compartilhados); repetida aqui porque esta skill é o lugar mais comum onde a regra é testada.

**Citações literais dos autos devem ser literais.** Nunca coloque aspas em palavras atribuídas ao(à) advogado(a) contrário(a), testemunha, juízo ou qualquer documento dos autos a menos que tenha a passagem exata diante de você e possa citar com pinpoint (folha dos autos / ID de movimentação CNJ / ata de audiência fl. X). Citação quase-certa é pior que paráfrase — distorce os autos, é punível se protocolada (CPC art. 80 II — alterar a verdade dos fatos), e vai ser pega. Quando você quer caracterizar o que alguém disse mas não acha as palavras exatas:

- **Parafraseie sem aspas**, atribuindo claramente: "O(a) advogado(a) contrário(a) sustentou que X `[verificar contra os autos — Ata da audiência fl. __]`."
- **Marque o placeholder:** `[verificar citação literal — pinpoint pendente]`
- **Nunca preencha a lacuna.** Citação inventada, ainda que uma palavra, é fabricação. A nota do revisor deve marcar toda `[verificar citação literal]` no output.

Antes de citar passagem com aspas, tenha a fonte aberta. Se está trabalhando de memória ou sumário, sem aspas.

**Pinpoints devem sustentar a proposição inteira.** Se o argumento é "o(a) advogado(a) contrário(a) disse X, Y e Z" e você cita um pinpoint, verifique que o pinpoint sustenta X E Y E Z. Se sustenta só Z, ou (a) divida a citação — "disse X (Ata fl. 10), Y (Ata fl. 12) e Z (Ata fl. 15)" — ou (b) estreite a proposição ao que o pinpoint efetivamente sustenta. Citação que sustenta parte da alegação é como tribunal pega você esticando. É a maneira mais comum de credibilidade do(a) advogado(a)/Defensor(a) erodir em juízo. Failure mode da "misgrounded citation": a citação existe, a passagem existe, mas a passagem não sustenta a proposição como posta.

## Candura sobre argumentos fracos

Quando o direito está contra você, diga. Quando um argumento é fraco — a autoridade corta para o outro lado, os fatos não sustentam, a inferência é forçada — não construa argumento frágil e apresente como se fosse sólido. Flag:

> "Este ponto é fraco — [autoridade] corta para o outro lado. Considere pressionar (eis como você enquadraria), conceder e pivotar para [ponto mais forte], ou deixar. `[review — chamada estratégica]`."

Asseverar argumento fraco sem flag erode a credibilidade do(a) advogado(a)/Defensor(a) com o tribunal e cria problema de candor (Código de Ética OAB art. 6º — vedação ao patrocínio de demanda manifestamente infundada; LC 80/94 art. 4º-A VI — atendimento jurídico orientado pela boa-fé). A minuta deve fazer o(a) usuário(a) mais inteligente, não confiante sobre posição ruim.

## Cobertura de extração de citações

Quando esta minuta é cite-check — por você, por outra skill, ou por revisor — a checagem deve ser exaustiva, não seletiva:

1. **Primeira passada: extrair.** Leia o documento todo e construa lista de toda citação — julgados, leis, regulamentos, citações aos autos, doutrina. Reporte a contagem: "Encontradas [N] citações."
2. **Segunda passada: conferir.** Confira cada uma contra a fonte. Não amostre. Não pare quando cansar.
3. **Reporte cobertura.** No final: "Conferi [N] de [M] citações. [K] não pude recuperar — verifique manualmente. [J] confirmadas. [I] flagged como potenciais miscitações. [H] flagged como sem sustentação (citação existe mas não sustenta a proposição)."
4. **Quando texto da fonte não está disponível, diga "não pude conferir", nunca "confirmada".** Falso positivo ("esta citação está ok" quando você não pôde ler a fonte) é pior que "não consegui conferir esta".
5. **Os erros mais difíceis de pegar são suporte parcial.** Uma citação que sustenta parte da alegação mas não toda. Leia a proposição que a peça faz, leia o que a fonte efetivamente decide, e compare elemento por elemento.

## Eco vs repetição

Eche enquadramentos-chave; não copie frases. Consistência com peças anteriores é boa — reforça sua tese do caso e faz os autos coerentes. Mas há uma linha entre echar e repetir.

- **Echo:** use os mesmos termos-chave, o mesmo enquadramento da questão central, a mesma caracterização da tese do outro lado.
- **Não:** copie frases inteiras, re-use formulações distintivas tantas vezes que o tribunal nota, ou repita o mesmo argumento literalmente sem avançar.

Uma réplica que soa como re-leitura da inicial perde terreno. A minuta deve avançar o argumento, não restatá-lo.

## Carregar contexto

`~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md` → tese do caso, estilo da casa (padrão de citação, estrutura, tom, normas de extensão).

**Gate de conflitos — não bypassável.** Antes de redigir, cheque `~/.claude/plugins/config/claude-for-legal/litigation-legal/matters/_log.yaml` para o slug do caso onde esta skill foi invocada. Se o caso não está em `_log.yaml`, recuse e roteie:

> "Não vejo [slug do caso] no log de casos. Rode `/litigation-legal:matter-intake` primeiro para a checagem de conflitos/impedimentos rodar e o workspace do caso ser setup. Não redijo produto substantivo em caso não-intaken — a checagem é o gate."

Não prossiga em caso não-intaken. Intake é o que roda conflitos/impedimentos, monta `matter.md` / `history.md`, e escreve a linha do `_log.yaml` que esta skill lê. Pular produz trabalho em local não gerenciado e bypassa a disciplina institucional/escritório de conflitos.

## Workflow

### Passo 1: Qual seção (ou qual peça)?

| Seção / Peça | O que faz | Inputs necessários |
|---|---|---|
| Fatos | Conta a história, no nosso enquadramento, citada aos autos | Cronologia, docs-chave, transcrições de oitiva |
| Fundamentos jurídicos | Faz o caso jurídico | Questão, autoridades, fatos |
| Pedidos | Pede a tutela específica | O que queremos |
| Petição inicial JEC | Inicial sumaríssima Lei 9.099/95 | Fatos + tese curta + pedidos claros |
| Petição inicial Comum CPC 319 | Inicial completa CPC | Causa de pedir + fundamentos + pedidos + provas + valor + tutela urgência se cabível |
| Contestação CPC 335-342 | Resposta com preliminares + mérito | Análise das alegações da inicial + teses defensivas + ônus do CPC 341 |
| Recurso inominado / Apelação / Agravo | Impugnação de decisão | Relatório sucinto + tese de reforma/anulação |
| Ofício institucional (DP) | Requisição administrativa para órgão | Pedido específico + base legal + prazo razoável |
| Notificação extrajudicial | Pré-litigação | Constituir em mora / dar ciência |

### Passo 2: Checagem de tese

Antes de escrever: o que esta seção precisa cumprir para a tese?

- Fatos: enquadre a história para que nossa tese seja a leitura natural.
- Fundamentos jurídicos: conecte o direito aos fatos de forma que sustente a tese.
- Para Defensor: as teses repetitivas (vide `references/element-templates.md`) carregam súmulas e Temas Repetitivos — invoque sempre os de nível A (vinculantes) e B (qualificados) primeiro.

Se a seção que você está prestes a redigir contradiz a tese — pare. Ou a tese está errada ou a abordagem da seção está errada. Flag, não maquile.

### Passo 3: Redigir em estilo da casa

**Pesquise as regras locais do juízo / da câmara e as portarias específicas para extensão, formatação, citação e requisitos de protocolização; não dependa de preferências. Cite fontes primárias (provimento da Corregedoria local, portaria do juízo) nas notas de redação. Verifique atualidade — regras locais mudam.**

Per `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md`:

- **Padrão de citação:** padrão CNJ + ABNT NBR 6023/10520, ou padrão híbrido por tipo de peça. Citação de jurisprudência: "STJ, REsp [número], Rel. Min. [nome], j. [data], DJe [data]". Citação de lei: "Lei [número], art. X". Doutrina: "Autor (ano, p. XX)" em corpo + nota completa em rodapé. Conforme estilo extraído da peça-semente.
- **Estrutura:** Como este escritório/unidade organiza argumentos? Tese-regra-fato-aplicação-conclusão (FIRAC)? Tópicos com cabeçalho descritivo ou argumentativo? Para Defensor em JEC: estrutura simples; em vara comum: estrutura completa com preliminares.
- **Tom:** Incisivo ("o pedido da requerida é manifestamente infundado") ou mensurado ("a prova produzida não sustenta a tese da requerida")? Case a peça-semente.
- **Extensão:** Per a regra do juízo / portaria — nunca dependendo de "o que este juízo costuma querer" quando a regra é conferível.

### Passo 4: Citar tudo

Todo fato → citação aos autos (folha dos autos / ID movimentação CNJ / exibição).
Toda proposição jurídica → julgado + dispositivo legal com pinpoint.

**Disciplina de marcadores — use liberalmente:**
- `[VERIFICAR: alegação factual específica]` — qualquer coisa não confirmada contra os autos
- `[INCERTO: proposição jurídica específica]` — qualquer coisa não confirmada contra autoridade atual
- `[CITAÇÃO NECESSÁRIA: citação específica — fato/regra acreditado mas citação ainda não pinned]`

Minuta com marcadores não resolvidos não é final. Os marcadores fazem o passo de verificação explícito.

**Sem suplementação silenciosa.** Se uma busca no MCP de pesquisa jurídica (JusRatio, BNP, CJF, TJAM, DataJud) retorna poucos ou nenhum resultado para autoridade que a minuta precisa, reporte o que foi achado e pare. NÃO preencha a lacuna de busca web ou conhecimento do modelo sem perguntar. Diga: "A busca retornou [N] resultados em [ferramenta]. Cobertura parece fina para [questão / tese]. Opções: (1) ampliar a query, (2) tentar ferramenta diferente, (3) buscar na web — resultados serão marcados `[busca web — verificar]` e deveriam ser checados contra fonte primária antes de confiar, ou (4) deixar o marcador `[CITAÇÃO NECESSÁRIA]` e parar aqui. Qual você prefere?" Defensor(a) ou sócio(a) decide se aceita fontes de menor confiança; a skill não decide por eles.

**Atribuição de fonte.** Marque toda citação na minuta com de onde veio: `[JusRatio]`, `[BNP]`, `[CJF]`, `[TJAM]`, `[DataJud]`, ou o nome da tool MCP para citações recuperadas de MCP de pesquisa jurídica; `[busca web — verificar]` para citações de busca web; `[conhecimento do modelo — verificar]` para citações lembradas do treino; `[usuário forneceu]` para citações que o(a) sócio(a)/Defensor(a) supriu. Citações marcadas `verificar` carregam maior risco de fabricação que citações recuperadas por ferramenta e devem ser conferidas primeiro. Nunca tire ou colapse as tags — são o sinal mais rápido para o(a) Defensor(a)/sócio(a) revisor(a) sobre quais citações conferir primeiro antes da peça ser protocolada.

### Passo 5: Output

**Antes da peça ser protocolada (o ato consequente — esta skill redige, mas o gate roda no passo de protocolização independente de quem aciona):** Leia `## Quem está usando` em `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md`. Se o Papel é Não-advogado:

> Protocolizar peça tem consequência jurídica — vira parte dos autos, vincula o(a) cliente nos argumentos e fatos asseverados, e assinatura traz certificação de boa-fé (CPC art. 77 + Código de Ética OAB). Você revisou isto com profissional habilitado(a)? Se sim, prossiga. Se não, eis um briefing para levar:
>
> [Gere sumário de 1 página: a seção redigida, vínculo com a tese, autoridades invocadas, marcadores `[VERIFICAR]` / `[INCERTO]` / `[CITAÇÃO NECESSÁRIA]` ainda não resolvidos, o que pode dar errado (misstatement factual, citação não sustentada, argumento fora da tese), o que perguntar ao(à) profissional antes de protocolar.]
>
> Se você precisa achar advogado(a)/Defensor(a) habilitado(a): OAB Seccional (Comissão de Assistência Judiciária Gratuita) tem orientação inicial. Defensoria Pública estadual atende hipossuficiente. NPJ de faculdade local pode atender em certas áreas.

Não trate a minuta como pronta-para-protocolar sem um sim explícito. Redação em si não exige o gate — protocolização exige.

A seção, em estilo da casa, com marcadores inline.

Cabeçalho (não na peça — nota ao(à) advogado(a)/Defensor(a) revisor(a)):

```markdown
[CABEÇALHO DE SIGILO — per plugin config ## Outputs — difere por papel; vide `## Quem está usando`]

## Notas de redação — [Seção/Peça] — [data]

**Vínculo com a tese:** [Como esta seção sustenta a tese do caso]
**Autoridades invocadas:** [lista — todas precisam de conferência: vigência atual da lei + Shepardização do julgado (overruling, modulação)]
**Citações aos autos a verificar:** [N] flagged inline
**Perguntas em aberto para o(a) sócio(a)/Defensor(a):** [qualquer coisa que a minuta assume que deveria ser confirmada]
**Extensão:** [palavras/páginas vs. norma da casa / limite da regra local]

---

**Cite-check antes de protocolar.** Citações nesta minuta foram geradas por modelo de IA e não foram verificadas contra fonte primária. Rode todo julgado, lei e regulamento através de JusRatio, BNP, CJF, TJAM, DataJud ou plataforma da casa para precisão, status (vigência, overruling, modulação) e tratamento subsequente. Citação fabricada ou mal-citada em peça protocolada pode resultar em sanções por litigância de má-fé (CPC art. 80) e infração ético-disciplinar (Código de Ética OAB; Provimento OAB 205/2021).

**Apenas minuta — não é protocolização.** Protocolizar esta seção inicia (ou participa de) processo e carrega exposição do CPC art. 77/80 + Código de Ética OAB. Advogado(a) habilitado(a) ou Defensor(a) revisa, edita, e assume responsabilidade profissional antes de ir aos autos. Não protocole sem revisão.
```

## Especificidades de fatos (seção)

Os fatos são advocacia por seleção e sequência, não argumento.

- Cronológicos salvo razão para não ser
- **Todo fato na seção de fatos deve citar aos autos — folha dos autos, número de ID de movimentação CNJ, número de exibição.** "Ou confessado" não substitui citação aos autos. Se o fato é estabelecido por confissão ou acordo, cite o documento ou a ata da audiência onde a confissão foi feita.
- Enquadre por seleção: que fatos lideram, que recebem uma linha, que são omitidos (se não-necessários e não-úteis)
- Sem argumento. "O contrato inequivocamente exigia X" é argumento. "O contrato estabelecia 'X.'" é fato.

## Especificidades de fundamentos jurídicos

- Lidere com a regra, não com os fatos (geralmente — estilo da casa pode diferir)
- Um argumento por seção. Se é realmente dois, são duas seções.
- Trate o melhor contra-argumento do outro lado. Não fuja — peça que ignora o contra óbvio é peça em que o juízo não confia.
- Parentéticos ganham seu espaço. Se um parentético não adiciona algo que a citação sozinha não, corte.

## Especificidades de petição inicial JEC

- Linguagem direta — Lei 9.099/95 valoriza simplicidade
- Pedidos quantificados (dano moral em R$ específico, não "a ser arbitrado")
- Valor da causa atento ao limite de 40 SM (alçada)
- Documentação probatória mínima essencial à inicial; o resto na audiência

## Especificidades de petição inicial Comum CPC 319

- Atenda aos incisos do art. 319 — emenda CPC 321 se omisso
- Tutela de urgência (CPC 300) ou de evidência (CPC 311) em seção própria, com preenchimento expresso dos requisitos
- Para Defensor: requerimento de gratuidade (CPC 98) com declaração de hipossuficiência presumida (Súmula 481 STJ)
- Opção de audiência de conciliação (CPC 334 §5º) — só dispensável se autor expressamente pedir; mesmo assim, juízo pode designar

## Especificidades de contestação

- Preliminares (CPC 337) primeiro — exaustivas, sob pena de preclusão (CPC 342)
- Impugnação especificada dos fatos (CPC 341) — fato não impugnado expressamente é presumido verdadeiro, salvo exceções
- Para Defensor em defesa: pedido de gratuidade + suspensão CPC 98 §3º; argumentos de prescrição/decadência se cabíveis; impugnação do valor da causa se inflacionado

## Especificidades de recurso

- **Recurso inominado JEC:** 10 dias corridos (Lei 9.099 art. 42, conforme STJ); razões devem demonstrar nulidade ou error in judicando
- **Apelação (CPC 1009):** 15 dias úteis em dobro para Defensor (CPC 186 = 30 dias); admite efeito suspensivo automático em regra (CPC 1012)
- **Agravo de instrumento (CPC 1015):** 15 dias úteis em dobro; rol legal — verificar cabimento (Tema 988 STJ ampliou interpretação para taxatividade mitigada)
- **Embargos de declaração (CPC 1022):** 5 dias úteis em dobro (10 para Defensor); contradição, omissão, obscuridade, erro material

## O que esta skill NÃO faz

- Produzir peça final. Produz minuta. Toda citação precisa de verificação, todo argumento precisa de olhos do(a) sócio(a)/Defensor(a).
- Decidir estratégia. Se há dois jeitos de argumentar a questão, flag os dois e deixe o(a) sócio(a)/Defensor(a) escolher.
- Protocolar coisa alguma. Nunca.
