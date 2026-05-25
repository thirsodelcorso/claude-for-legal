---
name: build-guide
description: >
  Ajuda o(a) Defensor(a)-Supervisor(a) ou Professor(a)-Orientador(a) a autorar
  um guia por área de atuação que configura como as skills voltadas ao(à)
  estagiário(a) se comportam — perguntas de intake, postura pedagógica (assist
  / guide / teach), gates de revisão, checagens cruzadas entre plugins, e
  regras locais. Use quando o(a) supervisor(a) quer construir ou revisar um
  guia por área de atuação, calibrar como as skills do plugin se comportam
  para o tipo de unidade ou NPJ, ou setar a filosofia pedagógica como
  configuração do plugin.
argument-hint: "[optional: practice area — e.g., 'immigration', 'housing']"
---

# /build-guide

1. Carregue `~/.claude/plugins/config/claude-for-legal/legal-clinic/CLAUDE.md` → papel (deve ser Defensor[a]-Supervisor[a] ou Professor[a]-Orientador[a]), áreas de atuação, jurisdição.
2. Use o workflow abaixo.
3. Se o(a) usuário(a) não for o(a) supervisor(a), pare e redirecione (estagiários[as] rodam `/legal-clinic:ramp`).
4. Percorra: área de atuação → perguntas de intake → postura pedagógica → gates de revisão → checagens cruzadas entre plugins → regras locais.
5. Escreva `~/.claude/plugins/config/claude-for-legal/legal-clinic/guides/<area-de-atuacao>.md`. Crie o diretório `guides/` se necessário.
6. Ofereça um test run — rode `/legal-clinic:draft` sob a postura configurada para que o(a) supervisor(a) veja o que o(a) estagiário(a) vê.

```
/legal-clinic:build-guide
```

Múltiplos guias são bem-vindos — um por área de atuação. Re-rode este comando para revisar. Edite o arquivo do guia direto para mudanças rápidas.

---

# Build Guide: Guia por Área de Atuação Autorado pelo(a) Supervisor(a)

## Propósito

O guia do(a) supervisor(a) é o dial que vira as skills voltadas ao(à) estagiário(a) de "tira o trabalho" para "ensina o(a) estagiário(a) a fazer o trabalho." Toda skill voltada ao(à) estagiário(a) neste plugin lê o guia antes de produzir output: intake pergunta as perguntas que o(a) supervisor(a) quer perguntadas, skills de redação escolhem postura pedagógica (assist / guide / teach), gates de revisão roteiam ao(à) supervisor(a) os itens com que o(a) supervisor(a) se importa, e checagens cruzadas entre plugins envelopam skills de outros plugins em uma camada de supervisão.

Esta skill ajuda o(a) supervisor(a) a autorar esse guia em 5-10 minutos por área de atuação. O guia é markdown puro em um caminho conhecido — edite na mão a qualquer momento.

**Público: o(a) Defensor(a)-Supervisor(a) ou Professor(a)-Orientador(a).** Não estagiários(as). Estagiários(as) rodam `/legal-clinic:ramp` e depois as skills voltadas a eles(as); não autoram guias.

## Cabeçalho de trabalho-produto

Todo output desta skill é artefato de configuração voltado ao(à) supervisor(a), não trabalho-produto de estagiário(a). NÃO prefixe `[MINUTA ASSISTIDA POR IA — exige análise do(a) estagiário(a) e revisão do(a) supervisor(a)]` ao output desta skill — esse rótulo é para outputs de estagiário(a). O arquivo de guia que esta skill escreve é um documento de configuração do(a) supervisor(a); fica ao lado do CLAUDE.md no diretório de config do plugin, não em uma pasta de caso.

## Coisas-chave que seu guia deve endereçar

Ofereça isto como um checklist que o(a) supervisor(a) pode pular ou usar como índice da entrevista:

- O que o(a) estagiário(a) precisa saber antes de tocar em um caso? (Regras éticas — Código de Ética OAB, Provimento OAB 205/2021, Resolução CNJ 332/2020; sigilo do(a) assistido(a) — LC 80/94 art. 4º-A V; escopo de atuação sob LC 80/94 art. 4º §6º)
- Quais os 3-5 erros mais comuns que estagiários(as) cometem nesta área de atuação, e como a skill deve apanhá-los?
- Quando o(a) estagiário(a) deve parar e pegar seu sign-off? (Protocolar, enviar ao(à) assistido(a), fazer uma representação, aconselhar sobre estratégia)
- Qual o nível de leitura para comunicações com o(a) assistido(a)? (Ensino fundamental II — 6º-9º ano — é o alvo usual em DP e NPJ atendendo população de baixa renda)
- Que regras locais (resoluções CSDPGE, regimento TJAM), formulários ou prazos todo(a) estagiário(a) deve saber?
- Quando a skill deve teach vs. do? (Por tipo de documento — você pode setar default e fazer override por tipo)

Percorra o checklist no início da entrevista para que o(a) supervisor(a) saiba o que vem e possa sinalizar quais itens já tem visão forte versus quais quer pensar. Pule qualquer item que o(a) supervisor(a) descartar; anote no guia como "não especificado — skill usa defaults."

## Workflow

### Passo 1: Cheque o papel

Esta é uma skill de supervisor(a). Leia `~/.claude/plugins/config/claude-for-legal/legal-clinic/CLAUDE.md` → `## Quem está usando` → Papel. Se o papel não for "Defensor(a)-Supervisor(a)" ou "Professor(a)-Orientador(a)", diga:

> Esta skill é para supervisores(as) — ela configura como as skills voltadas ao(à) estagiário(a) se comportam. Se você é o(a) supervisor(a), confirme que seu papel no perfil de atuação está setado como "Defensor(a)-Supervisor(a)" ou "Professor(a)-Orientador(a)" em `/legal-clinic:cold-start-interview`. Se você é estagiário(a), esta não é a skill certa — rode `/legal-clinic:ramp` para onboarding, ou peça ao(à) seu(sua) supervisor(a) para autorar um guia para a unidade ou NPJ.

Pare se o papel não for de supervisão.

### Passo 2: Qual área de atuação?

> Para qual área de atuação é este guia? (Família/Sucessões / Consumidor / Saúde Pública / Previdenciário (BPC/LOAS) / Locação / Possessória / Defesa em ação de cobrança / Outra)

Se a resposta for "Outra", peça um nome curto — esse nome vira o nome do arquivo (lowercase, com hífen: `familia-sucessoes.md`, `saude-medicamento.md`, etc.).

Cheque as áreas de atuação listadas em `CLAUDE.md` → `## Perfil da unidade / NPJ` → Áreas de atuação. Se a área escolhida não estiver listada lá, anote: "Vou escrever este guia, mas seu perfil de atuação não lista [área] como uma das áreas da unidade. Tudo bem — você pode adicionar depois com `/legal-clinic:cold-start-interview --redo` — mas as skills voltadas ao(à) estagiário(a) não vão rotear intakes para esta área até o perfil listar."

Se um guia já existe em `~/.claude/plugins/config/claude-for-legal/legal-clinic/guides/<area-de-atuacao>.md`, ofereça: "Um guia para [área] já existe em [caminho]. Você quer (a) revisar seção por seção, (b) começar do zero e sobrescrever, ou (c) ver o que está lá primeiro?"

### Passo 3: Perguntas de intake

> O que estagiários(as) devem perguntar a um(a) novo(a) assistido(a) nesta área de atuação? Vou começar com um intake genérico para [área de atuação] — me diga o que adicionar, remover ou mudar. Que bandeiras vermelhas estagiários(as) devem procurar? O que faz um caso ter perfil para a unidade vs. encaminhamento externo?

Mostre os defaults genéricos de intake para a área de atuação — use os mesmos defaults que `client-intake` usa (Família: relação, filhos[as], segurança Lei Maria da Penha, ordens existentes, audiências; Consumidor: tipo de obrigação, quem cobra, documentação, ações em curso, prazos prescricionais/decadenciais; Saúde Pública: quadro clínico, medicamento/leito/procedimento, negativa, urgência; Previdenciário: BPC/LOAS, indeferimento, perícia; Locação: tipo de locação, notificação, pagamentos, condições; Possessória: tipo de posse, esbulho/turbação, tempo, urgência). Para áreas fora dessas, peça ao(à) supervisor(a) para descrever o intake do zero.

Capture: perguntas a adicionar, perguntas a remover, perguntas a reformular, bandeiras vermelhas (lista), critérios de "perfil para unidade" (o que faz a unidade pegar o caso vs. encaminhar para outra DP / núcleo especializado / advogado dativo).

### Passo 4: Postura pedagógica

> Quanto as skills devem fazer vs. quanto o(a) estagiário(a) deve fazer?
>
> - **Guide (default):** A skill produz estrutura; o(a) estagiário(a) preenche a substância; a skill dá feedback. Balanceado — a maioria das unidades começa aqui.
> - **Assist:** A skill produz o trabalho-produto; o(a) estagiário(a) revisa e aprende editando. Mais rápido, menos pedagógico. Bom para unidades de alto volume ou quando os prazos apertam.
> - **Teach:** A skill não produz trabalho-produto — o(a) estagiário(a) redige, a skill dá feedback socrático e só mostra modelos depois de duas tentativas. Mais lento, mais pedagógico. Bom para NPJ com seminário ou quando aprender é o objetivo primário.
>
> Você pode setar isto por tipo de documento (ex.: teach para cartas ao(à) assistido(a), assist para memos internos).

Capture a postura default para a área de atuação, e quaisquer overrides por documento. Configurações por documento que as skills leem:

- `pedagogy_posture_default: assist | guide | teach`
- `pedagogy_posture_client_letter: [override]`
- `pedagogy_posture_memo: [override]`
- `pedagogy_posture_draft: [override]`

Se o(a) supervisor(a) nomear um tipo de documento que as skills atualmente não têm, registre a postura pretendida em bloco `pedagogy_posture_other:` com nota — skills futuras podem ler.

### Passo 5: Gates de revisão

> Que trabalho-produto precisa de sua revisão antes de ir ao(à) assistido(a)? Qual o(a) estagiário(a) pode enviar diretamente? Default: tudo voltado ao(à) assistido(a) precisa de revisão.

Apresente as opções como tabela que o(a) supervisor(a) preenche:

| Trabalho-produto | Gate |
|---|---|
| Sumário de intake | [estagiário(a) escreve; supervisor(a) revisa em reunião de equipe / supervisor(a) revisa antes do(a) assistido(a) ver / estagiário(a) guarda] |
| Memo (interno) | [supervisor(a) revisa / estagiário(a) guarda] |
| Carta ao(à) assistido(a) (atendimento / pedido de doc / status breve) | [supervisor(a) revisa / estagiário(a) envia diretamente] |
| Carta ao(à) assistido(a) (conselho substantivo / má notícia) | [sempre supervisor(a) — não pode override] |
| Minuta de peça (juízo / órgão administrativo) | [sempre supervisor(a) — não pode override] |
| Atualização de status para juízo | [sempre supervisor(a) — não pode override] |
| Roadmap de research-start | [estagiário(a) trabalha direto a partir disto] |

Alguns gates são não-negociáveis: cartas ao(à) assistido(a) com conselho substantivo, peças a protocolar e atualizações ao juízo sempre passam pelo(a) supervisor(a) per estrutura de supervisão da unidade. Sinalize esses como fixos; os configuráveis são os de rotina.

### Passo 6: Checagens cruzadas entre plugins

> Você quer que estagiários(as) usem skills de outros plugins (checagem de termos definidos, consistência documental, referências cruzadas de seção, verificação de pesquisa)? Posso envolver em supervisão — o(a) estagiário(a) roda a checagem, o output sinaliza incerteza para sua revisão, nada sai sem seu sign-off.

Ofereça exemplos concretos atrelados à área de atuação:

- **Família/Sucessões:** `litigation-legal:chronology` para construir timeline de fatos a partir de documentos do(a) assistido(a), sinalizada para revisão do(a) supervisor(a) antes de alimentar petição de divórcio litigioso ou de alimentos.
- **Consumidor:** `commercial-legal:review` para triagem de contratos de adesão antes de petição inicial em JEC, sinalizada para revisão do(a) supervisor(a) antes de ir para a contraparte.
- **Saúde Pública:** `litigation-legal:chronology` para timeline de negativas administrativas e laudos, sinalizada para revisão do(a) supervisor(a) antes de alimentar petição inicial com pedido de tutela de urgência.
- **Qualquer área:** `privacy-legal:triage` se o(a) estagiário(a) está lidando com matéria onde dados pessoais sensíveis (saúde, criança/adolescente, vítima de violência) são compartilhados fora da unidade.

Se o(a) supervisor(a) nomear uma skill cross-plugin que queira, registre: nome da skill, quando estagiários(as) devem usar, que envelope de supervisão se aplica (sempre revisor[a], só quando sinalizado, nunca sem supervisor[a]).

### Passo 7: Regras locais e jurisdição

> Em que juízo(s) sua unidade atua? Alguma resolução ou regimento local (CSDPGE, TJAM) que estagiários(as) precisam usar?

Cheque `CLAUDE.md` → `## Jurisdição` — UF e juízo primário já estão setados no cold-start. Este passo é para regras e formulários específicos da área de atuação (ex.: "Provimento TJAM sobre prazo da contestação em ação de despejo," "endereço de protocolo do INSS local para ações previdenciárias," "central de conciliação de Família e como acessar formulários"). Ofereça capturar uma lista curta de pointers que as skills voltadas ao(à) estagiário(a) devem usar ao redigir ou aconselhar.

### Passo 8: Escreva o guia

Escreva em `~/.claude/plugins/config/claude-for-legal/legal-clinic/guides/<area-de-atuacao>.md`. Crie o diretório `guides/` se não existir. Use esta estrutura:

```markdown
# Guia por área de atuação: [Área de atuação]

*Autorado pelo(a) Defensor(a)-Supervisor(a) ou Professor(a)-Orientador(a) via `/legal-clinic:build-guide`. Skills voltadas ao(à) estagiário(a) leem isto antes de produzir output. Edite direto a qualquer momento.*

**Última atualização:** [data]
**Autorado por:** [nome do(a) supervisor(a) do CLAUDE.md]

---

## Intake

**Perguntas a fazer** (suplementam/substituem os defaults genéricos):
- [pergunta 1]
- [pergunta 2]
- ...

**Bandeiras vermelhas** (surface no sumário de intake se presentes):
- [bandeira 1]
- [bandeira 2]

**Critérios de "perfil para unidade"** (casos que a unidade pega):
- [critério 1]
- [critério 2]

**Critérios de encaminhamento** (casos que a unidade não pega):
- [critério 1]
- [critério 2]

---

## Postura pedagógica

`pedagogy_posture_default: [assist | guide | teach]`

Overrides por documento (opcional):
- `pedagogy_posture_client_letter: [assist | guide | teach]`
- `pedagogy_posture_memo: [assist | guide | teach]`
- `pedagogy_posture_draft: [assist | guide | teach]`

**Razão:** [uma ou duas frases do(a) supervisor(a) sobre por que essa postura — ajuda o(a) supervisor(a) do termo seguinte a entender a escolha]

---

## Gates de revisão

| Trabalho-produto | Gate |
|---|---|
| Sumário de intake | [gate] |
| Memo (interno) | [gate] |
| Carta ao(à) assistido(a) — rotina | [gate] |
| Carta ao(à) assistido(a) — substantiva | supervisor(a) (fixo) |
| Minuta de peça | supervisor(a) (fixo) |
| Status voltado ao juízo | supervisor(a) (fixo) |
| Roadmap de pesquisa | [gate] |

---

## Checagens cruzadas entre plugins

| Skill | Quando estagiários(as) usam | Envelope de supervisão |
|---|---|---|
| [plugin:skill] | [situação] | [envelope] |

---

## Regras locais e jurisdição

**Juízo(s):** [do CLAUDE.md ou juízos adicionais para esta área de atuação]
**Regras e formulários locais específicos desta área de atuação:**
- [pointer 1]
- [pointer 2]
```

Preencha toda seção a partir das respostas do(a) supervisor(a). Deixe uma seção vazia só se o(a) supervisor(a) disse — não invente conteúdo.

Então diga ao(à) supervisor(a):

> Seu guia está em `~/.claude/plugins/config/claude-for-legal/legal-clinic/guides/<area-de-atuacao>.md`. Todo(a) estagiário(a) que usar o plugin para [área de atuação] vai ter skills que seguem ele. Edite o arquivo direto para mudar algo, ou re-rode `/legal-clinic:build-guide` para revisar uma seção. Você pode ter múltiplos guias — um por área de atuação.

### Passo 9: Ofereça um test run

> Quer ver como a postura pedagógica muda a experiência? Vou rodar `/legal-clinic:draft` com uma carta de exemplo ao(à) assistido(a) sob [postura] — você vê o que o(a) estagiário(a) vê.

Se o(a) supervisor(a) topar, simule a skill de redação lendo o guia que acabaram de escrever e produzindo output sob a postura configurada. Percorra um ciclo completo para que o(a) supervisor(a) veja exatamente o que um(a) estagiário(a) veria.

## Output

O "output" da skill é o arquivo escrito em `~/.claude/plugins/config/claude-for-legal/legal-clinic/guides/<area-de-atuacao>.md`. A conversa com o(a) supervisor(a) é a entrevista; o guia escrito é o artefato.

Depois de escrever, mostre confirmação curta:

> **Guia escrito.** `[area-de-atuacao]` agora está configurado:
>
> - Intake: [N] perguntas custom, [N] bandeiras vermelhas, [N] critérios de encaminhamento
> - Pedagogia: [postura default], com overrides para [listar se houver]
> - Gates de revisão: [sumário do que rota ao(à) supervisor(a) vs. estagiário(a)]
> - Cross-plugin: [N] skills conectadas
>
> Estagiários(as) vão ver essas mudanças na próxima vez que rodarem um comando do plugin para esta área de atuação. Edite `[caminho]` a qualquer momento para mudar algo, ou re-rode `/legal-clinic:build-guide` para revisar.

## O que esta skill NÃO faz

- **Configurar o plugin globalmente.** O guia é por-área-de-atuação. Para config plugin-wide (estilo de supervisão, jurisdição, áreas de atuação), isso é `/legal-clinic:cold-start-interview`.
- **Autorar trabalho-produto de estagiário(a).** Isto é configuração voltada ao(à) supervisor(a), não minuta para o(a) assistido(a).
- **Override do estilo de supervisão do cold-start.** O modelo de supervisão (fila formal / flags configuráveis / toque mais leve) é setado no setup. Gates de revisão no guia refinam esse modelo para esta área de atuação; não substituem.
- **Fazer uma skill de estagiário(a) pular o cabeçalho IA-assistida, as flags de confiança ou os pedidos de verificação.** Esses são baselines de guardrail compartilhado. O guia muda postura, não guardrails.
