---
name: draft
description: >
  Primeira minuta de documento da unidade — templates por área (petição
  inicial JEC Lei 9.099, petição inicial Comum CPC 319, contestação,
  recurso inominado, ofício institucional DP, notificação extrajudicial),
  formatação calibrada por vara, explicitamente ponto de partida exigindo
  análise do(a) estagiário(a) e revisão do(a) supervisor(a). Use quando
  estagiário(a) precisa de primeira minuta de petição, contestação, ofício,
  notificação, declaração ou outro documento da unidade.
argument-hint: "[tipo de documento — ex.: 'peticao-inicial-jec', 'contestacao-despejo', 'oficio-saude']"
---

# /draft

1. Carregue `~/.claude/plugins/config/claude-for-legal/legal-clinic/CLAUDE.md` → templates por área, jurisdição/vara, regras locais, modelo de supervisão.
2. Use o workflow abaixo.
3. Case tipo do documento com template. Colete fatos do(a) assistido(a) — flag faltantes, nunca chute.
4. Aplique formatação por vara. Minute com flags `[FATO NECESSÁRIO]`, `[VERIFICAR]`, `[INCERTO]` inline.
5. Output com rótulo de IA-assistida em destaque, checklist de revisão do(a) estagiário(a), roteamento para supervisão.

```
/legal-clinic:draft peticao-inicial-jec
```

```
/legal-clinic:draft oficio-saude-medicamento
```

---

# Draft: Geração de Primeira Minuta de Documento

## Propósito

Estagiários(as) gastam tempo enorme em primeiras minutas de documentos onde o valor pedagógico está na análise e estratégia, não em formatar endereçamento ou escrever "MM. Juiz(a)". Esta skill produz a primeira minuta a partir das notas do caso e templates por área para que o tempo do(a) estagiário(a) vá para o pensamento.

**Toda minuta é explicitamente ponto de partida.** Não é produto final. O(A) estagiário(a) analisa, revisa, e o(a) supervisor(a) revisa antes de qualquer protocolização.

## Carregar contexto

`~/.claude/plugins/config/claude-for-legal/legal-clinic/CLAUDE.md` → áreas, templates por área, jurisdição (UF + vara/comarca + provimentos da Corregedoria local ingest), modelo de supervisão.

Notas do caso ou sumário de intake para os fatos.

## Checagem pedagógica

Leia o guia do(a) supervisor(a) para esta área em `~/.claude/plugins/config/claude-for-legal/legal-clinic/guides/<area>.md`. Cheque a configuração `pedagogy_posture`:

- **`guide` (default):** Produz a estrutura e o checklist. Pede para estagiário(a) minutar cada seção. Dá feedback na minuta dele(a) (registro, nível de leitura, elementos obrigatórios, o que perdeu). Oferece preencher uma seção apenas quando estagiário(a) tentou uma vez.
- **`assist`:** Produz o produto. Flag itens para revisão. Estagiário(a) edita e aprende revisando.
- **`teach`:** Não produz o produto. Pede para estagiário(a) minutar. Dá feedback. Faz perguntas direcionadoras quando trava. Só mostra parágrafo modelo após duas tentativas, e só a seção em que trava. Rastreia o que acertou e errou para supervisor(a) ver progresso.

Se nenhum guia existe, use `guide`. Se o guia existe mas não seta postura, use `guide`.

Qualquer que seja a postura, o output sempre inclui: "**Modo pedagógico: [assist/guide/teach]** — setado pelo guia do(a) seu(sua) supervisor(a). Significa que [descrição do que o(a) estagiário(a) fez vs. o que a skill fez]."

**Suposição de jurisdição.** A minuta assume o UF, comarca/vara e regras locais setados no CLAUDE.md. Formato de endereçamento, requisitos de protocolização eletrônica (Lei 11.419/06 / e-SAJ TJAM), limites de página (se houver provimento), janelas de protocolização, e regras substantivas variam materialmente entre jurisdições e mesmo entre varas no mesmo tribunal. Se o caso é em vara diferente ou UF diferente, confirme com supervisor(a) antes de confiar em qualquer formato, prazo ou argumento.

## Workflow

### Passo 1: Que documento?

Case o pedido ao conjunto de templates da unidade (do `~/.claude/plugins/config/claude-for-legal/legal-clinic/CLAUDE.md`). Conjunto comum por área:

| Área de atuação | Documentos |
|---|---|
| **Família / Sucessões** | Petição inicial de divórcio (consensual / litigioso), alimentos, união estável, guarda, modificação de guarda; declaração de hipossuficiência |
| **Saúde Pública** | Petição inicial de fornecimento de medicamento (Tema 793 STF + Tema 106 STJ), leito UTI, internação compulsória, procedimento cirúrgico; pedido de tutela de urgência CPC 300; ofício administrativo prévio à secretaria de saúde |
| **Consumidor (JEC)** | Petição inicial Lei 9.099/95 (vício produto/serviço CDC 18-25; cobrança indevida CDC 42; recusa de fornecedor; publicidade enganosa CDC 37); contestação a ação de cobrança |
| **Previdenciário (JEF)** | Petição inicial BPC/LOAS; recurso administrativo INSS; petição de cumprimento de sentença |
| **Locação** | Contestação em ação de despejo (Lei 8.245/91); pedido de purgação da mora (art. 62 II) |
| **Possessória** | Petição inicial de reintegração / manutenção / interdito proibitório (CPC 554-568); contestação |
| **Defesa em cobrança** | Contestação CPC 335-342; embargos monitórios CPC 702; embargos à execução CPC 914-920 |
| **Geral** | Petição genérica, ofício institucional da DP, notificação extrajudicial, declaração, certidão |

Se o documento pedido não está no conjunto: "Os templates da unidade não incluem [X]. Posso tentar minuta a partir de princípios gerais, mas flag pesado — não foi afinado para sua área ou vara. Melhor perguntar ao(à) [Defensor(a)-Supervisor(a)] se há template existente."

### Passo 2: Colete os fatos

Leia o sumário de intake ou notas do caso. Para cada fato que o documento precisa: temos?

| Documento precisa | Tenho? | Fonte |
|---|---|---|
| [fato] | ✓ / ✗ | [intake / documento do(a) assistido(a) / preciso obter] |

Fatos obrigatórios faltantes → não chute. Marque: `[FATO NECESSÁRIO: data de início do contrato — obter da via assinada ou perguntar ao(à) assistido(a)]`.

### Passo 3: Aplique jurisdição

Per `~/.claude/plugins/config/claude-for-legal/legal-clinic/CLAUDE.md` jurisdição:

- **Formato de endereçamento:** padrão CNJ + regras locais da vara. Se provimentos locais foram ingest no cold-start, use. Se não, use padrão CNJ e flag: `[VERIFICAR ENDEREÇAMENTO: provimentos locais não carregados — confirme formato contra as regras vigentes da Corregedoria-Geral local]`.
   - Exemplo para vara DPEAM cível: `EXMO(A). SR(A). DR(A). JUIZ(A) DE DIREITO DA __ VARA CÍVEL DA COMARCA DE MANAUS — ESTADO DO AMAZONAS`
- **Requisitos de protocolização eletrônica:** sistema do tribunal (e-SAJ TJAM, PJe TRF, eproc TJDFT, etc.), Lei 11.419/06 art. 5º termo inicial. Vide regras de cada sistema.
- **Particularidades locais:** limites de página (se há provimento), tipo de letra (Padrão CNJ é Arial 12 ou Times 12), espaçamento, requisitos da Defensoria (timbre, assinatura digital). Aplique o que está ingest; flag o que não.
- **Prazos:** sempre dias úteis (CPC 219) salvo no rito sumaríssimo do JEC (Lei 9.099 — dias corridos por STJ); aplicar prazo em dobro Defensor (CPC 186) se aplicável.

### Passo 4: Minute

Use o template por área. Preencha o que pode ser preenchido com fatos. Deixe placeholders explícitos — nunca preencha com invenção plausível-sonora.

**Em todo lugar onde a minuta faz afirmação jurídica:** essa afirmação é hipótese que o(a) estagiário(a) verifica, não conclusão que a minuta garante. Marque conforme.

### Passo 5: Flag incerteza

Três tipos de flag, inline:

- `[FATO NECESSÁRIO: ...]` — o documento precisa de fato que as notas do caso não têm
- `[VERIFICAR: ...]` — alegação jurídica ou factual que precisa ser conferida antes de protocolar
- `[INCERTO: ...]` — a skill está genuinamente em dúvida e diz em vez de chutar

### Passo 6: Roteamento de supervisão

Protocolar documento ao juízo ou enviar a órgão administrativo (ofício DP) é ação consequente. O gate é o workflow de supervisão em `## Estilo de supervisão` no `~/.claude/plugins/config/claude-for-legal/legal-clinic/CLAUDE.md`, reforçado pela checagem de papel da Parte 0 que confirma supervisor(a) habilitado(a) é dono(a) do setup. Peças a protocolar sempre rotam por supervisão antes da protocolização, independentemente da escolha de estilo.

Per `CLAUDE.md` modelo de supervisão:
- **Fila formal:** minuta vai para fila, estagiário(a) vê "em fila para [supervisor(a)]"
- **Flags configuráveis:** se este tipo de documento é gatilho de flag (peças a protocolar geralmente são), output inclui "CHECAR COM [SUPERVISOR(A)] ANTES DE PROTOCOLAR"
- **Toque mais leve:** rótulo de salvaguarda padrão, sem gate adicional — mas peças a protocolar ainda vão ao(à) supervisor(a) antes da protocolização per estrutura existente

## Output

```markdown
═══════════════════════════════════════════════════════════════════════
  MINUTA ASSISTIDA POR IA — EXIGE ANÁLISE DO(A) ESTAGIÁRIO(A) E
  REVISÃO DO(A) SUPERVISOR(A)
  Este é ponto de partida, não produto final.
  Todo flag [VERIFICAR] e [FATO NECESSÁRIO] deve ser resolvido antes
  de protocolar.
═══════════════════════════════════════════════════════════════════════

[O documento — no formato do template por área, calibrado por vara,
com flags inline]

═══════════════════════════════════════════════════════════════════════

## Checklist de revisão do(a) estagiário(a)

Antes de mostrar ao(à) [supervisor(a)]:

- [ ] Leia o documento inteiro. Diz o que você quer que diga?
- [ ] Cada fato: é preciso per documentos efetivos do(a) assistido(a), não só per notas de intake?
- [ ] Cada flag [VERIFICAR]: resolvido com pesquisa ou cortado
- [ ] Cada flag [FATO NECESSÁRIO]: preenchido com informação verificada ou seção removida
- [ ] Tese jurídica: é o argumento certo? Há melhores? (Esta é sua análise, não da minuta.)
- [ ] Vara/jurisdição: endereçamento, protocolização, formato corretos per regras locais vigentes
- [ ] Prazo em dobro Defensor (CPC 186) computado se aplicável
- [ ] Hipossuficiência: pedido de gratuidade (CPC 98) + Súmula 481 STJ presente
- [ ] [Passo de supervisão per CLAUDE.md modelo]

## O que esta minuta NÃO faz

- Não decide estratégia. A minuta segue a abordagem mais comum para este tipo de documento — você decide se é certa para este(a) assistido(a).
- Não verifica as próprias asseverações jurídicas. Toda conclusão jurídica acima é hipótese até você pesquisar.
- Não se protocola. [Supervisor(a)] revisa, você protocola per procedimento da unidade (Sapiens-DPGU / sistema próprio / e-SAJ / PJe).

---

**Antes de sair da unidade.** Esta é minuta de estagiário(a) para revisão de supervisor(a) habilitado(a), não carta, peça, ou formulário final. Protocolizar com juízo ou órgão, ou enviar ao(à) assistido(a) ou contraparte, tem consequência jurídica para o(a) assistido(a). Supervisor(a) habilitado(a) revisa, edita, e assina antes de sair. Tire o cabeçalho de IA-assistida apenas depois desse sign-off. Não envie ou protocole esta minuta sem aprovação.

*Provimento OAB 205/2021 + Resolução CNJ 332/2020: uso de IA na advocacia exige competência, supervisão e verificação. Esta minuta é desenhada para ser supervisionada e verificada — não é desenhada para ser confiada sem isso.*
```

## O que esta skill NÃO faz

- **Produz produto final.** Primeira minuta apenas. Estagiário(a) revisa, supervisor(a) revisa.
- **Chuta fatos faltantes.** Flag para estagiário(a) obter.
- **Decide a tese jurídica.** Usa a abordagem comum; estagiário(a) decide se é a certa.
- **Substitui pesquisa específica da jurisdição/vara.** Aplica regras locais ingest; flag onde regras não foram ingest ou podem ter mudado.
