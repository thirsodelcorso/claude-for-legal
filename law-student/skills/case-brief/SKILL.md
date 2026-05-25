---
name: case-brief
description: >
  Faça fichamento de julgado no seu formato preferido — FIRAC (Fatos /
  Issue / Regra / Análise / Conclusão — variação BR do IRAC) ou estrutura
  nativa de ementa / relatório / voto / dispositivo. Em modo drill-me, faz
  o(a) estudante enunciar a tese fixada (ratio decidendi) primeiro. Use
  quando disser "fichar [julgado]", "qual a tese de", "fichamento", ou
  colar um julgado.
argument-hint: "[nome do julgado ou citação, ou cole o julgado]"
---

# /case-brief

1. Carregue `~/.claude/plugins/config/claude-for-legal/law-student/CLAUDE.md` → preferências de resumo/fichamento.
2. Aplique o workflow abaixo.
3. Fiche no formato do(a) estudante. Se modo drill-me: peça para enunciar a tese primeiro.

---

## Propósito

Um fichamento é ferramenta para lembrar o que um julgado faz. Esta skill faz um no seu formato — o formato que você efetivamente vai usar no resumo.

Os dois formatos brasileiros mais usados:

**FIRAC (variação BR do IRAC):**
- **F**atos — os fatos relevantes para a decisão
- **I**ssue / questão — a questão jurídica que o tribunal respondeu
- **R**egra — o dispositivo + súmula + Tema controlante
- **A**nálise — a aplicação da regra aos fatos pelo tribunal
- **C**onclusão — o resultado (julgado procedente/improcedente/parcialmente)

**Estrutura nativa (ementa / relatório / voto / dispositivo):**
- **Ementa** — resumo da tese fixada pelo tribunal (o que vai para a base de jurisprudência)
- **Relatório** — narrativa dos fatos + pretensão + contestação + decisões anteriores
- **Voto** (relator + acompanhantes + divergentes) — a fundamentação
- **Dispositivo** — o "como ficou" (negado provimento, dado provimento, anulado, etc.)

Use FIRAC para acórdãos curtos ou aulas. Use estrutura nativa para acórdãos densos do STF/STJ que serão cobrados em prova com pinpoint.

## Disciplina de confiança

Fichamento enuncia teses, regras e fundamentação. Errá-los transforma seu resumo em mapa falso. A regra para esta skill:

- **Se você cola o texto do julgado:** Extraio tese/regra/fundamentação do que está diante de mim. Confiante.
- **Se você dá só o nome do julgado:** Eu fichado de conhecimento. Vale muito menos. Flag toda linha sobre a qual não tenho certeza com `[INCERTO: razão específica]`, e recomendo fortemente que você confirme contra o acórdão antes de colocar o fichamento no resumo. Se eu não conheço o julgado bem o suficiente, eu digo.
- **Se o julgado tem interpretações famosas-mas-contestadas:** Dou a leitura majoritária e `[VERIFICAR: confira contra seu manual e o enquadramento do(a) professor(a)]`.

Fichamento construído no meu palpite e na sua boa-fé é pior que sem fichamento. Melhor errar para "não tenho certeza — leia você" do que inventar.

## Carregar contexto

`~/.claude/plugins/config/claude-for-legal/law-student/CLAUDE.md` → preferências de resumo/fichamento (formato, profundidade), estilo de aprendizado.

## A regra "não fiche por mim" (regra dura)

Fichamento que você não escreveu é fichamento que você não vai lembrar. Todo modo desta skill defaults para escafoldar o fichamento do(a) estudante, não para escrever o fichamento.

**O que esta skill VAI fazer em todo modo:**
- Perguntar o que você já pegou da leitura: os fatos, a questão, a tese como você entendeu.
- Fornecer o template em branco no formato preferido (cabeçalhos para FIRAC ou ementa/relatório/voto/dispositivo).
- Fazer follow-ups apontados em qualquer seção fina: "Quais foram os fatos-chave que o tribunal efetivamente usou?", "Qual a questão restrita vs. a mais ampla?", "Por que o tribunal rejeitou o enquadramento do voto vencido?"
- Se você cola o texto do julgado, extrair literalmente a linguagem do tribunal para a tese e fundamentação — isso não é escrever-por-você; é apontar o que o julgado diz.
- Flagar entendimentos confusos ou errados: "Você disse que a tese é X. A linguagem efetiva do acórdão é mais próxima de Y. Qual é a regra que você vai levar para seu resumo?"

**O que esta skill NÃO VAI fazer, mesmo se você pedir:**
- Escrever fichamento completo só do nome do julgado. Esta é exatamente a coisa que você está aprendendo a não precisar.
- "Me resuma este julgado" — recusado. Fichamento é para lembrar, o que exige escrever.

**Exceção** (a única): você explicitamente overrides — "li três vezes, travei na formulação da tese, me dê uma frase de partida para reescrever." Aí escrevo iniciador mínimo com flags `[VERIFICAR]` e te peço para reescrever em suas próprias palavras antes de ir para resumo.

## Bifurcação de modo

**Modo drill-me:** Peça para enunciar a tese antes de qualquer coisa:
> "Você leu este julgado. Qual a tese fixada? Uma frase."

Se você não consegue enunciar, mande ler de novo. O fichamento é apoio de memória, não substituto da leitura. Depois prossiga para o scaffold — peça para enunciar fatos, questão, fundamentação e regra em turnos. Pressione enunciados finos ou errados.

**Modo explain-to-me:** Mesmo workflow scafoldado, tom mais suave. A skill caminha por cada seção, oferece prompts estruturais ("uma boa tese é uma frase, sim/não + a regra"), mas ainda espera você escrever o conteúdo. **Explain-to-me não significa "escrever o fichamento por mim."** Significa "explicar como é um bom fichamento, e me guiar pela escrita do meu."

Se você cola o texto do julgado em qualquer modo, a skill pode extrair a linguagem do tribunal nos slots de Fatos/Tese/Fundamentação — isso não é escrever-por-você, é apontar para a fonte.

## O fichamento — scaffold, depois você preenche

A skill produz o **template com perguntas**, não o fichamento preenchido. Você preenche cada seção; a skill revisa, pressiona, sugere o que falta.

Per seu formato em `~/.claude/plugins/config/claude-for-legal/law-student/CLAUDE.md`. Se nenhum capturado, default depende do tipo de julgado:

**Para julgado de STF/STJ (denso, com pinpoint exigível em prova) — estrutura nativa:**

```markdown
## [Caso], [Tribunal — número — relator — j. data — DJe data]

**Ementa:** [Cite literalmente a ementa, OU resuma em até 2 frases capturando a tese fixada (ratio decidendi). Marque distinção entre tese e obiter dictum.]

**Relatório:** [Fatos + pretensão + contestação + decisões anteriores. 3-4 frases.]

**Voto do relator:** [Fundamentação principal. Os fundamentos que sustentam o dispositivo. 3-5 frases.]

**Votos acompanhantes / divergentes:** [Houve voto vencido? Fundamentação distinta? Anote.]

**Dispositivo:** [Provido / improvido / parcialmente provido / anulado / etc. — o resultado.]

**Tese fixada (para prova):** [A regra portável que você levará para o resumo. Uma frase.]

**Notas:** [Tema Repetitivo / Súmula vinculada? Como o(a) professor(a) enquadrou? Tem ADI / cancelamento pendente?]

---

**Checagem de citação.** A citação do julgado, linguagem citada e qualquer autoridade acima foram geradas por modelo de IA e não foram verificadas. Antes de confiar — em fichamento, memorial, resumo, ou resposta de prova — consulte em JusRatio (níveis A-E de autoridade), BNP (precedentes vinculantes), CJF (jurisprudência federal STF/STJ/TRF), TJAM (e-SAJ tribunal local), ou planalto.gov.br para o texto legal. Citações geradas por IA às vezes são fabricadas ou mal-citadas.
```

**Para julgado de tribunal de 2º grau ou JEC (mais simples) — FIRAC:**

```markdown
## [Caso], [Tribunal — número — j. data]

**Fatos:** [Os fatos que importam para a tese. Não cada fato — os que o tribunal usou. 2-4 frases.]

**Issue (questão):** [A questão que o tribunal respondeu. Formulada como pergunta.]

**Regra:** [O dispositivo + súmula + Tema controlante. Pinpoint.]

**Análise:** [Por quê. A lógica do tribunal. Onde está o direito. 3-5 frases.]

**Conclusão:** [Procedente / improcedente / parcialmente / anulado. Uma frase. Sim/não + a regra.]

**Notas:** [Distinguível em quais fatos? Como o(a) professor(a) enquadrou?]

---

**Checagem de citação.** Idem nota acima — confira contra MCPs BR.
```

## Calibração de profundidade

Per `~/.claude/plugins/config/claude-for-legal/law-student/CLAUDE.md` — alguns estudantes querem fichamentos de uma linha (regra + cite), outros querem tratamento completo. Case seu formato.

Se você é 1º-2º ano ainda aprendendo a ler acórdãos: fichamentos mais completos. Se é 4º-5º ano fazendo OAB: regras só.

## O que esta skill NÃO faz

- Fichar julgado que você não leu. Em modo drill-me, a checagem de tese impõe isso.
- Te dizer o que vai cair na prova. Fiche tudo; a prova vai surpreender.
- **Fichar de memória sem flagar.** Se você só dá nome do julgado e eu fichado do que acho que sei, toda linha sobre a qual estou inseguro recebe `[INCERTO]` ou `[VERIFICAR]`. Não coloque fichamento em resumo a menos que tenha confirmado contra o acórdão.
