---
name: privilege-log-review
description: Revisão de primeira passagem de rol de documentos sigilosos — faz as chamadas óbvias de sigilo e flagueia as difíceis para revisão do(a) advogado(a) sem fazer chamadas borderline. Use quando o usuário diz "revise o rol de sigilosos", "rol de sigilo", "cheque sigilo nestes docs", ou tem um rol para QA antes da produção.
argument-hint: "[log file, or document set]"
---

# /privilege-log-review

1. Carregue `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md` → protocolo de revisão, formato do rol.
2. Siga o workflow e a referência abaixo.
3. Para cada entrada: óbvio sigiloso / óbvio não-sigiloso / precisa revisão. Sinalize razões.
4. Output: rol revisado com flags. Advogado(a) revisa todas as flags antes da produção.

---

# Revisão de Rol de Documentos Sigilosos

## Restrições de uso de documento divulgado

Antes de trabalhar com conjunto de documentos do processo, pergunte: "Algum destes documentos veio de instrução em processo judicial, ou de exibição compelida (CPC arts. 396-404)?" Se sim:

- **Brasil — segredo de justiça (CPC art. 189):** documentos que tramitam em segredo de justiça têm acesso restrito às partes e seus(suas) procuradores(as). Usá-los fora da finalidade processual pode configurar quebra de segredo (CP art. 154; CPC art. 80).
- **Brasil — tutela exibitória (CPC arts. 396-404):** documento exibido por força judicial está afeto à finalidade da prova produzida; uso para outro caso, outra pretensão, ou fim comercial sem autorização é abuso.
- **Outras jurisdições:** restrições análogas costumam aplicar. Confira a regra local.

Confirme: "Este uso está dentro do processo em que os documentos foram divulgados, ou tenho autorização / consentimento da parte, ou os documentos já são públicos." Se não confirmado, sinalize: "⚠️ Documentos divulgados podem ter restrições de uso. Confirme antes de prosseguir."

## Contexto de caso

**Contexto de caso.** Cheque `## Workspaces de caso` no CLAUDE.md de nível-prática. Para litigation-legal o default é `Habilitado: ✓` — cada caso tem seu workspace. Se `Habilitado` é `✗` (você desligou porque trabalha um caso por vez), pule o resto deste parágrafo e use contexto nível-prática. Se habilitado e não há caso ativo, pergunte: "Qual caso é este? Rode `/litigation-legal:matter-workspace switch <slug>` ou diga `nível-prática`." Carregue o `matter.md` do caso ativo para contexto e overrides específicos. Grave outputs na pasta de caso em `~/.claude/plugins/config/claude-for-legal/litigation-legal/matters/<matter-slug>/`. Nunca leia arquivos de outro caso a menos que `Contexto cruzado entre casos` esteja `on`.

---

## Propósito

Um rol de sigilosos tem três tipos de entrada: obviamente sigilosa, obviamente não, e as que exigem pensamento. Esta skill classifica os dois primeiros tipos para que o tempo do(a) advogado(a) vá inteiro para o terceiro.

**Isto é primeira passagem. Advogado(a) revisa toda flag. Sem exceção.**

## Fidelidade do registro — pinpoints e cobertura de citação

Quando esta skill cita uma regra, variante local, ou autoridade para chamada de sigilo (CPC art. 388 IV, Lei 8.906/94 art. 7º XIX, julgado sobre escopo de quebra, julgado sobre propósito dominante), duas regras aplicam.

**Pinpoints devem sustentar a proposição inteira.** Se a revisão cita uma regra ou julgado para sustentar proposição multipartite — "o rol deve descrever cada documento e reter apenas materiais preparados em contemplação de litígio" — verifique que o pinpoint cobre cada elemento. Se cobre só um, divida o cite ou estreite a proposição. Cite que sustenta parte de uma posição de sigilo faz a posição ser rejeitada quando o(a) advogado(a) contrário(a) lê o cite e aponta que não alcança o elemento contestado. Este é o failure mode de "misgrounded citation": o cite existe, a passagem existe, mas não sustenta a proposição como posta.

**Extraia todas as citações antes de checar qualquer.** Quando esta revisão cita autoridade — ou quando cite-check separado é pedido sobre o rol, peça relacionada, ou petição de sustentação:

1. **Primeira passagem: extraia.** Leia o documento e construa lista de toda citação (regras, julgados, leis, ordens locais, cites de registro). Reporte a contagem: "Encontradas [N] citações."
2. **Segunda passagem: cheque.** Cheque cada uma contra a fonte. Não amostre. Não pare nos primeiros cinco.
3. **Reporte cobertura.** "Checadas [N] de [M] citações. [K] não puderam ser recuperadas — verifique manualmente. [J] confirmadas. [I] sinalizadas como potencial má-citação. [H] sinalizadas como mal-fundamentadas (cite existe mas não sustenta a proposição)."
4. **Quando texto-fonte indisponível, diga "não pude checar", nunca "confirmado".** Falso positivo é pior que "não pude checar" — deixa um cite ruim passar.
5. **Os erros mais difíceis são sustentação parcial.** Leia a proposição, leia a fonte, compare elemento por elemento.

## Carregar contexto

`~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md` → formato de rol de sigilo, protocolo de revisão.

**Gate de impedimentos — incontornável.** Antes de revisar rol, cheque `~/.claude/plugins/config/claude-for-legal/litigation-legal/matters/_log.yaml` para o slug. Se o caso não está em `_log.yaml`, recuse e route:

> "Não vejo [slug do caso] no log de casos. Rode `/litigation-legal:matter-intake` primeiro para a checagem de impedimentos rodar e o workspace ser montado. Não reviso rol de sigilosos em caso não-intaken — a checagem de impedimentos é o gate, e revisão de rol é trabalho-produto que precisa viver no arquivo do caso."

**Jurisdição importa.** Escopo de sigilo (comunicação A/C e trabalho preparatório), doutrina de quebra, e exigências de forma do rol variam materialmente entre tribunais e instâncias. Esta revisão aplica as regras para o foro especificado na config. Se o caso envolve foro diferente, caso transferido, produção multi-jurisdicional, ou questão de lei aplicável sobre sigilo, as chamadas aqui podem não transferir — rerode contra o foro controlante.

## Passo 0: Pesquise as regras de rol de sigilo do foro

**Antes de revisar entradas, pesquise as exigências de rol de sigilo do foro (CPC art. 188 — princípio de publicidade; CPC art. 189 — segredo de justiça; Lei 8.906/94 art. 7º XIX — sigilo profissional; CPC art. 388, IV — recusa de depoimento por sigilo), qualquer variante local, e ordens permanentes do(a) juiz(a). Identifique os campos exigidos, o nível de descrição, e quaisquer acomodações de rol por categoria ou rol por metadados. Cite fontes primárias.**

**Sem suplementação silenciosa.** Se consulta ao MCP de pesquisa configurado (JusRatio, BNP, CJF, TJAM, DataJud) retorna poucos ou nenhum resultado para a regra do foro, doutrina de quebra, ou variante local, reporte o que foi encontrado e pare. NÃO preencha a lacuna com busca web ou conhecimento do modelo sem perguntar. Diga: "A busca retornou [N] resultados de [ferramenta]. Cobertura parece fina para [regra / doutrina]. Opções: (1) ampliar a query, (2) tentar outra ferramenta, (3) buscar na web — resultados tagueados `[busca web — verificar]` e devem ser checados contra fonte primária antes de confiar, ou (4) deixar o marcador `[INCERTO]` e parar aqui. Qual prefere?" Um(a) advogado(a) decide se aceita fontes de menor confiança; a skill não decide por ele.

**Atribuição de fonte.** Tagueie cada referência a regra e autoridade no output da revisão com de onde veio: `[JusRatio]`, `[BNP]`, `[CJF]`, `[TJAM]`, `[DataJud]`, ou o nome do MCP para citações recuperadas; `[busca web — verificar]` para citações de busca web; `[conhecimento do modelo — verificar]` para citações de dados de treino; `[usuário forneceu]` para citações que o(a) advogado(a) revisor(a) forneceu. Citações tagueadas `verificar` carregam maior risco de fabricação e devem ser checadas primeiro. Nunca strip ou colapse as tags — são o sinal do(a) advogado(a) revisor(a) sobre quais autoridades re-confirmar antes da produção.

**Doutrina de quebra difere por tipo de sigilo:**

- **Quebra do sigilo profissional do(a) advogado(a)** é categoria distinta no Brasil — a Lei 8.906/94 art. 7º XIX é inviolabilidade institucional do escritório, comunicações e arquivos do(a) advogado(a) (ampliada pelo art. 7º XX — atendimento livre); CPC art. 388 IV permite que o(a) advogado(a) recuse a depor sobre fatos sobre os quais deva guardar sigilo profissional.
- **Trabalho preparatório (analogia ao "work product")** — não há categoria formal de "trabalho preparatório" em jurisprudência consolidada como nos EUA. Documentos internos do DJ corporativo, DPIAs LGPD, assessments de compliance e launch reviews **não são automaticamente blindados** contra autoridade fiscalizatória (ANPD, CARF/RFB, CVM, Bacen). O segredo de justiça processual (CPC art. 189) é distinto e mais restrito.

Confirme a doutrina de quebra do foro para cada sigilo invocado antes de recomendar produção de qualquer coisa. Flags `[INCERTO]` ficam em chamadas de quebra até o(a) advogado(a) confirmar.

## As chamadas

**Regra tri-estado. A skill nunca decide silentemente que um limiar subjetivo não foi atingido.** Em qualquer chamada incerta — propósito dominante pouco claro, contemplação de litígio borderline, conteúdo misto jurídico/de negócio, presença ambígua de terceiro — a skill mantém a designação de sigilo ON e adiciona flag ⚠️ para o(a) advogado(a). Sub-marcar quebra sigilo (porta de mão única); super-marcar é corrigido pelo(a) advogado(a) em revisão (porta dupla). Prefira o erro recuperável.

**Sigilo de advogado(a) interno(a) (DJ corporativo) é específico de jurisdição e contestado.** Antes de classificar qualquer comunicação com advogado(a) interno(a) como sigilosa, cheque a jurisdição:

- **Brasil:** comunicações com advogado(a) interno(a) inscrito(a) na OAB são protegidas pela Lei 8.906/94 art. 7º XIX (inviolabilidade do escritório/arquivo/comunicação do(a) advogado(a)) — não há jurisprudência consolidada distinguindo "papel de advogado(a)" vs. "papel de empregado(a)". A proteção institucional do(a) advogado(a) habilitado(a) é o gancho. Mas autoridades fiscalizatórias específicas (ANPD, RFB, CVM) podem ter prerrogativas requisitórias próprias que se sobrepõem.
- **UE (concorrência / Comissão Europeia):** Sob *Akzo Nobel Chemicals v. Commission* (C-550/07 P), comunicações com advogado(a) interno(a) NÃO são protegidas em procedimentos europeus de concorrência. Se o caso envolve concorrência UE ou Comissão Europeia, documentos de advogado(a) interno(a) são exigíveis.
- **Alemanha (Syndikusanwalt):** status híbrido. Sigilo depende da capacidade em que atuava.
- **UK:** sigilo de advogado(a) interno(a) geralmente reconhecido, com teste de "propósito dominante".

**Nunca classifique comunicação de advogado(a) interno(a) como "confiantemente sigilosa" sem dizer qual regime de sigilo aplica.** Se o caso envolve jurisdições não-BR, especialmente concorrência UE ou qualquer regulador UE: "Documentos de advogado(a) interno(a) podem NÃO ter sigilo em [jurisdição]. Sob *Akzo Nobel*, comunicações de internos são exigíveis em procedimentos europeus de concorrência. Sinalize para revisão por especialista em contencioso [jurisdição] antes de invocar sigilo."

A tier ✅ "confiantemente sigiloso, sem flag" abaixo é desenhada para bypassar revisão. É exatamente onde o risco *Akzo Nobel* vive. Quando a jurisdição é não-BR ou o caso toca reguladores UE, não há tier ✅ para comunicações de internos — tudo vai para 🟡 "flag para revisão com nota de jurisdição."

### Confiantemente sigiloso (✅) — mantém designação, sem flag

- Comunicação entre cliente e escritório externo / Defensor(a) buscando/dando parecer jurídico, sem terceiros copiados
- Comunicação entre cliente e advogado(a) interno(a) inscrito(a) OAB, claramente parecer jurídico (não conselho de negócio), sem terceiros
- Trabalho preparatório criado em contemplação de litígio, por ou para advogado(a) / Defensor(a)
- Comunicações dentro do grupo controlador sobre estratégia jurídica

### Incerto — mantém designação E sinaliza (✅ + ⚠️)

O default para qualquer coisa que não está confiantemente em ✅ ou ❌. A skill não retira designação de sigilo com base em sua própria avaliação de teste subjetivo. Exemplos:

- **Advogado(a) interno(a) fazendo jurídico e negócio** — esta comunicação era parecer jurídico ou conselho de negócio? A chamada de propósito dominante é do(a) advogado(a), não da skill.
- **Terceiro presente** — o terceiro está dentro do sigilo (interesse comum, agente, secretário(a) sob sigilo, estagiário(a) OAB) ou sua presença quebra? Mantenha a designação; sinalize.
- **Documentos de propósito misto** — parte jurídico, parte negócio. Tarjamento parcial? Retenção total? Produzir? Mantenha; sinalize para o(a) advogado(a) decidir o tratamento.
- **Anexos** — analise separadamente e mantenha cada designação a menos que confiantemente ❌; sinalize aqueles em que sigilo gira em chamada subjetiva.
- **Trabalho preparatório pré-litígio** — "contemplação razoável de litígio" é específico de fato; mantenha a designação; sinalize.
- **Risco de quebra** — histórico de compartilhamento posterior é ambíguo; mantenha; sinalize a questão de quebra.

Cada flag registra a questão aberta específica e a evidência cortando para cada lado, para que o(a) advogado(a) possa decidir sem reler o documento a frio.

### Confiantemente não-sigiloso (❌) — recomenda remover, mas anota a avaliação

Só para os casos inequívocos. O output ainda registra o racional para o(a) advogado(a) checar; não remove a designação do rol por conta própria.

- Nenhum(a) advogado(a) / Defensor(a) envolvido(a) em lugar nenhum
- Conselho de negócio com advogado(a) em cópia (CC ao Jurídico não torna sigiloso)
- Fatos subjacentes (fatos não são sigilosos — comunicações *sobre* fatos podem ser)
- Terceiro copiado claramente fora do sigilo (quebra a confidencialidade)
- Anexos que são independentemente não-sigilosos (o e-mail pode ser sigiloso; a planilha anexa de números de vendas não)

Se qualquer destes está *no limite* — o terceiro pode ser agente, o CC ao(à) advogado(a) pode estar em pedido jurídico — é incerto, não ❌. Route para o bucket incerto e sinalize.

## Fluxo de trabalho

### Passo 1: Checagem de formato

O rol tem o que precisa?

| Campo | Presente? |
|---|---|
| Data | |
| Autor | |
| Destinatários (todos — PARA, CC, CCO) | |
| Tipo de documento | |
| Sigilo invocado (A/C, trabalho preparatório, ambos) | |
| Descrição (suficiente para avaliar sem revelar conteúdo sigiloso) | |

Campos faltantes → sinalize para completar antes da revisão substantiva.

### Passo 2: Entrada-por-entrada

Para cada entrada:

```
Entrada [N] ([movimentação / ID]): [✅ Sigiloso | ✅ Sigiloso + ⚠️ Flag | ❌ Não sigiloso (avaliado)]
[Se ✅ (sem flag): razão de uma linha]
[Se ✅ + ⚠️: mantém designação; a questão específica que o(a) advogado(a) precisa responder; evidência cortando para cada lado]
[Se ❌: razão de uma linha — mas a designação fica no rol até o(a) advogado(a) remover]
```

**Nunca produza entrada que silentemente strip designação de sigilo baseado em chamada subjetiva da skill.** Um ❌ é recomendação logada junto à flag; o(a) advogado(a) age.

### Passo 3: Flags de padrão

Pelo rol:

- Mesmo issue repetindo? (Ex.: mesmo terceiro em 50 entradas — uma decisão resolve 50 flags)
- Padrão de super-designação? (Se tudo está designado sem diferenciação, aflore para o(a) advogado(a) — mas a chamada de estreitar o rol é do(a) advogado(a), não da skill. Sub-designação quebra; super-designação é corrigível.)
- Sub-descrição? (Descrições tão vagas que tribunal ordenaria revisão *in camera*)

## Output

**Antes do rol de sigilosos ser juntado nos autos contra a parte adversa (o ato consequencial — isto inclui o juntada do rol E designações de documentos como confidenciais, restritos ou em segredo de justiça per CPC art. 189):** Leia `## Quem está usando` em `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md`. Se o Papel é Não-advogado:

> Juntar rol de sigilosos e designar documentos em instrução têm consequências jurídicas — super-designação arrisca sanções e perda de credibilidade; sub-designação arrisca quebra; produção mal-designada pode ser irrecuperável. Você revisou com advogado(a) ou Defensor(a)? Se sim, prossiga. Se não, segue brief para levar:
>
> [Gere sumário de 1 página: o caso, contagem de entradas do rol, as flags ⚠️ e chamadas no limite, observações de padrão (super-designação, descrições vagas), postura de doutrina de quebra por tipo de sigilo, o que pode dar errado em juntada ou designação, o que perguntar ao(à) advogado(a).]
>
> Se precisa achar advogado(a) habilitado(a) ou Defensor(a) Público(a) na sua localidade: o serviço de referência da OAB Seccional do estado (ou da Defensoria Pública Estadual/União) é o ponto de partida mais rápido.

Não trate o rol como pronto-para-juntar sem um sim explícito. Revisão de primeira passagem, ordenação e flagging não exigem o gate — juntada e designação exigem.

```markdown
[CABEÇALHO DE SIGILO — por config do plugin ## Outputs — varia por papel; vide `## Quem está usando`]

## Revisão de Rol de Sigilosos: [Caso] — [data]

**Regra aplicável:** [CPC art. 388 IV / Lei 8.906/94 art. 7º XIX / CPC art. 189 / regimento interno — pinpoints] `[INCERTO — verificar atualidade]`
**Entradas revisadas:** [N]
**Resultados:** [N] ✅ sigilo confiante / [N] ✅+⚠️ sigilo mantido & sinalizado / [N] ❌ recomenda remover (advogado(a) confirma)

### ✅ + ⚠️ Sinalizadas — designação mantida, advogado(a) decide

| Entrada | Movimentação / ID | Issue | Evidência pró-sigilo | Evidência contra | Pergunta |
|---|---|---|---|---|---|
| [N] | [range] | [o que é subjetivo] | [uma linha] | [uma linha] | [a chamada específica a fazer] |

### ❌ Recomenda remover designação (advogado(a) confirma antes de strip)

| Entrada | Movimentação / ID | Razão |
|---|---|---|

*Registrado, não executado. A skill não remove designações de sigilo do rol — o(a) advogado(a) o faz, depois de revisar o racional.*

### ✅ Sigiloso (sem ação)

[Contagem. Lista disponível sob pedido.]

### Observações de padrão

[Issues repetindo, super-designação, problemas de descrição]

### Disciplina de marcador

- `[VERIFICAR: alegação factual sobre documento/custodiante/data]`
- `[INCERTO: chamada de sigilo no limite / escopo de quebra / questão de doutrina]`
- `[CITE FALTANDO: regra, variante local, ou autoridade sustentando uma chamada]`

---

**Advogado(a) deve revisar todas ⚠️ e ❌ antes de qualquer ação.**

**Material-fonte sigiloso.** Esta revisão lê entradas e documentos subjacentes que são, por definição, candidatos a sigilo. O output da revisão herda esse status — mantenha com materiais sigilosos, marque apropriadamente, e não circule fora do círculo de sigilo. Distribuir pode em si quebrar a proteção.
```

## O que esta skill enfaticamente não faz

- Faz chamadas no limite. ⚠️ significa "um humano decide". Em qualquer teste subjetivo (propósito dominante, contemplação razoável, escopo de interesse comum, quebra por compartilhamento posterior) a skill mantém a designação de sigilo on e sinaliza.
- Strip designação de sigilo do rol baseado em sua própria avaliação. ❌ é *recomendação* registrada para o(a) advogado(a), não ação tomada contra o rol.
- Produz ou retém documentos. Aconselha; advogado(a) decide; advogado(a) age.
- Garante correção em chamadas ✅. O(a) advogado(a) é responsável pelo rol. Isto é primeira passagem.

## Feche com a árvore de decisão de próximos passos

Feche com a árvore de decisão de próximos passos per CLAUDE.md `## Outputs`. Customize as opções para o que esta skill acabou de produzir — as cinco ramificações default (redigir o X, escalonar, pegar mais fatos, observar e esperar, outra coisa) são ponto de partida, não trava. A árvore É o output; o(a) advogado(a) escolhe.
