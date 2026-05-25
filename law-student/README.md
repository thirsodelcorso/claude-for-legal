# Plugin Estudante de Direito (Brasil)

Modo aprendizado, não modo resposta. Treino socrático que faz VOCÊ perguntas e te empurra quando o raciocínio está frouxo. Fichamento de julgados, montagem de resumo, flashcards, correção de IRAC, preparação para arguição oral (sustentação, sabatina, chamada de classe), feedback de redação que nunca reescreve por você, e previsão de prova a partir de provas antigas do mesmo professor. Calibrado para você — suas matérias, fase do Exame de Ordem, se quer ser sabatinado ou guiado.

**Cada saída é uma estrutura para estudo, não uma resposta pronta. O plugin estrutura seu raciocínio, te treina socraticamente e marca o que você errou. Ele não escreve o resumo, o fichamento ou a peça para você — isso anularia o propósito. Citações em materiais de estudo são marcadas para verificação contra a fonte.**

## Para quem é

Estudantes de Direito — 1º ano da graduação até preparação para 2ª fase OAB.

## Primeira execução: cold-start

Esta é sobre VOCÊ, não sobre uma organização. Suas matérias, sua fase do Exame de Ordem, seu estilo de aprendizado — me-sabate vs. me-explique. Traga material: resumos antigos, peças corrigidas em prática, provas passadas (especialmente do mesmo professor), questões da FGV/OAB, ementas, monografias. De 10 a 20 itens é o alvo; abaixo disso o perfil é marcado `DADOS LIMITADOS` e as skills downstream ficam mais finas até você adicionar mais.

```
/law-student:cold-start-interview
```

## Skills

Toda skill é invocada como `/law-student:<nome-da-skill>`.

| Skill | Função |
|---|---|
| `/law-student:cold-start-interview` | Entrevista sobre você + intake de materiais — matérias, fase OAB, estilo de aprendizado, materiais |
| `/law-student:socratic-drill [matéria]` | Treino socrático — pergunta, você responde, empurra de volta. Não dá a resposta. |
| `/law-student:case-brief [caso]` | Fichamento de julgado no seu formato preferido (ementa / relatório / voto / dispositivo, ou IRAC, ou híbrido) |
| `/law-student:outline-builder [matéria]` | Constrói ou expande resumo no seu formato a partir do material de aula |
| `/law-student:bar-prep-questions [matéria]` | Questões de OAB — 1ª fase objetiva (estilo FGV, 80 questões) ou 2ª fase prático-profissional (peça + 4 discursivas); sinaliza divergência STF / STJ / TST quando há |
| `/law-student:flashcards [matéria]` | Gera ou treina flashcards; baldes estilo Leitner; markdown por matéria; modo `--session <n>` |
| `/law-student:study-plan` | Constrói ou atualiza plano de estudo de longo prazo — fases, matérias por fraqueza, agenda diária adaptativa a partir do histórico |
| `/law-student:session <matéria> <n>` | Sessão focada de N questões em uma matéria; atualiza o plano com resultados |
| `/law-student:irac-practice` | Corrige seu IRAC — estrutura, identificação de questões, regras, análise. Rastreia padrões entre sessões. Nunca reescreve. |
| `/law-student:cold-call-prep [caso]` | Prep para arguição oral / chamada de classe / sabatina — antecipa perguntas do professor e treina |
| `/law-student:legal-writing [path-ou-cole]` | Feedback estrutural em qualquer minuta — nunca reescreve, jamais. Inclui checagem ABNT para monografia/TCC e estrutura de peças brasileiras (petição inicial / contestação / réplica / memoriais / recursos). |
| `/law-student:exam-forecast [matéria]` | Analisa provas passadas do mesmo professor; prevê padrões da próxima |

## O que "modo aprendizado" significa

Várias skills aqui (socratic-drill, case-brief em modo me-sabate, cold-call-prep, irac-practice, legal-writing) são deliberadamente construídas para *não* te dar a resposta nem escrever o que precisa fazer. O ponto é que você aprende fazendo. Se quer resposta ou minuta, use outra ferramenta. Este plugin é para o esforço.

**legal-writing é o mais estrito.** Lê sua minuta e diz o que está fraco, mas não reescreve. Pedir reescrita retorna recusa polida + oferta de feedback estrutural mais específico. Isso é feature.

**outline-builder e case-brief seguem a mesma regra em forma mais branda.** Outline-builder esqueletiza — árvore de tópicos, slots de subtópicos, placeholders de jurisprudência — e faz perguntas socráticas conforme você preenche as regras com suas próprias anotações e o manual doutrinário (Tartuce / Gonçalves / Marinoni / Didier / etc.). Não gera resumo populado só a partir da ementa do curso. Case-brief funciona igual em todo modo (me-sabate e me-explique): a skill dá o template e empurra sobre o que você escreveu; não ficha o julgado por você. Se você cola o texto do acórdão, ela extrai a linguagem do próprio tribunal para os slots — isso é apontar a fonte, não escrever por você.

## Integridade acadêmica e ética estudantil

Antes de usar este plugin em qualquer trabalho avaliado — prova com consulta, monografia/TCC, artigo para periódico, prática jurídica supervisionada — **confira o código de ética da sua instituição e a política do professor sobre uso de IA**. Muitas faculdades brasileiras estão ainda formando regras; algumas proíbem ou restringem IA em trabalhos avaliados, e a regra varia por curso e professor. Este plugin é para estudo e prática; usá-lo onde a sua escola proíbe é violação ética estudantil, e a consequência é sua, não da ferramenta. Na dúvida, pergunte ao professor por escrito.

Adicionalmente, mesmo em estudo livre, vale o Provimento OAB 205/2021: como futuro advogado, comece a internalizar agora o dever de revisão crítica do que IA produz. Saída de IA tratada como verdade revelada é caminho para erro material em peça real depois.

As skills modo-aprendizado aqui (socratic-drill, irac-practice, legal-writing, cold-call-prep) são deliberadamente projetadas para não te dar a resposta nem escrever por você — isso é a pedagogia. É também o pressuposto de tratar diferentemente usos permitidos (treino com cara de não-assistido) de proibidos (ghostwriting de memorando avaliado). Não burle os guardrails.

## Trabalho real com cliente (NPJ / Defensoria / clínica de prática)

Se você atua em **Núcleo de Prática Jurídica (NPJ)**, **escritório-escola**, **Defensoria estagiando** ou **clínica de prática jurídica**, este plugin **não é o lugar para atendimento real**. Use o `legal-clinic` (quando adaptado para BR) ou trabalhe sob supervisão direta do advogado/professor responsável — o trabalho real exige sigilo profissional, registro em sistema institucional e responsabilidade ética que este plugin de estudo não cobre.

**Regra do caso real (vale para todos):** se uma pergunta migra de hipótese de estudo para fato real com cliente identificável, o plugin pausa e redireciona — alunos de NPJ/clínica para o fluxo institucional aprovado; indivíduos com problema jurídico próprio para a OAB Seccional, Defensoria Pública do estado, ou serviço de assistência judiciária. Não cole fato real de cliente em ferramenta de estudo.

## Marcas de confiança

Skills que geram conteúdo sinalizam sua confiança inline. Uma regra ou cartão sem marca é algo em que a skill está confiante (mas ainda não substitui sua checagem na fonte antes da prova). Marcas usadas no plugin:

- `[VERIFICAR: alegação — checar fonte]` — afirmado como provavelmente correto, mas você deveria confirmar contra seu resumo, manual doutrinário, cursinho ou a fonte primária antes de confiar. Usado liberalmente em bar-prep-questions, case-brief, flashcards, legal-writing, irac-practice.
- `[INCERTO: razão específica]` — a skill não está confiante neste ponto (regra minoritária, questão controvertida, jurisdição/área que a skill não domina). Faça seu próprio juízo; cheque a fonte.
- `[LACUNA — preencher da aula]` — marca do outline-builder para tópico em que a skill não tem fonte confiável e não inventa regra. Você preenche do seu material.
- `[FALTA JURISPRUDÊNCIA — regra posta sem caso ilustrativo]` — marca onde a regra existe, mas falta julgado paradigmático.
- `[CHECAR AULA — professor pode ter enfatizado algo aqui]` — marca para áreas onde ênfase específica do professor importa e a skill não pode saber.
- `[EXCEÇÃO POUCO CLARA — manual menciona exceção, achar a regra]` — marca para exceção conhecida com detalhe não resolvido.
- `[INCERTO — enquadramento]` — marca da exam-forecast notando que previsão é peso para tempo de estudo, não predição.

Confie nas marcas mais do que na ausência delas — uma regra sem marca é algo em que a skill está confiante, mas a prova ainda exige checagem na fonte.

## Conectores e verificação de citações

**Conecte uma ferramenta de pesquisa primeiro — os guardrails de citação dependem dela.** Sem ela, cada citação é marcada `[verificar]` e a nota do revisor (você) acima de cada entregável registra que as fontes não foram verificadas. O plugin funciona sem; ele só faz mais da verificação por você quando há ferramenta conectada.

Os conectores neste plugin não são só fontes de dados — fazem diferença entre citação verificada e citação para checar. Uma citação via **JusRatio** (jurisprudência brasileira — STF, STJ, tribunais estaduais; níveis de autoridade A/B/C/D/E) é marcada com a fonte e rastreável. Uma citação do conhecimento do modelo ou de busca web é marcada `[verificar]` e deve ser checada contra fonte primária (manual doutrinário atualizado, vade mecum, sítio oficial do tribunal) antes que você confie. O plugin estratifica para seu tempo de verificação ir onde importa.

**Para questões da 1ª fase OAB**, JusRatio é especialmente útil para confirmar súmulas vigentes e teses repetitivas — vários gabaritos da FGV testam exatamente o conhecimento de Súmula Vinculante e Tema Repetitivo.

## Armazenamento

Seu perfil de estudo fica em `~/.claude/plugins/config/claude-for-legal/law-student/CLAUDE.md` e sobrevive às atualizações do plugin. Todo o resto fica no diretório de trabalho:

```
law-student/
├── flashcards/
│   └── [matéria]/cards.md             # Decks de flashcards por matéria
├── irac-sessions/
│   └── [aluno]/
│       ├── [data]-[tópico].md         # Feedback de sessão individual
│       └── tracker.md                 # Rastreamento de padrões entre sessões
├── writing-feedback/
│   └── [aluno]/
│       ├── [data]-[trabalho].md       # Feedback de sessão individual
│       └── tracker.md                 # Rastreamento de padrões entre sessões
└── exam-forecasts/
    └── [matéria]/
        └── forecast-[YYYY-MM-DD].md   # Previsões versionadas
```

## Como o plugin aprende

Seu perfil de estudo em `~/.claude/plugins/config/claude-for-legal/law-student/CLAUDE.md` não é estático — melhora conforme você usa. As skills te avisam quando um output usou um default que você deveria afinar. Você pode re-rodar o setup, editar o arquivo direto ou pedir para uma skill registrar uma nova posição.

## Notas

- Me-sabate vs. me-explique é definido no cold-start; troque por sessão se quiser.
- Fichamentos e resumos usam SEU formato. Se você tem resumos existentes, aponte o cold-start para eles.
- Bar-prep mira suas matérias fracas a partir de `~/.claude/plugins/config/claude-for-legal/law-student/CLAUDE.md`. Vai voltar nelas.
- Toda skill que gera conteúdo marca quando está incerta. Confie nas marcas mais que na ausência delas — regra sem marca é algo em que estou confiante; cheque a fonte mesmo assim antes da prova.

## Testing & QA
