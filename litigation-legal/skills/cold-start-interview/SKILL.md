---
name: cold-start-interview
description: Cold-start do plugin de contencioso — bifurca por papel (Defensor Público, departamento jurídico, advogado em sociedade, advogado autônomo) e por posição processual (autor, réu, ambos), captura calibração de risco, panorama e estilo da casa, e escreve o perfil de atuação no CLAUDE.md. Use em instalação fresca, quando o usuário quiser rodar setup ou refazê-lo, ou para re-checar integrações disponíveis.
argument-hint: "[--redo | --check-integrations]"
---

# /cold-start-interview

1. Cheque `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md`. Se já populado e sem `--redo`, pergunte antes de sobrescrever.
2. Siga o workflow e a referência abaixo.
3. Rode a Parte 0 (papel, posição, checagem de integrações). A entrevista bifurca por papel e posição.
   - **Papel** roteia a estrutura do perfil de atuação: **defensor-público** (portfólio por vara da atribuição, intake do(a) assistido(a), teses repetitivas, escalonamento institucional ao Defensor Público-Geral), **departamento-jurídico** (portfólio de casos, supervisão de escritórios externos, metodologia de provisão CPC 25, reporte a diretoria/conselho), **advogado-em-sociedade** (trabalho de caso — contexto do caso, tese e fato-pivô, peça-semente em estilo da casa, instrução probatória e rol de sigilosos), ou **advogado-autônomo** (carteira pessoal + economia de honorários ad exitum ou retainer + expectativas do(a) cliente + prescrição, depois as seções de tese e estilo de peça).
   - **Posição** roteia o vocabulário de calibração: **autor** (afirmando, valor da causa, ad exitum, prescrição/decadência), **réu** (respondendo, exposição, provisões quando aplicável, aviso de sinistro), ou **ambos/varia** (captura default e deixa skills por-caso re-perguntar).

   Depois da Parte 0, percorra as seções que casam com o papel selecionado. Não rode o caminho de departamento-jurídico para advogado autônomo — provisões CPC 25 e memo para diretoria não são o frame certo para advocacia individual. Não rode o caminho corporativo para Defensor Público — CPC 25 / CVM / D&O não se aplicam; o equivalente é escalonamento institucional ao DPG. Ofereça defaults; capture overrides livres. Peça documentos-semente em cada seção (sem pressão; note que compartilhar afia toda skill downstream).
4. Surface lacunas. Se o(a) usuário(a) não tem framework articulado de risco ou limiar de reporte, anote e ofereça pensar agora ou deixar `[PLACEHOLDER]` para preencher depois.
5. Migração: se houver um CLAUDE.md populado (sem marcadores `[PLACEHOLDER]`) em `~/.claude/plugins/cache/claude-for-legal/litigation-legal/*/CLAUDE.md` mas não no caminho config, copie para o caminho config e mostre ao(à) usuário(a) o que foi migrado.
6. Escreva `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md`. Date o rodapé.
7. Confirme com o(a) usuário(a) antes de finalizar: "Aqui está o que capturei — algo errado?"

## Flags

- `--redo` — re-roda a entrevista completa e sobrescreve `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md`.
- `--check-integrations` — re-escaneia conectores MCP disponíveis e atualiza a tabela `## Integrações disponíveis` em `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md` sem rodar a entrevista completa. Use depois de configurar um conector novo (DataJud, tjam-jurisprudencia, JusRatio, Sapiens-DPGU, armazenamento documental, Gmail, agenda).

Quando sondando: só reporte ✓ se uma tool MCP efetivamente respondeu com sucesso. Conectores configurados-mas-não-testados devem ser marcados ⚪ com uma linha de "como confirmar". Nunca reporte ✓ baseado só em declarações no `.mcp.json` — isso engana usuários para acharem que algo está ligado quando não está.

---

# Entrevista Cold-Start: Contencioso

## Propósito

Todo intake de caso, toda construção de cronologia, toda redação de peça, todo rollup de status lê deste arquivo. Se o frame não está capturado, o plugin faz triagem mais fraca e o(a) usuário(a) tem que pensar do zero a cada vez. Esta entrevista preenche o frame uma vez para que tudo downstream fique mais afiado.

O plugin atende quatro papéis distintos no contencioso — **Defensor(a) Público(a)** atendendo unidade com várias varas, **departamento jurídico interno** gerenciando portfólio de casos, **advogado(a) em sociedade** redigindo peças e fazendo instrução, e **advogado(a) autônomo(a)** rodando carteira. O vocabulário é diferente para cada um, e a entrevista bifurca para casar. Defensores Públicos têm caminho próprio (portfólio por vara, intake do(a) assistido(a), teses repetitivas, escalonamento ao DPG) — não rodar o caminho corporativo neles. Advogados autônomos têm caminho dedicado (carteira pessoal, contratação por ad exitum ou retainer, expectativas do(a) cliente) mais as seções de tese e peça que aplicam a quem redige.

A entrevista também pergunta qual posição o(a) usuário(a) majoritariamente representa — autor (afirmando pretensão), réu (respondendo), ambos, ou varia por caso. Calibração de risco, postura de notificação extrajudicial, postura na instrução probatória e enquadramento da cronologia diferem por posição, e o perfil carrega o default para skills downstream não re-perguntarem toda vez.

**Tom:** socrático, não checklist. Se o(a) usuário(a) não tem framework escrito, esta costuma ser a coisa que força a articulação. Aproveite. Não pule lacunas rapidamente — nomeie, ofereça pensar, permita "deixar para depois".

## Checagem cold-start

Leia `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md`:
- **Não existe** → comece a entrevista.
- **Contém `<!-- SETUP PAUSED AT: -->`** → cumprimente e ofereça retomar daquela seção.
- **Contém marcadores `[PLACEHOLDER]` mas sem comentário de pausa** → o template nunca foi completado; ofereça começar do zero ou retomar de onde os placeholders começam.
- **Populado (sem placeholders, sem comentário de pausa)** → já configurado; pule salvo `--redo`.

A estrutura do template vive em `${CLAUDE_PLUGIN_ROOT}/CLAUDE.md` — use como scaffold de seção. Escreva o perfil completado no caminho config, criando diretórios-pai conforme necessário. Se um CLAUDE.md existe no caminho cache antigo `~/.claude/plugins/cache/claude-for-legal/litigation-legal/*/CLAUDE.md` mas não aqui, copie para frente.

## Checagem do perfil compartilhado da unidade/empresa

Procure `~/.claude/plugins/config/claude-for-legal/company-profile.md`.

- **Se existe:** Leia. Mostre confirmação em uma linha: "Você é [nome], [tipo de atuação], em [unidade/empresa/escritório], [área/setor], atuando em [jurisdições/varas]. Certo? (Ou diga 'atualizar' para mudar o perfil compartilhado.)" Se confirmado, pule as perguntas de empresa/unidade — vá direto para as específicas do plugin.
- **Se não existe:** Você será o primeiro plugin que esta pessoa configura. Depois da orientação e bifurcação, faça as perguntas de empresa/unidade e escreva no perfil compartilhado (per template em `references/company-profile-template.md` na raiz do plugin), depois continue com as perguntas específicas. Diga: "Salvei seu perfil — os outros plugins jurídicos vão ler e pular estas perguntas."

As perguntas que pertencem ao perfil compartilhado (e NÃO devem ser re-perguntadas se ele existe): tipo de atuação, nome, área principal, o-que-faz, porte, jurisdições, reguladores, apetite ao risco, nomes de escalonamento. As específicas do plugin (posições de playbook, framework de revisão, estilo da casa, modelo de supervisão, etc.) ficam por-plugin.

## Checagem de escopo de instalação

Antes da orientação, se notar que o diretório de trabalho é dentro de um projeto (não o home do(a) usuário(a)), sinalize. Diga uma vez:

> **Atenção — parece que este plugin pode estar em escopo de projeto, o que significa que eu só posso ler arquivos em [diretório atual]. Se você quiser que eu leia documentos de outros lugares (Downloads, Documentos, Dropbox), instale em escopo de usuário — vide QUICKSTART.md. Você pode continuar com escopo de projeto, mas vai precisar mover arquivos para esta pasta.**

Peça confirmação antes de prosseguir: continuar com escopo de projeto, ou pausar para reinstalar em escopo de usuário. Se o diretório de trabalho *é* o home do(a) usuário(a), pule esta checagem silenciosamente.

## Antes de começar a entrevista

Abra com o preâmbulo bifurcação-primeiro. Mantenha em 3-4 linhas curtas. Pergunte rápido-ou-completo antes de tudo.

> **`litigation-legal` é para quem trabalha com contencioso — Defensor Público atendendo unidade com várias varas, departamento jurídico interno gerenciando portfólio, advogado(a) em sociedade ou autônomo(a) rodando carteira.** Não é sua área? `/legal-builder-hub:related-skills-surfacer`.
>
> **2 minutos** te dá seu papel (defensor-público / departamento-jurídico / advogado-em-sociedade / autônomo), atuação, posição default (autor / réu) e quantidade de casos ativos, mais defaults razoáveis para calibração de risco, estilo de peça e convenções de sigilo. **15 minutos** adiciona suas bandas reais de severidade × probabilidade, escala institucional de escalonamento (Defensor) ou alçada de transação (DJ) ou economia de honorários (autônomo), bench de DPs colaboradoras/escritórios externos, estilo de peça extraído de peça-semente, formato de rol de sigilosos, templates de notificação extrajudicial/ofício, e notas de panorama.
>
> Rápido ou completo? (Pode atualizar a qualquer momento com `/cold-start-interview --full`.)

**Caminho de quick start:** pergunte só a Parte 0 (papel, atuação, integrações) e a bifurcação. Escreva a config com marcadores `[DEFAULT]` em tudo o resto. Feche com: "Pronto. Você já pode usar os comandos. Usei defaults razoáveis para calibração de risco, estilo da casa e scaffold de tese. Quando o output de uma skill parecer estranho, normalmente é um default que você deveria afinar — ela vai te dizer qual. Rode `/litigation-legal:cold-start-interview --full` a qualquer momento para a entrevista completa, ou `/litigation-legal:cold-start-interview --redo <seção>` para refazer uma parte."

**Caminho de full setup:** o fluxo abaixo. Depois do(a) usuário(a) escolher, dê a orientação mais completa descrita em seguida, e prossiga para a Parte 0.

## Depois do(a) usuário(a) escolher rápido ou completo

Dê a orientação mais completa. Um parágrafo, na sua voz:

> "Este plugin mantém: seu perfil de atuação (calibração de risco, convenções de sigilo, estilo da casa), um ledger de casos (`_log.yaml`), arquivos por caso (cronologia, comunicações de dever de guarda, históricos, rol de sigilosos), e um arquivo de produto de trabalho. Suporta trabalho de contencioso seja você Defensor(a) Público(a) atendendo unidade com várias varas, departamento jurídico gerenciando portfólio, advogado(a) em sociedade redigindo peças e fazendo instrução, ou autônomo(a) fazendo as duas coisas. Aprende qual papel você está, sua calibração de risco ou tese, seu panorama ou setup de instrução, suas convenções da casa, e escreve em arquivo de texto puro que o plugin lê toda vez. Tudo que você responder pode ser mudado depois."

Depois, a nota de perfil fresco:

> "O setup constrói um perfil profissional fresco das suas respostas. Não lê seu histórico pessoal do Claude, outras conversas, ou seu CLAUDE.md do home. Se eu notar informação relevante no contexto da nossa conversa — ex.: você mencionou sua unidade/empresa ou caso antes — vou perguntar antes de usar. Nada pessoal entra na sua configuração de atuação a menos que você digite ou aprove."

Depois: "Pronto? Algumas perguntas rápidas primeiro."

**Por que isso importa** (ofereça se houver resistência ao custo de tempo). Todo intake de caso, todo status de portfólio, toda redação de peça lê da configuração que esta entrevista escreve. Configuração genérica dá output genérico — matriz de risco default, estilo de citação default, formato de rol genérico. Dizer ao plugin as bandas reais de severidade, a escala real de escalonamento, a estrutura real da peça é o que faz a diferença entre "uma ferramenta de IA jurídica" e "uma ferramenta que tria e redige do jeito que você faz". Especialmente load-bearing: o fato-pivô (se em sociedade/autônomo) e os documentos-semente.

Construa o perfil de atuação só a partir das respostas digitadas e dos documentos que o(a) usuário(a) subir durante a entrevista. Não leia `~/CLAUDE.md` nem puxe fatos de atuação do contexto ambiente. Se algo relevante já está visível nesta conversa, pergunte antes de usar.

## Cadência da entrevista

- **Assuma que a resposta existe em algum lugar.** Quando uma pergunta pede informação que provavelmente está escrita em algum lugar — descrição da unidade/empresa, playbook, matriz de escalonamento, guia de estilo, regimento, lista de varas, portfólio de casos — peça um link ou paste antes de pedir para o(a) usuário(a) digitar de memória. "Cole um link ou doc, ou me dê a versão curta" é o pedido default para qualquer coisa que seja mais que uma frase. Um(a) entrevistador(a) que faz a pessoa re-digitar o que ela já escreveu falhou na primeira função de um(a) entrevistador(a).

**Pause para respostas reais.** Algumas perguntas têm respostas tap-through rápidas. Outras precisam que o(a) usuário(a) digite, descreva, ou suba um exemplar (memo de diretoria, template de dever de guarda, notificação extrajudicial, memo de risco, memo de tese, peça-semente). Quando uma pergunta precisa de mais que tap rápido:

- **Tamanho do batch — conte sub-partes.** "Nunca pergunte mais de 2-3 perguntas em um turno" significa 2-3 *prompts respondíveis*, contando sub-partes. Uma pergunta com 5 sub-partes é 5 perguntas. O teste: o(a) usuário(a) consegue responder sem rolar? Se as perguntas não cabem em uma tela, são muitas. Prefira perguntas estruturadas tap-through quando possível.
- **Faça a pergunta e espere.** Diga explicitamente: "Esta precisa de resposta digitada — vou esperar." Não vá para a próxima até a pessoa responder. Isso importa mais na seção de tese (caminho em sociedade/autônomo) — não parafraseie uma meia-resposta e empurre.
- **Para uploads de documento-semente:** "Cole o conteúdo, compartilhe um caminho de arquivo, ou diga 'pular por ora.' Se pular, vou sinalizar a lacuna no perfil para você preencher depois." Depois efetivamente espere.
- **Antes de escrever o perfil:** revise toda resposta capturada. Liste qualquer pergunta pulada, respondida com placeholder, ou que produziu contradição. Diga: "Antes de escrever seu perfil, eis o que ainda está aberto: [lista]. Quer preencher algum agora, ou deixar como placeholder?" Depois espere.
- **Nunca** escreva perfil com lacunas silenciosas. Todo `[PLACEHOLDER]` deve ser escolha deliberada do(a) usuário(a) de pular, não pergunta que rolou para fora da tela. O rodapé `DADOS LIMITADOS` é só para escassez de documentos-semente — não para perguntas que a entrevista nunca de fato fez.
- **Pause e retome.** Diga ao(à) usuário(a) de antemão: "Se precisar parar, diga 'pause' (ou 'pare', ou 'deixa pra depois') e eu salvo seu progresso. Rode `/litigation-legal:cold-start-interview` de novo depois e eu pego de onde paramos." Quando pausar, escreva configuração parcial com comentário `<!-- SETUP PAUSED AT: [nome da seção] — rode /litigation-legal:cold-start-interview para retomar -->` no topo e marcadores `[PENDING]` (distintos de `[PLACEHOLDER]`) em campos não respondidos. Quando setup rerodar e achar config pausada, cumprimente: "Bem-vindo(a) de volta. Você pausou em [seção]. Suas respostas anteriores estão salvas. Pegar de onde paramos, ou começar do zero?" Não re-pergunte perguntas já respondidas.

**Verifique fatos jurídicos declarados pelo(a) usuário(a) à medida que aparecem no setup.** Quando responder a pergunta com citação específica de regra, número de dispositivo, nome de caso, prazo, limiar, jurisdição ou número de registro — e for algo que você pode sanity-check — faça a checagem antes de escrever na configuração. Se o que disse conflita com seu entendimento ou com algo que coloou, surface: "Você disse que o prazo é X; meu entendimento é Y — pode confirmar qual vai no perfil? `[premissa marcada — verificar]`" Fato errado escrito no CLAUDE.md propaga em todo output futuro; pegar aqui é um dos momentos de maior alavancagem do produto.

## Parte 0: Quem está usando + roteamento de papel

### Quem está usando?

> Quem vai usar este plugin no dia-a-dia? (Isso alimenta o cabeçalho de sigilo em todo briefing de caso, cronologia, rol de sigilosos e minuta de notificação — outputs de Defensor recebem o cabeçalho LC 80/94 art. 4º XI + Lei 8.906/94 art. 7º XIX; outputs de advogado(a) habilitado(a) recebem o cabeçalho Lei 8.906/94 art. 7º XIX; outputs de não-advogado recebem o cabeçalho "notas de pesquisa, revisar com advogado(a)/Defensor(a)".)
>
> 1. **Defensor(a) Público(a) (membro de unidade)** — você atua institucionalmente na Defensoria. Volume alto, teses repetitivas, atribuição definida por resolução do CSDPGE local.
> 2. **Advogado(a) habilitado(a) ou Estagiário(a) de Direito inscrito(a) na OAB** — advocacia privada ou interna, atuando sob direção de quem é habilitado(a).
> 3. **Não-advogado com acesso a advogado(a)/Defensor(a)** — fundador(a), líder de negócio, manager de contratos, RH, compras; tem acesso a profissional habilitado(a) que pode consultar.
> 4. **Não-advogado sem acesso regular a profissional habilitado(a)** — você está lidando com isso por conta própria.

Se a resposta é 3 ou 4, diga isso uma vez (não repita em todo output):

> Você pode usar todas as features aqui — pesquisa, revisão, redação, acompanhamento. Duas coisas mudam em como eu trabalho:
>
> 1. **Vou enquadrar outputs como pesquisa para revisão por profissional habilitado(a), não como veredictos.** Em vez de "VERDE — assine", você vai receber "eis o que achei e eis as perguntas para fazer antes de assinar". Mais útil que sinal verde do qual você não pode ter certeza.
> 2. **Vou pausar antes de passos com consequência jurídica** — enviar notificação extrajudicial, responder a ofício, emitir ou liberar dever de guarda, protocolar peça, designar documentos como sigilosos, encerrar caso, aceitar acordo. Pergunto se você revisou com profissional habilitado(a), e monto briefing curto para a conversa ser rápida.
>
> Isso não é disclaimer. É o plugin sabendo a diferença entre o que ele faz bem — pesquisa, organização, estrutura — e juízo jurídico habilitado sobre sua situação específica, que ferramenta não dá. Algumas horas de profissional habilitado(a) no momento certo costumam ser mais baratas que o erro.

Se a resposta é 4, adicione:

> Se você precisa encontrar profissional habilitado(a) na sua jurisdição: a OAB Seccional do seu estado tem serviço de orientação inicial (Comissão de Assistência Judiciária Gratuita, em geral). Defensoria Pública estadual atende quem comprovar hipossuficiência. NPJ (Núcleo de Prática Jurídica) de faculdade local pode atender em certas áreas. Muitos serviços oferecem consulta inicial gratuita ou de baixo custo.

### Papel (a pergunta de bifurcação — pergunte cedo)

> **Como você trabalha com contencioso?** (Isso determina quais pilares da entrevista rodam — Defensor Público pega portfólio por vara + intake do(a) assistido(a) + teses repetitivas + escalonamento institucional ao DPG; departamento jurídico pega provisão CPC 25 e memo para diretoria; advogado(a) em sociedade pega tese de caso e peça-semente; autônomo(a) pega economia de carteira mais o trabalho de peça em sociedade. Também seta defaults para /matter-intake, /portfolio-status, /oc-status e o vocabulário de toda outra skill.)
>
> **(a) Defensor(a) Público(a) (membro de unidade)** — você atua institucionalmente em vara(s) da sua atribuição. Volume alto, assistidos(as) ao longo da semana, teses repetitivas dominantes (BPC/LOAS, saúde, consumidor, alimentos, posse). Escalonamento institucional ao Defensor Público-Geral em casos específicos.
>
> **(b) Em departamento jurídico interno gerenciando portfólio** — casos, escritórios externos, prazos, notificações, deveres de guarda. Você gerencia muitos casos ao mesmo tempo, a maioria conduzida por escritórios externos. Rollups de status e memos para diretoria fazem parte do seu trabalho.
>
> **(c) Em escritório fazendo redação de peças, instrução, preparação de oitiva, revisão documental** — você é o(a) advogado(a) em sociedade ou auxiliar responsável por de fato produzir o produto de trabalho. Um ou poucos casos, profundo em cada.
>
> **(d) Autônomo(a) / pequeno escritório rodando carteira** — você faz intake, tria, aconselha e redige. Sem sócio acima; sem camada de provisão / memo para diretoria. Economia é ad exitum ou retainer, não horas faturadas para grande cliente.
>
> **(e) Outro** — descreva em uma frase.

Registre a resposta na seção `## Papel na advocacia` do topo do perfil (`defensor-publico | departamento-juridico | advogado-em-sociedade | advogado-autonomo | outro`). Skills downstream leem isto para escolher defaults (ex.: modo de cronologia, quais comandos são primários, qual vocabulário usar).

**Regras de bifurcação para o resto da entrevista:**

- `defensor-publico` → rode o **caminho Defensor Público** (Pilares D1–D4 abaixo). Pule as seções de departamento jurídico (CPC 25, memo para diretoria) e as de autônomo (honorários ad exitum, carteira pessoal).
- `departamento-juridico` → rode o **caminho Departamento Jurídico** (Pilares 1–3 abaixo). Pule as seções de em-sociedade/autônomo e Defensor.
- `advogado-em-sociedade` → rode o **caminho Em Sociedade** (Partes A–D abaixo). Pule as seções de DJ, autônomo e Defensor.
- `advogado-autonomo` → rode o caminho dedicado **Autônomo** (Seções S1–S3 abaixo) — carteira, expectativas do(a) cliente, ad exitum ou retainer, gestão do escritório — **depois** rode o caminho Em Sociedade (Partes A–D) porque autônomos(as) também redigem. NÃO rode o caminho DJ.
- `outro` → peça descrição em uma frase, depois pegue o caminho mais próximo.

### Qual posição você majoritariamente representa?

Pergunte logo depois da pergunta de papel. Load-bearing para enquadramento de calibração de risco, postura de notificação, postura na instrução e jeito que cronologias são construídas.

> **Qual posição você majoritariamente representa?** (Isso alimenta /demand-draft, /demand-received, /subpoena-triage, /chronology e /claim-chart — enquadramento autor trata notificações como afirmações e instrução como ofensiva; enquadramento réu trata como recebidas e responsivas.)
>
> **(a) Autor / Requerente** — você deduz pretensão para pessoas ou empresas. Notificações extrajudiciais e ofícios são afirmações que você redige e envia. Instrução é ofensiva. Prescrição é um penhasco contra o qual você trabalha. Para Defensor: assistido(a) é o(a) autor(a) na grande maioria dos casos.
>
> **(b) Réu / Requerido** — você defende empresas ou indivíduos contra pretensão. Notificações são recebidas e triadas. Instrução é defensiva. Exposição é avaliada, provisionada (DJ corporativo), comunicada ao seguro (quando aplicável). Para Defensor: defesa em ação de cobrança, em despejo, em embargos à execução, ou em ação penal por escala.
>
> **(c) Ambos** — sua prática regularmente inclui as duas. Peça um default (autor ou réu); skills individuais perguntam por caso quando importa.
>
> **(d) Varia por caso** — sem default forte; toda skill pergunta por caso.

Registre em `## Posição processual` no perfil (`autor | réu | ambos [default autor/réu] | varia`). Regras de bifurcação para calibração que segue:

- **Autor:** calibração de risco é valor da causa, economia de honorários ad exitum (não se aplica a Defensor — sucumbência vai ao Fundo da DP), expectativas do(a) cliente/assistido(a), exposição à prescrição. Notificações são a afirmação. Instrução é ofensiva. Conversas de transação são com o(a) cliente/assistido(a), não com DJ/Conselho (salvo escalonamento institucional do Defensor ao DPG em hipóteses específicas). (Para em-sociedade autor: revisão do sócio substitui escalonamento DJ.)
- **Réu:** calibração de risco é exposição, provisões (DJ corporativo só), alçada de transação, cobertura de seguros. Notificações são recebidas e triadas. Instrução é defensiva — respondendo, asseverando sigilo, restringindo.
- **Ambos / varia:** entrevista captura o default e as skills (`demand-draft`, `subpoena-triage`, `matter-intake`, `chronology`, `claim-chart`) perguntam por caso quando a posição muda o output.

### Atuação

> Qual descreve melhor onde você está atuando?
>
> 1. **Defensoria Pública estadual ou federal**
> 2. **Núcleo de Prática Jurídica (NPJ)** (acadêmico, com supervisor[a] habilitado[a])
> 3. **Solo / pequeno escritório (2–10)**
> 4. **Médio porte**
> 5. **Grande banca / escritório consolidado**
> 6. **In-house** (departamento jurídico de empresa)
> 7. **Governo / Procuradoria** (estadual, municipal, federal)
> 8. **Outro**

Isso afina linguagem de escalonamento / supervisão no perfil:

- **Defensoria (1):** vocabulário institucional — assistido(a), atribuição por resolução, DPG, Conselho Superior, Corregedoria-Geral, núcleos especializados, DPs colaboradoras.
- **NPJ (2):** rote para o plugin `legal-clinic` — este aqui é para a banca/atuação habilitada, não para supervisão didática de estagiário(a). Se o(a) usuário(a) também é Defensor(a)-Supervisor(a) de estágio, configure os dois plugins separadamente.
- **Solo / pequeno sem hierarquia (3):** reenquadre perguntas de cadeia de autoridade como "quando você convida banca externa ou colega para segunda opinião". Escalonamento mapeia para *consulta* não *roteamento para aprovação*.
- **Médio / grande banca / in-house / governo (4, 5, 6, 7):** faça a cadeia completa de escalonamento, alçada de autoridade, tabela de contatos internos.
- **Outro (8):** peça descrição em uma frase, pegue o caminho mais próximo.

**Atuações que não cabem nas caixas.** Se sua atuação não casa (arbitragem internacional, direito público internacional, amicus-only, consultoria acadêmica, advogado(a) dativo(a) só, justiça militar, marítimo, ou qualquer outra coisa que as categorias padrão assumam que não existe), ofereça: "Parece que sua atuação não cabe nas minhas categorias usuais. Me conte na sua voz — o que você faz, para quem, em que jurisdições e foros, como é o trabalho — e eu vou construir seu perfil disso em vez de te forçar em caixas que não casam. Vou pular ou adaptar as perguntas que não se aplicam." Depois construa o perfil da descrição livre, marcando quais campos foram preenchidos, adaptados, ou deixados vazios porque não se aplicam. Perfil construído por encaixe forçado é pior que perfil esparso construído do que é efetivamente verdadeiro.

### O que está conectado?

> Este plugin trabalha com: sistema interno (Sapiens-DPGU para DPU, sistema próprio para DPEs, software de gestão para escritórios), armazenamento documental (Google Drive, SharePoint, Box), Gmail, agenda, MCPs de pesquisa jurídica brasileira (JusRatio proprietário com níveis A-E; e os 4 open-source do consulta-jurisprudencia-mcp — BNP/STF-STJ vinculantes, CJF/STF-STJ-TRFs, TJAM/e-SAJ, DataJud/CNJ 61 tribunais com cascata e-SAJ TJAM). Vou checar quais conectores você tem configurados — features que precisam deles vão funcionar, e features que não, vão cair graciosamente em fallback em vez de falhar silenciosamente.

**Cheque o que está efetivamente conectado, não o que está configurado.** Conector listado no `.mcp.json` está *disponível*. Conector que está efetivamente respondendo está *conectado*. São coisas diferentes, e confundir destrói confiança. Para cada conector que este plugin usa:

- Se você pode testar (chamar uma tool MCP simples como list ou search), reporte ✓ só em resposta bem-sucedida.
- Se não pode testar (sem jeito de sondar daqui), reporte ⚪ "configurado mas não verificado — abra suas configurações MCP para confirmar" com uma linha de como.
- Nunca reporte ✓ baseado só em configuração.

Para conectores que aparecem como não conectados, diga ao(à) usuário(a) como conectar. Frase exemplo: "O DataJud não está conectado. Você precisa: (1) clonar `https://github.com/eamamtd/consulta-jurisprudencia-mcp` localmente, (2) `pip install -r requirements.txt`, (3) obter chave gratuita do DataJud em https://datajud-wiki.cnj.jus.br/api-publica/acesso/, (4) exportar `CONSULTA_JURISPRUDENCIA_MCP_DIR=<path do clone>` e `DATAJUD_API_KEY=<chave>`. Este plugin funciona sem — o acompanhamento processual cai para manual no e-SAJ — mas com, o `docket-watcher` agent puxa as movimentações automaticamente."

Depois reporte achados nesta forma:

> - ✓ [Integração] — conectada (testada)
> - ⚪ [Integração] — configurada mas não verificada. Abra suas configurações MCP para confirmar.
> - ✗ [Integração] — não encontrada. [Feature] vai cair em [alternativa manual]. [Como conectar.]

Você não precisa de todas. Features core funcionam só com acesso a arquivo.

Escreva uma seção `## Papel na advocacia`, `## Quem está usando`, e `## Integrações disponíveis` na config do plugin imediatamente depois da abertura. Adicione `## Outputs` com a regra do cabeçalho de sigilo per o template do CLAUDE.md.

---

## Caminho Defensor Público (papel == `defensor-publico`)

*Pule esta seção inteira se o papel é `departamento-juridico`, `advogado-em-sociedade` ou `advogado-autonomo`.*

> Quero capturar o frame contra o qual você tria os casos da unidade — atribuição, varas, teses repetitivas, escalonamento institucional, estilo de peça. Uma vez, para que todo intake de assistido(a) leia daqui. Vou oferecer defaults onde houver razoáveis. Você aceita, edita, ou deixa em branco para voltar depois.
>
> Vou pedir documentos-semente ao longo — peças exemplares de petição inicial JEC, contestação, recurso inominado, ofício de DP, formulário social usado no intake do(a) assistido(a). Dez a vinte total ao longo da entrevista é o alvo. Abaixo de dez, vou marcar o perfil como DADOS LIMITADOS no rodapé — skills ainda vão rodar, mas outputs mais finos. Templates-primeiro: se você subir exemplar, eu leio e pergunto só sobre lacunas em vez de andar a estrutura completa.

### Pilar D1 — Perfil da unidade

Contexto institucional. Se outro plugin `-legal` já tem bloco `## Perfil da unidade` populado, copie em vez de re-perguntar.

- Unidade (ex.: "DPEAM — 4ª DP dos JECs + 17ª e 34ª DPs Cíveis")
- Resolução de criação / atribuição (ex.: "Resolução 004/2019 DPEAM")
- Capital / interior (relevante para escala e cobertura)
- Defensor(a) titular + substituto(a) / suplência
- Quantidade de Defensores(as) na unidade
- Estagiários(as) sob supervisão (se houver — sinaliza para também rodar `legal-clinic`)
- Sistema interno (Sapiens-DPGU / sistema próprio AM / outro)

### Pilar D2 — Varas atendidas e atribuição

> Quais varas você atende, sob qual rito, e qual a cadência típica de audiência?

| Vara | Competência (Lei 9.099/95 ou CPC) | Tipo | Cadência típica de audiência |
|---|---|---|---|
| (preencher) | (JEC até 40 SM / Cível comum) | (JEC / Cível Comum / Família / Sucessões) | (semanal / quinzenal) |

Para Defensor cível comum (CPC 2015):
- Audiência de conciliação CPC 334 obrigatória salvo dispensa expressa de ambos
- Prazos em dias úteis CPC 219 + suspensão CPC 220 (20/12-20/1)
- Prazo em dobro Defensor CPC 186

Para Defensor JEC (Lei 9.099/95):
- Audiência de conciliação primeiro, depois instrução e julgamento (se contestar)
- Prazos em dias corridos (jurisprudência STJ; Lei 9.099 art. 12-A)
- Valor de alçada 40 SM; ius postulandi até 20 SM (Lei 9.099 art. 9º)
- Sentença com fundamentação sucinta admitida

### Pilar D3 — Calibração de risco humanitária

> Diferente do DJ corporativo (provisão CPC 25 / divulgação CVM) e do autônomo (valoração de causa), a calibração de risco do Defensor é majoritariamente humanitária. O frame:

**Apetite (1 min)** — qual a postura geral da unidade? Defensa de teses repetitivas até judicialização? Conciliação maximizada para reduzir caseload? Priorização de tutela de urgência?

**Bandas de severidade humanitária (3 min):**
- **Alta:** risco imediato à vida/saúde (negativa de medicamento essencial, leito UTI, internação), despejo iminente de família com criança, violência doméstica em curso, prescrição em 30 dias para tese principal, BPC/LOAS negado a idoso(a) ou pessoa com deficiência sem outra fonte
- **Média:** risco humanitário relevante mas não imediato (cobrança indevida cíclica, vício de produto durável, alimentos atrasados sem urgência, conflito de guarda sem violência, prescrição em 6 meses)
- **Baixa:** matéria patrimonial menor sem urgência, divergência negocial recuperável por mediação

**Escalonamento institucional (2 min)** — pergunte direto:

> Quando um caso pede algo acima da sua autoridade — tese inédita com impacto coletivo, acordo que renuncia parcela material do direito do(a) assistido(a), Termo de Ajustamento de Conduta (TAC), Ação Civil Pública — para quem vai?
>
> - Defensor(a) Coordenador(a) da área?
> - Defensor(a) Público(a)-Geral (LC 80/94 art. 8º)?
> - Conselho Superior da Defensoria?
> - Núcleo Especializado (Saúde, Idoso, Mulher, etc.)?

**Vedações institucionais (LC 80/94 art. 46)** — confirme: você não exerce advocacia privada, não recebe honorário por advocacia paralela, não emite parecer remunerado para parte privada, não exerce atividade político-partidária? Marca para confirmar antes de aprovar intake (impedimento por LC 80/94 art. 134).

### Pilar D4 — Estilo da casa Defensoria

> Antes das perguntas estruturadas: você tem manual interno da unidade, template de petição inicial JEC, template de contestação, ofício-modelo da DP, formulário social do intake do(a) assistido(a)? Cole o conteúdo, compartilhe caminhos, ou diga 'não' e eu vou pergunta a pergunta.

Se não:

- **Petição inicial JEC** (Lei 9.099/95 simplificada) — formato, tom (objetivo + sucinto), pedidos cumuláveis típicos. *Doc-semente:* petição inicial JEC anonimizada exemplar.
- **Petição inicial Comum** (CPC 319) — formato, requisitos do art. 319 + pedido de tutela de urgência CPC 300 quando aplicável, gratuidade CPC 98. *Doc-semente:* petição inicial cível comum.
- **Contestação** — formato, organização (preliminares + mérito), pedido contraposto se cabível. *Doc-semente:* contestação exemplar.
- **Recurso inominado** (JEC) ou **apelação** (Cível) ou **agravo** — quando usar cada, formato, prazos. *Docs-semente:* uma de cada.
- **Ofício institucional da DP** — formato, papel timbrado, signatário. *Doc-semente:* ofício para órgão administrativo (concessionária, hospital, secretaria).
- **Convenções de sigilo** — cabeçalho LC 80/94 art. 4º-A V + Lei 8.906/94 art. 7º XIX. Sigilo do(a) assistido(a) é regra; segredo de justiça CPC 189 quando cabível.
- **Padrão de citação** — padrão CNJ + ABNT NBR 6023/10520. Citação de jurisprudência: "STJ, REsp [número], Rel. Min. [nome], j. [data], DJe [data]".

**Oferta:** "Se você não subiu manual da unidade ou templates, quer que eu escreva suas regras de estilo da casa como memo separado para você compartilhar com a equipe / coordenação?"

---

## Caminho Departamento Jurídico (papel == `departamento-juridico`)

*Pule esta seção inteira se o papel é `defensor-publico`, `advogado-em-sociedade` ou `advogado-autonomo`.*

> Quero capturar o frame contra o qual você tria casos — calibração de risco, panorama do contencioso, e estilo de escrita. Uma vez, para que todo intake leia daqui. Vou oferecer defaults onde houver razoáveis. Você aceita, edita, ou deixa em branco.
>
> Vou pedir documentos-semente — memos antigos para diretoria, memos de provisão, templates de dever de guarda, notificações exemplares, memo de risco. Dez a vinte total. Abaixo de dez, sinalizo DADOS LIMITADOS.

### Pilar 0 — Perfil da empresa

Se outro plugin `-legal` tem bloco populado, copie.

- Pessoa jurídica / razão social
- Setor de atuação
- Capital aberto / fechado / subsidiária
- Status regulatório (CVM / ANS / ANPD / Bacen / RFB / etc.)
- Jurisdições principais
- Headcount + tamanho do DJ
- Contatos-chave (DJ, CFO, RH, Comunicação, CISO, Conselho)
- Seu nome e linha de reporte

### Pilar 1 — Calibração de risco

> Antes das perguntas estruturadas: você tem memo de calibração de risco existente, política de provisão (CPC 25), ou diretrizes de billing para escritório externo que eu possa ler? Cole conteúdo, compartilhe caminhos, ou diga 'não' e eu vou pilar pergunta a pergunta. Se compartilhar um, eu extraio as bandas, limiares e alçada e pergunto só sobre lacunas.

Se não:

**Apetite (2 min)** — em uma frase, como a empresa aborda contencioso? (Alimenta /matter-briefing e /portfolio-status — seta conservadorismo ou agressividade em todo briefing.)

**Severidade × probabilidade (3-5 min)** — ofereça matriz 3×3 default. Bandas de severidade (em R$ e não-monetárias). Bandas de probabilidade. Se não articulado: "Justo. Muitos advogados não têm. Quer esboçar agora, ou deixar o default?"

**Limiares de materialidade (2-3 min)** — gatilho de provisão CPC 25, gatilho de divulgação Formulário de Referência CVM (se aplicável), memo para diretoria/conselho, escalonamento só ao DJ. *Doc-semente:* template de memo de provisão.

**Alçada de transação (1-2 min)** — escala em R$, exceções estruturais.

**Escalonamento em português claro (1 min):**

> Quando um caso pede algo acima da sua autoridade — acordo acima da banda, notificação que você não pode responder sozinho(a), decisão de dever de guarda que precisa do DJ — para quem vai? Me dê nome, função, ou "eu decido sozinho(a)".

**Perfil de seguros (1-2 min)** — linhas em vigor (D&O, RC Profissional, Cyber, RC Geral), seguradoras, limites, franquias, protocolo de aviso de sinistro.

**Oferta:** "Se não subiu memo de calibração, quer que eu escreva sua calibração e cadeia de autoridade como memo standalone para compartilhar e manter?"

### Pilar 2 — Panorama

- Contexto de negócio (30s) — parágrafo único sobre o que fazemos e por que somos demandados.
- Padrões de demanda (2-3 min) — tipos, frequência, posição.
- Contrapartes frequentes (1-2 min).
- Bench de escritórios externos (2-3 min) — escritórios, sócios líderes, tipo de matéria, postura de honorários, contrato. *Doc-semente:* diretrizes ao escritório externo. (Alimenta /oc-status — redige status semanal a esses escritórios.)
- Foros frequentes (30s).
- Armazenamento documental (2-3 min) — onde vivem docs (filesystem, Drive, SharePoint, Box, Gmail, CLM, eDiscovery), padrão de pasta por caso, como compartilha com escritório externo.
- Checagem de conflitos (1-2 min) — como vocês rodam; quem; bloqueio duro ou paralelo.

### Pilar 3 — Estilo da casa

> Antes das perguntas estruturadas: você tem guia de estilo da casa, template de memo para diretoria, template de dever de guarda, ou notificações exemplares? Cole / compartilhe / 'não'.

Se não:
- Memo para diretoria/conselho — formato, tom, cadência. *Doc-semente:* memo recente (anonimizado).
- Memo de provisão — formato e aprovador. *Doc-semente:* memo exemplar.
- Diretrizes ao escritório externo — formato de e-mail, cadência, postura orçamentária.
- Convenções de sigilo — marcação; postura default em chamadas subjetivas (marcar e flag); mecânica de revisão.
- Dever de guarda — template, protocolo de emissão, cadência de renovação. *Doc-semente:* template.
- Escalonamento — canal, convenção de assunto.
- Notificação extrajudicial — *não perguntar aqui.* Postura por caso, não por prática. `/litigation-legal:demand-intake` e `/litigation-legal:demand-draft` perguntam quando precisam. O que cabe aqui: timing de aviso de sinistro ao seguro, e limiar de materialidade para criação de caso.

**Oferta:** "Se não subiu guia de estilo, quer que eu escreva suas regras como memo standalone?"

---

## Caminho Autônomo (papel == `advogado-autonomo`)

*Pule esta seção inteira se o papel é `defensor-publico`, `departamento-juridico` ou `advogado-em-sociedade`. Autônomos(as) rodam este caminho **e** o Em Sociedade que segue.*

> Advocacia autônoma é seu próprio frame — carteira, expectativas do(a) cliente, economia ad exitum ou contratual, gestão do escritório. O mundo corporativo (CPC 25, memo para diretoria, oversight de escritório externo, alçada até DJ) não se aplica, e eu não vou fingir que sim. As perguntas em-sociedade de provisão também não. O que eu preciso é o formato da sua carteira efetiva e como você roda a prática.
>
> Alguns documentos-semente ajudam — notificação anterior, contrato de honorários, e-mail de atualização ao(à) cliente. Tudo que conseguirmos aprender poupa volta depois.

### Seção S1 — Formato da prática e carteira

- **Tamanho da carteira** — aproximadamente quantos casos ativos você carrega ao mesmo tempo? O que é demais?
- **Mix de casos** — percentual aproximado: autor vs. réu, áreas (ex.: trabalhista, família, consumidor, empresarial pequeno porte, locação). Não precisa ser preciso; uma frase.
- **Jurisdições** — UF e juízos onde você primariamente atua. Inclua federal se relevante.
- **Duração típica do caso** — semanas, meses, anos? Útil para skills downstream escalarem esforço e horizonte de prazos.
- **Flags de capacidade** — há um ponto onde você para de aceitar casos? Como você sabe que está acima da capacidade?

### Seção S2 — Expectativas do(a) cliente e economia

*Isso substitui o que o caminho DJ chama de "calibração de risco / metodologia de provisão / alçada de transação". Autônomos(as) não rodam provisão e não escalam para DJ; as mesmas decisões aparecem como economia voltada ao(à) cliente.*

**Estrutura de honorários (o principal driver).** Pegue a que casa com a maioria do seu trabalho:

- **Ad exitum / contingenciado** (default em civil para autor — danos, trabalhista, consumidor): qual seu percentual padrão? Pré-suit vs. pós-suit? Postura de custas — cliente, escritório, híbrido? Em qual exposição você para de aceitar caso em ad exitum? Lembrar: Código de Ética OAB art. 38 limita; quota litis pura (>50% do proveito) é vedada.
- **Hora / retainer**: valor-hora, retainer padrão, mecânica de conta-vinculada (se houver).
- **Fixo**: tipos de caso, faixa de valor.
- **Misto**: descreva.

**Expectativas do(a) cliente (2 min).** Pergunte direto:

- Frequência com que atualiza o(a) cliente (semanal, mensal, por evento)?
- Forma da atualização — telefone, e-mail, carta, portal?
- Postura default em conversas de transação com o(a) cliente (impulso forte para acordo, deixar cliente liderar, depende do caso)?

**Leitura de valor da causa (autor).** Qual seu framework mental rápido para decidir se um caso vale a pena? Exemplos: "responsabilidade clara, danos > R$ 50K, prescrição com 1 ano ou mais, cliente crível" — sem julgamento sobre as específicas; só capturar a sua.

**Leitura de exposição (réu — menos comum mas possível).** Qual seu modelo mental de exposição aceitável vs. reportável ao(à) cliente?

**Quando você liga pedindo ajuda.** Autônomos(as) não têm DJ ou sócio acima, mas a maioria tem alguém — co-counsel, mentor, listserv local, comissão de OAB. Quem você liga para segunda opinião, e em que tipo de matéria?

> Me dê nome, função, ou "ninguém — decido por conta própria."

**Atualizações ao(à) cliente por escrito (1 min).** *Doc-semente:* e-mail ou carta de atualização recente (anonimizada). Isto é o equivalente autônomo do memo para diretoria — é como você comunica status ao(à) seu(sua) stakeholder.

### Seção S3 — Gestão do escritório e panorama

*Pule qualquer pergunta onde a resposta é óbvia do contexto anterior.*

- **Controle de prescrição** — como você acompanha cortes de prescrição na carteira? (Calendário, software de gestão, agenda em papel, memória — o que for real.) Equivalente autônomo do "materialidade / gatilho de provisão" DJ, porque perder prescrição é o failure mode que encerra carreira.
- **Software de gestão** — LawDesk, Themis, Projuris, ADVBOX, Astrea, Tikal Tech, arquivo em papel, planilha, outro.
- **Armazenamento documental** — Google Drive, Dropbox, OneDrive, filesystem local, storage do software de gestão.
- **Foros frequentes** — juízos onde você efetivamente comparece.
- **Contrapartes / banca contrária frequentes** — repeat players que você regularmente vê do outro lado.
- **Bench de co-counsel / banca de referência** — quem você associa para casos fora da sua área? Quem refere para você?
- **Checagem de conflitos** — como você roda? A versão autônoma costuma ser informal (memória + lista de clientes), tudo bem — capture o que é. Base normativa: EAOAB art. 17 + Código de Ética OAB arts. 19-21.

### Estilo da casa autônomo

Pule as perguntas de memo para diretoria / memo de provisão / diretrizes ao escritório externo. Estilo da casa autônomo é:

- **Atualização ao(à) cliente** — formato, tom, cadência. *Doc-semente:* carta ou e-mail recente.
- **Contrato de honorários** — template. *Doc-semente:* exemplar (anonimizado).
- **Convenções de sigilo** — marcação; mecânica de revisão.
- **Dever de guarda** — mesmo para autônomo, preservação importa quando litigação é antecipada. Template, se houver.
- **Notificação extrajudicial** — *não perguntar aqui.* Postura por caso.

**Oferta:** "Se não subiu exemplar de atualização ou contrato, quer que eu escreva suas regras de estilo como memo reutilizável?"

Depois da Seção S3, continue para o **Caminho Em Sociedade** abaixo. Autônomos(as) redigem peças, constroem cronologias, e preparam oitivas como em-sociedade.

---

## Caminho Em Sociedade (papel == `advogado-em-sociedade` ou `advogado-autonomo`)

> Antes de tocar em documento, eu preciso da tese. Qual nossa história? Qual a deles? Em que o caso pivota? Depois eu preciso ver como seu escritório escreve — uma peça que você está orgulhoso — para minhas minutas não parecerem que vieram de outro lugar.

### Parte A: O caso (2 min)

- Nome do caso, cliente, número CNJ, juízo
- Nossa posição (autor / réu)
- Sócio(a) e advogado(a) sênior (pule se autônomo / pequeno sem hierarquia)
- Fase (postulatória, instrução, fase decisória, recurso)
- Datas-chave próximas

### Parte B: A tese — isso é tudo (3-4 min)

> Me conte a tese do caso. Não a petição — a história. Se você tivesse que contar a um(a) juiz(a) em duas frases por que ganhamos, quais são?

- Nossa tese em um parágrafo
- A tese deles em um parágrafo (saber o outro lado)
- **O fato-pivô** — o fato no qual o caso pivota
- Fatos-chave a favor de nós
- Fatos-chave contra nós (os que te preocupam)
- A questão jurídica que mais importa

### Parte C: Documentos-semente (3-4 min)

> Duas coisas:
>
> 1. **O memo de tese**, se existir. Se a tese vive na cabeça de alguém e não no papel, tudo bem — acabamos de capturar acima.
>
> 2. **Uma peça anterior em estilo da casa.** Não deste caso — qualquer caso. A melhor que você tem. Eu vou aprender seu padrão de citação, estrutura, tom, organização de argumentos. (Alimenta /brief-section-drafter — toda seção futura é redigida no seu padrão de citação extraído, estrutura de cabeçalho e tom, não em template genérico.)

**Da peça:** padrão de citação (padrão CNJ + ABNT NBR 6023/10520, ou misto por tipo de peça), estrutura de seções, convenções de cabeçalho, tom (incisivo / mensurado), normas de extensão.

### Parte D: Setup de instrução probatória (1-2 min)

> Antes das perguntas: você tem formato de rol de sigilosos, formato de cronologia, ou doc de protocolo de revisão? Cole / compartilhe / 'não'.

Se não:
- Plataforma de gestão documental (LegalDesk, Themis, Projuris, Astrea, ADVBOX, Tikal Tech)
- Protocolo de revisão — categorias de codificação, quem decide sigilo
- Formato de rol de sigilosos
- Custodiantes-chave e faixa de datas

**Oferta:** "Se não subiu formato de rol ou cronologia, quer que eu escreva seu protocolo de revisão e formato como referência standalone para compartilhar com equipe?"

---

## Antes de escrever — re-leia

Antes de comitar a config do plugin, re-leia toda resposta capturada em ordem. Pega três categorias de erro:

1. **Contradições entre respostas** — ex.: usuário(a) disse "litiga tudo" em apetite e "transaciona rápido" em default de notificação. Surface ambas, peça qual governa.
2. **Específicos que derivaram** — nomes, datas, limiares que mudaram entre seções. Confirme o valor final.
3. **Lacunas puladas que merecem ser nomeadas** — seções deixadas em branco que o(a) usuário(a) talvez queira completar agora em vez de via `--redo`.

Também: se o papel é `advogado-em-sociedade`, confira se o fato-pivô e a peça-semente foram capturados. Eles são load-bearing. Se algum está ausente, nomeie explicitamente antes de escrever.

## Escrevendo o perfil de atuação

Escreva o perfil completado na config do plugin, usando o template em `${CLAUDE_PLUGIN_ROOT}/CLAUDE.md` como scaffold de seção. Preencha toda seção capturada; deixe `[PLACEHOLDER]` para seções puladas. Date o rodapé.

**Gating de seção por papel:**

- `defensor-publico` → estrutura Defensor (Perfil da unidade D1, Varas atendidas D2, Calibração humanitária D3, Estilo da casa D4 com peças JEC/Comum/recurso/ofício). Omita seções de CPC 25 / CVM / D&O.
- `departamento-juridico` → estrutura DJ corporativa completa (Perfil da empresa, Calibração de risco com CPC 25 / provisão / memo para diretoria, Bench de escritórios externos). Omita ou marque N/A seções autônomo-only.
- `advogado-em-sociedade` → estrutura em-sociedade (tese de caso, fato-pivô, revisão por sócio, peça-semente). Omita seções de provisão / memo para diretoria / CPC 25; omita seções autônomo.
- `advogado-autonomo` → estrutura autônomo (carteira, estrutura de honorários, expectativas do(a) cliente, prescrição, retainer ou ad exitum, gestão do escritório) **mais** as seções em-sociedade (tese, peça-semente). Omita seções DJ / CPC 25 / memo para diretoria / alçada-até-DJ — não são frame certo, e incluir como placeholder adiciona ruído.

Onde uma seção do template carrega vocabulário DJ-only ("provisões CPC 25", "memo para diretoria / conselho"), ou omita a seção para papéis não-DJ ou traduza o vocabulário no equivalente autônomo ou em-sociedade. Equivalente autônomo de "memo para diretoria" é "carta de atualização ao(à) cliente". Equivalente autônomo de "metodologia de provisão" é "leitura de valor da causa" (autor) ou "leitura de exposição" (réu). Para Defensor, "memo para diretoria" não tem equivalente direto — é comunicação ao Defensor Público-Geral em casos específicos da LC 80/94 art. 8º. Não carregue linguagem de norma contábil em perfil autônomo ou Defensor.

**Flag DADOS LIMITADOS:** se menos de 10 documentos-semente foram compartilhados, adicione nota `> DADOS LIMITADOS` no topo (sob a data): "Este perfil de atuação foi escrito de [N] documentos-semente e respostas da entrevista. Skills downstream vão rodar mas outputs serão mais finos até que mais exemplares sejam adicionados. Re-rode `/cold-start-interview --redo` depois de coletar mais templates para afiar calibração."

## Surface de lacunas

Depois da entrevista, antes de escrever, sumarize e **espere uma resposta**:

> Aqui está o que capturei. Lacunas que notei:
> - [lista de seções puladas, placeholders deixados, perguntas onde a pessoa disse "deixa pra depois"]
>
> Quer preencher alguma agora, ou deixar como placeholder? Você também pode preencher depois via `/litigation-legal:cold-start-interview --redo` ou editando direto a config do plugin. Esta vale a pena pensar antes de eu escrever: [nomeie a lacuna mais importante e o porquê].

Não prossiga para escrever até a pessoa responder.

## Depois de escrever

**Mostre o que este plugin pode fazer.** Antes de fechar, ofereça:

> **Quer ver com o que eu posso ajudar?**

Se sim, mostre esta lista calibrada (não template genérico — são as coisas concretas que este plugin faz melhor):

> **No que eu sou bom em prática de contencioso:**
>
> - **Fazer intake de um caso novo** — ex.: "Perguntas uniformes de intake, escreve matter.md + history.md, anexa no log do portfólio. Para Defensor: formulário social do(a) assistido(a), hipossuficiência presumida (Súmula 481 STJ), urgência humanitária." Try: `/litigation-legal:matter-intake`
> - **Triar notificação recebida** — ex.: "Análise de opções, cross-check com portfólio, handoff para criação de caso se graduar." Try: `/litigation-legal:demand-received`
> - **Redigir notificação extrajudicial / ofício** — ex.: "Gate de confidencialidade negocial (Lei 13.140/15 art. 30), gera .docx, checklist pós-envio, oferta de criação de caso. Para Defensor: ofício institucional ou notificação extrajudicial com timbre da DP." Try: `/litigation-legal:demand-draft`
> - **Construir roteiro de oitiva (AIJ)** — ex.: "Docs + tópicos + impugnação + exibições, amarrado à tese." Try: `/litigation-legal:deposition-prep`
> - **Emitir ou renovar dever de guarda** — ex.: "Redigir memo de dever de guarda, atualizar o log, agendar renovação." Try: `/litigation-legal:legal-hold`
> - **Rollup de portfólio** — ex.: "Distribuição de risco, prazos próximos (CPC 219 dias úteis), casos parados, audiências agendadas no portfólio ativo. Para Defensor: por vara da atribuição." Try: `/litigation-legal:portfolio-status`
>
> **Minha sugestão para sua primeira:** Rode `/portfolio-status` — mostra rapidamente onde o portfólio está, e custa zero input. Ou me diga o que está na sua mesa e eu escolho.

Isso resolve o problema cold-start (a pessoa não sabe o que fazer primeiro) e o problema de proposta-de-valor (não sabe o que o plugin pode fazer) em uma oferta. Faça a lista específica. Pule este passo se a pessoa já nomeou tarefa concreta durante a entrevista.


- Se `defensor-publico`: "O perfil Defensor está escrito — atribuição, varas, calibração humanitária, escalonamento institucional. Todo intake do(a) assistido(a) vai ler daqui. Quer rodar `/litigation-legal:matter-intake` no(a) primeiro(a) assistido(a) que aparecer na escala para ver?"
- Se `departamento-juridico`: "O perfil DJ corporativo está escrito. Todo intake vai ler daqui. Quer rodar `/litigation-legal:matter-intake` em seu caso mais ativo para ver em ação?"
- Se `advogado-em-sociedade`: "Aqui está a tese como eu capturei. Leia o fato-pivô — peguei certo? Qual o próximo prazo? Vamos começar daí."
- Se `advogado-autonomo`: "Seu perfil autônomo está escrito — formato da carteira, economia de honorários, como você roda o escritório — mais o trabalho de tese e estilo para um caso ativo. Quer rodar `/litigation-legal:matter-intake` no seu caso mais ativo?"

### Feche com nota "você pode mudar tudo depois"

> "Seu perfil de atuação está em `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md` — arquivo de texto puro que você lê e edita diretamente. Tudo que você respondeu pode ser mudado:
>
> - Edite o arquivo diretamente para mudança rápida
> - Rode `/litigation-legal:cold-start-interview --redo` para re-entrevista completa
> - Rode `/litigation-legal:cold-start-interview --new-matter` para reusar o perfil em um caso novo (em-sociedade / autônomo)
> - Rode `/litigation-legal:cold-start-interview --check-integrations` para re-checar o que está conectado
>
> As seções que pessoas mais ajustam: para Defensor, as **bandas de severidade humanitária** e os **núcleos especializados de referência**; para DJ, os **limiares de severidade × probabilidade** e o **bench de escritórios externos**; para em-sociedade, a **tese do caso** (especialmente o fato-pivô) e o **estilo de peça da casa** extraído da peça-semente; para autônomo, a **estrutura de honorários** (percentual ad exitum ou valor-hora) e a **posição default** (autor / réu) — default errado aí enviesa todo output de notificação e cronologia. Quando output parece estranho, a correção costuma estar aqui."

### Antes do seu primeiro caso

**Conecte um MCP de pesquisa.** Sem um, eu vou marcar toda citação como não verificada — com um, eu verifico contra base atual. Para Defensor / BR: rode `pip install -r requirements.txt` no clone do `consulta-jurisprudencia-mcp` e exporte as env vars; JusRatio (proprietary) também ajuda com níveis A-E de autoridade.

<!-- COLLATERAL LINKS: quando colateral de onboarding existir, adicione:
     "Quer um walkthrough? [Assista ao intro de 3 minutos](URL) ou [leia o getting-started guide](URL)." -->

### Seu perfil aprende

Depois de escrever o perfil, feche com esta nota:

> **Seu perfil de atuação aprende.** Melhora à medida que você usa os plugins:
>
> - Quando o output de uma skill parecer estranho, normalmente é posição a afinar. O output vai te dizer qual.
> - Você sempre pode dizer "atualize meu playbook para preferir X" ou "mude meu limiar de escalonamento para Y" e a skill relevante escreve a mudança.
> - Rode `/cold-start-interview --redo <seção>` para re-entrevista de uma parte, ou edite a config diretamente.
>
> Dez minutos de setup te dá perfil funcional. Um mês de uso te dá um que lê como se você tivesse escrito você mesmo(a).

## O que esta skill NÃO faz

- Decidir o framework pelo(a) usuário(a). Defaults são pontos de partida; o juízo da pessoa é o conteúdo efetivo.
- Fingir que lacunas não estão lá. Melhor deixar `[PLACEHOLDER]` honesto que inventar limiar.
- Brigar com o(a) usuário(a). Se diz "ainda não tenho isso", anote e siga.
- Ler `~/CLAUDE.md` pessoal ou contexto ambiente sem perguntar.
