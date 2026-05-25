---
name: chronology
description: Construa ou atualize cronologia a partir de fontes documentais declaradas e uploads — eventos datados extraídos, deduplicados, tagueados por significância conforme a tese do caso. Use quando o usuário pede para construir cronologia ou timeline de uma produção ou pasta de caso, diz "cronologia da produção" ou "o que aconteceu quando", ou precisa de timeline de trabalho, peça de fatos ou específica de testemunha.
argument-hint: "[slug] [--format=working|sof|witness-[name]]"
---

# /chronology

1. Carregue `~/.claude/plugins/config/claude-for-legal/litigation-legal/matters/[slug]/matter.md` → tese, fato pivô, fatos-chave.
2. Carregue `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md` → fontes de armazenamento documental, padrão de pasta de caso default.
3. Siga o workflow e a referência abaixo.
4. Identifique fontes na ordem: paths fornecidos pelo usuário nesta sessão, pasta de caso default, fontes declaradas em `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md`.
5. Para fontes legíveis: extraia eventos datados. Para fontes inacessíveis: anote em Lacunas.
6. Deduplique, agrupe com lista de fontes por evento.
7. Tagueie significância (🔴/🟡/⚪) conforme tese do caso.
8. Grave `~/.claude/plugins/config/claude-for-legal/litigation-legal/matters/[slug]/chronology.md` (ou variante de formato per flag).
9. Se versão anterior existe: número de versão incrementa, sumário de diff apresentado ao usuário.
10. Confirme antes de finalizar: "Eis o que construí. Passe os olhos nas entradas 🔴 — algo que classifiquei errado?"

---

# Cronologia

## Restrições de uso de documento divulgado

Antes de trabalhar com conjunto de documentos do processo, pergunte: "Algum destes documentos veio de instrução probatória em processo judicial, ou de divulgação compelida em sede de tutela cautelar de exibição (CPC arts. 396-404)?" Se sim:

- **Brasil — segredo de justiça (CPC art. 189):** documentos que tramitam em segredo de justiça têm acesso restrito às partes e seus(suas) procuradores(as). Usá-los fora da finalidade processual a que se destinam pode configurar quebra de segredo (CP art. 154 — violação de segredo profissional; CPC art. 80 — litigância de má-fé).
- **Brasil — tutela exibitória (CPC arts. 396-404):** documento exibido por terceiro ou parte adversa por força de decisão judicial está afeto à finalidade da prova produzida; uso para outro caso, outra pretensão, ou finalidade comercial sem autorização judicial é abuso.
- **Outras jurisdições:** restrições análogas costumam aplicar. Confira a regra local.

Confirme: "Este uso está dentro do processo em que os documentos foram divulgados, ou tenho autorização judicial / consentimento da parte, ou os documentos já são públicos." Se não confirmado, sinalize: "⚠️ Documentos divulgados podem ter restrição de uso. Confirme que este uso é permitido antes de prosseguir."

## Propósito

Fatos acontecem em ordem. A cronologia é a espinha em que cada narrativa se pendura — a peça de fatos numa razão, memos de provisão, memos de acordo, preparação de oitiva, preparação de testemunha. Construir cronologia à mão é lento; IA é boa em extração estruturada. O catch: lixo entra, lixo sai. Esta skill puxa das fontes que a configuração declara e do que o usuário fizer upload.

## Modos

Esta skill atende dois settings de prática. Escolha o default a partir do `## Papel` do usuário no CLAUDE.md de configuração do plugin; o usuário pode sobrescrever per-run com flag.

- **modo `--matter` (default para contencioso em DJ ou Defensoria).** Focado em histórico de caso. Lê a tese e fatos-chave do caso em `matter.md`, puxa de fontes de armazenamento documental declaradas (Google Drive, SharePoint, Gmail, sistema de gestão jurídica, CLM — o que a seção `## Panorama` do CLAUDE.md declara), e trata `history.md` como o log interno corrente (decisões, devers de guarda, memos de provisão — intencionalmente fora da cronologia). Output é centrado em caso: o que aconteceu ao longo da disputa, tagueado para uso advocatício.
- **modo `--documents` (default para advogado(a) em sociedade / paralegal).** Focado em documento de produção. Lê a tese do caso da configuração, depois extrai de uma exportação de plataforma de gestão documental, conjunto de arquivos por custodiante, ou produção numerada por movimentação CNJ. Output é centrado em produção: o que os documentos mostram, com citações de movimentação, tagueado conforme a tese do caso.

Ambos os modos convergem para a mesma estrutura de output (timeline, tags de significância 🔴/🟡/⚪, lacunas, variante de peça de fatos). A diferença é o perfil de fonte e o frame de significância.

Se `## Papel` é `advogado-autonomo` ou `outro`, default para `--matter` mas mencione ambos os modos na primeira execução e deixe o usuário escolher.

## Frame de polo (tags de significância)

O mesmo evento é significativo de formas diferentes conforme o(a) profissional está provando ou desprovando uma pretensão. Leia `## Posição processual` no perfil de atuação (e a postura por caso, se o caso sobrescreve o default):

- **Autor (frame ofensivo)** — 🔴 marca eventos que *estabelecem* elementos da pretensão (responsabilidade, nexo, danos, ciência), *fecham* lacunas que a defesa tentará abrir, ou *disparam* contagens de prescrição (CC arts. 205-206) ou decadência a favor do autor. 🟡 marca eventos que sustentam a pretensão mas estão sujeitos a impugnação. ⚪ é contexto de fundo.
- **Réu (frame defensivo)** — 🔴 marca eventos que *quebram* elementos da pretensão (falha de nexo, ciência, confiança), *abrem* defesas de prescrição/decadência ou de competência, ou *sustentam* defesas autônomas (quitação, renúncia, assunção de risco, culpa concorrente). 🟡 marca eventos que minam a narrativa do autor. ⚪ é fundo.
- **Ambos / varia** — pergunte ao usuário por-cronologia qual frame aplicar para tags de significância. A timeline subjacente é neutra; só a leitura de significância muda.

Anote o frame aplicado no topo do output: `Tags de significância aplicadas do ponto de vista de [autor / réu].` Ao produzir variante peça de fatos, use o default do polo a menos que o usuário especifique diferente.

## Carregar contexto

Comum:
- CLAUDE.md de configuração do plugin → contexto de tese do caso (DJ: `## Panorama` para fontes documentais; advogado em sociedade: `## Tese do caso` e `## Revisão documental` para plataforma + custodiantes), `## Outputs` para o cabeçalho de trabalho-produto, `## Postura de decisão` para a regra de flagging de sigilo.
- `chronology.md` anterior deste caso, se existe.
- Quaisquer arquivos que o usuário fizer upload ou paths que fornecer in-session.

modo `--matter` também lê:
- `~/.claude/plugins/config/claude-for-legal/litigation-legal/matters/[slug]/matter.md` → tese, fatos-chave, fato pivô (para tagging de significância), datas-chave.
- Padrão de pasta de caso default do CLAUDE.md → onde docs deste slug vivem.

modo `--documents` também lê:
- Metadados da plataforma de gestão documental jurídica se conector disponível — por custodiante + faixa de data.
- Manifesto de produção ou índice de produção se o usuário aponta para um.

**Gate de impedimentos — incontornável (modo `--matter`).** Antes de construir a cronologia, cheque `~/.claude/plugins/config/claude-for-legal/litigation-legal/matters/_log.yaml` para o slug. Se o caso não está em `_log.yaml`, recuse e route:

> "Não vejo [slug do caso] no log de casos. Rode `/litigation-legal:matter-intake` primeiro para a checagem de impedimentos rodar e o workspace ser montado. Não construo cronologia em caso não-intaken — a checagem de impedimentos é o gate."

Não prossiga em caso não-intaken. Intake é o que roda impedimentos e grava a linha de `_log.yaml` que esta skill lê. modo `--documents` (rodando contra um conjunto ad-hoc sem slug) é isento do gate, mas seus outputs devem ser tratados como pesquisa pré-caso e não arquivados como se fossem trabalho-produto de caso.

## Fluxo de trabalho

### Passo 0: Gate de sigilo (roda primeiro, sempre)

Trabalho de cronologia puxa de documentos. Documentos frequentemente são sigilosos (comunicação advogado-cliente Lei 8.906/94 art. 7º XIX, trabalho preparatório, interesse comum, defesa conjunta — para Defensor, sigilo do(a) assistido(a) LC 80/94 art. 4º-A V) — pastas internas de caso em DJ frequentemente são por default; produções na gestão documental, especialmente produções iterativas ou produções de interesse comum, frequentemente contêm material sigiloso ou não-revisado. Extrair conteúdo de documento sigiloso para uma cronologia que depois é compartilhada pode *arriscar* quebra de sigilo, dependendo de quem recebe e sob que doutrina (interesse comum, defesa conjunta, e o sigilo profissional do(a) advogado(a) podem aplicar). Análise de quebra é específica do caso — obtenha sign-off antes de distribuir.

A skill não extrai até o usuário escolher uma postura de sigilo:

> Antes de extrair: como as fontes foram filtradas para sigilo?
>
> - **A. Todas as fontes liberadas** — você já filtrou. Extraio sem flags de sigilo. Output é postura pronta para instrução; ainda marcado como trabalho-produto.
>
> - **B. Misto ou ainda não filtrado** — extraio e tagueio cada entrada com flag `priv`: `ok` (de material claramente não-sigiloso), `flag` (de material potencialmente sigiloso — A/C, trabalho preparatório, interesse comum), ou `review` (fonte pouco clara). Entradas flagueadas são visualmente marcadas no output, e a variante peça de fatos as filtra por default.
>
> - **C. Abortar — filtrar primeiro** — pausa a skill. Filtre as fontes. Volte e rerode.

Registre a escolha no cabeçalho da cronologia como `privilege_posture: A-cleared | B-mixed | C-aborted`. Se B ou C, registre o racional brevemente.

**Por que gate e não só aviso:** um aviso é lido uma vez e esquecido. Um gate força a decisão de postura para o registro, o que significa que cada arquivo de cronologia carrega sua própria proveniência — qualquer um que leia depois sabe se as entradas vieram de material filtrado para sigilo.

### Passo 1: Identificar fontes documentais

**modo `--matter`:**

1. **Paths fornecidos pelo usuário** — qualquer coisa dropada nesta sessão (paths de arquivo, links de drive, exportações de e-mail).
2. **Pasta de caso default** — do padrão de armazenamento documental do CLAUDE.md, expandido para este slug (ex.: `G:/Juridico/Casos/silva-vs-cemig-2026`).
3. **Fontes declaradas** — a tabela `Armazenamento documental` no CLAUDE.md, filtrada para as que este caso possa tocar (ex.: arquivo Gmail para comunicações lado-remetente, pasta SharePoint Jurídico).
4. **Pergunte** — se as fontes parecem finas, pergunte: "Posso construir com o que tenho, mas a cronologia ficará incompleta. Algo mais para apontar? E-mails-chave, contratos, memos internos, cartas de produção?"

**modo `--documents`:**

1. **Exportação de produção / conjunto por movimentação CNJ** — o usuário aponta para o diretório de produção ou um manifesto; a skill lê por movimentação + data.
2. **Conector de gestão documental** — se MCP de plataforma de gestão documental jurídica está disponível, puxe por custodiante + faixa de data.
3. **Arquivos por custodiante** — se o usuário fornece mailboxes ou exportações de drive crus, leia também.
4. **Pergunte** — se cobertura parece fina para custodiante-chave ou faixa de data, pergunte.

### Passo 2: Puxar + ler

Para cada fonte com arquivos legíveis:

- **PDFs, e-mails (.eml), .docx, .txt** — leia diretamente.
- **Arquivos de e-mail (Gmail, Outlook)** — se MCP autenticado, consulte por faixa de data + contraparte / termos-chave; senão o usuário exporta threads relevantes para uma pasta.
- **Plataformas de gestão documental** — se conector disponível, puxe por custodiante + faixa de data; senão o usuário fornece exportação.

Se a skill não consegue acessar fonte declarada, nomeie explicitamente na seção Lacunas do output em vez de prosseguir silenciosamente.

**Sem suplementação silenciosa.** Se cobertura de fonte para uma era do caso é fina — menos documentos que o esperado para uma janela de tempo alegada, custodiante cujo mailbox não é acessível, produção que ainda não aterrissou — reporte o que foi encontrado e pare. NÃO preencha lacunas com busca web, busca em registro público, ou conhecimento do modelo sobre o caso sem perguntar. Diga: "Fontes retornaram [N] eventos para [período / custodiante]. Cobertura parece fina. Opções: (1) aponte para fontes adicionais (movimentação, pasta, mailbox), (2) tente um MCP diferente se configurado, (3) busque na web por eventos de registro público nesta janela — resultados serão tagueados `[busca web — verificar]` e devem ser checados contra fonte primária antes de confiar, ou (4) pare aqui e anote a lacuna. Qual prefere?" Um(a) advogado(a) decide se aceita fontes de menor confiança; a skill não decide por ele.

**Atribuição de fonte.** Tagueie cada entrada da cronologia com de onde o evento veio: path do arquivo, ID de movimentação CNJ, conector MCP, ou fonte declarada de armazenamento documental para eventos extraídos de documentos recuperados (já capturado na coluna Fontes). Para qualquer evento ou data que não pode ser rastreado até um documento recuperado — ex.: fato lembrado de dados de treino do modelo, evento de registro público achado via busca web — tagueie inline: `[busca web — verificar]`, `[conhecimento do modelo — verificar]`, ou `[usuário forneceu]` onde o usuário declarou o fato in-session. Entradas tagueadas `verificar` carregam maior risco de fabricação que entradas com fonte documental e devem ser checadas primeiro. Nunca strip ou colapse as tags — são o sinal mais rápido do(a) advogado(a) sobre quais entradas verificar antes de puxar para razões ou peça de fatos.

**Tagging chega a toda seção que afirma conclusão jurídica, prazo ou data computada — não só entradas de timeline.** A timeline tem fonte em documentos. A seção Lacunas, a seção Eventos-chave, as linhas de amarração com a tese, e qualquer afirmação sobre prescrição, evento interruptivo, prazo de protocolo, encerramento de instrução, ou determinação de sigilo é análise jurídica que a skill escreve do conhecimento do modelo a menos que tenha fonte. Toda tal afirmação carrega tag de proveniência: `[computado de: <regra citada com tag>]`, `[conhecimento do modelo — verificar]`, `[usuário forneceu]`, ou tag de conector de pesquisa se recuperado nesta sessão. Uma janela de prescrição sem tag default para `[conhecimento do modelo — verificar]`. Uma linha de "evento-chave" que caracteriza a significância jurídica de um fato é análise e precisa da tag. A regra é simples: se é afirmação sobre a lei, não afirmação sobre o que um documento diz, deve carregar a mesma tag de proveniência que as entradas da timeline. Quando nenhum conector de pesquisa está alcançável e a skill está computando prazos ou citando regras, registre na linha **Fontes:** da nota do revisor (vide CLAUDE.md `## Outputs`) — não emita banner solto.

### Passo 3: Extrair eventos

Para cada documento, identifique eventos datados:

- **E-mail:** `[data] [remetente] disse a [destinatário] [assunto/conteúdo]`
- **Reunião:** `[data] [presentes] reuniram-se sobre [tópico]` (per entrada de calendário ou notas)
- **Decisão:** `[data] [decisor] decidiu [o quê]` (per doc memorialístico)
- **Protocolo / peça:** `[data] [parte] protocolou [peça/inicial/manifestação]`
- **Evento externo:** `[data] [coisa aconteceu]` (contrato assinado, produto lançado, regulador agiu, evento cruzou limiar)

Um evento por documento usualmente. Ocasionalmente zero (não datado ou nenhum evento estabelecido). Às vezes múltiplos (sumário de reunião cobrindo várias decisões).

**Flag de sigilo por entrada (só quando privilege_posture == B-mixed). Regra tri-estado — nunca decida silentemente que um teste subjetivo de sigilo não é atendido:**

- `priv: ok` — fonte é **confiantemente** não-sigilosa (peças protocoladas, correspondência regulatória, docs públicos, comunicações com contraparte sem nosso(a) advogado(a)). Use só quando não há teoria plausível de sigilo.
- `priv: flag` — fonte é confiantemente ou provavelmente sigilosa (comunicações com advogado(a)/Defensor(a), memos preparatórios, minutas sigilosas, material de defesa conjunta). **Default para qualquer coisa incerta** — se a chamada de propósito dominante é apertada, ou litígio em contemplação é borderline, ou o conteúdo é misto, vai aqui, não em `ok`.
- `priv: review` — fonte pouco clara em sua face, mas a skill não conseguiu fazer a chamada (sem metadados de remetente/destinatário, ilegível, etc.).

Quando `priv: flag` ou `priv: review`, adicione `[SME VERIFICAR: status de sigilo]` inline para que o(a) advogado(a) veja na revisão. Sub-flag quebra sigilo (porta de mão única); super-flag é corrigida pelo(a) advogado(a) em revisão (porta dupla). Prefira o erro recuperável.

### Passo 4: Deduplicar

O mesmo evento aflora em vários documentos: uma reunião está em três agendas e produz e-mail-sumário — isso é **um evento com quatro fontes**, não quatro eventos. Mescle. A entrada mesclada cita todas as fontes.

### Passo 5: Tagueie significância — per tese do caso

Leia o fato pivô e fatos-chave de `matter.md` (modo `--matter`) ou da seção `## Tese do caso` da configuração (modo `--documents`). Tagueie cada evento:

- 🔴 **Chave** — evento é parte do fato pivô ou fato-chave a favor/contra nós
- 🟡 **Relevante** — contexto, padrão evidencial, sustenta argumento secundário
- ⚪ **Fundo** — útil para completude, não vai na razão

**Disciplina:** cronologia de 300 entradas com 300 tags 🔴 não tem tags. Reserve 🔴 para eventos que efetivamente moveriam um juízo. Em dúvida, 🟡.

**Tagging borderline:** quando uma entrada está entre 🔴 e 🟡 (ou 🟡 e ⚪), tagueie no nível inferior e adicione `[SME VERIFICAR — chamada borderline de significância]` inline. O juízo do(a) advogado(a) sobrescreverá a chamada da skill. Cronologia que confiantemente super-tagueia é menos útil que uma que aflora sua incerteza.

### Passo 6: Gravar

Output default é a cronologia de trabalho. Variantes sob pedido.

## Formatos de output

### Cronologia de trabalho (default)

Local: `~/.claude/plugins/config/claude-for-legal/litigation-legal/matters/[slug]/chronology.md`. Completa, tagueada, anotada. O doc de referência do qual o(a) advogado(a) trabalha.

```markdown
[CABEÇALHO DE SIGILO — por config do plugin ## Outputs — varia por papel; vide `## Quem está usando`]

> **Herança de sigilo.** Esta cronologia deriva de documentos do caso que podem ser sigilosos por comunicação advogado-cliente, material preparatório, interesse comum / defesa conjunta, ou misto. Herda o status de proteção das fontes. Distribuí-la além do círculo de sigilo — a stakeholders de negócio fora do patrocínio, a advogado(a) contrário(a), a regulador — pode quebrar a proteção tanto da cronologia quanto das fontes subjacentes. Armazene com material sigiloso do caso, marque consistente com convenções de sigilo da casa, e tome decisões de distribuição deliberadamente. A escolha de postura de sigilo capturada abaixo é o carimbo de proveniência para qualquer decisão de distribuição posterior.

# Cronologia — [Nome do caso]

> Tags de significância (🔴/🟡/⚪) e flags de sigilo (🔒) são leituras de primeira passagem exigindo `[SME VERIFICAR]` antes de uso em qualquer trabalho-produto externo (razões, peça de fatos, memo para diretoria, entregável a escritório externo).

**Caso:** [slug]
**Modo:** matter | documents
**Construído:** [YYYY-MM-DD]
**Fontes:** [N] documentos em [tipos de fonte]
**Entradas:** [N] ([N] 🔴 / [N] 🟡 / [N] ⚪)
**Fato pivô:** [uma frase]
**Postura de sigilo:** A-cleared | B-mixed | C-aborted
**Entradas flagueadas:** [N] 🔒 *(só presente quando postura == B-mixed)*

---

## Timeline

| Data | Evento | Tag | 🔒 | Fontes |
|---|---|---|---|---|
| [YYYY-MM-DD] | [o que aconteceu, uma frase] | 🔴/🟡/⚪ | [vazio / 🔒-flag / 🔒-review] | [paths ou movimentações] |

---

## Eventos-chave (só 🔴)

[Puxados, cada um com linha sobre por que importa para a tese.]

### [data] — [título do evento]
- O quê: [uma linha]
- Amarração com tese: [por que importa]
- Fontes: [lista]

---

## Lacunas

**Faixas de data sem eventos:**
[faixas — onde estão os documentos deste período?]

**Esperados mas ausentes:**
[eventos que esperaríamos ver documentados mas não estão — ex.: "aditivos contratuais entre 2024-06 e 2025-03 — não produzidos"]

**Fontes ilegíveis:**
[fontes declaradas em CLAUDE.md mas não acessíveis neste run — ex.: "produção da plataforma de gestão documental — sem conector MCP; exportação necessária"]

---

## Disciplina de marcador

- `[VERIFICAR: alegação factual — data, presentes, conteúdo]` — ainda não confirmado contra o doc subjacente
- `[INCERTO: caracterização jurídica — ex.: se um evento estabelece gatilho regulatório]`
- `[CITE FALTANDO: ID de movimentação CNJ / pinpoint da peça / fl. da ata]`
- `[SME VERIFICAR: status de sigilo | chamada borderline de significância]` — juízo de advogado(a) necessário

---

## Versão
- v[N] construído em [data] de [sumário de fontes]
- v[N-1] construído em [data] (anterior, superseded)
```

### Cronologia peça-de-fatos (sob pedido)

Filtre para 🔴 e 🟡 relevantes apenas. Apresente como prosa em ordem narrativa cronológica — o esqueleto para a seção de fatos de uma razão. Cada parágrafo é um evento ou cluster estreitamente ligado, com citações do registro.

**Filtro de sigilo default:** quando `privilege_posture == B-mixed`, entradas 🔒-flag e 🔒-review são **excluídas** por default. A variante peça-de-fatos é destinada a uso externo eventual (razões, divulgações, negociação com contraparte) — entradas 🔒 não pertencem ali até o(a) advogado(a) confirmar status de sigilo. Se o usuário quer entradas 🔒 incluídas mesmo assim, exija acknowledgment explícito `--include-flagged`; capture no cabeçalho do output como registro permanente.

### Cronologia específica de testemunha (sob pedido)

Filtre para eventos onde uma testemunha nomeada é remetente, destinatário, presente ou sujeito. Alimenta preparação de testemunha e ajuda a reconstruir o que uma testemunha sabia e quando.

## Builds incrementais

Se `chronology.md` existe:

- Leia versão anterior
- Construa cronologia nova das fontes atuais
- Diff: eventos novos (desde último build), entradas modificadas (novas fontes adicionadas a eventos existentes), entradas removidas (raro; anote o porquê)
- Preserve o número de versão anterior; grave versão nova com `v[N+1]`
- Output sumário do que mudou

## Integração com matter.md / history.md

**Intencionalmente separados** (modo `--matter` em DJ ou Defensoria). `history.md` é o log corrente do(a) advogado(a) — decisões, updates, marcos procedimentais, notas internas de estratégia. `chronology.md` é a timeline advocatícia dos fatos. Sobrepõem mas não se fundem:

- Um dever de guarda foi emitido → vai em history.md (ação interna). Usualmente não em cronologia (não é fato da disputa).
- A contraparte mandou notificação de mora em 14 de março → vai em chronology.md (🟡 — estabelece a ciência dela). Também em history.md se o intake referenciou.
- Nosso memo de recomendação de provisão foi redigido → só history.md.

Quando o(a) advogado(a) quer eventos do histórico na cronologia, pode colar. O default é ficarem separados.

## O que esta skill não faz

- **Resolve contradições.** Quando dois documentos dizem coisas diferentes sobre quando um evento aconteceu, ambas as entradas entram com flag. Resolução é chamada do(a) advogado(a); pode exigir entrevista de testemunha ou mais instrução probatória.
- **Inventa eventos que não estão nas fontes.** Se não está nos documentos (e não em matter.md ou na configuração como fato capturado), não está na cronologia — mas "Lacunas" pode apontar como ausente.
- **Garante completude.** Uma cronologia é só tão boa quanto as fontes. Se a produção da gestão documental está em andamento e só 20% aterrissou, a cronologia reflete. Nomeie a limitação.
- **Decide status de sigilo pelo usuário.** O gate do Passo 0 força a escolha de postura; o flag `priv` por entrada captura classificação de primeira passagem. Determinações reais de sigilo são chamadas do(a) advogado(a) per flags `[SME VERIFICAR]`.
