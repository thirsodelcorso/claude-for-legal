---
name: matter-intake
description: Intake de novo caso — perguntas uniformes cobrindo identificação, conflitos/vedações institucionais, fonte, triagem de risco (humanitário para Defensor; CPC 25 para DJ), materialidade ou escalonamento institucional, escritório externo ou DPs colaboradoras, owners internos, dever de guarda, datas-chave; escreve matter.md e history.md e anexa linha estruturada ao _log.yaml. Use quando disser "novo caso", "fazer intake deste caso", ou quiser trazer caso novo para o portfólio. Para Defensor Público, vira intake do(a) assistido(a) — formulário social + hipossuficiência presumida (Súmula 481 STJ) + urgência humanitária.
argument-hint: "[nome opcional do caso]"
---

# /matter-intake

1. Carregue `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md` → calibração de risco (para triagem), panorama (para contexto, método de checagem de impedimento/conflito), stakeholders (para quem envolver).
2. Siga o workflow e a referência abaixo.
3. Rode o intake uniforme: identificação, checagem de conflitos/impedimentos, fonte, triagem de risco, materialidade ou escalonamento institucional, escritório externo ou DPs colaboradoras/núcleos, owners internos, dever de guarda, datas-chave, postura inicial.
4. Gere slug a partir do nome do caso (minúsculo, hifens, ano).
5. Crie `~/.claude/plugins/config/claude-for-legal/litigation-legal/matters/[slug]/matter.md` — intake narrativo completo.
6. Crie `~/.claude/plugins/config/claude-for-legal/litigation-legal/matters/[slug]/history.md` — semeado com o intake como primeira entrada.
7. Anexe linha estruturada em `~/.claude/plugins/config/claude-for-legal/litigation-legal/matters/_log.yaml`.
8. Confirme com o(a) usuário(a): "Esta é a linha que vou escrever — alguma edição?"

---

# Intake de Caso

## Propósito

Todo caso novo passa pelo mesmo intake para o portfólio ficar comparável. Linhas uniformes em `_log.yaml` permitem que a skill de status faça rollup. Narrativa em `matter.md` captura o que a linha não captura. Arquivo de histórico semeado aqui vira o registro de eventos.

**Para Defensor Público,** "caso" e "cliente" são lidos como "atendimento" e "assistido(a)" — o vocabulário muda, a engenharia uniforme do intake permanece. Formulário social, hipossuficiência presumida (Súmula 481 STJ) e urgência humanitária aparecem onde DJ corporativo teria "due-diligence financeiro" e "memo de provisão CPC 25".

## Carregue contexto

- `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md` — calibração de risco (limiares de triagem, materialidade ou escalonamento institucional, alçada), panorama (stakeholders, bench de escritórios externos ou DPs colaboradoras), atribuição (varas, escala da unidade — só Defensor).
- `~/.claude/plugins/config/claude-for-legal/litigation-legal/matters/_log.yaml` — para confirmar unicidade do slug.

## O intake

### 1. Identificação

- Nome do caso (como referenciado, ex.: "ACME v. Nós 2026" ou para Defensor: "Maria S. — fornecimento medicamento oncológico — 2026")
- Contraparte (ou ré, se o(a) assistido(a) é autor(a))
- Tipo de caso: `civel-consumidor | civel-saude | civel-previdenciario | familia | sucessoes | locacao | possessoria | trabalhista | empresarial | regulatorio | investigacao | outro`
- Nossa posição: `autor | requerente | impugnante | reu | requerido | impugnado | investigado | terceiro/amicus`
  - Se a `## Posição processual` do perfil é `autor`, `réu`, ou variante "ambos — default X", pré-preencha a posição daquele default e confirme. Se a posição é `varia por caso`, pergunte friamente. Nunca assuma silenciosamente uma postura que o perfil não setou.
  - Para Defensor: posição é majoritariamente autor (assistido(a) deduzindo pretensão). Defesa em ação de cobrança, despejo, embargos à execução ou ação penal por escala roteia para frame réu.
  - A posição direciona skills downstream: autor roteia triagem para valor da causa / urgência humanitária (Defensor) / honorários ad exitum (autônomo); réu roteia para exposição / provisões CPC 25 (DJ) / cobertura de seguros.
- Jurisdição (juízo, câmara arbitral, ou órgão regulatório)
- **Para Defensor:** vara da atribuição (1ª/12ª JEC, 19ª/20ª Cível Comum, etc.) e rito (Lei 9.099/95 vs CPC 2015)

### 2. Checagem de conflitos / impedimentos institucionais

Antes de avançar, rode o passo per `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md` → Checagem de conflitos.

- **Status:** `cleared | pending | not-run | waived`
- **Método:** case o que o `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md` declara (`defensoria-publica | corporate-legal | escritorio-externo | system-check | informal | outro`). Se o método declarado é `informal`, diga — o registro ainda captura que checagem de juízo do(a) advogado(a)/Defensor(a) foi a base.
- **Cleared por:** nome / equipe / escritório
- **Cleared em:** AAAA-MM-DD
- **Checado contra:** lista breve dos nomes/entidades efetivamente rodados (contraparte, afiliadas conhecidas, advogado(a) adverso(a) se conhecido(a), testemunhas-chave). Para Defensor: lista de impedimentos pessoais (parentes da contraparte, casos antes patrocinados em escritório anterior, etc.) + checagem de vedações institucionais LC 80/94 art. 46.
- **Notas:** qualquer coisa sinalizada mas cleared (ex.: "Silva membro do nosso conselho atuou no conselho da contraparte 2019-2021 — cleared como não-sobreposto a este caso"; para Defensor: "Já patrocinei pelo NPJ caso similar 2018, sem conflito atual").

Comportamento por status:

- `cleared` → prosseguir.
- `pending` → prosseguir com intake; flag em destaque no `matter.md` e na linha do log que conflitos estão pendentes; surface de novo em todo `/matter-update` e em `/portfolio-status` até resolvido.
- `waived` → raro; exige racional de renúncia (escrever a renúncia está fora desta skill — capture que uma existe, quem assinou, e onde vive). Para Defensor: renúncia institucional via decisão fundamentada do(a) próprio(a) Defensor(a) submetida ao(à) Coordenador(a) em casos sensíveis.
- `not-run` → **PARE. Isso é um gate.** A skill não cria `matter.md`, `history.md`, ou entrada `_log.yaml` até a postura de conflitos ser resolvida. Três caminhos aceitáveis:

  **Caminho 1 — Rode conflitos agora.** Pause este intake. Faça clear per `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md` Checagem de conflitos. Retorne com `status: cleared` ou `status: waived` com racional.

  **Caminho 2 — Marque pending com owner + prazo.** Permitido apenas quando `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md` Checagem de conflitos declara intake-paralelo aceitável. Capture: quem está rodando, quando esperado retornar, quais entidades checando. Intake prossegue; linha do caso carrega `conflicts.status: pending`; `/portfolio-status` flag em todo run; `/matter-update` re-pergunta até resolvido.

  **Caminho 3 — Bypass com racional documentado.** Apenas se o(a) usuário(a) explicitamente reconhece o bypass. Registre em `conflicts.override`:

  ```yaml
  conflicts:
    status: not-run               # preservado como está
    override:
      by: [nome do usuário]
      date: [AAAA-MM-DD]
      rationale: [por que conflitos foram bypassed — registro permanente; não expira automaticamente]
  ```

  Este campo é visível em todo `/portfolio-status`, todo `/matter` briefing, e todo `/matter-update` até removido. Nunca é removido pela skill — só por edição explícita do(a) usuário(a) no `_log.yaml` depois que conflitos forem efetivamente cleared.

  **Não prossiga silenciosamente.** "Eu faço depois" não é resposta aceitável. Um dos Caminhos 1/2/3 deve ser escolhido, e a escolha é capturada no registro.

Este passo não é sobre a skill decidir se conflito existe — isso é juízo do(a) usuário(a). É sobre garantir que a checagem aconteceu e o registro reflete.

### 3. Fonte

Como isto chegou?
- `notificacao-extrajudicial | peticao-inicial-citacao | oficio | requisicao-administrativa | denuncia-interna | ameaça-pre-litigatoria | atendimento-na-defensoria | encaminhamento-CRAS-CREAS | encaminhamento-156`
- *Oportunidade de doc-semente:* "Se você tem o documento iniciante (petição inicial, notificação, ofício, formulário social do(a) assistido(a)), anexe ou compartilhe o caminho. Afia o intake."

### 4. Triagem de risco — contra a calibração da casa

- Severidade: alta | média | baixa (referencie as bandas em `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md`)
- Probabilidade: alta | média | baixa (referencie as bandas em `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md`)
- Risk rating resultante (per a matriz): alto | médio | baixo | crítico
- Exposição em R$ (faixa estimada) — para autor: valor da causa; para réu: exposição
- Exposição não-monetária:
  - DJ/banca: tutela inibitória, decisão coletiva precedente, publicidade adversa
  - **Defensor:** urgência humanitária (vida/saúde/dignidade em risco), prescrição próxima, impacto coletivo da tese
- **Para Defensor — risco humanitário separado:** vida/saúde em risco (BPC negado a idoso sem outra fonte, medicamento essencial não fornecido, despejo iminente com criança, violência doméstica em curso); prescrição em 30 dias para tese principal.

Se a calibração de risco em `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md` é fina, não simule precisão. Use o juízo do(a) usuário(a) e anote a finura.

### 5. Materialidade ou escalonamento institucional

**Para DJ corporativo:** contra os limiares da casa em `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md`:
- `provisionado | divulgado | monitorado | nenhum`
- Se `provisionado`: valor da provisão (CPC 25) e se o financeiro foi notificado
- Se `divulgado`: localização da divulgação no Formulário de Referência CVM (se companhia listada)

**Para Defensor Público:** escalonamento institucional:
- `escalado-DPG | escalado-coordenador | enviado-nucleo-especializado | monitorado | nenhum`
- Se `escalado-DPG`: tese inédita com impacto coletivo, acordo que renuncia parcela material do direito, TAC (Termo de Ajustamento de Conduta), Ação Civil Pública
- Se `enviado-nucleo-especializado`: qual núcleo (Saúde / Idoso / Consumidor / Mulher Maria da Penha / Fazenda Pública / Criminal / Infância / LGBTQIA+)

**Para autônomo / em-sociedade:** revisão por sócio ou consulta a co-counsel:
- `escalado-socio | consultado-co-counsel | monitorado | nenhum`

### 6. Escritório externo / DPs colaboradoras / núcleos especializados

**Para DJ / em-sociedade / autônomo — escritório externo:**
- Escritório
- Sócio líder
- **E-mail do sócio líder** (usado pelo `/oc-status` para redigir pedidos de status)
- Status de contrato de honorários: `assinado | pendente | nenhum`
- Autorização orçamentária: valor e aprovador
- *Doc-semente:* "Caminho do contrato de honorários, se assinado."

**Para Defensor — DP colaboradora ou núcleo especializado:**
- Núcleo (Saúde / Idoso / Mulher / Consumidor / Fazenda / Criminal / Infância / LGBTQIA+)
- Defensor(a) responsável do núcleo (se conhecido)
- Tipo de cooperação: `consultoria | atuação conjunta | encaminhamento | parecer técnico`
- Doc-semente: nota de encaminhamento ao núcleo, se já feita.

Se risco é médio ou maior e nenhum escritório externo / núcleo está atribuído — flag.

### 7. Owners internos

De `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md` panorama — quais stakeholders internos estão envolvidos?
- **DJ corporativo:** business lead, RH (se trabalhista), Comunicação (se reputacional), CISO (se dados/cyber), DPO (se LGPD).
- **Defensor:** outro(a) Defensor(a) com atuação subsidiária (substituição em férias, etc.); estagiário(a) sob supervisão (se aplicável); servidor(a) administrativo(a) responsável pela pasta; assessoria psicossocial da DP (caso envolva vítima de violência, criança, idoso(a)).
- **Em-sociedade:** sócio supervisor, outros(as) advogados(as) na equipe do caso.
- Outro

### 8. Dever de guarda documental

- Emitido? Se sim: data, escopo, custodiantes (lista de nomes).
- Próxima renovação (default: seis meses da emissão; ajuste por caso).
- Se não e isto é litigação ativa ou razoavelmente antecipada: flag urgente; ofereça rodar `/litigation-legal:legal-hold [slug] --issue` depois do intake completar.
- *Doc-semente:* "Comunicação de dever de guarda, se emitida."

Para Defensor: relevância menor (DP raramente é parte com dever de preservar provas corporativas), mas pode aplicar em ação coletiva ou em casos onde o(a) próprio(a) assistido(a) é detentor de documentos relevantes — orientar para preservação.

### 9. Datas-chave

- Prazo de resposta (contestação, impugnação, oposição, manifestação) — **em dias úteis (CPC art. 219)**, suspensão CPC art. 220 (20/12-20/1)
- **Para JEC (Lei 9.099/95):** dias corridos (jurisprudência STJ)
- **Para Defensor:** aplicar prazo em dobro CPC art. 186
- Próxima audiência / conciliação (CPC 334 — obrigatória salvo dispensa expressa)
- Corte de prescrição (se aplicável) — CC arts. 205-206
- Decadência (CC art. 178) — se aplicável
- Qualquer prazo administrativo

### 10. Postura inicial

Tese de um parágrafo:
- Qual nossa história?
- Qual a deles?
- Qual o fato-pivô?
- Postura inicial: `litigar | transacionar | investigar | aguardar | tutela-urgência-imediata` (último para Defensor em casos de risco humanitário grave)

## Escrevendo os outputs

### Slug

Minúsculo, hifens, ano no final. Exemplos: `acme-v-nos-2026`, `maria-s-medicamento-2026`, `silva-divorcio-2026`.

Confirme que o slug é único em `_log.yaml` antes de escrever.

### `~/.claude/plugins/config/claude-for-legal/litigation-legal/matters/[slug]/matter.md`

```markdown
[CABEÇALHO DE SIGILO — per plugin config ## Outputs — difere por papel; vide `## Quem está usando`]

# [Nome do Caso]

**Slug:** [slug]
**Aberto:** [AAAA-MM-DD]
**Nossa posição:** [autor / réu / etc.]
**Status:** [status]
**Vara da atribuição (Defensor):** [ex.: 19ª Vara Cível Comum — Capital de Manaus]

---

## Identificação

[contraparte, jurisdição/vara, tipo de caso, fonte, número CNJ se já distribuído]

## Conflitos / impedimentos institucionais

**Status:** [cleared / pending / not-run / waived]
**Método:** [defensoria-publica / corporate-legal / escritorio-externo / system-check / informal / outro]
**Cleared por:** [nome]
**Cleared em:** [AAAA-MM-DD]
**Checado contra:** [entidades rodadas + vedações LC 80/94 art. 46 se Defensor]
**Notas:** [qualquer flag cleared, referência de renúncia se aplicável]

## Triagem de risco

**Severidade:** [banda] — [por quê, com referência às definições de severidade da casa]
**Probabilidade:** [banda] — [por quê]
**Risk rating:** [alto/médio/baixo/crítico]
**Exposição R$:** [faixa]
**Risco humanitário (Defensor):** [descrição da urgência, se aplicável: vida, saúde, despejo iminente, violência, prescrição próxima]
**Exposição não-monetária:** [tutela inibitória, decisão coletiva, publicidade, impacto coletivo]

## Materialidade ou escalonamento

[provisionado/divulgado/monitorado/nenhum (DJ) — com valor da provisão e local de divulgação, ou racional se "nenhum"]
[escalado-DPG/escalado-coordenador/enviado-nucleo/monitorado/nenhum (Defensor) — com justificativa]

## Escritório externo / DP colaboradora / núcleo especializado

[escritório+sócio+contrato+budget (DJ/banca/autônomo)]
[núcleo+Defensor responsável+tipo de cooperação (Defensor)]

## Owners internos

[stakeholders e por que cada está envolvido]

## Dever de guarda

[status, data, escopo]

## Datas-chave

[lista — com cálculo CPC 219 dias úteis + prazo em dobro Defensor onde aplicável]

## Tese inicial

[um parágrafo: nossa história, a deles, fato-pivô, postura inicial] `[SME VERIFICAR — tese no intake é hipótese de trabalho; confirme com escritório externo / DP colaboradora / co-counsel antes de qualquer protocolização ou comunicação material que assuma este enquadramento]`

## Perguntas em aberto

[qualquer coisa ainda não sabida que importa — ex.: "aviso de sinistro pendente", "não está claro se temos cobertura para X", "aguardando laudo psicossocial"]

---

## Documentos-semente

| Doc | Caminho / ponteiro |
|---|---|
| [ex.: petição inicial / notificação / formulário social do(a) assistido(a)] | [caminho ou "ainda não compartilhado"] |
```

### `~/.claude/plugins/config/claude-for-legal/litigation-legal/matters/[slug]/history.md`

Semeie o arquivo de histórico com o intake como entrada zero:

```markdown
# Histórico: [Nome do Caso]

Log append-only de eventos. Mais recente no topo.

---

## [AAAA-MM-DD] — Caso aberto

[Fonte, quem trouxe (escala da unidade / referência), sumário da triagem inicial, escritório externo ou núcleo especializado atribuído, dever de guarda emitido sim/não, urgência humanitária se aplicável.]
```

### Anexar a `~/.claude/plugins/config/claude-for-legal/litigation-legal/matters/_log.yaml`

Adicione linha per o schema. Exemplo:

```yaml
- id: maria-s-medicamento-2026
  name: "Maria S. — fornecimento medicamento oncológico (TJAM 19ª Vara Cível)"
  type: civel-saude
  role: autor
  counterparty: "Estado do Amazonas + Município de Manaus"  # solidariedade Tema 793 STF
  jurisdiction: "TJAM — 19ª Vara Cível — Capital"
  numero_cnj: "0123456-78.2026.8.04.0001"
  # status é derivado da fonte:
  #   source: pre-suit-threat | demand-letter | atendimento-na-defensoria  → status: pre-protocolo
  #   source: complaint-served | subpoena | regulator-inquiry | peticao-inicial-citacao → status: ativo
  #   source: internal-report                           → status: pre-protocolo (default) ou ativo se processo formal já iniciou
  status: ativo
  stage: postulatoria
  source: atendimento-na-defensoria
  outside_counsel:            # para Defensor: núcleo especializado
    firm: "Núcleo de Saúde DPEAM"
    lead: "Dra. (Defensora do Núcleo)"
    email: "nucleo.saude@defensoria.am.def.br"
    engagement: nao-aplicavel
  conflicts:
    status: cleared
    method: defensoria-publica
    cleared_by: "[Defensor titular]"
    cleared_date: 2026-05-20
    override:                   # populado apenas em Caminho 3 bypass
      by: null
      date: null
      rationale: null
  risk: alto
  risco_humanitario: "Medicamento essencial (carboplatina) negado pelo SUS; ciclo oncológico interrompido; risco à vida"
  materiality: escalado-nucleo  # ou outro enum por papel
  exposure_range: "R$ 8K/mês (custeio mensal do tratamento)"
  internal_owners:
    business_lead: null         # N/A para Defensor
    defensor_substituto: "Dr. (suplente em férias)"
    assessoria_psicossocial: true
  legal_hold:
    issued: false
    issued_date: null
    scope: null
    custodians: []
    last_refresh: null
    next_refresh: null
    released: null
  related_matters: []
  opened: 2026-05-20
  prazo_em_dobro_defensor: true   # CPC 186 — todo prazo deste caso é dobrado
  next_deadline: 2026-05-25       # tutela de urgência CPC 300 — 5 dias úteis
  last_updated: 2026-05-20
  path: matters/maria-s-medicamento-2026/
```

## Confirmar antes de escrever

Mostre ao(à) usuário(a) a linha e o conteúdo do matter.md:

> Eis o que vou escrever. Flag qualquer coisa errada ou fina antes de eu comitar.

## Feche com a árvore de decisão de próximos passos

Termine com a árvore per CLAUDE.md `## Outputs`. Customize as opções ao que esta skill acabou de produzir — as cinco branches default (redigir o X, escalonar, pegar mais fatos, observar e esperar, outra coisa) são ponto de partida, não lock-in. A árvore É o output; o(a) Defensor(a)/advogado(a) escolhe.

## O que esta skill não faz

- **Rodar a checagem de conflitos / impedimentos em si.** Registra o resultado, status, método, e as entidades checadas. O clearance efetivo acontece em qualquer sistema (ou juízo) que o perfil da casa declara. Se a pessoa diz "cleared", a skill aceita e captura os metadados.
- Decidir a tese inicial. Captura o que a pessoa diz; não inventa.
- Emitir o dever de guarda. Flag se ausente. Usuário(a) emite.
- **Para Defensor:** não decide se o(a) assistido(a) é hipossuficiente — a presunção da Súmula 481 STJ rege; cabe ao(à) Defensor(a) registrar e à contraparte impugnar se quiser.
