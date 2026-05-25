---
name: study-plan
description: >
  Monta ou atualiza plano de estudos de longo prazo para OAB (ou prova de
  faculdade) — fases, disciplinas ponderadas por fragilidade, agenda diária
  de sessões, adaptativo ao histórico em study-plan.yaml. Use quando o(a)
  usuário(a) disser "monta um plano de estudo", "planeja minha OAB", "agenda
  meus estudos", ou "como devo estudar para [X]".
argument-hint: "[--build | --update | --status | --cram]"
---

# /study-plan

1. Carregue `~/.claude/plugins/config/claude-for-legal/law-student/CLAUDE.md` → OAB Seccional, fase da OAB, data alvo, disciplinas frágeis, horas de estudo/dia alvo, cursinho.
2. Carregue `~/.claude/plugins/config/claude-for-legal/law-student/study-plan.yaml` se existir.
3. Aplique o framework abaixo.
4. Roteie pela flag:
   - `--build` (default se não existe plano): caminhe pelo gate de inputs (prova, disciplinas, horas/semana, dias de folga, métodos). Monte estrutura de fases + agenda diária para as duas primeiras semanas. Escreva `study-plan.yaml`.
   - `--update` (default se plano existe): releia `session_history`, ajuste prioridades de disciplina e weekly_hours, preencha o próximo trecho de agenda diária.
   - `--status`: o que está agendado hoje / esta semana, tendência de pontuação, disciplinas escorregando, próxima sessão agendada por disciplina.
   - `--cram`: força modo cram — priorização 80/20 alto-rendimento, volume diário de objetivas, tapering nos últimos 2-3 dias.
5. Antes de escrever: sumarize o plano em prosa e confirme com o(a) estudante. Ajuste com base na resposta.
6. Sempre rode sanity-check de horas/semana contra as restrições de vida declaradas pelo(a) estudante. Planos super-ambiciosos falham.

---

## Propósito

Sentar para estudar e não saber o que estudar é como semanas desaparecem. Esta skill monta plano — semanas até a prova, sessões por dia, disciplinas por semana, tipos de sessão — e então adapta conforme o(a) estudante efetivamente faz as sessões. É plano vivo, não exportação para calendário.

Também dá às skills downstream (bar-prep, flashcards, drill, irac) agenda compartilhada para honrar, para que o(a) estudante não seja perguntado(a) "o que você quer estudar hoje" toda vez que abre uma sessão.

## Disciplina de confiança

Plano é opinião, não doutrina. A skill enuncia claramente o que é estimativa:

- **Estimativas de tempo-por-tópico** são orientação geral (com base em pesos típicos de cursinhos: CERS, Damásio, Estratégia OAB, Mege, Praetorium, Supremo TV, Ênfase). Marque como estimativas — o ritmo real do(a) estudante vai diferir.
- **Pesos por disciplina** são derivados das disciplinas frágeis declaradas pelo(a) estudante e do histórico de sessões. Confiante.
- **Priorização de tópicos de alto-rendimento em modo cram** é baseada em padrões multi-ano de OAB FGV (frequência por disciplina nas 80 questões da 1ª fase). Marque qualquer alegação "isto está garantido na prova" como `[INCERTO — frequência passada não é predição]`.

## Carregar contexto

`~/.claude/plugins/config/claude-for-legal/law-student/CLAUDE.md`:
- OAB Seccional, fase da OAB, data alvo
- Disciplinas atuais (para uso não-OAB)
- Disciplinas frágeis (1ª fase / 2ª fase se aplicável)
- Cursinho
- Horas de estudo alvo/dia

`~/.claude/plugins/config/claude-for-legal/law-student/study-plan.yaml` se existir — estenda, não sobrescreva.

## Workflow

### Passo 1: Para o que estamos planejando

> Para o que estamos montando plano?
>
> 1. **Exame de Ordem (OAB)** (você tem data alvo em mente)
> 2. **Prova específica da faculdade ou conjunto de finais**
> 3. **Cadência geral de estudo do semestre** (resumos, leituras, drills em todas as disciplinas)

Para (1) OAB: leia data alvo do perfil, confirme. Se não capturada, pergunte. Confirme fase (1ª ou 2ª).
Para (2) prova da faculdade: pergunte qual disciplina, que data, que formato.
Para (3) semestre: pergunte a data de fim de período como âncora.

### Passo 2: Inputs — um por vez, espere cada

**Pergunte e espere.** Não junte todas as perguntas em um prompt e siga.

- **Data da prova:** confirmada? (Se OAB: pergunte OAB Seccional se não estiver no perfil — conteúdo de estudo depende; e fase, 1ª ou 2ª.)
- **Disciplinas a cobrir:** para OAB 1ª fase, leia do edital vigente FGV (17 disciplinas: Ética, Filosofia, Constitucional, DH, Internacional, Tributário, Administrativo, Ambiental, Civil, Empresarial, Consumidor, ECA, Penal, Proc. Penal, Trabalho, Proc. Trabalho, Proc. Civil). Para 2ª fase, a área escolhida pelo(a) estudante. Para disciplina de faculdade, o plano de aula. Confirme com estudante — "alguma disciplina para adicionar ou tirar?"
- **Disciplinas mais fortes:** prioridade menor. Ainda revisadas, não drilam pesado.
- **Disciplinas mais fracas:** prioridade maior. Recebem mais sessões.
- **Horas por semana disponíveis:** realistas, não aspiracionais. "Posso fazer 20 horas" é diferente de "vou fazer 20 horas por 8 semanas". Pergunte o que efetivamente sustenta.
- **Sanity check de contexto de vida — force.** Depois que o(a) estudante dá um número, pergunte (uma pergunta de cada vez — não pule):

  > Você disse [N] horas por semana. Antes de montar, me conte o que mais tem na semana — trabalho/estágio (horas/semana), família (filhos, cuidado), deslocamento, exercício, terapia, NPJ/escritório-escola, qualquer coisa significativa. O plano deve caber na sua vida, não o contrário. Plano que você não consegue seguir é pior que plano mais leve que segue.

  Espere a resposta. Depois rode sanity-check das horas declaradas contra a carga reportada:

  > Isso é ~[X] horas/dia em [N] dias de estudo, em cima de [trabalho/estágio + família + deslocamento + outros]. Na minha experiência isso é [realista / apertado / insustentável]. Quer ajustar a meta horas/semana antes de eu montar, ou manter e ver como vai a semana 1?

  Não pule este passo mesmo se o número de horas alvo do perfil já tenha sido capturado no cold-start. O perfil captura o que o(a) estudante disse; o sanity-check de vida captura se sustenta. Se o check produz número menor, use o menor para o plano e anote o ajuste no bloco `confidence_flags`.

  Se o(a) estudante recusa compartilhar contexto de vida ("só monta"), respeite — mas adicione entrada em `confidence_flags`: "Sanity check de contexto de vida recusado; plano assume [N] horas/semana é sustentável. Revisite no fim da semana 2 se aderência abaixo de [X]%."
- **Métodos de estudo preferidos:** múltiplos. Questões OAB FGV / peças prático-profissionais / flashcards / resumos / drill / releitura. Pondere a agenda aos métodos que vai efetivamente fazer.
- **Dias de folga por semana:** dias de descanso importam. Planos que agendam 7/7 dias falham na semana 3.

### Passo 2.5: Suplementar vs. substituir (usuários(as) de cursinho)

Se `~/.claude/plugins/config/claude-for-legal/law-student/CLAUDE.md` → `Cursinho OAB` é **CERS**, **Damásio**, **Estratégia OAB**, **Mege**, **Praetorium**, **Supremo TV**, **Ênfase**, ou qualquer outro cursinho estruturado (isto é, NÃO `autodidata` ou `N/A`), o(a) estudante já tem calendário do cursinho. O plano desta skill deve escolher um de dois papéis — não pode rodar currículo paralelo completo ao lado do cursinho sem queimar o(a) estudante.

Pergunte, uma pergunta, espere:

> Seu perfil diz que você está no [CERS / Damásio / Estratégia OAB / Mege / Praetorium / Supremo TV / Ênfase]. Eles publicam cronograma dia-a-dia com toda disciplina e tarefa agendada. Dois jeitos deste plano funcionar — escolha um:
>
> 1. **Suplementar.** O cursinho é seu currículo primário. Este plano preenche lacunas: drill extra de questões nas disciplinas frágeis, prática focada de peça/discursiva, loops de flashcard nos tópicos que você está errando. Não reconstruo o cronograma do cursinho; sobreponho.
> 2. **Substituir.** Você não está seguindo o cronograma do cursinho (talvez porque o ritmo não bate com sua vida). Monto o plano todo — disciplinas, horas, fases, agenda — e você abandona o cronograma do cursinho.
>
> Não escolha os dois. Rodar dois currículos completos um contra o outro é como estudantes explodem na semana 4.

Espere a resposta. Registre no yaml como `prep_course_mode: supplement | replace`.

Se **suplementar**: a agenda diária do plano é mais leve — só adiciona drill de disciplinas frágeis e prática focada, não duplica cobertura do cursinho. Marque em `confidence_flags`: "Modo suplemento — este plano assume que você está em dia com [cursinho] para cobertura primária. Se atrasar no cursinho, me diga e replanejamos."

Se **substituir**: monte o plano completo conforme especificado abaixo.

Se o cursinho é `autodidata` ou `N/A`, pule este passo — nada a suplementar.

### Passo 3: Monte a agenda

Calcule semanas-até-prova de hoje. Então:

**Modo normal (4+ semanas):**
- Divida semanas em fases:
  - **Fase de aprendizagem** (primeiros ~60% do tempo): uma disciplina a cada ~3-5 dias, misturando resumo/leitura com flashcards e algumas questões objetivas/discursivas em material fresco.
  - **Fase de drill** (próximos ~30%): mais volume de questões objetivas FGV, mais prática de peça/discursiva, condições simuladas, todas disciplinas em rotação.
  - **Fase de revisão** (últimos ~10%): focado em subtópicos mais fracos do session_history, simulados completos, revisão leve de áreas fortes.
- Pondere disciplinas por fragilidade: disciplinas frágeis recebem ~2x as horas das fortes.
- Agende dia-a-dia: que disciplina, que método, quanto tempo. Deixe folga para a vida real do(a) estudante.

**Modo cram (< 4 semanas):**
- Sinalize: "Você está a menos de quatro semanas. Isto é modo cram — o plano prioriza tópicos de alto-rendimento sobre cobertura completa. Você vai deixar lacunas. Esse é o tradeoff a essa altura."
- Priorização 80/20: as disciplinas OAB que historicamente aparecem mais (Civil, Processo Civil, Constitucional, Ética, Trabalho) recebem a parte do leão. Disciplinas mais estreitas recebem cobertura mínima viável.
- Agenda diária: blocos de questões FGV todo dia (volume importa agora), prática de peça/discursiva dia sim, dia não, um simulado completo por semana.
- Durma e faça tapering nos últimos 2-3 dias. Não agende drill pesado no dia anterior à prova. Isto é real — estudantes que viram a noite antes pontuam pior.

### Passo 4: Escreva

Escreva em `~/.claude/plugins/config/claude-for-legal/law-student/study-plan.yaml`:

```yaml
plan_type: oab  # ou law-school-exam ou semester
exam_date: 2026-07-28
seccional: SP
exam_phase: 1  # ou 2
created: 2026-05-08
last_updated: 2026-05-08
weeks_to_exam: 12
hours_per_week: 25
days_per_week: 6
mode: normal  # ou cram
phases:
  - name: learning
    start: 2026-05-08
    end: 2026-06-20
    focus: resumos, flashcards, primeiras questões FGV
  - name: drilling
    start: 2026-06-21
    end: 2026-07-18
    focus: volume de questões FGV, prática de peça, simulados
  - name: review
    start: 2026-07-19
    end: 2026-07-27
    focus: revisão de subtópicos frágeis, simulados completos
subjects:
  civil:
    priority: high  # frágil
    weekly_hours: 5
    methods: [oab1, flashcards, discursiva]
  constitucional:
    priority: medium
    weekly_hours: 3
    methods: [oab1, outline-review]
  # etc.
schedule:
  - date: 2026-05-08
    day: Quinta
    sessions:
      - subject: Civil
        method: outline-review
        duration_min: 90
      - subject: Civil
        method: oab1
        duration_min: 60
        n_questions: 25
  - date: 2026-05-09
    day: Sexta
    sessions:
      - subject: Processo Civil
        method: flashcards
        duration_min: 45
      - subject: Processo Civil
        method: discursiva
        duration_min: 60
  # etc.
session_history: []  # acrescido por bar-prep, flashcards, drill, irac conforme sessões completam
```

### Passo 5: Confirme com o(a) estudante

**Cabeçalho — obrigatório em toda apresentação in-chat e em qualquer documento de plano em prosa salvo ao lado do YAML.** A primeira linha do sumário (e a primeira linha de qualquer arquivo `study-plan.md` companheiro) deve ser o cabeçalho literal da config do plugin `## Outputs`:

```
MATERIAL DE ESTUDO — NÃO É PARECER JURÍDICO
```

O cabeçalho não vai dentro do YAML (é arquivo de dados), mas vai no sumário em prosa que você mostra ao(à) estudante e em qualquer documento de plano legível por humano salvo ao lado do YAML. Não é disclaimer pós-coisa — é a identidade do output. Não omita, reformule, ou realoque.

Sumarize o plano em prosa (não YAML cru) antes de salvar, com o cabeçalho no topo:

> MATERIAL DE ESTUDO — NÃO É PARECER JURÍDICO
>
> Aqui está o que montei. [X] semanas para a [prova]. [Y] horas/semana em [Z] dias. Disciplinas frágeis (Civil, Processo Civil) recebem 2x as horas. Três fases: aprendizagem até [data], drill até [data], revisão nos últimos [N] dias. Agendei as duas primeiras semanas dia-a-dia. Além disso é alocado por semana — vou preenchendo a agenda diária conforme você completa sessões, para o plano se adaptar ao seu ritmo real.
>
> Faz sentido? Ambicioso demais? Leve demais? Falta alguma disciplina?

Ajuste com base na resposta. Depois escreva.

## Adaptando o plano

Após cada sessão (via bar-prep-questions, flashcards, drill, irac), a skill correspondente acrescenta a `session_history`:

```yaml
session_history:
  - date: 2026-05-08
    subject: Civil
    type: oab1
    n_questions: 10
    score: 6
    weak_subtopics: [prescricao, decadencia]
```

Na próxima rodada `/law-student:study-plan --update` (ou quando qualquer skill detecta que o plano está obsoleto):
- Disciplinas com pontuação consistentemente baixa sobem em `priority` e `weekly_hours`.
- Subtópicos frágeis dentro de disciplina são sinalizados para a próxima sessão agendada nela.
- Se o(a) estudante está atrasando (sessões agendadas não aparecendo no histórico), ajuste: ou comprima cobertura ou anote a lacuna e pergunte.
- Se está adiantado(a), abra tempo para drill mais profundo nas disciplinas frágeis.

## Modos

`--build` (default) — plano fresco
`--update` — releia session_history e ajuste pesos, preencha próxima agenda diária
`--status` — o que está hoje / esta semana, tendência de pontuação, o que está escorregando
`--cram` — força modo cram mesmo se mais de 4 semanas (override do(a) usuário(a))

## Integração

- `/law-student:session <disciplina> <n>` escreve resultados no `session_history` deste plano.
- `/law-student:bar-prep-questions` lê o plano para saber que disciplina está agendada para hoje.
- `/law-student:flashcards` pode `--session <n>` e resultados caem no plano.
- `/law-student:socratic-drill` e `/law-student:irac-practice` completam sessões e também acrescentam.

## O que esta skill não faz

- **Garantir aprovação.** O plano é andaime. O trabalho é seu.
- **Predizer a prova.** Modo cram usa frequência histórica de disciplina; alto-rendimento ≠ garantido-cobrado.
- **Substituir o cronograma do seu cursinho.** Se está no CERS/Damásio/Estratégia OAB/Mege/Praetorium/Supremo TV/Ênfase, este plano pode suplementar — não rode dois currículos completos um contra o outro. Use um como primário.
- **Agendar sua vida.** Horas disponíveis é o que você me conta. Se super-estima, o plano quebra na semana 2. Seja honesto.
