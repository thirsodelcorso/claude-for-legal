---
name: customize
description: >
  Customização guiada do seu perfil de estudo do law-student — mude uma
  coisa sem rerodar todo o cold-start interview. Ajuste disciplinas atuais,
  estilo de aprendizado, preferências de resumo, disciplinas OAB,
  materiais semente, ou cadência de sessões de estudo. Use quando o(a)
  usuário(a) disser "muda meu [coisa]", "adiciona uma disciplina",
  "atualiza meu perfil", "novo semestre", ou "customize".
argument-hint: "[nome da seção, ou descreva o que quer mudar]"
---

# /customize

## Quando isto roda

O(A) usuário(a) digitou `/law-student:customize`. Quer mudar algo no perfil
de estudo — uma disciplina, uma preferência de estilo de aprendizado, uma
disciplina OAB — sem rerodar o cold-start interview inteiro e sem editar
YAML à mão.

## O que fazer

1. **Leia a config.** Leia
   `~/.claude/plugins/config/claude-for-legal/law-student/CLAUDE.md`.
   Se a config do plugin não existir ou ainda contiver valores `[PLACEHOLDER]`,
   diga:

   > Você ainda não rodou o setup. Rode `/law-student:cold-start-interview`
   > primeiro — customize é para ajustar perfil que você já tem.

2. **Mostre o mapa customizável.** Liste o que está no perfil, agrupado, com
   sumário de uma linha do valor atual:

   - **Perfil do(a) estudante** — nome, IES, ano (1º/2º/3º/4º/5º ano / bacharel(a) /
     estagiário(a)), OAB Seccional alvo, NPJ ou periódicos em que está
     matriculado(a)
   - **Disciplinas atuais** — nome da disciplina, professor(a), caminho do plano de
     aula, formato da avaliação (com/sem consulta, dissertativa/objetiva/
     mista), estilo de chamada de classe
   - **Estilo de aprendizado** — socrático vs. resumo, quanta pressão você quer,
     se o plugin reescreve seu trabalho ou só critica estruturalmente
   - **Preferências de resumo** — formato do resumo (FIRAC/CREAC/estilo
     fichamento), nível de detalhe de regra, se inclui discussão de
     princípio, templates salvos
   - **Preparação OAB** — fase (1ª/2ª), área da 2ª fase se escolhida,
     disciplinas em rotação, sinalização de disciplina frágil, cadência de
     objetivas FGV vs. peça/discursiva
   - **Materiais semente** — caminhos do manual, resumos anteriores,
     dissertativas corrigidas, provas antigas, conjuntos de questões OAB FGV,
     planos de aula, trabalhos
   - **Workflow de estudo** — duração da sessão, agenda de buckets Leitner de
     flashcard, cadência de previsão de prova, tempo de preparação para
     chamada de classe
   - **Integrações** — status de armazenamento documental / app de flashcard (se
     houver), fallbacks

3. **Pergunte o que quer mudar.**

   > O que você gostaria de ajustar? Escolha uma seção, ou descreva a
   > mudança nas suas próprias palavras.

4. **Faça a mudança.** Mostre o valor atual, peça o novo, explique o que muda
   downstream, confirme, escreva na config.

   Exemplos:
   - *Adicionando nova disciplina:* "`/outline-builder` vai andaimar um novo
     resumo para esta disciplina. `/flashcards` adiciona novo bucket de
     disciplina. `/cold-call-prep` vai perguntar sobre tópico quando você
     invocar para esta disciplina."
   - *Estilo de aprendizado socrático → resumo-primeiro:* "`/socratic-drill`
     não vai te pedir para responder primeiro — vai apresentar a regra e
     exemplo, depois te testar na aplicação."
   - *Adicionando disciplina OAB:* "`/bar-prep-questions` vai incluir esta
     disciplina na rotação e pondera mais alto se marcar como frágil."

5. **Encerre.**

   > Pronto. Seu próximo output reflete a mudança. Mais alguma coisa? Você
   > pode rodar `/law-student:customize` a qualquer momento.

## Guardrails

- **Nunca apague seção.** Se o(a) usuário(a) quer "tirar" uma disciplina,
  ofereça marcar `[Arquivada — reter materiais semente]` e explique o que
  muda em comportamento de flashcard e resumo.
- **Sinalize inconsistência interna.** Se a mudança torna o perfil
  inconsistente (ex.: estilo "resumo-primeiro" + setting socrático "máxima
  pressão"), sinalize a tensão.
- **Sinalize degradação de guardrail.** A regra "sem reescrita do seu
  trabalho" em `/legal-writing` e `/irac-practice` é load-bearing — o valor
  da skill é feedback estrutural, não ghost-writing. Se o(a) usuário(a) pede
  para desligar, confirme que entende que o plugin não vai escrever o
  trabalho por ele(a).
- **Uma mudança por vez.** Não rereperguntar o entrevista inteira.
