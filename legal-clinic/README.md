# Estágio Supervisionado (Brasil)

*Expandindo o acesso à justiça pela Defensoria Pública e pelos núcleos de prática jurídica das faculdades, com IA sob supervisão real.*

Plugin para o(a) **Defensor(a)-Supervisor(a) de estágio** na Defensoria Pública e para o(a) **Professor(a)-Orientador(a)** em núcleo de prática jurídica (NPJ) — ambientes em que estudantes de Direito, sob supervisão de membro habilitado, prestam atendimento jurídico gratuito a quem não pode pagar advogado. Famílias em conflito, despejos, consumidor, BPC/LOAS, saúde pública via SUS, locação, posse, defesa em ações de cobrança, alimentos, divórcio.

**Toda saída é minuta para análise do(a) estagiário(a) e revisão do(a) supervisor(a) — marcada, gateada e logada. O plugin estrutura o trabalho; o(a) estagiário(a) raciocina por dentro; o(a) supervisor(a) revisa. Nada sai da unidade ou do NPJ sem passar pelo modelo de supervisão que o(a) supervisor(a) escolheu na configuração inicial.**

## O problema que isto resolve

Defensorias e NPJs são estruturalmente limitados em capacidade. Um(a) Defensor(a) titular de unidade supervisiona alguns estagiários; um(a) Professor(a) de NPJ supervisiona 5–10 alunos. Cada estagiário(a) carrega alguns casos enquanto cumpre disciplinas. A turma rotaciona a cada semestre (ou a cada termo de estágio). Tarefas administrativas — intake do(a) assistido(a), primeiras minutas, ponto de partida de pesquisa, atualizações de status — consomem horas que poderiam ir para análise estratégica e atendimento. O resultado: filas longas no acolhimento, casos parados, pessoas que desistem de esperar.

Este plugin reduz o custo de tempo de tudo *em volta* da advocacia, para que a mesma equipe atenda significativamente mais assistidos(as) — e para que o(a) estagiário(a) gaste mais tempo na análise e na estratégia que fazem a educação prática valer a pena.

**Acelera as partes não-pedagógicas. Preserva o trabalho analítico.** Esse é o princípio de design.

## Quem usa

| Papel | Roda | Recebe |
|---|---|---|
| **Defensor(a)-Supervisor(a) / Professor(a)-Orientador(a)** | `/cold-start-interview` (uma vez), `/supervisor-review-queue` (se revisão formal estiver habilitada) | Contexto da unidade ou NPJ configurado, trabalho do(a) estagiário(a) revisado |
| **Estagiários(as)** | `/ramp` (início do termo de estágio), depois `/client-intake`, `/draft`, `/memo`, `/research-start`, `/status`, `/client-letter` | Pontos de partida — nunca produto final |

## Comandos

| Comando | O que faz | O que NÃO faz |
|---|---|---|
| `/cold-start-interview` | **Supervisor(a).** Configuração única da unidade ou NPJ: áreas de atuação, vara/comarca, modelo de supervisão, escala, upload de handbook/regimento/resoluções | — |
| `/build-guide` | **Supervisor(a).** Guia por área de atuação: perguntas de intake, postura pedagógica (assist / guide / teach), gates de revisão, checagens cruzadas com outros plugins | Não substitui `/cold-start-interview` — afina skills para uma área específica |
| `/ramp` | **Estagiários(as).** Onboarding do termo de estágio: procedimentos da unidade, tour dos sistemas (e-SAJ, PJe, Sapiens, sistema interno), exercícios de baixo risco | Não substitui orientação presencial do(a) supervisor(a) |
| `/client-intake` | Intake estruturado: templates por área, identificação cruzada de outras pretensões (issue spotting), bandeiras de hipossuficiência presumida (Súmula 481 STJ), triagem | Não decide se o caso é atendível — flag para o(a) supervisor(a) |
| `/draft [doc]` | Primeira minuta: petição inicial JEC (Lei 9.099/95), petição comum (CPC 319), contestação, recurso inominado, ofício, notificação extrajudicial, cota — calibrada por vara | Não produz peça final |
| `/memo` | Análise jurídica em FIRAC (Fatos / Issue / Regra / Análise / Conclusão), com lacunas de pesquisa marcadas | Não escreve a análise — estrutura |
| `/research-start [tese]` | Roteiro de pesquisa: dispositivos do CC/CDC/CPC/CF, área de jurisprudência (STF/STJ/TJAM), termos para os MCPs JusRatio/BNP/CJF/TJAM | **Pistas, não citações verificadas** — estagiário(a) confirma tudo |
| `/status [audiência]` | Sumário do status do caso: para o(a) assistido(a), interno (revisão do(a) supervisor(a)), ou pronto para juízo | Não protocoliza nada |
| `/client-letter [tipo]` | Correspondência rotineira ao(à) assistido(a): confirmação de audiência, pedido de documentos, atualização breve — em linguagem simples (LC 80/94 art. 4º-A III) | Não dá orientação substantiva — isso é `/status assistido` ou conversa direta |
| `/deadlines` | Controle de prazos do caso: adição, rollup cruzando casos, alertas em 14/7/3/1 dias, flag de prazo vencido. Cálculo em dias úteis (CPC art. 219) com nota sobre suspensão CPC art. 220 (20/12–20/1) | Não calcula o prazo a partir do termo inicial — o(a) estagiário(a) faz a contagem conforme a regra processual aplicável |
| `/client-comms-log [caso]` | Log append-only de comunicação por caso — telefonemas, e-mails, ofícios, presencial | Não armazena análise jurídica substantiva; só registro de contato |
| `/semester-handoff` | Offboarding de fim de termo — memorando de handoff por caso para a próxima turma | Não encerra casos; casos que efetivamente se resolvem no termo recebem `/status interno` final e são marcados como encerrados no documento de handoff |
| `/supervisor-review-queue` | **Supervisor(a), se revisão formal habilitada.** O que está esperando, aprovar/editar/devolver | Opcional — um dos três modelos de supervisão |

## Pré-condições éticas e de confidencialidade

Antes de usar este plugin com casos reais, confirme com o(a) Defensor(a)-Supervisor(a) (na DP) ou Professor(a) Coordenador(a) do NPJ e com o TI institucional:

1. **Tier da conta Claude e políticas de retenção e treinamento.** Contas Team, Enterprise, Work, Education e individuais têm garantias diferentes sobre retenção, uso para treinamento e tratamento de subprocessadores. Confirme o que se aplica à conta da unidade/NPJ.
2. **Prática de consentimento e divulgação ao(à) assistido(a) sobre o uso de IA** conforme o **Provimento OAB 205/2021**, a **Resolução CNJ 332/2020**, e o **Código de Ética e Disciplina da OAB**. Decida se e como a unidade/NPJ divulga o uso de IA ao(à) assistido(a); documente.
3. **Como material sigiloso e confidencial será tratado** — o que é colado em sessões, onde os outputs são armazenados, quem tem acesso, por quanto tempo o material é retido, como a rotatividade do termo afeta o acesso. O sigilo do(a) assistido(a) está blindado pela **LC 80/94 art. 4º-A V**; o sigilo do(a) advogado(a) (extensível ao(à) Defensor(a) e ao(à) estagiário(a) inscrito[a] na OAB) pela **Lei 8.906/94 art. 7º XIX**.
4. **Se alguma área da unidade/NPJ envolve sigilo reforçado** — Lei Maria da Penha, ECA, idoso, vítimas de tortura, refugiados, identidade de gênero — que exige proteções adicionais ou segredo de justiça processual (CPC art. 189) — e decida se o plugin é apropriado para esses tipos de caso.

Não pule esta etapa. O `cold-start-interview` (`/legal-clinic:cold-start-interview`) captura essas decisões na Parte 0 antes de qualquer outra configuração.

## Marcadores de confiança

As skills marcam confiança inline para que estagiários(as) e supervisor(a) vejam onde o andaime está incerto vs. onde está afirmando. Todo marcador é um pedido para verificar — nada marcado é confiável sem checagem.

- `[MINUTA ASSISTIDA POR IA — exige análise do(a) estagiário(a) e revisão do(a) supervisor(a)]` — rótulo baseline aplicado a todo output. Rótulo de revisão, não parte do conteúdo destinado ao(à) assistido(a); retirar antes que algo saia da unidade.
- `[INCERTO: razão específica]` — a skill está genuinamente em dúvida (tese minoritária, questão debatida, jurisdição/comarca que a skill não conhece bem). Usado em memo, intake, status, draft.
- `[VERIFICAR: alegação — confirmar fonte]` — alegação posta como provável mas não verificada. Estagiário(a) deve confirmar antes de confiar — citações, formato local da comarca, texto exato de dispositivo. Usado fortemente em research-start, draft, status, memo.
- `[PESQUISA NECESSÁRIA: ...]` — marcador no andaime do memo onde uma afirmação de regra é uma lacuna de pesquisa, não uma conclusão. Estagiário(a) roda `/research-start` e preenche.
- `[ANÁLISE DO(A) ESTAGIÁRIO(A): ...]` — marcador no andaime do memo onde a aplicação está em branco por design. O raciocínio do(a) estagiário(a) preenche.
- `[CONCLUSÃO DO(A) ESTAGIÁRIO(A): ...]` — marcador onde a conclusão está em branco por design.
- `[FATO NECESSÁRIO: ...]` — marcador na minuta onde um fato exigido está ausente das notas do caso. Estagiário(a) obtém o fato; sem adivinhar.
- `CHECAR COM [SUPERVISOR(A)] ANTES DE ENVIAR` / `ANTES DE PROTOCOLAR` — rótulo de supervisão aplicado no modo "flags configuráveis" para outputs em temas marcados.

Confie mais nas flags do que na ausência delas. Uma afirmação sem flag significa que a skill está confiante — não significa que o(a) estagiário(a) ou o(a) supervisor(a) pula a verificação. O Provimento OAB 205/2021 exige verificação independentemente.

## Salvaguardas embutidas

Todo output de toda skill inclui:

- **Rótulo de IA-assistida** — exige análise do(a) estagiário(a) e revisão do(a) supervisor(a)
- **Indicadores de confiança** — `[INCERTO: ...]` onde genuinamente em dúvida, em vez de chutar
- **Pedidos de verificação** — coisas específicas a fact-checar antes de confiar no output
- **Lembretes éticos calibrados à tarefa** — Provimento OAB 205/2021, Resolução CNJ 332/2020, Código de Ética OAB

Esses são desenhados para reforçar o modelo de educação prática: o(a) estagiário(a) faz o raciocínio, o plugin faz o trabalho braçal em volta.

**Outputs de pesquisa especificamente:** `/research-start` dá pistas e roteiros que o(a) estagiário(a) verifica e desenvolve. Explicitamente **não** fornece citações como autoritativas. Isso é salvaguarda ética e feature pedagógica — estagiários(as) ainda aprendem a pesquisar e a usar juízo crítico; só começam de um ponto melhor.

## Workflow de supervisão (configurável)

Se o plugin inclui um workflow formal de revisão — minuta do(a) estagiário(a) → revisão do(a) supervisor(a) → aprovado — é uma questão de design real. Algumas unidades querem gate duro; outras acham excessivamente prescritivo para a estrutura de supervisão que já têm.

O `cold-start-interview` pergunta ao(à) supervisor(a) qual escolher:

1. **Fila de revisão formal** — output destinado ao(à) assistido(a) ou ao juízo entra em fila, supervisor(a) aprova, tudo logado
2. **Flags configuráveis, revisão informal** — gatilhos específicos rotulam o output "CHECAR COM SUPERVISOR(A)", sem mecanismo de fila
3. **Toque mais leve** — rótulos padrão de salvaguarda em tudo, supervisão flui pela estrutura existente da unidade (reunião de equipe, atendimento conjunto, conversa de orientação)

Mutável depois editando `~/.claude/plugins/config/claude-for-legal/legal-clinic/CLAUDE.md`. Sua configuração fica nesse caminho versão-independente e sobrevive a updates do plugin.

## Rotatividade de termo: a solução `/ramp`

A cada termo, as unidades reconstroem do zero. Estagiários(as) novos(as) levam semanas para aprender procedimentos, sistemas, fundamentos por área. `/ramp` é o onboarding interativo — lê o handbook/regimento interno que o(a) supervisor(a) subiu no setup e ensina, com exercícios de baixo risco (intake fictício, prática de minuta, roteiro de pesquisa) antes do(a) estagiário(a) tocar caso real.

`/ramp --card` gera o cartão de uma página de referência: comandos, no que o Claude pode e não pode ajudar, hábitos de verificação. Distribua no primeiro dia.

## Marco regulatório

O marco ético-regulatório dentro do qual este plugin opera no Brasil:

- **Provimento OAB 205/2021** — uso de IA na advocacia exige revisão crítica do(a) advogado(a)/Defensor(a)/estagiário(a) inscrito(a), transparência com o(a) assistido(a) quando o uso de IA influenciar materialmente o produto, vedação de delegar à IA o juízo profissional, responsabilidade ético-disciplinar integral do(a) advogado(a)/Defensor(a).
- **Resolução CNJ 332/2020** — uso de IA no Poder Judiciário (transparência, auditabilidade, supervisão humana, não-discriminação, governança). Peças produzidas com auxílio de IA e protocolizadas podem ser questionadas por contraparte ou juízo — esteja preparado(a) para explicar processo e responsabilidade.
- **Código de Ética e Disciplina da OAB** — sigilo profissional, dever de competência, proibição de captação de clientela, lealdade processual.
- **LC 80/94 art. 4º §6º** — estágio na Defensoria Pública por bacharelando(a) com inscrição na OAB.
- **Resoluções do CSDPGE** (Conselho Superior da Defensoria estadual) — regulam o estágio na unidade da Federação.

Defensores(as) e professores(as) de NPJ estão entre as pessoas mais conscientes em educação jurídica sobre responsabilidade profissional. O plugin é desenhado para operar como eles(as) gostariam que operasse.

## Skills

| Skill | Função |
|---|---|
| **cold-start-interview** | Configuração única do(a) supervisor(a) — áreas, vara/comarca, modelo de supervisão, seed docs |
| **build-guide** | Guia do(a) supervisor(a) por área — intake, postura pedagógica (assist/guide/teach), gates de revisão, checagens cruzadas |
| **ramp** | Onboarding de estagiário(a) no termo — procedimentos, sistemas, exercícios práticos |
| **client-intake** | Intake específico por área com identificação cruzada de pretensões, hipossuficiência presumida, triagem |
| **draft** | Primeira minuta — templates por área, calibrada por vara (JEC vs. Comum), explicitamente ponto de partida |
| **memo** | FIRAC com lacunas de pesquisa marcadas — a análise é do(a) estagiário(a) |
| **research-start** | Roteiro de pesquisa — pistas, não autoridades; estagiários(as) verificam e desenvolvem |
| **status** | Sumários por audiência — assistido(a) / interno / juízo |
| **client-letter** | Correspondência rotineira em linguagem simples |
| **supervisor-review-queue** | Workflow opcional de revisão formal — só ativo se o(a) supervisor(a) escolheu |
| **deadlines** | Controle de prazos por caso, rollup cruzando casos, cadência de alerta, flag de vencidos |
| **client-comms-log** | Registro append-only de comunicação por caso — telefonemas, e-mails, ofícios, presencial |
| **semester-handoff** | Memos de offboarding de fim de termo; espelho de `/ramp` |

*(Duas skills descontinuadas — `form-generation`, `plain-language-letters` — redirecionam para `/draft` e `/client-letter` + `/status assistido` respectivamente.)*

## Conectores e verificação de citações

**Conecte um MCP de pesquisa primeiro — os guardrails de citação dependem disso.** Sem nenhum, toda citação é marcada `[verificar]` e a nota do revisor acima de cada entregável registra que as fontes não foram verificadas. O plugin funciona de qualquer jeito; só faz mais da verificação por você quando há MCP conectado.

Os MCPs de pesquisa jurídica deste plugin não são só fontes de dado — são a diferença entre uma citação verificada e uma citação que você tem que checar. Uma citação recuperada via **BNP-API** (precedentes vinculantes STF/STJ), **CJF-Jurisprudência** (STF/STJ/TRF1-5), **TJAM-Jurisprudência** (Tribunal de Justiça do Amazonas via e-SAJ), **DataJud** (acompanhamento processual em 61 tribunais via CNJ) ou **JusRatio** (proprietário com níveis A-E de autoridade) é taggada com sua fonte e pode ser rastreada de volta. Uma citação do conhecimento do modelo ou de busca web vai marcada `[verificar]` ou `[verificar-pinpoint]` e deve ser checada contra fonte primária antes de qualquer protocolização. O plugin escalona suas citações para que seu tempo de verificação vá para onde importa.

## Integrações

Os 7 MCPs configurados no `.mcp.json` (vide arquivo):

- **JusRatio** — pesquisa jurisprudencial brasileira proprietária, níveis A-E de autoridade
- **BNP-API** — precedentes vinculantes STF/STJ (open-source, sem chave)
- **CJF-Jurisprudência** — STF/STJ/TRF1-5 (open-source, sem chave)
- **TJAM-Jurisprudência** — TJAM via e-SAJ (open-source, sem chave)
- **DataJud** — 61 tribunais via API CNJ + cascata e-SAJ TJAM (open-source, API key gratuita do CNJ)
- **Slack** — mensagens da equipe da unidade
- **Google Drive** — busca, lê e recupera documentos da pasta do(a) assistido(a)

Os 4 MCPs abertos (BNP, CJF, TJAM, DataJud) vêm do repositório `consulta-jurisprudencia-mcp` (https://github.com/eamamtd/consulta-jurisprudencia-mcp). Clone localmente, instale dependências (`pip install -r requirements.txt`) e exporte `CONSULTA_JURISPRUDENCIA_MCP_DIR` apontando para o clone. Para DataJud, obtenha chave gratuita em https://datajud-wiki.cnj.jus.br/api-publica/acesso/ e exporte `DATAJUD_API_KEY`.

A questão do tier da conta (Team vs. Enterprise) para sigilo do(a) assistido(a) é uma decisão aberta para a TI e o(a) Coordenador(a) institucional. A arquitetura desktop do Cowork processa dados localmente.

## Como aprende

Seu perfil de prática em `~/.claude/plugins/config/claude-for-legal/legal-clinic/CLAUDE.md` não é estático — melhora conforme você usa o plugin. As skills te avisam quando um output usou um default que você deveria afinar. Você pode rerodar o setup, editar o arquivo diretamente, ou pedir para uma skill registrar uma nova posição.

## Estrutura de arquivos

```
legal-clinic/
├── .claude-plugin/plugin.json
├── .mcp.json                          # 7 MCPs: JusRatio + 4 do consulta-jurisprudencia-mcp + Slack + Drive
├── CLAUDE.md                          # Config da unidade/NPJ — escrita pelo cold-start
├── README.md
├── deadlines.yaml                     # ledger operacional de prazos
├── skills/                            # cada skill é também o slash command /legal-clinic:<skill>
│   ├── cold-start-interview/          # Supervisor(a) — setup único
│   ├── build-guide/                   # Supervisor(a) — guia por área
│   ├── ramp/                          # Estagiários(as) — onboarding do termo
│   ├── client-intake/
│   │   └── references/intake-templates/
│   ├── draft/
│   ├── memo/
│   ├── research-start/
│   ├── status/
│   ├── client-letter/
│   ├── supervisor-review-queue/       # Supervisor(a), se revisão formal habilitada
│   │   └── references/review-queue.yaml
│   ├── deadlines/
│   ├── client-comms-log/
│   ├── semester-handoff/
│   ├── form-generation/               # descontinuada → /draft (só referência)
│   └── plain-language-letters/        # descontinuada → /client-letter, /status assistido (só referência)
├── handoffs/                          # memos de handoff por termo
│   └── [AAAA-termo]/
│       ├── _summary.md
│       └── [id-caso].md
├── client-comms/                      # logs de comunicação por caso
│   └── [id-caso]/
│       └── log.md
└── hooks/hooks.json
```

## Pré-requisitos

Algumas features referem integrações externas (gestão documental, acompanhamento processual, sistema interno da unidade). Estas não são empacotadas — se você tiver um MCP server para alguma delas no seu ambiente, as features relevantes vão usar. Sem nenhum, o plugin volta para upload manual de arquivos e workflows manuais. Rode `/legal-clinic:cold-start-interview --check-integrations` para ver o que está disponível no seu ambiente.
