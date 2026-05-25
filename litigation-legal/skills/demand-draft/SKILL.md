---
name: demand-draft
description: >
  Redige notificação extrajudicial (ou ofício institucional da DP) a partir
  de intake completado, com gate de sigilo, confidencialidade negocial
  (Lei 13.140/15 art. 30), renúncia, confissão, output .docx, checklist
  pós-envio, e oferta de criação de caso. Use quando disser redigir a
  notificação, escrever o ofício, redigir cessar-e-desistir, ou tiver
  intake completo pronto para virar minuta enviável. Para Defensor —
  ofício institucional ao órgão administrativo (concessionária, secretaria,
  hospital) ou notificação extrajudicial do(a) assistido(a) contra
  contraparte privada.
argument-hint: "[slug] [--skip-gate] [--version=N]"
---

# /demand-draft

1. Carregue `~/.claude/plugins/config/claude-for-legal/litigation-legal/demand-letters/[slug]/intake.md`. Recuse se ausente ou bloco estratégico vazio (para notificações materiais).
2. Carregue `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md` → prática de notificação extrajudicial, estilo da casa, tabela de docs-semente.
3. Siga o workflow e referência abaixo.
4. Rode o gate pré-redação: filtro de sigilo, risco de confissão, satisfação inadvertida, postura de confidencialidade negocial, scan de renúncia, tom, precisão factual.
5. Seleção de template: doc-semente se fornecido em `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md`; senão template suave para o tipo de notificação.
6. Minuta em chat para revisão. Itere até aprovação do(a) usuário(a).
7. Escreva `~/.claude/plugins/config/claude-for-legal/litigation-legal/demand-letters/[slug]/draft-v[N].docx` usando a skill docx.
8. Escreva `~/.claude/plugins/config/claude-for-legal/litigation-legal/demand-letters/[slug]/checklist.md` (checklist pós-envio).
9. Avalie materialidade por heurística; ofereça criar caso. Se sim: handoff para `matter-intake` com campos pré-populados.

---

# Redação de Notificação Extrajudicial / Ofício

## Propósito

Pegue intake completado e produza minuta enviável. Maior parte do valor está em recusar redigir até sigilo, renúncia, confissão e postura de confidencialidade negocial terem sido conscientemente endereçados — o failure mode é uma notificação que quebra sigilo ou constitui confissão porque ninguém pausou para checar.

## Quatro tipos principais no Brasil

1. **Notificação extrajudicial cartorial** — registrada em Tabelionato de Notas, com fé pública. Modalidade tradicional para constituir em mora, interpelar, cessar-e-desistir.
2. **Notificação extrajudicial postal com AR** — Carta com Aviso de Recebimento, comprovação simples mas suficiente para fins de constituição em mora (CC art. 397 par. único).
3. **Notificação extrajudicial por e-mail** — admitida quando há cláusula contratual prevendo essa via OU quando há prova robusta de recebimento.
4. **Ofício institucional da Defensoria Pública** — papel timbrado da DP, signatário Defensor(a) responsável, dirigido a órgão administrativo (concessionária de serviço público, hospital, secretaria de saúde, fornecedor de medicamento, Município/Estado). Pressuposto de fé institucional. Não é notificação extrajudicial do(a) assistido(a) — é ato institucional da DP em nome do(a) assistido(a).

## Fidelidade aos autos — citações e pinpoints

Notificações são advocacia, e toda linha citada de contrato, e-mail ou comunicação anterior vira asseveração que a contraparte vai testar. Declaração canônica nos guardrails compartilhados do `CLAUDE.md`; repetida aqui.

**Citações literais devem ser literais.** Nunca coloque aspas em palavras atribuídas à contraparte, advogado(a) deles, testemunha, ou qualquer documento a menos que tenha a passagem exata diante de você. Quando você quer caracterizar sem palavras exatas:

- **Parafraseie sem aspas**, com placeholder: "Seu e-mail de [data] declarou X `[verificar citação literal — referência do e-mail pendente]`."
- **Nunca preencha a lacuna.** Cláusula contratual mal-citada em notificação é o jeito mais rápido de perder credibilidade com o(a) advogado(a) contrário(a) na primeira rodada.
- Toda `[verificar citação literal]` deve ser sinalizada na nota do revisor antes da notificação sair.

**Pinpoints devem sustentar a proposição inteira.** Se a notificação assevera "Cláusula 4.2 exige pagamento dentro de 30 dias do recebimento da fatura", a cláusula citada deve cobrir a obrigação E o gatilho E o prazo. Se cobre só um, divida (ex.: "Cláusula 4.2 (obrigação de pagamento); Cláusula 4.3 (prazo de 30 dias)") ou estreite a proposição.

## Candor sobre argumentos fracos

Quando o direito ou os autos estão contra um ponto, não dress up como sólido. Quando um argumento na notificação é fraco — a cláusula é ambígua, a autoridade corta para o outro lado, a teoria de danos é forçada — flag para o(a) signatário(a):

> "A [tese / pretensão] aqui é fraca porque [autoridade / fato]. Opções: (a) pressionar e enquadrar como `[enquadramento alternativo]`, (b) deixar e basear em [pretensão mais forte], (c) manter como gancho mas hedge a linguagem. `[review — chamada estratégica]`."

Notificação que sobre-assevera recebe resposta que cataloga cada overreach, transfere alavancagem, e queima a próxima rodada. Notificação mais forte é a que concede o fraco para a contraparte não conceder.

## Echo vs repetição

Se o caso tem correspondência anterior, eche os termos-chave — a mesma caracterização do inadimplemento, o mesmo enquadramento da obrigação central, o mesmo nome para a transação. Não copie frases inteiras. Notificação que lê como cópia-cola da anterior sinaliza que nada mudou; a nova deve avançar a postura (novos fatos, novo prazo, nova consequência), não restatá-la.

> **Entregável externo:** a notificação minutada é enviada à contraparte ou ao órgão administrativo. NÃO inclua cabeçalho `SIGILOSO — TRABALHO DE ADVOGADO` ou `SIGILOSO — TRABALHO DE DEFENSOR PÚBLICO` na carta que sai. O checklist pós-envio e o arquivo de intake são produtos internos e carregam o cabeçalho.

## Contexto de posição

Redigir notificação é inerentemente assertivo — o(a) signatário(a) faz pretensão. Leia `## Posição processual` no perfil:

- **Autor / requerente** (default desta skill): notificação alinha com a postura. A carta é a pretensão.
- **Defensor Público:** notificação extrajudicial em favor do(a) assistido(a) (constituir em mora, interpelar privado, cessar-e-desistir contra fornecedor); OU ofício institucional para órgão administrativo (concessionária, secretaria, hospital — pedido administrativo antes da via judicial).
- **Réu / requerido:** notificações são menos comuns de defesa mas acontecem — contra-notificação, notificação para constituição em mora em cumprimento, ou em matéria não-relacionada. Confirme antes de redigir.
- **Ambos / varia:** pergunte por minuta qual postura aplica.

## Postura para este caso

Antes do gate pré-redação, confirme a postura no nível do caso. Tom e termos são caso-a-caso, não default de prática. Confirme (lendo seção `## Postura` do intake se presente; perguntando se não):

> **Postura para este caso.**
> - **Tom:** mensurado / assertivo / agressivo? (depende da relação, do valor, e se litígio é provável)
> - **Janela de resposta:** o que é razoável dada a pretensão? (15 dias é comum para pagamento; 30 dias para purgação; 5-10 dias para cessar-e-desistir — mas o contrato ou protocolo pode setar)
> - **Marcação:** isto precisa de "sem prejuízo" / "sem prejuízo de medidas judiciais cabíveis"? (comunicações negociais carregam confidencialidade Lei 13.140/15 art. 30; asseverações de pretensão frequentemente não)
> - **Signatário:** você, o(a) cliente, o DJ, advogado(a) instruído(a), Defensor(a) titular, Defensor(a)-Geral?
> Não assuma. Leia correspondência anterior no arquivo do caso se houver — estabelece o registro.

As respostas direcionam escolha verbal de tom, a linguagem de consequência, o cabeçalho de marcação (ou ausência), o bloco de assinatura, e o prazo de cumprimento. Postura não capturada no intake é capturada aqui — não caia em default de nível-prática.

## Suposição de jurisdição

Esta minuta assume a jurisdição identificada no intake e a regra aplicável de confidencialidade negocial (Lei 13.140/15 art. 30 no Brasil para mediação; sigilo profissional do(a) advogado(a)/Defensor(a) por Lei 8.906/94 art. 7º XIX + LC 80/94 art. 4º-A V; CDC art. 51 IV nulidade de cláusulas que dispensam direito do consumidor). Regras, prazos, fee-shifting e ganchos legais variam materialmente por jurisdição. Se os fatos subjacentes tocam outro foro, contraparte estrangeira, ou questão de lei aplicável, a minuta pode não se aplicar como escrita — confirme antes de enviar.

## Carregar contexto

- `~/.claude/plugins/config/claude-for-legal/litigation-legal/demand-letters/[slug]/intake.md` — obrigatório; recuse prosseguir se ausente
- `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md` → Prática de notificação extrajudicial (caminhos de doc-semente, timing de aviso de sinistro ao seguro, limiar de materialidade para criação de caso), estilo da casa (marcações de sigilo, formato de diretrizes ao escritório externo para referência de tom). **Tom, período de cumprimento, marcação e signatário vêm de `## Postura para este caso` — são nível-caso, não nível-prática.**
- `~/.claude/plugins/config/claude-for-legal/litigation-legal/matters/_log.yaml` — para checar casos existentes relacionados (mesma contraparte) e oferecer cross-link

### Manejo de bloco estratégico pulado

Se o intake tem `strategic_block: skipped` ou `partial`, prompt antes de rodar o gate pré-redação:

> O intake pulou [tudo / parte] do bloco estratégico (alavanca, BATNA, tom, filtros de sigilo). Redigir agora vai produzir notificação usável mas as seções estratégicas serão genéricas e sinalizadas `[SME VERIFICAR]`.
>
> - **Completar bloco estratégico agora** — pause, retorne para `/demand-intake [slug] --resume-strategic`
> - **Prosseguir mesmo assim** — continue para o gate; seções downstream sinalizadas

Se "prosseguir mesmo assim", toda seção que depende de pergunta estratégica pulada recebe `[SME VERIFICAR: [pergunta específica]]` inline.

## Flags

- `--skip-gate` → bypassa o checklist pré-redação. Disponível mas logado; use só quando o checklist foi rodado separadamente e documentado.
- `--version=N` → minuta como `draft-vN.docx` (default: próximo número de versão)

## O gate pré-redação

**Isto roda antes de qualquer redação. Se o(a) usuário(a) não engaja, pare.**

```
CHECKLIST PRÉ-REDAÇÃO — [slug]

1. Filtro de sigilo
   Per filtros de sigilo do intake: [lista]
   Confirme: nenhum destes aparecerá na minuta?  [s/n]

2. Risco de confissão
   Per risco de confissão do intake: [lista]
   Para cada, a formulação está controlada ou removida?  [s/n por item]

3. Satisfação inadvertida (accord-and-satisfaction)
   Per intake: [risco sinalizado, se algum]
   A notificação inadvertidamente satisfaz ou aceita pretensão separada?  [s/n]

4. Postura de confidencialidade negocial
   Pesquise as proteções de confidencialidade negocial aplicáveis no foro
   (Lei 13.140/15 art. 30 em mediação; CC art. 422 boa-fé pré-contratual;
   marcação "sem prejuízo" como expressão típica). Note que proteção
   adere a conduta e contexto, não meramente a rotular a comunicação.
   Intake diz: [protegido / não protegido / caso-a-caso]
   Minuta vai [incluir / omitir] marcadores de comunicação negocial, e
   será estruturada para que a substância — não só o rótulo — sustente
   a postura. Confirme.

5. Scan de renúncia de sigilo
   Alguma frase na minuta revelará a substância da nossa análise jurídica
   interna (não só a conclusão)?  [s/n]
   Se sim, reformule antes de redigir.

6. Postura de tom
   Intake diz: [preservando-relacionamento / mensurado / terra-arrasada]
   Isto vai direcionar escolha de verbo, enquadramento, e linguagem de
   consequência. Confirme.

7. Precisão factual
   Todo fato na minuta deve ser verificado. Não "provavelmente verdade" —
   verificado. Liste qualquer fato ainda não verificado, e serão sinalizados
   [VERIFICAR: ___] inline.
```

Só prossiga quando engajado com cada item. Checklist em branco-reconhecido é pior que sem checklist.

## Seleção de template

### Passo 1: Doc-semente

Cheque `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md` → Prática de notificação extrajudicial → tabela de doc-semente para o tipo de notificação do intake.

- **Doc-semente fornecido:** leia. Case estrutura, tom, bloco de assinatura, marcações de sigilo, ordenação típica de seções. O doc-semente é o template.
- **Sem doc-semente:** use o template suave abaixo para o tipo.

### Passo 2: Templates suaves (usados só quando sem doc-semente)

Cada é esqueleto — cabeçalhos e conteúdo esperado. Desvie quando os fatos exigirem.

**Skeleton de notificação para pagamento (constituir em mora):**
1. Identificação das partes e contexto da relação (1 parágrafo)
2. Fatos — a obrigação e sua fonte (cláusula contratual / fatura / pedido), datas
3. O inadimplemento — o que é devido, quando vencido, o que aconteceu (ou não)
4. Notificação — valor específico, prazo (15 dias é comum), método de pagamento
5. Consequências — encaminhamento ao(à) advogado(a)/Defensor(a), juros (CC 406 + Selic), multa, custas, ação judicial
6. Preservação (se relevante)
7. Bloco de assinatura

**Skeleton de notificação de inadimplemento / purgação (cure notice):**
1. Identificação das partes e do contrato (data de início, partes)
2. A obrigação alegadamente inadimplida — cláusula, linguagem clara
3. O inadimplemento — fatos específicos, datas, evidência disponível
4. Purgação — o que especificamente purgaria; prazo (do contrato ou razoável)
5. Consequências do não-cumprimento — resolução (CC 475), perdas e danos, tutela específica do contrato
6. Preservação de direitos
7. Bloco de assinatura

**Skeleton de cessar-e-desistir:**
1. Partes e nossos direitos (marca/direito autoral/contrato/direito comum — identifique o direito)
2. A infração / violação — atos específicos, datas, evidência
3. Notificação — cessar imediatamente, retirar, prestar contas pelo uso passado, confirmar cumprimento por escrito
4. Prazo de cumprimento (tipicamente 5-10 dias)
5. Consequências do não-cumprimento — ação judicial, tutela inibitória CPC 497, dano material e moral, custas
6. Notificação de preservação (documentos, metadados, sistemas relacionados à conduta alegada)
7. Bloco de assinatura

**Skeleton de rescisão de contrato de trabalho / cumprimento de obrigação trabalhista:**
1. Partes e contexto da relação (ex-empregado(a), datas de emprego)
2. A obrigação — obrigações pós-contratuais inadimplidas (sigilo, não-concorrência, IP); cite o termo de rescisão / contrato
3. A conduta alegada
4. Notificação — cessar, devolver propriedade/IP, confirmar cumprimento, reforço de não-difamação se aplicável
5. Consequências — ação judicial trabalhista (TST), tutela inibitória, perdas e danos
6. Oferta de resolução informal (se estrategicamente apropriada)
7. Notificação de preservação
8. Bloco de assinatura

**Skeleton de notificação de preservação:**
1. Partes e contexto — qual disputa é antecipada
2. Escopo — categorias de documentos, dados, sistemas, comunicações
3. Custodiantes — nomes esperados de ter material relevante
4. Faixa de datas
5. Obrigação afirmativa de preservação — suspender auto-delete, preservar metadados, preservar dispositivos
6. Consequências de spoliação — presunção contrária (CPC 379), sanções (CPC 80, CPC 161)
7. Pedido de reconhecimento
8. Bloco de assinatura

**Skeleton de ofício institucional da DP (para Defensor):**
1. Identificação do ofício (número/ano, ofício destinatário, papel timbrado)
2. Identificação do(a) assistido(a) (nome + breve qualificação)
3. Pretensão administrativa específica — fornecer medicamento X, custear leito Y, conceder benefício Z, etc.
4. Base normativa (CF art. 196 se saúde; Lei 8.742/93 se BPC; CDC se consumo público; etc.)
5. Prazo razoável de resposta (10-30 dias úteis, conforme natureza)
6. Consequência da inércia — judicialização imediata, eventualmente com pedido de tutela de urgência CPC 300, com responsabilização por crime de desobediência se ordem judicial for descumprida
7. Cordialidade institucional (lembre — DP e órgão são poderes públicos)
8. Assinatura: Defensor(a) responsável + carimbo/timbre da DP

## Regras de redação

1. **Especificidade sobre adjetivos.** "Em 14 de março de 2026, você enviou X" vence "Você repetidamente e indevidamente enviou X." Adjetivos são o tell do(a) redator(a) de que os fatos são finos.

2. **Fatos rastreáveis a fontes.** Toda asseveração factual mapeia a documento, data, ou testemunha. Se ainda não verificável: `[VERIFICAR: alegação específica]`.

3. **Citações como placeholders.** `[CITE: lei/seção/julgado]` onde quer que autoridade jurídica vá. Não invente citações. Se a pessoa forneceu autoridades no intake, use fielmente.

4. **Linguagem de consequência casa com postura de tom.**
   - `preservando-relacionamento`: "Esperamos resolver isto sem outras providências."
   - `mensurado`: "Não purgado dentro de [N] dias, consideraremos nossas opções, incluindo medidas judiciais cabíveis."
   - `terra-arrasada`: "Não cumprida a presente no prazo de [N] dias, serão imediatamente ajuizadas as medidas judiciais cabíveis, incluindo [tutela específica + dano moral + perdas e danos]."

5. **Formulações alternativas inline.** Onde tom pode variar, a minuta inclui alternativa compacta. Formato:
   > *A fatura em anexo no valor de R$ X permanece inadimplida.* [ou mais assertivo: *V. Sa. deixou de pagar a fatura em anexo no valor de R$ X, vencida em [data].*]

6. **Sem discussão negocial nos autos salvo intencional.** Se o intake flagged a comunicação como não carregando proteção de confidencialidade negocial no foro, a minuta não inclui qualquer oferta de transação, enquadramento "sem prejuízo", ou linguagem que possa ser caracterizada como comunicação negocial. Lembre que proteção adere de conduta e contexto; rotular sozinho não é cura.

7. **Marcações de sigilo per estilo da casa.** Aplique convenções do `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md` exatamente.

## Output

### Primário: `~/.claude/plugins/config/claude-for-legal/litigation-legal/demand-letters/[slug]/draft-v[N].docx`

Use a skill `docx` para produzir notificação formato carta:
- Papel timbrado / bloco de endereço do(a) remetente (para Defensor: timbre da DPEAM)
- Data
- Bloco de endereço do(a) destinatário(a)
- Linha "Ref.:" (concisa; não revela estratégia sigilosa)
- Saudação
- Corpo (per template + regras de redação)
- Fechamento
- Bloco de assinatura per intake

### Revisão em chat

Mostre a minuta como texto legível para revisão e pedidos de edição. Itere antes de escrever o .docx final. Uma vez aprovado, escreva no disco.

### Gate de envio (nota de fechamento na minuta)

Anexe o seguinte, separado do corpo, à apresentação em chat e a qualquer preview interno — é nota voltada ao(à) revisor(a), não texto da carta, e é retirada antes da carta sair:

> Esta é minuta de notificação extrajudicial / ofício para revisão profissional, não carta pronta para enviar. Enviar pode constituir comunicação profissional, criar implicações de confidencialidade negocial (Lei 13.140/15), e começar o relógio em disputas, contra-pretensões e prescrição. Profissional habilitado(a) (advogado(a) ou Defensor(a)) revisa, edita, e assume responsabilidade profissional antes de enviar. Não envie minuta não-revisada.

### Verificação de citação

Todo placeholder `[CITE:___]` — e qualquer citação puxada do intake ou doc-semente — não é verificado até humano rodar contra fonte primária. Antes de enviar, rode pass de verificação: cheque cada julgado, lei e regulamento contra MCP de pesquisa (JusRatio, BNP, CJF, TJAM, DataJud) ou planalto.gov.br para precisão, status (vigência, overruling, modulação) e tratamento subsequente. Citações fabricadas ou mal-citadas em notificações enviadas e documentos protocolados resultaram em sanções por litigância de má-fé e infração ético-disciplinar.

**Atribuição de fonte.** Marque toda citação na minuta com de onde veio: `[JusRatio]`, `[BNP]`, `[CJF]`, `[TJAM]`, `[DataJud]`, ou nome da tool MCP para citações recuperadas; `[busca web — verificar]` para web; `[conhecimento do modelo — verificar]` para citações lembradas; `[usuário forneceu]` para citações fornecidas no intake. Citações marcadas `verificar` carregam risco mais alto de fabricação que recuperadas por ferramenta e devem ser conferidas primeiro. Nunca tire ou colapse as tags — são o sinal mais rápido do(a) signatário(a) sobre quais conferir antes da carta sair.

**Sem suplementação silenciosa.** Se busca em MCP retorna poucos ou nenhum resultado para autoridade que a minuta precisa, reporte o que achou e pare. NÃO preencha de busca web ou conhecimento do modelo sem perguntar. Diga: "A busca retornou [N] resultados em [ferramenta]. Cobertura parece fina para [questão]. Opções: (1) ampliar query, (2) tentar ferramenta diferente, (3) buscar web — resultados marcados `[busca web — verificar]` e devem ser checados contra fonte primária, ou (4) deixar `[CITE:___]` e parar. Qual?" Profissional decide se aceita fontes de menor confiança; a skill não decide.

### `~/.claude/plugins/config/claude-for-legal/litigation-legal/demand-letters/[slug]/checklist.md` — checklist pós-envio

```markdown
[CABEÇALHO DE SIGILO — per plugin config ## Outputs — difere por papel. Este cabeçalho aplica ao checklist interno; a notificação que sai NÃO carrega.]

# Checklist Pós-Envio — [slug]

**Versão da minuta enviada:** [v1 / v2 / etc.]
**Data de envio:** [AAAA-MM-DD — preenchido após envio]
**Signatário:** [nome]

## Pré-envio (antes da carta sair)

- [ ] Releitura final pelo(a) signatário(a)
- [ ] Precisão factual: todas as flags [VERIFICAR] resolvidas
- [ ] Citações: todos os placeholders [CITE] preenchidos e rodados em MCP (verificar se está em vigor)
- [ ] Marcações de sigilo per estilo da casa — nota: este é entregável externo; não inclua cabeçalho `SIGILOSO — TRABALHO DE ADVOGADO/DEFENSOR PÚBLICO` na versão enviada à contraparte
- [ ] Marcadores de comunicação negocial [presente / ausente] como o intake especificou, e substância alinha com postura
- [ ] Cópias internas autorizadas (per lista de distribuição do intake)
- [ ] Aviso de sinistro enviado ao seguro (se exigido — DJ corporativo)
- [ ] Conflitos / impedimentos confirmados (se ainda não cleared)

**Antes da carta ser enviada (o ato consequente):** Leia `## Quem está usando` em `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md`. Se o Papel é Não-advogado:

> Enviar esta notificação tem consequência jurídica — cria registro, pode disparar prescrição e contra-pretensões, e pode renunciar sigilos ou constituir confissões. Você revisou com profissional habilitado(a)? Se sim, prossiga. Se não, eis briefing para levar:
>
> [Gere sumário de 1 página: contraparte e disputa, a notificação e prazo, postura de tom, status confidencialidade negocial (Lei 13.140/15 art. 30), riscos de sigilo e confissão sinalizados no gate, o que pode dar errado, o que perguntar antes de enviar.]
>
> Se você precisa achar profissional habilitado(a): OAB Seccional (Comissão de Assistência Judiciária Gratuita) tem orientação inicial. Defensoria Pública estadual atende hipossuficiente. NPJ local pode atender em certas áreas.

Não marque como enviado — não execute o mecanismo de envio abaixo — sem um sim explícito.

## Mecanismo de envio

- [ ] Método de entrega executado: [cartorial / AR postal / e-mail / protocolo presencial / ofício institucional]
- [ ] Prova de entrega retida (certidão cartorial, AR, leitura de e-mail, protocolo)
- [ ] Cópias enviadas per lista de distribuição

## Após envio

- [ ] Prazo de cumprimento agendado: [AAAA-MM-DD]
- [ ] Plano de escalonamento se sem resposta: [próximo passo + data — tipicamente: judicializar com tutela de urgência CPC 300]
- [ ] Follow-up check-in agendado: [data — tipicamente prazo + 2 dias úteis]
- [ ] Caso criado em `_log.yaml`: [sim / não — vide materialidade abaixo]

## Chamada de materialidade

**Heurística diz:** [material / imaterial]
**Razão:** [tipo de notificação / exposição / tipo de contraparte / urgência humanitária Defensor]
**Sua chamada:** [material → criar caso] [imaterial → registro só em demand-letters]

Se material: `/litigation-legal:matter-intake` com `source: notificacao-extrajudicial` (ou `oficio` para DP) pré-populado deste intake.
```

### Oferta de auto-criação de caso

Depois de redigir e escrever o checklist, avalie materialidade per heurística:

- **Default sim se QUALQUER de:**
  - Tipo de notificação é `cessar-desistir`, `purgação-mora-contrato`, `separação-trabalho`, `preservação`, ou `oficio-DP` para órgão administrativo com pretensão humanitária
  - Valor pretendido R$ ≥ banda de severidade-média do `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md`
  - Contraparte é cliente, competidor, ou adversário frequente per panorama
  - **Para Defensor:** urgência humanitária presente (medicamento, saúde, despejo, violência)
- **Default não caso contrário**

Apresente a chamada:
> Heurística de materialidade: [resultado]. [Razão em uma frase.]
> Criar caso rastreado em `~/.claude/plugins/config/claude-for-legal/litigation-legal/matters/_log.yaml`? (default: [sim/não])

Se aceita: dispare `matter-intake` com campos pré-populados (contraparte, tipo, vara/jurisdição, `source`, tese inicial, stakeholders internos, urgência humanitária se Defensor). Pessoa revisa pré-preenchidos e confirma.

Se recusa: atualize intake `status: drafted` (depois `enviado` quando confirmar). Registro fica em `~/.claude/plugins/config/claude-for-legal/litigation-legal/demand-letters/` só.

## Versionamento

Nunca sobrescreva minuta que foi enviada. Se revisando após envio, `draft-v2.docx`. O histórico da versão-enviada é o próprio registro do que a contraparte recebeu.

## O que esta skill NÃO faz

- **Envia a carta.** Redação apenas. A pessoa envia.
- **Pesquisa citações.** Placeholders `[CITE:___]` ficam como placeholders. Se a pessoa forneceu autoridades no intake, são usadas; senão, em branco. Inventar é exposição ético-disciplinar.
- **Bypassa o gate pré-redação.** Mesmo com `--skip-gate`, a skill anota no arquivo da minuta que o gate foi pulado e por quê.
- **Reescreve o intake.** Se o intake é fino, volte para `demand-intake`. A minuta é só tão boa quanto o que ela lê.
- **Decide materialidade.** A heurística oferece default; a chamada do(a) usuário(a) é o registro.
