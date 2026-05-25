---
name: exam-forecast
description: >
  Analisa provas antigas do(a) mesmo(a) professor(a) ou da mesma banca (FGV
  na OAB) para surface padrões — peso por disciplina, armadilhas recorrentes
  de identificação de issue, tipos de hipótese favoritos, mix doutrina-vs-
  jurisprudência — e prevê ênfases prováveis para a próxima prova. Use
  quando o(a) usuário(a) disser "o que vai cair na prova", "analisa provas
  antigas", "prevê a prova", ou compartilhar provas antigas.
argument-hint: "[nome da disciplina, com provas antigas compartilhadas ou caminhos]"
---

# /exam-forecast

1. Carregue `~/.claude/plugins/config/claude-for-legal/law-student/CLAUDE.md` → disciplina, professor(a), formato da avaliação, plano de aula.
2. Aplique o workflow abaixo.
3. Receba as provas antigas (PDF, colado, ou caminhos). Confirme tamanho da amostra.
4. Analise cada prova: formato, cobertura de tópicos, estilo de questão, densidade de enunciado, armadilhas recorrentes.
5. Análise cross-prova: o que é estável, o que varia.
6. Combine com o plano de aula atual para produzir previsão: pesos por tópico, formato, temas-cavalo-de-batalha, ênfase de estudo.
7. Escreva `~/.claude/plugins/config/claude-for-legal/law-student/exam-forecasts/[disciplina]/forecast-[YYYY-MM-DD].md`. Enquadre como heurística de ponderação, não predição.

---

## Propósito

Toda prova de professor(a) tem digital. As mesmas estruturas de hipótese reaparecem. As mesmas armadilhas voltam. As mesmas proporções por matéria se repetem. Estudantes com provas antigas estudam mais inteligentemente; estudantes sem elas estudam mais duro. Esta skill analisa as provas antigas que você tem e surface os padrões.

Vale também para OAB FGV — a banca tem padrões fortes (peso por disciplina nas 80 questões da 1ª fase, formato de peça e discursivas na 2ª fase) que se repetem entre edições. Para preparação OAB, combine esta skill com sua reta final do cursinho (CERS / Damásio / Estratégia OAB / Mege / Praetorium / Supremo TV / Ênfase).

Não é mágica. É previsão, não predição. A skill não pode te dizer o que vai cair na prova — pode te dizer o que caiu nas provas anteriores e o que tem chance de recorrer com base na cobertura do plano de aula.

## Disciplina de confiança

- Análise de padrão (que matérias apareceram, quantas questões por tópico, frequência política-vs-aplicação-de-regra) — confiante onde as provas estão claramente à minha frente.
- Inferência sobre ênfase provável na próxima prova — `[INCERTO]` é o default; são previsões, não certezas. Enquadre explicitamente como "com base nas [N] provas antigas que você compartilhou, [tópico] apareceu em [M]. Sua próxima prova pode enfatizar isso, ou o(a) professor(a) pode rotacionar — use como ponderação para tempo de revisão, não predição."
- Se só 1-2 provas antigas estão disponíveis, diga explicitamente — padrão inferido de 1 prova é ruído.
- Se o(a) professor(a) é novo(a) (sem provas antigas disponíveis), a skill não pode prever. Diga; recue para "estes são os tópicos cobertos" baseado só no plano de aula.

## Carregar contexto

- `~/.claude/plugins/config/claude-for-legal/law-student/CLAUDE.md` → disciplinas atuais, formatos de prova, plano de aula se capturado
- Provas antigas fornecidas pelo(a) usuário(a) (PDF, texto colado, caminhos)
- Opcional: plano de aula da disciplina atual (para "o que foi coberto até agora")

**Se as provas antigas têm nome de professor(a), use para casar padrões** (provas do(a) mesmo(a) professor(a) são o input de maior sinal). **Se não, case por matéria e estrutura.** Não peça para o(a) usuário(a) digitar o nome do(a) professor(a) — use o que está nos materiais. Se voluntariar em conversa, ok; não pergunte.

## Workflow

### Passo 1: Recebimento

- Qual disciplina estamos prevendo?
- Quantas provas antigas deste(a) professor(a) (ou desta banca, no caso OAB) estão disponíveis?
- São do mesmo curso, ou de cursos diferentes do(a) mesmo(a) professor(a)?
- Algumas são da variante prova-com-consulta / livro aberto / formato diferente, vs. o formato típico da sua próxima prova?
- Plano de aula da disciplina atual?

Se menos de 3 provas antigas: sinalize amostra fina. Inferência de padrão é mais fraca.
Se provas são de cursos diferentes: alguns padrões transferem (estilo de questão, proporção política-vs-doutrina); padrões específicos por matéria não.

### Passo 2: Leia cada prova antiga

Para cada uma:

- Formato (número de questões, extensão, tempo, com/sem consulta)
- Cobertura de tópicos (que temas testados, em que proporção)
- Estilo de questão (identificação de issue / questão única em profundidade / ensaio de política / questão objetiva múltipla escolha estilo FGV / mix)
- Densidade do enunciado (enunciados fato-pesados, fatos esparsos com foco doutrinário, ou prompts de política sem fatos)
- Armadilhas recorrentes (ex.: professor(a) sempre esconde questão de competência num enunciado de fato limpo; professor(a) sempre pergunta sobre a exceção e não a regra; FGV sempre cobra súmula vinculante específica em Constitucional)
- Proporção política/princípio vs. doutrina/dispositivo
- Estruturas incomuns (objetiva + discursiva híbrida, simulação de júri, peça processual etc.)

### Passo 3: Análise cross-prova

Consolide o que é consistente entre provas:

**Padrões estáveis (apareceram na maioria/todas):**
- Pesos por matéria (ex.: "negócio jurídico e validade respondem por 30% dos pontos consistentemente")
- Estilo de questão (ex.: "sempre uma questão longa de identificação de issue + duas hipóteses curtas")
- Temas-cavalo-de-batalha do(a) professor(a) (ex.: "sempre cobra terceiro beneficiário mesmo quando é tópico menor em aula"; para FGV, "sempre cobra prazo decadencial do CDC art. 26")

**Padrões variáveis (apareceram em algumas mas não todas):**
- Ensaios de política (ex.: "apareceu em 2 de 4 provas antigas — usualmente quando o semestre cobriu tópico denso em política tarde")
- Diferenças prova-com-consulta vs. sem consulta
- Diferenças prova em casa vs. em sala

**Padrões ausentes dignos de nota:**
- Tópicos cobertos em aula que NUNCA foram testados em provas antigas — não pule, mas não pondere alto
- Tópicos testados em provas antigas que não estão no seu plano de aula atual — provavelmente não voltam

### Passo 4: Previsão para a próxima prova

**Cabeçalho — obrigatório, primeira linha da previsão, tanto in-chat quanto no arquivo salvo.** Conforme config do plugin `## Outputs`, todo output de estudo carrega o cabeçalho literal de notas de estudo. A previsão é output de estudo. Não omita, reformule ou realoque. O cabeçalho não é um disclaimer que o(a) estudante pode pedir para tirar; é a identidade do output e evita que a previsão seja confundida com prova prevista ou parecer jurídico:

```
MATERIAL DE ESTUDO — NÃO É PARECER JURÍDICO
```

Combine análise de padrão com plano de aula atual:

```markdown
MATERIAL DE ESTUDO — NÃO É PARECER JURÍDICO

# Previsão de prova — [disciplina / professor(a)] — [data]

**Provas antigas analisadas:** [N]
**Confiança da amostra:** [fina (<3) / moderada (3-5) / forte (6+)]
**Ressalvas:** [ex.: "uma das provas antigas foi prova final com consulta; sua próxima é sem consulta. Transferência de padrão é parcial."]

---

## Ponderação por matéria (histórica)

| Tópico | Peso histórico em prova (média) | No plano de aula atual? | Peso previsto |
|---|---|---|---|
| [tópico 1] | [%] | [sim/parcial/não] | [mais pesado / estável / mais leve] |

## Previsão de estilo de questão

- **Formato provável:** [X questões de identificação de issue + Y curtas + Z política, ou similar]
- **Densidade de enunciado:** [fato-pesado / esparso / misto]
- **Estilo do chamamento:** [um chamamento amplo / múltiplos específicos / subitens]

## Temas-cavalo-de-batalha do(a) professor(a) a observar

- [tópico A] — apareceu em [M de N] provas antigas. Pondere 3-5x sua participação no plano de aula.
- [tópico B] — [padrão]
- [padrão de armadilha] — ex.: "esconde questão de competência em fatos limpos"

## Tópicos cobertos este semestre mas raramente testados

[lista — não pule, mas não super-pondere]

## Recomendação de ênfase de estudo

Com base em padrões de provas antigas E cobertura do plano de aula atual:

**Pesado:** [tópicos com chance de ancorar a prova — 40-50% do tempo de estudo]
**Moderado:** [tópicos de apoio — 30-40%]
**Sanity check:** [tópicos cobertos mas historicamente sub-representados — 10-20%, por garantia]

## [INCERTO — enquadramento]

Esta previsão é derivada de [N] provas antigas. Professores(as) variam. Professores(as) rotacionam. Tópicos enfatizados em anos anteriores podem ser desenfatizados quando o plano de aula muda. Trate como heurística de ponderação para tempo de estudo, não predição. A prova vai incluir surpresas.
```

### Passo 5: Local de output

Escreva em `~/.claude/plugins/config/claude-for-legal/law-student/exam-forecasts/[disciplina]/forecast-[YYYY-MM-DD].md`. Versionado — se o(a) estudante consegue outra prova antiga no meio do semestre, rode de novo e acrescente.

## Integração

- **outline-builder:** pesos da previsão alimentam decisões de profundidade do resumo — concentre profundidade em tópicos pesados
- **flashcards:** tópicos pesados na previsão geram mais cards
- **bar-prep-questions:** irrelevante para preparação OAB de 1ª fase aberta (essa tem seu próprio modelo de previsão — padrões do edital FGV); exam-forecast é para provas específicas de disciplina da IES e para 2ª fase de OAB (onde a área é escolhida e a banca FGV tem padrão de peça)
- **irac-practice:** use os tópicos da previsão como áreas para prática de FIRAC

## Encerre com a árvore de decisão de próximos passos

Encerre com a árvore conforme CLAUDE.md `## Outputs`. Customize as opções ao que esta skill acabou de produzir — os cinco ramos default (rascunhar, escalar, mais fundamentos, observar, outra coisa) são ponto de partida, não trava. A árvore é o output; você escolhe.

## O que esta skill não faz

- **Predizer questões específicas.** Provas antigas mostram padrões; não mostram o prompt de amanhã.
- **Trabalhar sem provas antigas.** Se você não tem provas anteriores deste(a) professor(a), a skill não prevê — recua para "aqui está o que o plano de aula cobre, estude isso."
- **Substituir estudar tudo do plano de aula.** Previsão é ponderação, não eliminação. Pular tópico porque é historicamente sub-representado é como estudantes se queimam.
- **Contabilizar mudanças que você não conhece.** Se o(a) professor(a) mudou o foco este ano (ex.: enfatizou um julgado novo em aulas), a skill não vê a menos que você conte.
- **Trabalhar de forma confiável com 1-2 provas antigas.** Amostra fina. Sinalize como tal.
