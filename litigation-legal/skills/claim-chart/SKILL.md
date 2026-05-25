---
name: claim-chart
description: >
  Constrói ou revisa matriz de elementos — matriz cível (qualquer causa de
  pedir ou defesa, com base no element-templates.md de teses brasileiras:
  BPC/LOAS, saúde pública, plano de saúde, consumidor, alimentos, divórcio,
  união estável, posse, despejo, defesa em cobrança) ou, secundariamente,
  matriz de patente (LPI 9.279/96) — com cada célula com pinpoint e detecção
  de lacuna como output prioritário. Use quando pedir matriz de elementos,
  matriz cível, mapeamento elemento-por-elemento, ou perguntar o que falta
  para provar uma tese.
argument-hint: "[--civil | --patente] [--assercao | --invalidade | --review] [--target <slug>]"
---

# /claim-chart

1. Carregue `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md` → papel, cabeçalho de sigilo, postura de decisão, armazenamento documental.
2. Se workspaces de caso habilitados, confirme ou selecione o caso ativo; carregue `matter.md` (posição, jurisdição/vara, fase, tese, peças).
3. Siga o workflow e a referência abaixo.
4. Seleção de modo:
   - `--civil` → matriz cível. Exija a causa de pedir (ou defesa) e a posição.
   - `--patente` → matriz de patente (escopo reduzido — LPI 9.279/96). Exija número do registro e ao menos uma reivindicação asseverada.
   - Sem flag → pergunte qual.
5. Para modo cível: consulte `references/element-templates.md` no diretório da skill para a lista-baseline de elementos (matriz Defensoria BR — Família, Saúde, Previdenciário, Consumidor, Locação/Possessória, Defesa em cobrança). Confirme a base legal/súmula/Tema controlante com o(a) usuário(a) antes de mapear.
6. Para modo patente: parse das reivindicações asseveradas em elementos, flag termos disputados, aplique qualquer parecer pericial técnico.
7. Mapeie elementos contra o alvo (produto acusado / anterioridade / corpo probatório / matriz sob revisão). Cada célula com pinpoint. Aplique a neutralização de prefixo apóstrofo antes de escrever qualquer valor de célula começando com `=`, `+`, `-`, `@`, tab, ou CR.
8. Produza a lista de lacunas (cível) ou lista de evidência necessária (patente) — o output prioritário.
9. Escreva markdown, CSV (valores + `_sources` companion), e Excel ou Sheets per preferência do(a) usuário(a). Cabeçalho de sigilo em todo output.
10. Escreva na pasta `claim-charts/` do caso se há caso ativo; senão na pasta `claim-charts/` de nível-prática. Anexe entrada de uma linha em `history.md` se há caso ativo.
11. Retorne sumário: tese(s), alvo(s), jurisdição/vara, fase, contagens de elementos por estado, a lista de lacunas, caminhos de arquivo, e o lembrete que toda célula é um lead.

---

# Matriz de Teses (Claim Chart)

## Restrições de uso de documentos exibidos

Antes de trabalhar com um conjunto de documentos de contencioso, pergunte: "Algum desses documentos foi obtido por exibição/intimação judicial?" Se sim:

- **Brasil:** Documentos juntados aos autos sob segredo de justiça (CPC art. 189) só podem ser usados na esfera processual em que foram juntados. Documentos da DP carregam sigilo do(a) assistido(a) (LC 80/94 art. 4º-A V) — não podem ser usados em matéria estranha sem consentimento.
- **Para Defensor:** documentos da pasta do(a) assistido(a) ficam restritos a este atendimento. Reuso para outro(a) assistido(a) (mesmo com tese similar) exige consentimento e anonimização.

Confirme: "Este uso está dentro do processo em que os documentos foram juntados, OU tenho consentimento, OU os documentos são públicos." Se não confirmado, flag: "⚠️ Documentos exibidos podem ter restrições de uso. Confirme que este uso é permitido antes de prosseguir."

## UMA MATRIZ É UMA MINUTA, NÃO UM ACHADO OU UMA TESE PROTOCOLADA

**Coloque isto no topo de todo output. Não deixe cair. Não suavize.**

> Esta matriz é minuta para análise e verificação do(a) advogado(a)/Defensor(a), não tese protocolada, peça de mérito, sustentação oral, ou parecer jurídico. Cada mapeamento é um lead que o(a) profissional deve verificar contra a fonte. Os elementos listados vêm de doutrina, base legal, ou parse da reivindicação — a base legal **controlante** na jurisdição (lei vigente, súmula, Tema Repetitivo, jurisprudência local consolidada) pode diferir e sempre controla. Detecção de lacuna é ponto de partida para instrução probatória ou pedido; não é conclusão sobre o mérito.

Sub-flagar lacuna é porta de mão única — petição inicial sem demonstração de elemento, defesa sem prova para elemento disputado, ou caso julgado sem prova de dano. Super-flagar é porta dupla — o(a) profissional limpa flags em revisão. O default tende para a porta dupla.

---

## Contexto do caso

Cheque `## Workspaces de caso` no CLAUDE.md de nível-prática. Se `Habilitado` é `✗` (default para DJ corporativo), pule o resto deste parágrafo — skills usam contexto de nível-prática e a maquinaria de caso é invisível. Se habilitado e não há caso ativo, pergunte: "Para qual caso? Rode `/litigation-legal:matter-workspace switch <slug>` ou diga `nível-prática`." Carregue o `matter.md` do caso ativo — especialmente a tese, a petição inicial / contestação (para os elementos efetivamente alegados), a vara/jurisdição, qualquer parecer pericial técnico ou interpretação consolidada, e a fase. Escreva outputs na pasta do caso em `~/.claude/plugins/config/claude-for-legal/litigation-legal/matters/<slug>/claim-charts/`. Nunca leia arquivos de outro caso a menos que `Contexto cruzado entre casos` esteja `ligado`.

---

## Carregar contexto

- `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md` → papel, cabeçalho de sigilo, postura de decisão, armazenamento documental, scaffold de tese
- `matter.md` do caso ativo — teses, defesas, posição, vara, fase, tese
- Para modo cível: a petição inicial ou contestação (para as causas efetivamente articuladas), a contestação (para as defesas efetivamente articuladas), a base legal aplicável, a súmula ou Tema relevante, e o corpo probatório — transcrições de oitiva, declarações, documentos juntados, laudos periciais.
- Para modo patente: a carta-patente, as reivindicações asseveradas, o relatório descritivo, histórico do depósito se disponível, o produto acusado ou anterioridade.

Se `CLAUDE.md` tem marcadores `[PLACEHOLDER]`, surface esta bifurcação:

> Notei que você ainda não configurou seu perfil de atuação — é assim que eu calibro risco, panorama e estilo da casa.
>
> **Duas escolhas:**
> - Rode `/litigation-legal:cold-start-interview` (2 minutos) para configurar, depois rodo isto calibrado para a SUA prática.
> - Diga **"provisional"** e eu rodo contra defaults genéricos — jurisdição BR, apetite médio, papel advogado(a), sem playbook — e marco todo output como `[PROVISIONAL — configure seu perfil para output calibrado]`.

### Modo provisional

Se a pessoa diz "provisional", monte a matriz normalmente usando estes defaults genéricos: apetite médio, papel advogado(a), jurisdição BR, sem playbook de nível-prática (trabalhe das peças do caso e dos elementos das teses como articuladas). Marque a nota do revisor e cada linha da matriz com `[PROVISIONAL]`. No final, anexe:

> "Esta foi rodada genérica contra defaults. Rode `/litigation-legal:cold-start-interview` para output calibrado para a SUA prática — sua calibração de risco, seu panorama, seu estilo. 2 minutos."

**Gate de conflitos — não bypassável.** Antes de construir matriz, cheque `~/.claude/plugins/config/claude-for-legal/litigation-legal/matters/_log.yaml` para o slug do caso. Se o caso não está em `_log.yaml`, recuse e roteie:

> "Não vejo [slug] no log. Rode `/litigation-legal:matter-intake` primeiro para a checagem de conflitos/impedimentos rodar e o workspace ser setup. Não construo matriz em caso não-intaken — a checagem é o gate."

Não prossiga em caso não-intaken.

---

## Seleção de modo

Pergunte no topo, antes de tudo:

> Qual tipo de matriz?
>
> 1. **Matriz cível** — elementos de uma causa de pedir (ou defesa) mapeados contra a prova. Para checagem de adequação da petição inicial, planejamento de instrução, preparação para julgamento, esboço de ordem de prova. **Para Defensor: este é o modo principal** — teses repetitivas (BPC/LOAS, saúde, consumidor, alimentos, posse, despejo) com súmulas e Temas STF/STJ aplicáveis.
> 2. **Matriz de patente** — mapeamento elemento-por-elemento de reivindicação contra produto acusado (asserção de infração), anterioridade (nulidade), ou outra matriz para revisão. Para contencioso de PI sob LPI 9.279/96. **Escopo reduzido** — Defensor cível raramente atua em PI; advocacia privada de PI usa este modo.

Mais intake (comum a ambos):

- **Posição.** Asseverando ou defendendo? (Em cível inverte o ônus probatório; em patente inverte enquadramento de infração/nulidade.)
- **Jurisdição / vara.** UF e juízo — base legal e jurisprudência variam por jurisdição. Em patente, varas especializadas (Justiça Federal RJ/SP em geral) seguem regras técnicas próprias. Flag qual controla.
- **Fase.** Pré-protocolização, fase postulatória, instrução, fase decisória, recurso. A matriz é a mesma; o enquadramento do output muda.
- **Matriz existente?** Se `--review`, carregue.

---

# MODO 1 — Matriz Cível (foco do piloto Defensor)

Mapeie os elementos de uma causa de pedir (ou defesa) contra a prova. Os outputs matadores são (a) matriz que diz qual prova vai com qual elemento e (b) lista de lacunas que diz ao(à) profissional o que falta.

## Workflow

### Passo 1: Identifique a tese

- Qual causa de pedir? (Ou defesa?) Se múltiplas, matriz cada separadamente.
- Qual posição? Assistido(a)/autor(a) demonstrando os elementos da pretensão; Defensor(a)/réu(ré) mapeando lacunas e defesas (desafiando os elementos). Leia `## Posição processual` no perfil para o default — `autor` defaulta para mapear a pretensão (provar os elementos); `réu` defaulta para mapear lacunas e defesas (afastar ou esvaziar os elementos). Confirme antes de começar.
- Qual jurisdição/vara? UF, comarca, juízo. **Elementos e jurisprudência variam por jurisdição.** A biblioteca de templates é baseline; a súmula ou Tema controlante controla.
- Qual peça? Carregue a petição inicial / contestação / réplica para a matriz rastrear as causas efetivamente articuladas, não versão genérica.

### Passo 2: Carregue os elementos

Três caminhos:

**(a) Biblioteca de templates.** Referencie `references/element-templates.md` (no diretório desta skill). Elementos baseline para causas de pedir e defesas recorrentes em DP cível brasileira (Família, Saúde, Previdenciário, Consumidor, Locação/Possessória, Defesas em cobrança), com referência à base legal + súmula/Tema aplicável + nota sobre hipossuficiência presumida (Súmula 481 STJ). Selecione o template que casa com a causa articulada.

**(b) Custom.** Usuário(a) define elementos, ou cola texto de dispositivo / súmula / Tema / um trecho da inicial para parse. Parse em elementos numerados.

**(c) Defesas.** Também suportamos mapear defesas — prescrição (CC 205-206), decadência (CC 178), ilegitimidade (CPC 17), litispendência, coisa julgada, perempção, prescrição intercorrente (Tema 566 STJ), purgação da mora em despejo (Lei 8.245 art. 62 II), exceção de impenhorabilidade (CPC 833), etc. Defesas têm seus próprios elementos que o(a) réu(ré) deve provar (ou, para algumas, o(a) autor(a) deve negar uma vez arguida).

**Formulações específicas por jurisdição/tema — surface proativamente.** Se o perfil ou o `matter.md` do caso indica varas do TJAM, surface a posição local de TJAM e Turmas Recursais sobre a tese, comparando com STJ. Não force o(a) usuário(a) a ensinar a skill sobre a posição local — a skill oferece e o(a) usuário(a) escolhe.

Súmulas/Temas a surface sem ser pedido (não-exaustivo — adicione ao perfil conforme padrões recorrem):

| Tese | Súmula/Tema canônico | Notas |
|---|---|---|
| Fornecimento de medicamento SUS | Tema 793 STF (RE 855.178 RG) — responsabilidade solidária dos entes; Tema 106 STJ (REsp 1.657.156) — requisitos para medicamento não-RENAME; Tema 6 STF (RE 566.471) — alto custo | Aplicar de plano em ação contra qualquer ente; pedir tutela urgência |
| BPC/LOAS | Tema 27 STF (RE 567.985 RG) — miserabilidade por outros meios; Súmula 80 TNU — laudo médico; Tema 350 STF — prévio requerimento administrativo | Justiça Federal; DP estadual encaminha à DPU se houver |
| Plano de saúde | Súmula 469 STJ — CDC aplica; Súmula 302 STJ — limite temporal abusivo; Súmula 597 STJ — abusividade de limites em contratos individuais | CDC + Lei 9.656/98 |
| Cobrança indevida + dano moral | CDC art. 42 par. único; Súmula 385 STJ — inscrição indevida; Tema 929 STJ — repetição em dobro (Repetitivo 622) | JEC ou Cível comum |
| Alimentos | CC 1.694-1.710; Súmula 358 STJ — exoneração na maioridade; Lei 5.478/68 | Família |
| Divórcio | CC 1.571-1.582; EC 66/2010 dispensa prazo; Lei 11.441/07 + CPC 731-734 (extrajudicial) | Família |
| União estável | CC 1.723-1.727; ADI 4.277/ADPF 132 STF — homoafetiva; RE 646.721 + RE 878.694 — sucessórios | Família |
| Despejo | Lei 8.245/91 arts. 9º + 59-66; purgação CPC + Lei 8.245 art. 62 II | Cível comum |
| Possessória | CPC 554-568 + CC 1.196-1.224 | Cível |
| Vício de produto | CDC arts. 18-25; Súmula 297 STJ — CDC para bancos | JEC se ≤ 40 SM |

Quando uma formulação varia, a matriz abre com callout de uma linha:

> **Nota jurisdicional:** Você me disse que isto é um caso em [vara/jurisdição]. Aqui está como a posição local difere do baseline: [divergência]. A matriz abaixo usa a formulação [local/STJ]. Se está errado, diga e eu recarrego.

Confirme a lista de elementos com o(a) usuário(a) antes de mapear. Para Defensor: sempre confirme se há súmula ou Tema posterior ao último update da biblioteca.

### Passo 3: Mapear

Para cada elemento:

- **Prova sustentadora** — o que prova este elemento? Cite a fonte com pinpoint.
  - Depoimento — `[Depoimento de Maria S., fl. 42 / movimento ID 145]`
  - Declaração — `[Declaração de hipossuficiência da assistida, fl. 12]`
  - Documento juntado — `[Doc 5 da inicial — laudo médico Dr. X, fl. 18]`
  - Confissão — `[Resposta a impugnação fl. 56]`
  - Exibição — `[Doc juntado em audiência, ID 220]`
  - Laudo pericial — `[Laudo perito médico judicial, fl. 89]`
  - Resposta à intimação — `[Ofício SUS-AM em resposta à intimação, fl. 34]`
  - Lei / julgado — para elementos puramente jurídicos
- **Citação literal** onde a prova é testemunhal ou documental. Sem paráfrase.
- **Prova contrária** — o que corta para o outro lado? Cite. É a vulnerabilidade da linha.
- **Força** — `forte` / `moderada` / `fraca` / `ausente`. Mantenha simples. Notas de força sobre-calibradas são ruído; `fraca` e `ausente` são as linhas que importam.
- **Estado por célula** — `sustentado` / `parcial` / `controvertido` / `lacuna` / `requer-instrução`.

### Passo 4: Detecção de lacuna — o output matador

Depois de mapear, produza lista de lacunas. Este é o ponto da matriz.

> **Elementos com prova fina ou nenhuma:** [lista]
>
> - Se asseverando (autor): estas comprometem a plausibilidade da inicial (CPC 330 I — inépcia), defesa em impugnação a contestação, ou caso em julgamento. Feche-as antes da próxima petição.
> - Se defendendo: estes são seus alvos de defesa / impugnação. O(a) autor(a) tem que provar cada elemento; uma lacuna é defesa.
> - Se pré-instrução: estas são suas prioridades de prova — depoimentos, intimações, perícias que viram lacuna em `sustentado` ou confirmam `ausente`.

Detecção de lacuna não é conclusão sobre o mérito. É mapa de onde o caso é fino.

### Passo 5: Enquadramento por fase

Pergunte a fase. Mesma matriz; enquadramento diferente do output:

- **Pré-protocolização / postulatória.** A petição inicial alega cada elemento com plausibilidade (CPC 330 I)? Qualquer elemento alegado sem fundamento factual é alvo de inépcia.
- **Instrução.** Para cada `lacuna` ou `requer-instrução`, qual prova é necessária? Quais testemunhas, quais documentos, quais perícias.
- **Fase decisória.** Para cada elemento, há controvérsia real de fato material? Célula `sustentada` para o(a) movente sem prova contraditória é munição; célula `controvertida` impede julgamento antecipado.
- **Audiência de instrução e julgamento.** Ordem de prova. Quais testemunhas provam cada elemento, quais documentos provam, quem autentica. A matriz vira o roteiro da audiência.

### Passo 6 (sub-modo review): Auditoria

Para defesa adversária, contestação do(a) réu(ré), ou minuta de escritório externo: para cada elemento, a prova citada efetivamente comprova? Onde a matriz deles está fina? Qual seu contra mais forte?

## Guardrails do modo cível (além dos compartilhados)

- **Jurisdição.** A lista é baseline. Sempre confirme a súmula/Tema controlante. Indique a fonte na planilha `_elements`.
- **Causas articuladas só.** Mapeie o que é efetivamente articulado. Não adicione causa que a inicial não alega só porque os fatos podem sustentar — é análise diferente.
- **Defesas.** Se mapeando defesas, note se o ônus é do(a) réu(ré) (a maioria) ou se levantar a defesa transfere ônus ao(à) autor(a) (algumas, como prescrição alegada).
- **"Lacuna" ≠ "caso perdido".** Lacuna é lead. Instrução, declaração, ou laudo podem fechar. A matriz mostra onde cavar.

---

# MODO 2 — Matriz de Patente (escopo reduzido)

*Defensor cível raramente atua em PI. Esta seção é referência para casos isolados ou para advocacia privada/in-house de PI que use o plugin. Mantida em forma resumida.*

## Sub-modos

- `--asserção` — elementos da reivindicação vs. produto acusado (inicial de obrigação de não-fazer por infração ao direito de patente; expert reports em ação de infração)
- `--invalidade` — elementos da reivindicação vs. anterioridade (ADC/declaratória de nulidade administrativa ou judicial)
- `--review` — auditar matriz produzida por outro

## Workflow patente (resumido)

1. **Parse das reivindicações.** Preâmbulo, transição (compreendendo/consistindo de), elementos numerados [1a], [1b], [1c]. Marque termos de função (LPI art. 24) e termos estruturais que podem ser controvertidos.
2. **Checagem de interpretação.** Termos disputados — definidos no relatório? Modificações via histórico do depósito? Termos relativos? Funcionais (cuidado com indefinição)? Aplicar pareceres periciais técnicos existentes.
3. **Mapear** cada elemento contra o alvo. Estados possíveis: `literal` / `literal-interpretação-dependente` / `equivalente` (apenas asserção) / `antecipação` (apenas invalidade — todo elemento em uma única anterioridade) / `obviedade-combinação` (apenas invalidade — anterioridade primária + secundária com motivação) / `parcial` / `não-encontrado` / `requer-evidência`.
4. **Reivindicações dependentes.** Produzir linhas para cada uma — não gesticular. A matriz deve conter linhas para cada reivindicação asseverada.
5. **Equivalentes — função/modo/resultado.** Quando o mapeamento literal é interpretação-dependente, produzir linha pareada de equivalente com sketch de função/modo/resultado.
6. **Para invalidade:** ônus é elevado (presunção de validade da patente concedida; doutrina BR pede prova robusta). Antecipação requer um único documento contendo todos os elementos; obviedade requer combinação com motivação justificada.

## Guardrails do modo patente

- Base legal: LPI Lei 9.279/96; matérias em Justiça Federal (varas especializadas RJ/SP); INPI como parte/litisconsorte em nulidade.
- Doutrina relevante: Denis Borges Barbosa, Newton Silveira; comparados em obviedade e equivalência.
- Toda matriz é minuta — laudo pericial técnico do(a) perito(a) judicial é o que efetivamente decide.

---

# Chassi compartilhado (ambos os modos)

## Output

Prefixe o cabeçalho de sigilo do `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md` `## Outputs`.

### Tabela markdown (sempre)

Uma tabela por tese/defesa/reivindicação por alvo.

**Exemplo modo cível (BPC/LOAS contra INSS):**

```markdown
| [#] | Elemento | Prova sustentadora (pinpoint) | Prova contrária | Força | Estado | Verificado |
|---|---|---|---|---|---|---|
| 1 | Idade ≥ 65 anos OU deficiência | [Doc 2 — RG do assistido — 68 anos] | nenhuma | forte | sustentado | ☐ |
| 2 | Miserabilidade (renda per capita < 1/4 SM ou Tema 27 STF) | [Doc 3 — CadÚnico — renda familiar R$ 220/mês com 4 pessoas; per capita = R$ 55] | nenhuma | forte | sustentado | ☐ |
| 3 | Não enquadramento em benefício securitário | [Doc 4 — CNIS sem vínculo formal por 20 anos] | nenhuma | forte | sustentado | ☐ |
| 4 | Prévio requerimento administrativo (Tema 350 STF) | [Doc 1 — indeferimento INSS NB X em 12/03/2026] | nenhuma | forte | sustentado | ☐ |
| 5 | Cadastro CadÚnico atualizado | [Doc 3 — cabeçalho CadÚnico data 02/2026] | nenhuma | forte | sustentado | ☐ |
```

**Exemplo modo cível (Fornecimento de medicamento — defesa do Estado):**

```markdown
| [#] | Elemento (autor deve provar) | Prova autor | Lacuna defensiva | Estado |
|---|---|---|---|---|
| 1 | Prescrição médica fundamentada | [Doc 5 — receita Dr. X — privada, não SUS] | Receita não-SUS pode ser questionada quanto à compatibilidade com protocolos | parcial |
| 2 | Hipossuficiência financeira | [Declaração de hipossuficiência presumida — Súmula 481 STJ] | Hipossuficiência presumida do(a) assistido(a) DP — difícil afastar | sustentado |
| 3 | Imprescindibilidade do medicamento | [Doc 6 — laudo] | Tema 106 STJ exige: laudo + comprovação inadequação dos disponíveis + registro ANVISA | parcial — falta comprovação inadequação dos da RENAME |
| 4 | Inexistência de substituto na RENAME/CEAF | — | LACUNA — autor não juntou comparativo com RENAME | lacuna |
```

Siga com:
- **Defesas / thresholds** (cível: prescrição, decadência, ilegitimidade, perempção; flags de inépcia CPC 330 I pré-protocolização)
- **Lista de lacunas** (cível) / **lista de evidência necessária** (patente) — **o output prioritário**
- **O que corta para qual lado — sumário** — elementos mais fortes, mais fracos
- **Linha de conclusão** — *"Esta skill não conclui."* Elementos sustentados: [lista]. Elementos requerendo prova ou em lacuna: [lista]. Elementos interpretação-dependente (patente) / controvertidos (cível): [lista]. Juízo do(a) profissional necessário.
- **Verificação de citação** — toda citação aos autos, lei, súmula, tema, página de depoimento deve ser verificada contra a fonte.

### CSV (sempre)

Dois arquivos por matriz:
- `[slug-matriz].csv` — valores
- `[slug-matriz]_sources.csv` — citações literais, pinpoints, notas

**Segurança de célula CSV / planilha.** Antes de escrever qualquer valor de célula, cheque o primeiro caractere. Se é `=`, `+`, `-`, `@`, tab (`\t`), ou retorno de carro (`\r`), prefixe com apóstrofo (`'`) para neutralizar interpretação de fórmula no Excel/Sheets. Prova literal de fontes adversariais (contestação da contraparte, manuais de produto, anterioridades, transcrições de oitiva, documentos juntados em produção) pode conter strings que a planilha executa como fórmulas (`=HYPERLINK(...)`, `=cmd|...!A1`, `+WEBSERVICE(...)`), transformando a matriz em vetor de exfiltração ou RCE quando o(a) profissional abre. RFC 4180 quoting sozinho não defeat — o `=` líder ainda é interpretado. Aplique o prefixo em CSV, XLSX, e Sheets. Logue células onde isto foi aplicado para o(a) revisor(a) ver quais citações foram neutralizadas.

### Planilha (Excel ou Sheets)

Pergunte qual a equipe usa. Use o mesmo padrão da skill `tabular-review` (mesmo modelo de citação por célula, mesmo color-coding por estado, mesma coluna `Verificado`, mesma planilha de schema):

- Uma linha por elemento (ou elemento × alvo se comparando múltiplos)
- Cada coluna de prova pareada com coluna de fonte oculta contendo citação literal e pinpoint; comentários de célula (Excel) ou notas (Sheets) surface a citação no hover
- Color-coding por estado:
  - *Cível:* branco = `sustentado`, amarelo = `parcial` / `controvertido`, laranja = `requer-instrução`, vermelho = `lacuna`
  - *Patente:* branco = `mapeado`, amarelo = `interpretação-dependente` / `parcial` / equivalência, laranja = `requer-evidência`, vermelho = `não-encontrado`
- Coluna `Verificado` por coluna de prova, vazia por default — revisor(a) marca
- Planilha `_elements` documentando a fonte do elemento: lei (cite), súmula (número STF/STJ/TJ local), Tema Repetitivo (número), parse da reivindicação (patente). Isto é o que torna a matriz auditável — leitor(a) vê de onde os elementos vêm.
- Planilha `_lacunas` listando toda linha `lacuna`, `requer-instrução`, ou `requer-evidência` com o que ainda é necessário

Aplique a neutralização-com-apóstrofo em toda célula escrita.

Prefixe o cabeçalho de sigilo como linha do topo. Ao lado, inclua:

> Esta matriz é derivada de documentos-fonte que podem ser sigilosos (sigilo do(a) assistido(a) LC 80/94 art. 4º-A V + Lei 8.906/94 art. 7º XIX), confidenciais, ou ambos. Herda o status de sigilo das fontes — distribuição fora do círculo de sigilo pode constituir violação ético-disciplinar. Armazene com arquivos do caso e tome decisões de distribuição deliberadamente. Nada nesta matriz foi protocolado; é minuta para revisão.

### Nome de arquivo e localização

- Cível: `matriz-civel-[tese-slug]-[lado]-AAAA-MM-DD.{md,csv,xlsx}`
- Patente asserção: `matriz-patente-asserção-[numero]-[claim]-[alvo]-AAAA-MM-DD.{md,csv,xlsx}`
- Patente invalidade: `matriz-patente-invalidade-[numero]-[claim]-[ref]-AAAA-MM-DD.{md,csv,xlsx}`
- Review: `revisao-matriz-[assunto]-AAAA-MM-DD.{md,csv,xlsx}`

Se workspaces ativados e caso ativo: `~/.claude/plugins/config/claude-for-legal/litigation-legal/matters/<slug>/claim-charts/`. Senão: `~/.claude/plugins/config/claude-for-legal/litigation-legal/claim-charts/`. Surface o caminho. Anexe entrada de uma linha em `history.md`.

## Sumário

Depois da matriz escrita, dê um readout de uma tela:

- Tese(s) / causa(s) / reivindicação(ões), alvo(s), vara/jurisdição, fase
- Elementos: sustentados/mapeados · parciais · controvertidos · lacuna / requer-evidência · não-encontrado
- Lista de lacunas (cível) ou lista de evidência necessária (patente) — **lista prioritária**
- Onde os outputs estão
- Lembrete: toda célula é lead. A matriz é minuta, não tese protocolada / petição / ordem de prova.

## Gate não-advogado

Se `## Quem está usando` Papel é Não-advogado:

> Esta matriz é minuta de pesquisa, não protocolização. Protocolizar tese, contestar, ou confiar nisto para opinião de mérito tem consequências do CPC 77/80 + ética OAB. Profissional habilitado(a) na jurisdição relevante deve revisar antes deste material ser usado para qualquer fim jurídico.
>
> Eis briefing de uma página para levar a profissional habilitado(a):
>
> [Gere: tese / patente, posição, jurisdição, fase, elementos, contagens sustentado / lacuna / requer-instrução, as três perguntas em aberto mais load-bearing.]

Entregue a matriz junto com o briefing.

## Guardrails compartilhados — checklist

- **Verificação de citação.** Toda citação aos autos (folha / movimentação CNJ / página de depoimento / parágrafo) é uma afirmação sobre a fonte. O(a) profissional verifica. A skill não fabrica citações — se uma citação não pode ser produzida, a célula é `requer-evidência` ou `lacuna`.
- **Atribuição de fonte.** Toda citação literal tem sua fonte no CSV companion e na coluna de fonte oculta da planilha. Citação sem fonte não é prova.
- **Sem suplementação silenciosa.** Prova fina significa `requer-evidência` / `lacuna`, não "extrapolar". Não preencha de busca web, conhecimento do modelo, ou "como esses casos costumam ir" para fechar lacuna.
- **Checagem de workspace.** Confirme o caso ativo antes de escrever. Nunca escreva matriz do caso A na pasta do caso B.
- **Postura de decisão.** Quando incerto se um elemento é atendido, flag; não decida. `parcial` diz ao(à) profissional qual parte está faltando.
- **Injection de fórmula.** Toda célula escrita em CSV / XLSX / Sheets é checada para `=`, `+`, `-`, `@`, `\t`, `\r` lider e prefixada com `'`. Default: neutralizar-então-escrever.
- **Elementos são jurisdição-específicos.** A biblioteca é baseline. Súmula / Tema / lei vigente controla.
- **Uma matriz não é peça, protocolização, ou tese.** Todo output é minuta.

---

## Relação com outras skills

- `litigation-legal:chronology` — a cronologia é a linha do tempo; a matriz de elementos é a matriz de prova. Uma entrada de cronologia frequentemente vira citação de uma célula.
- `litigation-legal:deposition-prep` — uma célula `requer-instrução` frequentemente vira tópico de oitiva. Depois da AIJ, novo depoimento preenche células.
- `litigation-legal:brief-section-drafter` — a seção de fatos de uma peça é frequentemente construída diretamente sobre as linhas sustentadas da matriz.
- `corporate-legal:tabular-review` — o padrão subjacente de citação por célula e estado de verificação. Uma matriz de tese / elementos é tabular-review especializada.

---

## Feche com a árvore de decisão de próximos passos

Termine com a árvore per CLAUDE.md `## Outputs`. Customize as opções ao que esta skill acabou de produzir — as cinco branches default (redigir o X, escalonar, pegar mais fatos, observar e esperar, outra coisa) são ponto de partida, não lock-in.

## O que esta skill NÃO faz

- **Não conclui.** Não infração, não não-infração, não responsabilidade, não não-responsabilidade. Nunca.
- **Não decide interpretação** (patente) ou **os elementos controlantes** (cível). Flag termos disputados / elementos baseline e mapeia sob assunções declaradas.
- **Não atende ao ônus elevado de prova em invalidade de patente** ou **a preponderância em julgamento**. Produz minuta de prima facie para revisão.
- **Não substitui análise pericial.** Laudo pericial técnico, oitiva especializada, parecer perito são produtos separados que esta matriz roteia para, não substitui.
- **Não protocola, intima, ou assina coisa alguma.** Todo output é minuta. Profissional habilitado(a) protocola e assina.
- **Não extrapola.** Se a prova não está aí, a célula é `requer-instrução` / `lacuna` — nunca palpite.
