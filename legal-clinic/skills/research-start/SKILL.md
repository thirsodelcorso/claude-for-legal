---
name: research-start
description: >
  Roteiro de pesquisa para uma tese jurídica — dispositivos a checar
  (planalto.gov.br), áreas de jurisprudência a investigar (BNP/CJF/TJAM),
  frameworks de pesquisa, termos para JusRatio/MCPs brasileiros. Pistas e
  roteiros, NÃO citações autoritativas; estagiários(as) verificam e
  desenvolvem tudo. Use quando estagiário(a) pergunta por onde começar a
  pesquisar, quer roteiro de pesquisa para uma questão, ou precisa
  identificar lacunas em pesquisa existente.
argument-hint: "[tese ou questão jurídica]"
---

# /research-start

1. Carregue `~/.claude/plugins/config/claude-for-legal/legal-clinic/CLAUDE.md` → jurisdição/vara, áreas de atuação.
2. Use o workflow abaixo.
3. Enquadre a questão especificamente. Construa roteiro: pontos de partida normativos (não-verificados), áreas de jurisprudência (não julgados específicos), fontes secundárias, termos de busca.
4. Se estagiário(a) tem pesquisa existente subida: sintetize e identifique lacunas.
5. Output com header "pistas, não autoridades" em destaque. Tudo é ponto de partida que o(a) estagiário(a) verifica.

```
/legal-clinic:research-start "defesa de habitabilidade em ação de despejo por falta de pagamento — Manaus, AM"
```

---

# Research Start: Roteiro, Não Pesquisa

## Propósito

Pesquisa jurídica é essencial à educação em estágio supervisionado. Mas a fase inicial — descobrir *o que* pesquisar, achar o dispositivo certo, entender o framework — é frequentemente o mais demorado e menos pedagógico. Estagiários(as) gastam horas achando o ponto de partida antes de poder fazer a pesquisa efetiva.

Esta skill produz o ponto de partida: dispositivos a checar, áreas de jurisprudência a investigar, termos de busca para JusRatio (proprietário, níveis A-E) e os MCPs open-source BNP/CJF/TJAM/DataJud. **Nada disso é verificado. Nada disso é autoritativo. Tudo é pista para o(a) estagiário(a) correr atrás.**

**Esta é salvaguarda pedagógica, não só ética.** Estagiários(as) ainda aprendem a pesquisar. Só começam de um lugar melhor.

## Carregar contexto

`~/.claude/plugins/config/claude-for-legal/legal-clinic/CLAUDE.md` → jurisdição/vara (UF), áreas de atuação.

## Workflow

### Passo 0: Docs-semente primeiro

**Antes de construir o roteiro, leia os docs-semente da unidade.** O(A) supervisor(a) subiu no cold-start (regimento, regras locais, formulários de intake, exemplo de caso, memos anteriores) — são pré-vetados, jurisdição-específicos, e vão bater qualquer query no JusRatio nos primeiros 20 minutos de pesquisa.

1. Leia `~/.claude/plugins/config/claude-for-legal/legal-clinic/CLAUDE.md` → `## Documentos-semente`. Identifique qualquer item cujo propósito ou nome de arquivo case com a área de pesquisa (ex.: "Manual do Núcleo de Saúde DPEAM" para questão de medicamento; exemplo de caso anonimizado na mesma área; memo anterior sobre a mesma questão).
2. Para cada match, surface como bloco **Docs-semente a ler primeiro** no topo do roteiro. Nomeie o arquivo, diga por que importa para esta questão específica, e diga o que provavelmente cobre vs. onde pesquisa externa ainda vai ser necessária.
3. Se nenhum doc-semente casa com a questão, diga clarmente ("Nenhum doc-semente casa com esta questão — indo direto a fontes primárias"). Não fabrique match.
4. Se a unidade tem flag `DADOS LIMITADOS` em `## Documentos-semente`, adicione nota: "Unidade tem menos de 10 docs-semente; precedente do(a) supervisor(a) é fino — pesar mais em fontes primárias e flag o que falta para supervisor(a)."

O roteiro ainda cobre dispositivos, áreas de jurisprudência, fontes secundárias, e termos de busca — docs-semente são primeira pista, não substituto do resto.

### Passo 1: Enquadre a questão

Qual a pergunta de pesquisa? Seja específico. Não "defesas de despejo" — "defesa de habitabilidade em ação de despejo por falta de pagamento, vara cível de Manaus, especificamente se aquecedor solar quebrado qualifica e se locatário(a) tinha que dar aviso por escrito".

Se a pergunta é muito ampla, restrinja com estagiário(a): "Isto são três perguntas. Vamos uma de cada vez. Qual primeiro?"

### Passo 2: Construa o roteiro

**Pontos de partida normativos:**
Liste dispositivos *provavelmente* relevantes. Indique explicitamente que são prováveis, não confirmados.

> **Dispositivos provavelmente relevantes** (NÃO-VERIFICADO — confirme vigência e aplicabilidade contra planalto.gov.br):
> - [Lei 8.245/91 — Lei do Inquilinato] art. 9º (hipóteses de despejo) e art. 62 II (purgação da mora)
> - CDC arts. 18-25 se relação de consumo (vício do serviço de locação se imóvel comercial)
> - Provimentos da Corregedoria-Geral do TJAM para particularidades locais
> - `[VERIFIQUE cada citação contra fonte oficial — dispositivos podem ter sido alterados; Lei 8.245 teve alterações pela Lei 12.112/09]`

**Áreas de jurisprudência a investigar:**
Não julgados específicos — *áreas*. Estagiário(a) acha os julgados.

> **Áreas de jurisprudência:**
> - STJ — decisões consolidadas sobre vício do imóvel locado e purgação da mora (use BNP-API para precedentes vinculantes e Temas Repetitivos)
> - TJAM — Câmara Cível e Turma Recursal sobre habitabilidade em locação na comarca de Manaus (use TJAM via e-SAJ)
> - Súmulas STJ sobre locação (Súmula 214 STJ — perda da fiança em caso de prorrogação)
> - Tese fixada em Tema Repetitivo se houver

**Fontes administrativas / regulatórias:**
Se aplicável.

> **Fontes administrativas:**
> - Para Saúde: RENAME (Relação Nacional de Medicamentos Essenciais); Lei 12.401/11 (incorporação CONITEC); resoluções da CONITEC e da CMED para preços
> - Para Previdenciário: Lei 8.213/91 + Decreto 3.048/99; Súmulas da TNU; informativos do INSS
> - Para Consumidor: orientações do Senacon e PROCONs
> - Para Família: Estatuto do Idoso (Lei 10.741/03), ECA (Lei 8.069/90), Lei Maria da Penha (Lei 11.340/06) — quando há vetor cruzado

**Fontes secundárias para orientar:**
Onde pegar o framework antes de mergulhar em fonte primária.

> **Fontes secundárias (para framework, não para citar):**
> - Manuais doutrinários BR de referência por área: Tartuce / Gonçalves (Civil), Marinoni / Didier (Proc. Civil), Pedro Lenza (Constitucional), Cláudia Lima Marques (Consumidor), Maurício Godinho (Trabalho)
> - Artigos de revistas como RT, Revista de Processo, Revista do Consumidor
> - Para Defensor: publicações da ESDPGE ou ESDPU sobre teses institucionais
> - Notas de prática do(a) seu(sua) supervisor(a) (se compartilhadas)

**Termos de busca:**
Para JusRatio (sintaxe `+termo`, `-termo`, `"frase"`) ou os MCPs open-source.

> **Termos de busca a tentar:**
> - **JusRatio:** `+"habitabilidade" +"locação" +"purgação da mora"` (pesquisar_documentos; filtra por nível A vinculante primeiro)
> - **BNP-API (STF/STJ vinculantes):** `+"locação" +"habitabilidade"` (buscar_precedentes — Tema Repetitivo aplicável?)
> - **CJF (STF/STJ/TRF):** `locação E habitabilidade E TJAM` (ou local equivalente)
> - **TJAM (e-SAJ):** `habitabilidade locação` (sintaxe permite E, OU, NAO sem acento; aspas para frase)
> - Refine baseado no que vier — estas são queries de partida

### Passo 3: Flag o que é incerto

Se a skill está em dúvida se uma fonte é relevante ou vigente:

> `[INCERTO: se o TJAM tem orientação consolidada sobre este recorte específico vs. doutrina comum — a busca vai te dizer]`

Incerteza é declarada, não escondida.

> **Sem suplementação silenciosa.** Esta skill produz pistas, não citações autoritativas — por design, estagiários(as) correm atrás. Mas se uma query a MCP retorna poucos ou nenhum resultado para regra ou julgado específico, diga e pare. NÃO fabrique citações de busca web ou conhecimento do modelo para preencher resultado fino sem perguntar. Diga: "A busca retornou [N] resultados em [ferramenta]. Cobertura parece fina para [regra]. Opções: (1) ampliar query, (2) tentar ferramenta diferente, (3) buscar na web — resultados marcados `[busca web — verificar]` e devem ser checados contra fonte primária, ou (4) parar e flag a lacuna para supervisor(a). Qual?" Supervisor(a) decide se aceita fontes de menor confiança.
>
> **Atribuição de fonte.** Marque toda citação sugerida com de onde veio: `[JusRatio]`, `[BNP]`, `[CJF]`, `[TJAM]`, `[DataJud]`, ou nome da tool MCP; `[busca web — verificar]` para web; `[conhecimento do modelo — verificar]` para citações lembradas do treino; `[usuário forneceu]` para citações fornecidas pelo(a) supervisor(a) ou pelo caso. Citações marcadas `verificar` carregam risco mais alto de fabricação e devem ser checadas primeiro. Nunca tire ou colapse as tags — dizem ao(à) estagiário(a) quais leads são pesquisa pura e quais são palpites do modelo a verificar contra fonte primária.

### Passo 4: Sintetize pesquisa subida (se houver)

Se o(a) estagiário(a) já fez pesquisa e sobe: leia, identifique o que está coberto e o que falta.

> **Da sua pesquisa até agora:**
> - Você tem: [sumário do que está coberto]
> - Lacuna: [o que o roteiro sugere e você ainda não achou]
> - `[VERIFICAR: o julgado que você citou — [nome] — rode no JusRatio ou BNP para conferir se está em vigor; pode ter sido superado por Tema Repetitivo mais recente]`

## Output

```markdown
═══════════════════════════════════════════════════════════════════════
  ROTEIRO DE PESQUISA — PISTAS, NÃO AUTORIDADES
  Nada abaixo é citação verificada. Cada dispositivo, cada área de
  jurisprudência, cada termo de busca é ponto de partida para SUA
  pesquisa. Você verifica vigência, aplicabilidade e precisão. Você acha
  os julgados efetivos. Se algo abaixo se revelar errado ou desatualizado,
  é esperado — este é mapa de onde olhar, não substituto de olhar.
═══════════════════════════════════════════════════════════════════════

# Roteiro de Pesquisa: [Questão]

**Vara/jurisdição:** [comarca + vara] | **Área de atuação:** [área]

## Docs-semente a ler primeiro

[Per Passo 0. Liste docs-semente da unidade que casam com a questão com nota de uma linha "o que provavelmente cobre". Se nenhum: "Nenhum doc-semente casa com esta questão — indo a fontes primárias."]

## Pontos de partida normativos (NÃO-VERIFICADOS)

[lista com flags VERIFICAR contra planalto.gov.br]

## Áreas de jurisprudência a investigar

[áreas, não julgados — direcionando para BNP (STF/STJ vinculantes), CJF (STF/STJ/TRF), TJAM (local)]

## Fontes administrativas / regulatórias

[se aplicável — agências, ANS, ANATEL, INSS, SUS conforme área]

## Fontes secundárias (para framework, não citação)

[manuais doutrinários BR por área]

## Termos de busca

**JusRatio:** [queries com sintaxe + - "frase"]
**BNP-API:** [queries para precedentes STF/STJ vinculantes]
**CJF:** [queries com operadores E OU NAO]
**TJAM (e-SAJ):** [queries para jurisprudência local]
**DataJud (se quer ver processos do mesmo tipo):** [exemplos de números CNJ similares na comarca]

## Flags de incerteza

[Em todo lugar onde o roteiro genuinamente está em dúvida]

---

## O que fazer com isto

1. Comece com fonte secundária para pegar o framework (manual doutrinário)
2. Ache e leia os dispositivos primários — confirme as citações acima contra planalto.gov.br
3. Rode as buscas no JusRatio primeiro (níveis A/B de autoridade priorizados), depois BNP/CJF/TJAM
4. Para todo julgado citado em peça, verifique no JusRatio se segue em vigor (sem overruling/modulação)
5. Volte e rode `/memo` para escafoldar sua análise em FIRAC quando tiver a regra

## O que este roteiro NÃO faz

- **Não te dá citações para você usar.** Cada cite acima é pista a verificar, não autoridade para confiar.
- **Não faz a pesquisa.** Você faz. Isto te leva ao ponto de partida mais rápido.
- **Não substitui MCPs.** Eles têm os julgados efetivos. Isto te diz onde apontar.

---

**Verificação de citação — exigida antes do uso.** Citações acima foram geradas por modelo de IA e não foram verificadas. Antes de confiar em julgado, dispositivo, súmula, Tema — ou incluir em produto para assistido(a) — rode no JusRatio (níveis A-E), BNP (precedentes vinculantes STF/STJ), CJF (federal), TJAM (e-SAJ local), ou planalto.gov.br para dispositivo legal. Flag citações não-verificadas ao(à) seu(sua) supervisor(a).
```

## O que esta skill NÃO faz

- **Fornece citações autoritativas.** Explicitamente, por design. Estagiário(a) verifica toda cite antes de usar.
- **Substitui pesquisa jurídica.** Acelera a fase "por onde começo"; a pesquisa em si ainda é do(a) estagiário(a).
- **Garante que o roteiro está completo.** É conjunto de pistas iniciais. A pesquisa pode revelar fontes que o roteiro perdeu — tudo bem, é pesquisa.

## Feche com a árvore de decisão de próximos passos

Termine com a árvore per CLAUDE.md `## Outputs`. Customize as opções ao que esta skill acabou de produzir.
