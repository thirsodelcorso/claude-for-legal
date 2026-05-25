---
name: cold-start-interview
description: >
  Setup único do(a) supervisor(a) — áreas de atuação, vara/comarca, modelo
  de supervisão (fila formal / flags configuráveis / toque mais leve), e
  upload de regimento/resoluções/regras locais. Escreve CLAUDE.md para toda
  outra skill e todo(a) estagiário(a) que rode /ramp ler do mesmo contexto.
  Use em instalação fresca, quando CLAUDE.md tem placeholders, ao refazer
  setup com --redo, ou re-checar integrações com --check-integrations.
argument-hint: "[--redo] [--check-integrations]"
---

# /cold-start-interview

1. Cheque `~/.claude/plugins/config/claude-for-legal/legal-clinic/CLAUDE.md`. Se populado e sem `--redo`, confirme antes de sobrescrever.
2. Rode a entrevista voltada ao(à) supervisor(a) abaixo, começando com Parte 0 (checagem do papel de Defensor(a)-Supervisor(a) ou Professor(a)-Orientador(a) → pré-condições éticas → disponibilidade de integração). Se a pessoa não é supervisor(a), pare e redirecione.
3. Docs-semente: regimento da unidade ou NPJ, resoluções CSDPGE (DP) ou CNE/CES + IES (NPJ), regras locais do TJAM (ou tribunal local), formulário(s) de intake, um exemplo de pasta de caso anonimizada.
4. Decisão-chave: modelo de supervisão (fila formal / flags / toque mais leve).
5. Migração: se um CLAUDE.md populado (sem marcadores `[PLACEHOLDER]`) existe em `~/.claude/plugins/cache/claude-for-legal/legal-clinic/*/CLAUDE.md` mas não no caminho config, copie e mostre o que foi migrado.
6. Escreva `~/.claude/plugins/config/claude-for-legal/legal-clinic/CLAUDE.md` incluindo `## Quem está usando` e `## Integrações disponíveis`. Mostre escolha de supervisão e templates por área para confirmação.
7. Ofereça preview de `/legal-clinic:ramp`.

```
/legal-clinic:cold-start-interview
```

**`--check-integrations`:** Re-roda apenas a checagem de disponibilidade de integração da Parte 0 (sistema interno Sapiens-DPGU ou próprio, armazenamento documental, DataJud, MCPs de pesquisa). Atualiza `## Integrações disponíveis` em `~/.claude/plugins/config/claude-for-legal/legal-clinic/CLAUDE.md` sem tocar no papel, pré-condições éticas, modelo de supervisão ou templates. Use depois de adicionar/remover conector MCP.

Quando sondando: só reporte ✓ se uma tool MCP efetivamente respondeu com sucesso. Conectores configurados-mas-não-testados devem ser marcados ⚪ com uma linha de "como confirmar". Nunca reporte ✓ baseado só em declarações no `.mcp.json`.

---

# Entrevista Cold-Start: Estágio Supervisionado (DP / NPJ)

## Propósito

Unidades de Defensoria Pública e Núcleos de Prática Jurídica (NPJ) são estruturalmente limitados em capacidade. Um(a) Defensor(a)-Supervisor(a) na DP supervisiona alguns(mas) estagiários(as) inscritos(as) na OAB sob LC 80/94 art. 4º §6º; um(a) Professor(a)-Orientador(a) em NPJ supervisiona 5-10 alunos(as) sob Resolução CNE/CES 5/2018 + convênio + regimento da IES. Cada estagiário(a) carrega alguns casos enquanto cumpre disciplinas, e a turma rotaciona a cada termo / semestre. A fila no acolhimento cresce. Pessoas desistem de esperar.

A função deste plugin é reduzir o custo de tempo de tudo *em volta* da advocacia — intake do(a) assistido(a), primeiras minutas, ponto de partida de pesquisa, atualizações de status — para que a mesma equipe atenda significativamente mais assistidos(as), e para que os(as) estagiários(as) gastem mais tempo na análise e na estratégia que fazem o estágio supervisionado valer a pena.

Esta entrevista monta o contexto da unidade ou NPJ uma vez, para que cada estagiário(a) que faz onboarding via `/ramp` e cada skill que roda depois trabalhe do mesmo entendimento de como *esta* unidade opera.

**Audiência: o(a) Defensor(a)-Supervisor(a) (na DP) ou Professor(a)-Orientador(a) (no NPJ).** Estagiários(as) não rodam este setup — rodam `/ramp`.

## Checagem cold-start

Leia `~/.claude/plugins/config/claude-for-legal/legal-clinic/CLAUDE.md`:
- **Não existe** → comece a entrevista.
- **Contém `<!-- SETUP PAUSED AT: -->`** → cumprimente e ofereça retomar.
- **Contém `[PLACEHOLDER]` mas sem comentário de pausa** → template nunca completado; ofereça começar do zero ou retomar.
- **Populado (sem placeholders, sem pausa)** → já configurado; pule salvo `--redo`.

## Checagem do perfil compartilhado da unidade

Procure `~/.claude/plugins/config/claude-for-legal/company-profile.md`.

- **Se existe:** Leia. Confirme em uma linha: "Você é [nome], [tipo de atuação: Defensoria / NPJ / etc.], em [unidade / IES], atuando em [áreas], comarca [X]. Certo?" Se confirmado, pule perguntas de unidade — vá direto às específicas do plugin.
- **Se não existe:** Você é o primeiro plugin que esta pessoa configura. Depois da orientação e bifurcação, faça as perguntas de unidade e escreva no perfil compartilhado (per template em `references/company-profile-template.md` na raiz do plugin), depois continue com as perguntas específicas. Diga: "Salvei o perfil — os outros plugins jurídicos vão ler e pular estas perguntas."

## Checagem de escopo de instalação

Antes da orientação, se o diretório de trabalho está dentro de um projeto (não home), sinalize uma vez:

> **Atenção — parece que este plugin pode estar em escopo de projeto. Só posso ler arquivos em [diretório atual]. Se quiser que eu leia documentos de outros lugares (Downloads, Documentos, Drive), instale em escopo de usuário — vide QUICKSTART.md. Pode continuar com escopo de projeto, mas vai precisar mover arquivos para esta pasta.**

Confirme antes de prosseguir. Se diretório de trabalho *é* o home, pule silenciosamente.

## Antes de começar a entrevista

Preâmbulo primeiro (3-4 linhas curtas, nada mais):

> **`legal-clinic` é para Defensor(a)-Supervisor(a) de estágio na Defensoria Pública ou Professor(a)-Orientador(a) em Núcleo de Prática Jurídica (NPJ), configurando e fazendo onboarding de estagiários(as).** Não é sua área? `/legal-builder-hub:related-skills-surfacer`.
>
> **2 minutos** te dá área(s) de atuação, vara/comarca, e fundamentos do modelo de supervisão — mais defaults razoáveis para formato de carta ao(à) assistido(a), scaffold FIRAC, e cadência de prazos. **15 minutos** adiciona registro das pré-condições éticas, gatilhos de flag de supervisão, templates de documento por área a partir das suas peças, conteúdo do regimento alimentando `/ramp`, regras locais do TJAM (ou tribunal local) alimentando `/draft`, e datas do termo de estágio.
>
> Rápido ou completo? (Atualize a qualquer momento com `/cold-start-interview --full`.)

## Depois de escolher rápido ou completo

Oriente. Cubra, na sua voz:

- **O que este plugin mantém:** seu perfil de unidade/NPJ (áreas de atuação, modelo de supervisão, templates da casa), arquivos por caso (intake, prazos, log de comunicação, memos de handoff), e fila de revisão do(a) supervisor(a).
- **O que este setup faz:** suporta a unidade ou NPJ — intake do(a) assistido(a), memos FIRAC, cartas ao(à) assistido(a), atualizações de status, prazos — nas suas áreas, com supervisão embutida. Aprende áreas, vara/comarca, modelo de supervisão, e escreve em arquivo texto que toda skill lê e que o `/ramp` do(a) estagiário(a) lê. Tudo mudável depois. Uma vez feito, comandos funcionam como a unidade opera, não como template genérico.
- **Fontes de dado:** setup constrói perfil fresco das respostas do(a) supervisor(a) e dos documentos subidos (regimento, regras locais, formulários de intake, exemplos de caso). Não lê histórico pessoal do Claude, outras conversas, ou CLAUDE.md do home. Se algo relevante apareceu antes na conversa (ex.: unidade ou área), pergunto antes de incorporar.
- **Próximo:** Parte 0 — quem está rodando o setup e as pré-condições éticas.

**Por que isso importa.** Todo `/ramp` de estagiário(a), todo `/client-intake`, todo `/draft`, todo `/client-letter`, todo `/status` lê da configuração que esta entrevista escreve. Configuração genérica dá output genérico — modelo de supervisão default, formatos default, tom default. A primeira semana de termo é gasta corrigindo o que a ferramenta assumiu sobre a unidade. Dizer ao plugin as áreas, o modelo, e as regras locais é o que faz a diferença entre "ferramenta de IA para clínica" e "ferramenta que roda como esta unidade roda".

### Quick start ou full setup — bifurcação

A pessoa escolheu rápido ou completo. Bifurcação:

**Quick start:** pergunte só o básico (área, vara/comarca, modelo de supervisão). Escreva config com marcadores `[DEFAULT]` em tudo o resto. Feche com: "Pronto. Use os comandos já. Usei defaults razoáveis para formato de carta, scaffold FIRAC, e cadência de prazos. Quando output parecer estranho, normalmente é default a afinar — vai te dizer qual. Rode `/legal-clinic:cold-start-interview --full` para entrevista completa, ou `--redo <seção>` para refazer uma parte."

**Full setup:** fluxo abaixo.

## Cadência da entrevista

- **Assuma que a resposta existe em algum lugar.** Quando uma pergunta pede info que provavelmente está escrita em algum lugar — regimento, lista de áreas, lista de varas, resolução do CSDPGE, formulário de intake — peça link ou paste antes de pedir digitação de memória.

**Pause para respostas reais.** Parte 0 tem tap-through de papel e integração. Pré-condições éticas, Partes 1-5, especialmente Parte 4 (docs-semente) precisam de resposta digitada ou upload. Quando uma pergunta precisa de mais que tap:

- **Faça e espere.** Diga explicitamente: "Esta precisa de resposta digitada — vou esperar." Não vá para a próxima até a pessoa responder.
- **Para uploads (regimento, regras locais, formulários, exemplo de caso, peças exemplares, cartas exemplares):** "Cole o conteúdo, compartilhe caminho de arquivo, ou diga 'pular por ora.' Se pular, flag a lacuna no perfil para preencher depois — e nota o que isso significa para `/ramp`, `/draft` e `/client-letter` (vão ficar mais finos ou fall back para defaults)." Depois efetivamente espere.
- **Antes de escrever o perfil:** revise. Liste pergunta pulada ou respondida com placeholder. Diga: "Antes de escrever, eis o que está aberto: [lista]. Preencher agora, ou deixar?" Espere.
- **Nunca** escreva perfil com lacuna silenciosa. Todo placeholder é escolha deliberada de pular — não pergunta que rolou para fora da tela.
- **Tamanho do batch — conte sub-partes.** 2-3 prompts respondíveis, contando sub-partes. Teste: a pessoa responde sem rolar?
- **Pause e retome.** "Se precisar parar, diga 'pause' (ou 'pare', ou 'deixa pra depois') e eu salvo. Rode `/legal-clinic:cold-start-interview` de novo e eu pego de onde paramos." Quando pausar, escreva configuração parcial com `<!-- SETUP PAUSED AT: [seção] — rode /legal-clinic:cold-start-interview para retomar -->` e `[PENDING]` em campos não respondidos.

**Verifique fatos jurídicos declarados.** Quando a pessoa responde com citação específica de regra, dispositivo, súmula, prazo ou jurisdição que você pode sanity-check, faça antes de escrever. Se conflita, surface: "Você disse [X]; meu entendimento é [Y] — pode confirmar? `[premissa marcada — verificar]`"

## A entrevista

### Parte 0: Quem está rodando este setup, pré-condições éticas, e o que está conectado (antes de tudo)

#### Quem está rodando este setup?

> Você é o(a) Defensor(a)-Supervisor(a) (na DPEAM ou outra DP) ou Professor(a)-Orientador(a) do NPJ? Você precisa ter inscrição na OAB ativa (ou ser membro da DP) e estar supervisionando estagiários(as) sob LC 80/94 art. 4º §6º (DP) ou Resolução CNE/CES 5/2018 + convênio (NPJ). Setup escreve o contexto governante da unidade — modelo de supervisão, regras de dados do(a) assistido(a), pré-condições éticas — e deve ser feito por quem será responsável pelo trabalho.
>
> 1. **Sim, sou o(a) Defensor(a)-Supervisor(a) ou Professor(a)-Orientador(a).** Continuar.
> 2. **Não, sou estagiário(a) / servidor(a) / administrador(a).** Pare. Setup só pode ser rodado por quem supervisiona. Peça para o(a) responsável rodar `/legal-clinic:cold-start-interview`. Estagiários(as) rodam `/legal-clinic:ramp` para onboarding a cada termo.

Se a resposta é 2, pare e surface. Não prossiga.

Se a resposta é 1, registre na config do plugin sob `## Quem está usando` (Papel: Defensor-Supervisor ou Professor-Orientador; nome e seccional capturados) e continue.

*Por que isso importa:* a unidade roda em norma de regência que exige supervisão por profissional habilitado(a) (Defensor(a) integrante da DP, ou advogado(a) inscrito(a) na OAB para NPJ). Decisões de cold-start — modelo de supervisão, gating de ação consequente, pré-condições éticas — são chamada do(a) supervisor(a). A pergunta de papel gateia essas decisões à pessoa certa.

#### Pré-condições éticas e de confidencialidade

Antes da entrevista do(a) supervisor(a) começar — e antes de qualquer estagiário(a) usar este plugin em caso real — confirme o seguinte com a Coordenação da unidade (ou direção do NPJ), TI / Corregedoria. Não pule.

1. **Tier da conta e termos de manejo de dados.** Sua conta Claude (Team, Enterprise, Work, Education, individual) tem garantias diferentes sobre retenção, uso para treinamento, e tratamento de subprocessadores. Confirme em qual a unidade está e o que os termos aplicáveis dizem sobre dados do(a) assistido(a). Documente na config.

2. **Práticas de consentimento e divulgação ao(à) assistido(a) sobre trabalho assistido por IA.** Reveja o **Provimento OAB 205/2021** (uso de IA na advocacia), a **Resolução CNJ 332/2020** (uso de IA no Judiciário), e o **Código de Ética e Disciplina da OAB** (dever de competência, sigilo, lealdade, vedação a captação de clientela). Para Defensor: LC 80/94 art. 4º-A V (sigilo do(a) assistido(a)) e LC 80/94 art. 4º §6º (estágio). Decida se e como a unidade ou NPJ divulga uso de IA ao(à) assistido(a), e documente.

3. **Como material sigiloso e confidencial é manejado.** O que é colado em sessões, onde outputs são armazenados, quem tem acesso, por quanto tempo material é retido localmente, como a rotatividade do termo afeta acesso. Documente as regras de manejo que estagiários(as) devem seguir.

4. **Considerações de sigilo reforçado por área.** Casos envolvendo Lei Maria da Penha (violência doméstica), criança/adolescente (ECA), idoso (Estatuto), refugiado(a), saúde mental, identidade de gênero carregam expectativas reforçadas de sigilo e segurança que vão além do baseline — risco de exposição da contraparte, risco de requisição, risco à segurança de vítimas. Confirme se alguma área da unidade requer salvaguardas adicionais (limitar fatos colados em sessões, redação adicional, não usar o plugin para certo tipo de caso).

Capture as respostas. Se alguma pré-condição está não-resolvida, flag na config e nota que estagiários(as) não devem usar o plugin em caso real até resolução.

#### O que está conectado?

> Este plugin trabalha com sistema interno da unidade (Sapiens-DPGU para DPU, sistema próprio AM para DPEAM, software acadêmico para NPJ), armazenamento documental (Google Drive, SharePoint, Box), Gmail, MCPs de pesquisa jurídica brasileira (JusRatio proprietário com níveis A-E; e os 4 open-source do consulta-jurisprudencia-mcp — BNP/STF-STJ vinculantes, CJF/STF-STJ-TRFs, TJAM/e-SAJ, DataJud/CNJ 61 tribunais com cascata e-SAJ TJAM). Vou checar quais conectores estão configurados — features que precisam vão funcionar, e que não, fall back para manual graciosamente em vez de falhar silenciosamente.

**Cheque o efetivamente conectado, não o configurado.** Para cada conector:

- Se você pode testar (chamar tool MCP simples), reporte ✓ só em resposta com sucesso.
- Se não pode testar, reporte ⚪ "configurado mas não verificado — abra configurações MCP para confirmar".
- Nunca reporte ✓ baseado só em configuração.

Para conectores não conectados, diga como conectar. Para os 4 MCPs do consulta-jurisprudencia-mcp: "(1) Clone https://github.com/eamamtd/consulta-jurisprudencia-mcp localmente, (2) `pip install -r requirements.txt`, (3) obtenha chave gratuita do DataJud em https://datajud-wiki.cnj.jus.br/api-publica/acesso/, (4) exporte `CONSULTA_JURISPRUDENCIA_MCP_DIR=<path>` e `DATAJUD_API_KEY=<chave>`. Plugin funciona sem — acompanhamento processual cai para manual no e-SAJ — mas com, o `docket-watcher` agent puxa as movimentações automaticamente."

Depois reporte achados nesta forma:

> - ✓ [Integração] — conectada (testada)
> - ⚪ [Integração] — configurada mas não verificada. Abra configurações MCP para confirmar.
> - ✗ [Integração] — não encontrada. [Feature] vai cair em [alternativa manual]. [Como conectar.]

Você não precisa de todas. Features core — intake, draft, carta ao(à) assistido(a), research-start, prazos, handoff de termo, revisão de supervisor — funcionam só com acesso a arquivo.

Escreva respostas da Parte 0 na config sob `## Quem está usando` e `## Integrações disponíveis`. Se um CLAUDE.md populado existe no caminho cache antigo `~/.claude/plugins/cache/claude-for-legal/legal-clinic/*/CLAUDE.md` mas não aqui, copie primeiro.

### Abertura

> Este é o setup único da unidade ou NPJ. 10-15 minutos. Vou perguntar sobre áreas, vara/comarca, como você supervisiona, e depois vou pedir que aponte para regimento e regras locais do TJAM (ou tribunal). Tudo que aprendo aqui alimenta o `/ramp` que seus(suas) estagiários(as) vão rodar no início de cada termo, e todo outro comando.
>
> Nada disto substitui seu juízo ou a análise dos(as) estagiários(as). O objetivo é cortar horas gastas em formatação, estruturação, e escrita — para que mais tempo dos(as) estagiários(as) vá para a advocacia, e mais assistidos(as) sejam atendidos(as).
>
> Vou pedir materiais ao longo — regimento, regras locais, formulários de intake, exemplo de pasta de caso, peças exemplares protocoladas, cartas exemplares ao(à) assistido(a). 10-20 documentos ao longo da entrevista é o alvo. Mais é melhor. Abaixo de dez, vou flagar o perfil como DADOS LIMITADOS — plugin funciona mas `/ramp` fica fino (cobre comandos mas não procedimentos específicos), `/draft` fall back para defaults estaduais em vez de formatação local, `/client-letter` usa templates genéricos em vez de casar com sua voz. Templates-primeiro: se você sobe doc, leio e caso seu formato em vez de pedir descrição.

### Parte 1: A unidade ou NPJ (2-3 min)

**Que tipo?** (Área alimenta /client-intake e /draft — cada área tem template próprio.)
- Tipo: Defensoria Pública estadual / DPU / NPJ acadêmico / clínica de prática real / escritório-escola
- Nome da unidade (ex.: "4ª DP dos JECs + 17ª e 34ª DPs Cíveis — Capital de Manaus") ou nome do NPJ + IES
- Áreas de atuação: **Família/Sucessões, Consumidor, Saúde Pública, Previdenciário (BPC/LOAS), Locação, Possessória, Defesa em ação de cobrança, outro** (pode ser múltiplas — DPs cíveis frequentemente acumulam)

   **Áreas que não cabem nas caixas.** Se a unidade tem práticas que não casam (refugiado(a), militar, ambiental, atendimento a comunidades indígenas/tradicionais, ou qualquer outra não-cível-padrão), ofereça: "Parece que sua unidade não cabe nas categorias usuais. Me conte na sua voz — o que a unidade faz, quem atende, em que jurisdições, como é o trabalho — e eu construo o perfil disto em vez de te forçar nas caixas. Vou pular ou adaptar perguntas que não se aplicam."
- Quantos(as) estagiários(as) neste termo? Quantos casos ativos simultaneamente, aproximadamente?
- Quantos(as) Defensores(as) ou Professores(as) supervisionando?

**Quem são os(as) assistidos(as)?**
- Situações típicas — quem chega ao acolhimento, o que enfrenta?
- Línguas além do português? (Atendimento a comunidades indígenas é relevante na DPEAM e em outras DPs do Norte/Nordeste — tradução pode ser necessária.)
- Fontes de encaminhamento comuns (CRAS, CREAS, ONGs, 156, encaminhamento espontâneo)?

### Parte 2: Jurisdição (1-2 min)

(Alimenta /draft, /research-start, /memo, /deadlines — vara determina formato de peça, escopo de pesquisa, e cálculos default de prazo.)

- UF (ex.: AM). Direciona tudo jurisdição-aware — Lei 8.245/91 cálculos de despejo, procedimentos de medida protetiva da Lei 11.340/06, formatos de peça.
- Vara(s) primária(s): quais varas a unidade atende mais? (Para DPEAM piloto: 1ª e 12ª Varas dos JECs Cíveis + 19ª e 20ª Varas Cíveis Comuns.)
- Provimentos da Corregedoria local ou portarias específicas do juízo que divergem do padrão CNJ/CPC?

### Parte 3: Modelo de supervisão (2-3 min — pergunta-chave de design)

> Unidades variam muito em quanto o trabalho do(a) estagiário(a) é revisado antes de sair. Algumas querem cada minuta em fila formal — estagiário(a) submete, supervisor(a) aprova, depois sai. Outras são toque-leve — estagiário(a) check in, supervisor(a) aprova informalmente, estrutura mais conversacional. Qual o seu? (Alimenta /supervisor-review-queue e a lógica de gatilho de flag em /draft, /client-letter, /status — fila formal liga a skill supervisor-review-queue; flags configuráveis só surface gatilhos; toque-leve suprime a fila.)

Três opções:

**Fila de revisão formal:** Output que vai ao(à) assistido(a) ou ao juízo entra em fila. Supervisor(a) revisa, aprova ou edita, libera. Toda aprovação logada. (Skill `supervisor-review-queue` ligada.)

**Flags configuráveis, revisão informal:** Gatilhos certos (prazos, temas sensíveis, peças a protocolar) flag o output com "CHECAR COM [SUPERVISOR(A)] ANTES DE ENVIAR" — sem fila mecânica. Estagiário(a) responsável por procurar. (Sem fila; estagiários(as) flag diretamente quando gatilho acontece e procuram você.)

**Toque mais leve:** Outputs carregam rótulo padrão de IA-assistida e pedidos de verificação, mas sem gates adicionais. Supervisor(a) supervisiona via estrutura existente da unidade (reunião de equipe, atendimento conjunto, conversa de orientação), não via plugin. (Sem fila ou flags adicionais; confio na estrutura existente.)

> Não há resposta certa — depende do nível de experiência dos(as) estagiários(as), do caseload, e como você já roda a supervisão. Você muda depois editando CLAUDE.md.

Capture a escolha e, se fila formal ou flags configuráveis: que deve disparar flag? (Peças a protocolar sempre? Qualquer menção de prazo? Temas como Lei Maria da Penha, ECA, saúde mental, criminal por escala?)

**Dial pedagógico.** Depois da escolha de supervisão, pergunte:

> **Quanto as skills devem fazer?** Esta é a configuração mais importante. Três opções:
>
> - **Guide (default):** Skill produz estrutura; estagiários(as) preenchem substância; skill dá feedback. Balanceado — onde maioria das unidades começa.
> - **Assist:** Skill produz produto; estagiários(as) revisam, editam, aprendem vendo. Mais rápido, mais produtivo, menos pedagógico. Bom para unidades de alto volume.
> - **Teach:** Skill não produz produto — estagiários(as) redigem, skill faz perguntas socráticas e dá feedback, e só mostra modelo depois de duas tentativas. Mais lento, mais pedagógico. Bom para NPJs onde aprendizado é objetivo primário.
>
> Você pode setar por tipo de documento depois com `/legal-clinic:build-guide`. Por ora, pegue um default.

Escreva no perfil como `pedagogy_default: assist | guide | teach` (default `guide` se não escolher).

**Guia por área de atuação.** Depois do pedagógico, ofereça:

> Quer autorar um guia por área que afina como as skills funcionam para sua unidade — perguntas de intake, overrides de pedagogia por documento, gates de revisão? Posso te ajudar a construir em 5-10 min com `/legal-clinic:build-guide`. Pode fazer depois também. Por ora, skills usam defaults: o pedagógico que você escolheu, e tudo destinado ao(à) assistido(a) flagado para sua revisão.

Note a resposta no estado de setup — se quer construir guia, surface como próximo passo depois que a entrevista fechar (sob Passo 3 da seção "Depois de escrever"). Não interrompa esta entrevista para rodar `/legal-clinic:build-guide` inline; termine o perfil primeiro, depois ofereça handoff.

### Parte 4: Docs-semente (3-4 min)

> Três coisas, o que tiver. (Regimento alimenta /ramp; regras locais alimentam /draft; formulário de intake vira espinha do /client-intake.)
>
> 1. **Seu regimento da unidade ou manual de procedimentos.** O que você dá ao(à) estagiário(a) no primeiro dia. Vou usar para construir o onboarding `/ramp` para estagiários(as) terem walkthrough guiado em vez de PDF que dão scroll.
>
> 2. **Regras locais e provimentos.** Qualquer coisa que diz como formatar endereçamento, onde protocolar, o que o juízo da vara quer. Para DPEAM piloto: provimentos da Corregedoria-Geral da DPEAM e regras locais do TJAM. Alimentam `/draft` para primeiras minutas serem jurisdição-corretas desde o início.
>
> 3. **Seu formulário de intake, e se tiver, exemplo de pasta de caso anonimizada.** Formulário de intake vira espinha do `/client-intake`. Exemplo de caso me mostra como é caso bem-documentado na sua unidade.

**Do regimento:** procedimentos da unidade, convenções de gestão de caso, expectativas do(a) estagiário(a), lembretes éticos. Isto é o que `/ramp` vai ensinar.

**De regras locais / provimentos:** formato de endereçamento (ex.: "JUÍZO DA 19ª VARA CÍVEL DA COMARCA DE MANAUS"), requisitos de protocolização eletrônica (Lei 11.419/06, e-SAJ TJAM), peculiaridades de prática local do juízo. Isto é o que `/draft` vai aplicar.

**Do formulário de intake:** campos específicos por área. Se a unidade tem formulários separados por área (consumidor vs. saúde), pegue todos.

### Parte 5: Templates por área de atuação (1-2 min)

Para cada área que a unidade trata: quais 3-5 documentos os(as) estagiários(as) minutam mais frequentemente? (Alimenta /draft — cada documento listado vira template que a skill pode começar, e qualquer um não listado fall back para primeira passada genérica.)

| Área de atuação | Documentos comuns |
|---|---|
| Família/Sucessões | Petição inicial de divórcio consensual / litigioso, alimentos, união estável, guarda; declaração de hipossuficiência |
| Saúde Pública | Petição inicial de fornecimento de medicamento (Tema 793 STF), leito UTI, internação CAPS; pedido de tutela de urgência CPC 300 |
| Consumidor (JEC) | Petição inicial Lei 9.099/95 (vício, cobrança indevida, dano moral) |
| Previdenciário | Petição inicial BPC/LOAS (Justiça Federal); recurso administrativo INSS |
| Locação | Defesa em ação de despejo (Lei 8.245/91 art. 62 II — purgação da mora) |
| Possessória | Petição inicial de reintegração / manutenção / interdito proibitório (CPC 554-568); contestação |

Estes viram o conjunto de templates para `/draft`. Se você tem templates existentes, ingest. Se não, anote quais construir.

**Se não subiu regimento ou formulário de intake:** no final desta seção, ofereça: "Quer que eu redija regimento inicial e formulário de intake do que você me contou? Mesmo conteúdo que acabei de capturar — modelo de supervisão, áreas, vara — em formato que você edita e compartilha com a próxima turma."

## Antes de escrever — re-leia

Antes de comitar o perfil no plugin, re-leia toda resposta capturada em ordem. Pega:

1. **Contradições entre respostas** — ex.: "fila formal" em modelo de supervisão mas "toque mais leve, via reunião" em descrição de como revisão efetivamente acontece. Surface ambas e pergunte qual governa.
2. **Específicos que derivaram** — nomes, juízos, datas que mudaram entre seções. Confirme valores finais.
3. **Lacunas puladas que merecem ser nomeadas** — áreas listadas sem templates, modelo escolhido sem gatilhos populados, regimento prometido mas não subido. Ofereça completar agora.

## Escrevendo o perfil

Per o template CLAUDE.md. Seções-chave:

- **Perfil da unidade / NPJ** — nome, IES (se NPJ), áreas, vara/comarca, contagem de estagiários(as)
- **Modelo de supervisão** — qual dos três, e gatilhos de flag se aplicável
- **Templates por área de atuação** — templates de intake e de documento por área
- **Jurisdição** — UF, vara/comarca, provimentos ingest
- **Termo / semestre** — quando os(as) estagiários(as) rotacionam (para `/ramp` saber quando será necessária, e `/semester-handoff` saber quando será disparada)
- **Caminho do regimento** — onde o regimento ingest vive, para `/ramp` ler

**Flag DADOS LIMITADOS:** se menos de 10 materiais foram compartilhados, adicione nota `> DADOS LIMITADOS` no topo do CLAUDE.md (sob data), declarando: "Este perfil foi escrito de [N] materiais. Skills downstream vão rodar mas outputs mais finos — `/ramp` cobre comandos mas não procedimentos específicos, `/draft` usa defaults estaduais em vez de formatação local, `/client-letter` usa templates genéricos. Re-rode `/legal-clinic:cold-start-interview --redo` depois de coletar mais exemplares para afiar."

## Framing de salvaguarda embutido

Escreva no plugin os padrões de salvaguarda que toda skill vai aplicar:

```markdown
## Salvaguardas de output (aplicadas por toda skill)

Todo output inclui:
- **Rótulo de IA-assistida:** "[MINUTA ASSISTIDA POR IA — exige análise do(a) estagiário(a) e revisão do(a) supervisor(a)]"
- **Indicadores de confiança:** Onde a skill está incerta, diz explicitamente
- **Pedidos de verificação:** Coisas específicas a fact-checar antes de confiar no output
- **Lembretes éticos calibrados à tarefa:** ex., outputs de /draft lembram requisitos do Provimento OAB 205/2021

Não são opcionais nem configuráveis. São o baseline.
```

## Depois de escrever

**Mostre o que este plugin pode fazer.** Antes de fechar, ofereça:

> **Quer ver com o que eu posso ajudar?**

Se sim, mostre esta lista calibrada (não template genérico — coisas concretas que este plugin faz melhor):

> **No que eu sou bom em prática de estágio supervisionado:**
>
> - **Intake de um(a) novo(a) assistido(a)** — ex.: "Caminhar um(a) estagiário(a) por intake específico de área com identificação de bandeira vermelha e checagem de impedimento." Try: `/legal-clinic:client-intake`
> - **Redigir carta ao(à) assistido(a) em linguagem simples (LC 80/94 art. 4º-A III)** — ex.: "Produzir confirmação de audiência ou atualização breve em linguagem clara; estagiário(a) edita e você aprova." Try: `/legal-clinic:client-letter`
> - **Construir scaffold de memo FIRAC** — ex.: "Dar ao(à) estagiário(a) a estrutura e lista de lacunas de pesquisa para memo de caso — pedagogia default é guide." Try: `/legal-clinic:memo`
> - **Acompanhar prazos no caseload ativo** — ex.: "Ver o que está vencendo nos próximos 14/7/3/1 dias úteis com alertas (CPC 219, dobro Defensor CPC 186)." Try: `/legal-clinic:deadlines`
> - **Ramp up de turma nova** — ex.: "Onboarding deste termo aos procedimentos da unidade, sistemas, e normas de manejo de caso." Try: `/legal-clinic:ramp`
> - **Handoff de termo** — ex.: "Construir memos de transição por caso para a próxima turma." Try: `/legal-clinic:semester-handoff`
>
> **Minha sugestão para sua primeira:** Rode `/ramp` você mesmo(a) primeiro para ver o que estagiários(as) vão ver no início do termo. Ou me diga o que está na sua mesa e eu escolho.

Isto resolve o problema cold-start (a pessoa não sabe o que fazer primeiro) e o problema de proposta-de-valor (não sabe o que o plugin pode fazer) em uma oferta. Faça a lista específica.

1. **Mostre a escolha de modelo de supervisão.** "Você escolheu [fila formal / flags / toque mais leve]. Significa [o que significa na prática]. Chamada certa?"

2. **Mostre a tabela de templates por área.** "Estes são os documentos que `/draft` vai saber começar. Faltando algo?"

3. **Ofereça preview de `/ramp`.** "Quer ver como vai parecer o onboarding de estagiário(a)? Posso caminhar como se você fosse novo(a) estagiário(a)."

4. **Note o que não foi fornecido.** Se sem regimento: "`/ramp` vai ficar fino até subir um — vai cobrir comandos mas não procedimentos específicos." Se sem regras locais: "`/draft` vai usar defaults estaduais para formatação — suba provimentos da Corregedoria-Geral local quando tiver."

5. **Se DADOS LIMITADOS flag:** "Perfil fino — skills downstream vão ser genéricas até mais materiais. Maior lacuna: [específico — ex.: sem regimento significa /ramp cobre só comandos]. Maior ganho fácil: [específico — ex.: suba 2-3 peças recentes que você protocolou, e /draft fica dramaticamente mais afiado nas suas convenções de formatação]."

6. **Antes do primeiro caso, conecte MCP de pesquisa.** Diga: "Antes do primeiro caso ou memo: conecte JusRatio (proprietário) ou os 4 MCPs do consulta-jurisprudencia-mcp (BNP, CJF, TJAM, DataJud — open-source, instruções acima). Sem nenhum, vou flagar toda citação como não verificada — com, verifico contra base atual. Em Cowork: Settings → Connectors. Em Claude Code: autorize quando uma skill pedir."

   <!-- COLLATERAL LINKS: quando colateral de onboarding existir, adicione:
        "Quer walkthrough primeiro? [Assista intro de 3 min](URL) ou [leia getting-started](URL)." -->

7. **Feche com nota "você pode mudar tudo depois":**

> Pronto. A configuração da sua unidade está em `~/.claude/plugins/config/claude-for-legal/legal-clinic/CLAUDE.md` — texto puro que você lê e edita diretamente. Tudo respondido pode ser mudado:
>
> - Edite diretamente para mudança rápida
> - Rode `/legal-clinic:cold-start-interview --redo` para re-entrevista completa
> - Rode `/legal-clinic:cold-start-interview --check-integrations` para re-checar conectado
>
> Coisas que unidades mais ajustam: áreas (quando a unidade pega área nova), modelo de supervisão (fila formal vs. flags vs. toque mais leve — muitas unidades começam um jeito e mudam após primeiro termo), e jurisdição / regras locais (quando caso cai em juízo incomum). Sua config melhora à medida que estagiários(as) usam o plugin — quando `/ramp` perde algo ou `/draft` usa formato errado, a correção costuma estar aqui.

## Seu perfil aprende

Depois de escrever o perfil, feche com esta nota:

> **Seu perfil aprende.** Melhora à medida que você usa os plugins:
>
> - Quando output de skill parecer estranho, normalmente é posição a afinar. O output vai te dizer qual.
> - Você sempre pode dizer "atualize meu playbook para preferir X" e a skill relevante escreve a mudança.
> - Rode `/cold-start-interview --redo <seção>` para re-entrevista de uma parte, ou edite a config direto.
>
> Dez minutos de setup te dá perfil funcional. Um mês de uso te dá um que lê como se você tivesse escrito.

## O que esta skill NÃO faz

- **Decide o modelo de supervisão.** É chamada do(a) supervisor(a); esta entrevista só pergunta e registra.
- **Substitui o sistema oficial da unidade.** Se a unidade usa Sapiens-DPGU, sistema próprio AM, ou outro, este plugin trabalha ao lado (vide `.mcp.json`).
- **Faz onboarding de estagiários(as).** Isso é `/ramp`. Este é o setup único do(a) supervisor(a).
