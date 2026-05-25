---
name: subpoena-triage
description: Triagem de ofício requisitório / intimação para terceiro recebida pela parte — classifica, analisa escopo/onerosidade/sigilo, cross-check com portfólio, e produz framework de impugnação, plano de cumprimento e calendário de prazos. Use quando o usuário diz "recebemos ofício", "fomos intimados como terceiros", ou compartilha ofício, requisição administrativa ou pedido de documentos para avaliar.
argument-hint: "[path-to-subpoena] [--slug=custom-slug]"
---

# /subpoena-triage

1. Leia o ofício do path fornecido.
2. Classifique (terceiro-docs / terceiro-oitiva / parte / requisição-administrativa / requisição-criminal).
3. Se requisição criminal → pare, escalone per `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md`. Caso contrário continue.
4. Carregue `~/.claude/plugins/config/claude-for-legal/litigation-legal/matters/_log.yaml` para cross-check. Carregue `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md` → panorama, convenções de sigilo, normas de escalonamento.
5. Siga o workflow e a referência abaixo.
6. Extraia campos-chave, analise escopo/onerosidade/sigilo, produza framework de impugnação + plano de cumprimento + calendário de prazos.
7. Grave `~/.claude/plugins/config/claude-for-legal/litigation-legal/inbound/[slug]/triage.md`. Copie ou linke o ofício para `~/.claude/plugins/config/claude-for-legal/litigation-legal/inbound/[slug]/incoming.[ext]`.
8. Handoff: `/legal-hold --issue` se dever de guarda não está em vigor; `/matter-intake` se materialidade justifica; `/matter-briefing [slug]` se ofício de parte em caso existente.

---

# Triagem de Ofício / Intimação para Terceiro

## Propósito

Ofícios chegam com prazos. Os failure modes: perder o prazo, super-produzir (quebra de sigilo, ônus que devíamos ter impugnado), sub-produzir (exposição a multa por desobediência — CP art. 330 e/ou astreintes CPC art. 537), ou perder janela de impugnação. Esta skill classifica, analisa e produz plano de cumprimento com framework de impugnação.

## Assunção jurisdicional

A regra citada no Passo 0 é a operativa para este ofício neste foro. Prática de ofício/requisição varia materialmente: CPC 2015 (cível geral — arts. 380-389 contra terceiros; arts. 396-404 contra partes); Lei 9.099/95 (JEC); rito penal (CPP); rito administrativo (Lei 9.784/99); requisições de regulador (CVM Lei 6.385/76; ANPD Lei 13.709/2018 art. 55-J; ANS, Bacen, RFB); MPF/CGU em improbidade (Lei 8.429/92, Lei 14.230/21). Todo regulamento, regimento interno, ordem permanente do juízo, e o tipo de ofício (cível geral, depoimento, exibição) mudam prazos de impugnação, limites de pertinência, exigências de rol de sigilo, e custeio. Toda regra emitida aqui é heurística ponto-de-partida — confirme atualidade e variante local antes de afirmar por escrito.

## Contexto de polo

Esta skill é inerentemente defensiva — um ofício foi expedido contra a parte recebedora e a postura é responder/impugnar/cumprir. Leia `## Posição processual` no perfil de atuação. Se o polo default do usuário é **autor**, note que receber ofício é comum para autores também (intimações de testemunha, pedidos a terceiro direcionados aos próprios registros do(a) autor(a)) mas o framing aqui é sempre "ofício expedido contra nós, como respondemos". Se o usuário é **réu** (típico), o framing alinha com o default. Se o caso tem postura diferente do default (ex.: profissional defensivo recebendo ofício em caso onde está em jus postulandi por familiar), pergunte ao usuário para confirmar polo antes de prosseguir.

## Carregar contexto

- O ofício (usuário fornece path ou dropa in-session)
- `~/.claude/plugins/config/claude-for-legal/litigation-legal/matters/_log.yaml` — para lookup de caso relacionado e status de dever de guarda
- `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md` → panorama (reguladores com que lidamos), convenções de sigilo da casa, normas de escalonamento

## Fluxo de trabalho

### Passo 0: Pesquise a regra aplicável

**Antes de analisar este ofício, pesquise o regulamento aplicável para o foro (CPC arts. 380-389 para exibição contra terceiros; CPC arts. 396-404 contra partes; Lei 9.099/95 para JEC; rito específico para CVM/ANPD/Bacen/RFB) e o tipo de ofício (exibição documental, testemunho, requisição de regulador). Identifique: limites de pertinência (CPC art. 370 — pertinência da prova), prazos de impugnação (estes frequentemente correm da intimação válida — CPC art. 219 prazos em dias úteis; lembrar prazo em dobro CPC art. 186 se Defensor), exigências de rol de sigilo, e quem suporta custos. Cite com pinpoints. Verifique atualidade — regras e variantes mudam. Sinalize requisições criminais para escalonamento imediato a criminalista.**

**Sem suplementação silenciosa.** Se consulta ao MCP de pesquisa configurado (JusRatio, BNP, CJF, TJAM, DataJud) retorna poucos ou nenhum resultado para a regra, variante ou pinpoint do foro, reporte o que foi encontrado e pare. NÃO preencha a lacuna com busca web ou conhecimento do modelo sem perguntar. Diga: "A busca retornou [N] resultados de [ferramenta]. Cobertura parece fina para [regra / foro / variante]. Opções: (1) ampliar a query, (2) tentar outra ferramenta, (3) buscar na web — resultados tagueados `[busca web — verificar]` e devem ser checados contra fonte primária antes de confiar, ou (4) parar aqui. Qual prefere?" Um(a) advogado(a) decide se aceita fontes de menor confiança; a skill não decide por ele.

**Atribuição de fonte.** Tagueie cada referência a regra, julgado, lei e regulamento no output da triagem com de onde veio: `[JusRatio]`, `[BNP]`, `[CJF]`, `[TJAM]`, `[DataJud]`, ou o nome do MCP para citações recuperadas; `[busca web — verificar]` para citações de busca web; `[conhecimento do modelo — verificar]` para citações de dados de treino; `[usuário forneceu]` para citações fornecidas pelo usuário (ex.: do ofício ou trabalho prévio). Citações tagueadas `verificar` carregam maior risco de fabricação e devem ser checadas primeiro. Nunca strip ou colapse as tags — são o sinal mais rápido do(a) advogado(a) sobre quais citações verificar antes de afirmar em impugnações ou petições.

### Passo 1: Classifique

Ofícios vêm em sabores com regras diferentes; confirme os específicos contra a regra que acabou de pesquisar:

- **Ofício para exibição contra terceiro (cível)** — não somos parte do litígio; alguém quer nossos documentos. Categorias de impugnação usuais: pertinência (CPC art. 370), onerosidade excessiva (CPC art. 380 §2º), sigilo (CPC art. 388), competência territorial / impossibilidade de cumprimento.
- **Intimação de terceiro para depor** — alguém quer empregado(a) para testemunhar. Escopo, pertinência, onerosidade; possível pedido de revogação; preparação de testemunha exigida (vide `/litigation-legal:deposition-prep`).
- **Ofício / intimação dirigida a parte** — SOMOS parte; isto é instrução probatória em litígio que estamos rastreando. Trate como instrução, não como recebido — deve mapear a um caso existente.
- **Requisição administrativa (RFB/ANPD/CVM/ANS/Bacen/MPF/CGU/AG/PROCON estadual ou municipal)** — regras diferentes, postura diferente; frequentemente mais deferencial mas também mais consequencial. ANPD tem prerrogativa requisitória própria (Lei 13.709/2018 art. 55-J); RFB tem amplos poderes do art. 195 e ss. CTN.
- **Requisição em sede criminal (delegacia, MPF, juízo criminal)** — escalone imediatamente a criminalista; caminho de skill diferente (fora do escopo desta skill — sinalize para escalonamento).

### Passo 2: Extraia campos-chave

- **Autoridade expedidora** — juízo (qual), órgão (qual), advogado(a) (se cível)
- **Parte requerente** — quem requereu (se cível)
- **Autos / capa do caso** — o litígio sobre o qual estamos sendo consultados
- **Categorias de documentos requeridas** — lista numerada
- **Tópicos de testemunho** (se oitiva) — escopo
- **Prazo para resposta/impugnação** — data de intimação + cálculo da janela de resposta per regulamento aplicável (lembrar prazo em dobro CPC art. 186 se Defensor)
- **Data de cumprimento** — data até a qual documentos devem ser exibidos
- **Escopo geográfico** — custodiantes, localidades, sistemas implicados
- **Designação de custódia de registro** — quem na parte é a testemunha/signatário

### Passo 3: Cross-check de portfólio

- **Ofício dirigido a parte → relacionado a caso existente:** verifique que a capa bate com caso em `_log.yaml`. Se sim, route para o workflow daquele caso; esta triagem é informacional.
- **Ofício a terceiro → capa que não reconhecemos:** capture as partes; logue como standalone inbound.
- **Múltiplos ofícios do mesmo caso:** sinalize expedição coordenada; estratégia única de resposta pode aplicar.

### Passo 4: Analise escopo, onerosidade, sigilo

**Escopo / pertinência**
- As categorias mapeiam para documentos que plausivelmente temos?
- Alguma categoria é expedição de pesca (excessivamente ampla, desconectada de pretensões/defesas do caso subjacente — CPC art. 370)?
- Alcance geográfico / lugar de cumprimento — aplique a regra pesquisada; limites diferem por tipo de ofício.

**Onerosidade**
- Custodiantes implicados, sistemas buscados, período
- Volume estimado (rough: pequeno / médio / grande / extremo)
- Custo — terceiros respondentes podem ter custeio disponível; cheque a regra pesquisada (CPC art. 380 §2º — terceiro tem direito a indenização das despesas).

**Sigilo**
- Sigilo profissional do(a) advogado(a) ou trabalho preparatório provavelmente implicado? (Quase sempre sim para qualquer coisa jurídica; frequentemente sim para comunicações envolvendo advogado(a) interno(a) ou externo(a) — Lei 8.906/94 art. 7º XIX.)
- Outros sigilos — segredo industrial / comercial (CC art. 195), sigilo bancário (LC 105/2001), sigilo fiscal (CTN art. 198), sigilo médico (CFM Resolução 1.931/2009 — Código de Ética Médica art. 73), dados pessoais sensíveis LGPD art. 11
- Rol de sigilo será exigido — sinalize o formato per `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md`

**Outros fundamentos de impugnação**
- Confidencialidade — segredo de justiça CPC art. 189 necessário?
- Duplicativo — já têm isto de outra parte?
- Não-possuído — não temos o que pedem (documente com especificidade)
- Vício de intimação — cheque exigências de intimação da regra pesquisada

### Passo 5: Framework de impugnação

Redija outline estruturado de impugnações — não a peça final de impugnação, mas o outline do que aplica e por quê. O usuário (frequentemente com escritório externo) finaliza.

Cada impugnação:
- Fundamento jurídico — cite o pinpoint da regra pesquisada no Passo 0
- Aplicação específica a este ofício (quais categorias, quais custodiantes)
- Força (forte / razoável / fraca)

### Passo 6: Plano de cumprimento

Mesmo impugnando, frequentemente produzimos algo do que foi pedido. Plano:

- **Escopo de produção provável** — após impugnações, o que produziríamos
- **Custodiantes a buscar** — nomes e sistemas
- **Faixa de data**
- **Protocolo de revisão** — quem revisa para sigilo (nós, externo, paralegais)
- **Formato de produção** — per o ofício ou per protocolo negociado (PDF, nativo, com OCR)
- **Exigências de rol de sigilo** — formato, campos

### Passo 7: Prazos

Use os prazos identificados na pesquisa do Passo 0. Note que prazos de impugnação frequentemente correm da intimação válida — não default para um único número sem checar regulamento aplicável e variante local. Lembrar prazo em dobro CPC art. 186 se Defensor.

- **Prazo de resposta** — per regulamento pesquisado; note se usuário precisa de mais tempo (petição de dilação é padrão)
- **Prazo de impugnação** — per regulamento pesquisado (CPC + variante local)
- **Data de produção** — se nenhuma impugnação prevalece
- **Janela de pedido de revogação** — se perseguindo este caminho, timing é crítico

Agende tudo. Item de ação imediata.

### Passo 8: Gravar triagem

Output: `~/.claude/plugins/config/claude-for-legal/litigation-legal/inbound/[slug]/triage.md`.

```markdown
[CABEÇALHO DE SIGILO — por config do plugin ## Outputs — varia por papel; vide `## Quem está usando`]

# Triagem de Ofício / Intimação

> **NÃO SUBSTITUI ESCRITÓRIO EXTERNO.** Esta é classificação estruturada e leitura de escopo para apoiar decisões rápidas sobre prazos, deveres de guarda e contratação. Toda referência a regra é heurística ponto-de-partida; análise específica de jurisdição, finalização de impugnações, prática de pedidos de revogação, e chamadas de mérito sobre sigilo exigem advogado(a) habilitado(a) familiar com o foro. Contrate escritório externo para qualquer ofício acima de escopo rotineiro de docs a terceiro.

**Slug:** [slug]
**Intimação em:** [YYYY-MM-DD]
**Intimado(a):** [entidade / receptor legal]
**Arquivo recebido:** [path]
**Classificação:** [terceiro-docs / terceiro-oitiva / parte / requisição-administrativa / requisição-criminal]

---

## Campos-chave

- **Autoridade expedidora:** [juízo/órgão]
- **Parte requerente:** [nome]
- **Capa do caso:** [capa]
- **Prazo de resposta:** [data — em dobro se Defensor]
- **Data de produção:** [data]
- **Janela de pedido de revogação:** [faixa de data]

## Categorias requeridas (sumário)

[lista numerada, concisa]

## Custodiantes / sistemas provavelmente implicados

[lista]

---

## Cross-check de portfólio

**Caso relacionado:** [slug ou "nenhum"]
**Se ofício a parte:** [roteado para caso existente ou novo?]
**Se a terceiro:** [standalone inbound]

---

## Análise de escopo & onerosidade

**Escopo:** [avaliação de pertinência por categoria]
**Estimativa de onerosidade:** [pequeno / médio / grande / extremo — com razão]
**Issues de alcance geográfico:** [qualquer]

## Análise de sigilo

*Escopo de sigilo é leitura de primeira passagem; chamada final é do(a) advogado(a), não desta skill.*

**Sigilo profissional / trabalho preparatório provavelmente implicado:** [sim/não + quais categorias] `[SME VERIFICAR]`
**Outros sigilos:** [segredo comercial, bancário, fiscal, médico, dados sensíveis LGPD] `[SME VERIFICAR]`
**Formato de rol de sigilo exigido:** [per `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md`]

---

## Framework de impugnação

*Cada linha abaixo exige `[SME VERIFICAR]` antes de afirmar por escrito — jurisdição, atualidade da regra, risco de quebra.*

| Impugnação | Fundamento jurídico | Aplica a | Força | SME verificado? |
|---|---|---|---|---|
| Impertinência | CPC art. 370 | [categorias] | [forte/razoável/fraca] | [ ] |
| Onerosidade excessiva | CPC art. 380 §2º | [categorias] | | [ ] |
| Sigilo | Lei 8.906/94 art. 7º XIX; CPC art. 388 | [todos docs produzindo] | forte (sempre) | [ ] |
| Duplicativo | [regra/doutrina] | [se aplicável] | | [ ] |
| [outro] | | | | [ ] |

---

## Plano de cumprimento (se respondendo)

- **Escopo de produção provável:** [após impugnações]
- **Custodiantes / sistemas:** [lista]
- **Faixa de data:** [faixa]
- **Protocolo de revisão:** [quem, como]
- **Formato de produção:** [formato]
- **Rol de sigilo:** [formato, entradas estimadas]

---

## Prazos (agende)

*Todos os prazos abaixo vêm da pesquisa de regra do Passo 0. `[SME VERIFICAR]` confirma a regra, variante, e cálculo para este foro e tipo de ofício — variantes locais diferem. Lembrar prazo em dobro CPC art. 186 se Defensor.*

- **Prazo de resposta:** [data] `[SME VERIFICAR]`
- **Prazo de impugnação:** [data] — cite: [regra + pinpoint] `[SME VERIFICAR]`
- **Petição de dilação até:** [data] (tipicamente antes do prazo de impugnação) `[SME VERIFICAR]`
- **Data de produção:** [data]

---

## Ações imediatas

- [ ] Dever de guarda emitido — [sim/não] — se não, rode `/legal-hold [slug] --issue` com escopo do ofício
- [ ] Escritório externo contratado — [sim/quem/TBD]
- [ ] Petição de dilação agendada — [data]
- [ ] Caso criado no log — [sim/não/TBD — usualmente sim para qualquer coisa acima do menor ofício a terceiro]
- [ ] Análise de seguro / custeio — [se onerosidade é grande]
- [ ] Escalonamento interno — [quem]

---

## Recomendação

[Dois parágrafos: o que fazer. Postura de impugnação. Postura de produção. Se externo trata impugnações ou nós. Se pedir revogação.]

---

## Verificação de citações

Toda referência a regra, julgado, lei e regulamento nesta triagem — incluindo as citações de pesquisa do Passo 0, fundamentos de impugnação, e ponteiro de formato de rol de sigilo — é gerada por IA e não verificada. Antes de confiar em qualquer cite (especialmente em impugnações, pedido de revogação, ou correspondência com a parte requerente), rode pesquisa de verificação contra ferramenta de pesquisa (JusRatio, BNP, CJF, TJAM, DataJud, ou plataforma da banca) para acurácia, status de "ainda bom direito" e variantes locais. Citações fabricadas ou mal-citadas em documentos protocolados podem resultar em sanções (CPC art. 80; Provimento OAB 205/2021). Tags de fonte em cada citação (ex.: `[JusRatio]`, `[busca web — verificar]`) mostram de onde veio; tags `verificar` carregam maior risco de fabricação e devem ser checadas primeiro.
```

### Passo 9: Handoff

**Antes de responder ao ofício (juntar impugnações, exibir documentos, comparecer para depor, ou pedir revogação — qualquer resposta substantiva à parte requerente ou juízo):** Leia `## Quem está usando` em `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md`. Se o Papel é Não-advogado:

> Responder a ofício tem consequências jurídicas — perder prazo arrisca multa por desobediência (CP art. 330) ou astreintes (CPC art. 537); super-produzir quebra sigilo; sub-produzir arrisca sanções. Você revisou com advogado(a) ou Defensor(a) Público(a)? Se sim, prossiga. Se não, segue brief para levar:
>
> [Gere sumário de 1 página: o tipo de ofício, autoridade expedidora, prazos, escopo do requerido, framework de impugnação e força, issues de sigilo e onerosidade, postura proposta de resposta, o que pode dar errado, o que perguntar ao(à) advogado(a).]
>
> Se precisa achar advogado(a) habilitado(a) ou Defensor(a) Público(a) na sua localidade: o serviço de referência da OAB Seccional do estado (ou da Defensoria Pública Estadual/União) é o ponto de partida mais rápido.

Não prossiga além deste gate sem um sim explícito. Triagem, escopo, e agendamento interno não exigem o gate — a resposta à autoridade requerente exige.

- Se classificado como **requisição criminal** → pare, sinalize para escalonamento per `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md`, não prossiga com triagem padrão.
- Se classificado como **requisição administrativa**: sinalize que normas específicas do regulador aplicam; recomende externo regulatório.
- Caso contrário: ofereça criar caso (usualmente sim — ofícios são quase sempre materiais o suficiente para rastrear).
- Se dever de guarda não está emitido com escopo do ofício, handoff para `/legal-hold --issue` imediatamente.

## Feche com a árvore de decisão de próximos passos

Feche com a árvore de decisão de próximos passos per CLAUDE.md `## Outputs`. Customize as opções para o que esta skill acabou de produzir — as cinco ramificações default (redigir o X, escalonar, pegar mais fatos, observar e esperar, outra coisa) são ponto de partida, não trava. A árvore É o output; o(a) advogado(a) escolhe.

## O que esta skill não faz

- **Redige a peça final de impugnação.** Produz o framework; a peça é redigida pelo usuário + externo (futuro: skill dedicada de redação de impugnação).
- **Move pedido de revogação.** Aflora a opção; o pedido é trabalho jurídico que exige análise específica de jurisdição.
- **Valida regras entre jurisdições.** A pesquisa do Passo 0 produz a regra operativa para este ofício; a skill não confirma independentemente atualidade ou variantes locais. Sinalize para verificação por advogado(a) antes de agir.
- **Lida com requisições criminais.** Escalona. Está fora do escopo da triagem.
