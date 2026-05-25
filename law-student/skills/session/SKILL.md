---
name: session
description: >
  Roda sessão focada de N questões numa disciplina — 1ª fase OAB FGV
  (objetiva), 2ª fase (peça/discursiva), ou flashcards. Rastreia desempenho
  e atualiza o plano de estudos. Use quando o(a) usuário(a) disser "roda
  10 questões de [disciplina]", "faz uma sessão de [disciplina]", "vamos
  fazer 5 cards de [disciplina]", ou quer drilar número fixo de questões
  e ter o plano se adaptando.
argument-hint: "<disciplina> <n> [--oab1 | --oab2 | --flashcards]"
---

# /session

1. Parse `$ARGUMENTS` — disciplina e N. Se faltar, pergunte:
   > Qual disciplina, e quantas questões? (ex.: `Civil 10` ou `Trabalho 5 --oab2`.)
2. Carregue `~/.claude/plugins/config/claude-for-legal/law-student/CLAUDE.md` → fase da OAB, formato da prova, disciplinas frágeis.
3. Carregue `~/.claude/plugins/config/claude-for-legal/law-student/study-plan.yaml` se existir. Leia `session_history` para esta disciplina para ponderar subtópicos para onde o(a) estudante esteve fraco(a).
4. Roteie pela flag de método:
   - `--oab1` (default para disciplinas de preparação OAB 1ª fase): carrega skill `bar-prep-questions`, roda N questões objetivas estilo FGV (1ª fase, 5 alternativas, 1 correta). Aplica tratamento jurisdicional (vide seção daquela skill). Marca cada uma como `[FGV — edital vigente]` ou `[Súmula vinculante / orientação jurisprudencial específica]`.
   - `--oab2`: carrega `bar-prep-questions`, roda N enunciados prático-profissionais (peça processual ou discursiva), aplicando espelho de correção da banca. Corrige por rubrica.
   - `--flashcards`: carrega skill `flashcards`, roda N cards em modo `--drill`.
5. Roda N questões uma por vez. Após cada, explica certo/errado e marca corpo-de-regra quando dispositivos divergem (CPC/CPP, CC/CDC, etc.).
6. No fim da sessão, escreve resultados:
   - Se `study-plan.yaml` existe: acrescenta a `session_history` conforme schema na skill `study-plan`.
   - Se não: escreve em `~/.claude/plugins/config/claude-for-legal/law-student/session-history.yaml`.
7. Reporta:
   - Pontuação: X/N (percentual)
   - Erradas: lista com tags de subtópico
   - Subtópicos frágeis nesta sessão
   - Padrão vs. sessões anteriores nesta disciplina (se histórico tem 2+ anteriores)
   - O que o plano agora recomenda a seguir
