---
name: client-intake
description: >
  Intake estruturado do(a) assistido(a) — templates por área de atuação da
  Defensoria Pública (Família/Sucessões, Consumidor, Saúde, BPC/LOAS,
  Locação, Possessória, Defesa em cobrança), identificação cruzada de
  pretensões, flags de impedimento institucional, classificação de triagem
  por urgência humanitária. Produz sumário de caso formatado que o(a)
  estagiário(a) analisa e o(a) Defensor(a)-Supervisor(a) revisa. NÃO decide
  aceitação. Use ao iniciar atendimento novo, rodar entrevista de intake,
  ou escrever situação de novo(a) assistido(a).
argument-hint: "[opcional: dica de área de atuação]"
---

# /client-intake

1. Carregue `~/.claude/plugins/config/claude-for-legal/legal-clinic/CLAUDE.md` → áreas de atuação, templates de intake, modelo de supervisão, gatilhos de flag.
2. Use o workflow abaixo.
3. Rote para template por área. Escute pretensões cruzadas ao longo do atendimento.
4. Flags de impedimento. Classificação de triagem.
5. Output: sumário formatado com rótulo de IA-assistida, pedidos de verificação, roteamento para supervisão.

```
/legal-clinic:client-intake
```

---

# Intake do(a) Assistido(a)

## Propósito

Intake é um dos maiores gargalos em unidades de DP e NPJ. Um(a) estagiário(a) pode gastar 45 min entrevistando, mais 1 hora escrevendo, mais tempo identificando pretensões. Enquanto isso, a fila do acolhimento cresce.

Esta skill estrutura a conversa, produz a redação, identifica pretensões entre áreas, e flag impedimentos — para que o tempo do(a) estagiário(a) vá para análise, não transcrição.

**O que NÃO faz:** decide se o caso é atendido. Isso é análise do(a) estagiário(a) e juízo do(a) Defensor(a)-Supervisor(a) ou Professor(a)-Orientador(a). Claude acelera a coleta de informação e estruturação, não a advocacia.

## Carregar contexto

`~/.claude/plugins/config/claude-for-legal/legal-clinic/CLAUDE.md` → áreas de atuação, templates de intake (por área se múltiplas), modelo de supervisão, vara/jurisdição, gatilhos de flag.

## Leia o guia do(a) supervisor(a)

Cheque por guia por área em `~/.claude/plugins/config/claude-for-legal/legal-clinic/guides/<area>.md`. Se um existe, use suas perguntas de intake, bandeiras vermelhas e critérios de atendimento em vez dos defaults genéricos abaixo. Se não existe, use intake genérico e nota no fim do sumário: "Foi intake genérico — seu(sua) supervisor(a) pode afinar perguntas para sua unidade com `/legal-clinic:build-guide`."

Quando o intake começa antes da área ser roteada (Passo 1 do workflow), re-cheque o guia após rotear — o caminho do guia depende de qual área o intake caiu.

## Workflow

### Passo 1: Roteamento por área

Qual área o(a) assistido(a) traz? O(A) assistido(a) pode não saber — sabe o problema dele(a), não a categoria jurídica.

> "Me conta o que está acontecendo — o que te trouxe à Defensoria hoje?"

Da resposta, rote para o template apropriado. Se a unidade trata múltiplas áreas e o problema atravessa (assistida de Família menciona violência doméstica, assistido de Consumidor menciona despejo iminente), note todas — identificação cruzada de pretensões é feature, não bug.

### Passo 2: Intake específico por área

Cada área pergunta diferente. Use o template em `~/.claude/plugins/config/claude-for-legal/legal-clinic/CLAUDE.md` para esta área. Defaults se nenhum:

**Família / Sucessões:**
- Relação (cônjuge, união estável, ex-companheiro(a), pais separados, filiação)
- Filhos envolvidos — idades, arranjo atual de guarda
- Segurança: há violência, ameaça, medo? (cuidado — vide flags cruzadas Lei Maria da Penha)
- Ordens judiciais existentes (medidas protetivas, alimentos provisórios)
- Patrimônio / partilha em discussão
- Hipossuficiência: comprovação de renda (até 3 SM tipicamente para presunção)
- Urgência: alguma audiência marcada? Prescrição próxima?

**Saúde Pública (CF art. 196):**
- Pretensão: medicamento (cite o nome e princípio ativo), leito UTI, internação (CAPS / clínica), procedimento cirúrgico, exame, terapia
- Prescrição médica (preferencialmente do SUS — mas privada também serve com fundamentação)
- Tentativa administrativa (SUS Estadual / Municipal contatado? Negativa por escrito?)
- Quadro clínico (laudo médico — quando emitido, gravidade, prognóstico, urgência)
- Custo privado se não obtido pelo SUS (alegação de inviabilidade financeira)
- Tema 793 STF aplicável (solidariedade entre União, Estado, Município)
- Tema 106 STJ aplicável (requisitos para medicamento não-RENAME)

**Consumidor (CDC, JEC se ≤ 40 SM):**
- Tipo de problema (vício de produto, vício de serviço, cobrança indevida, cobrança vexatória CDC 42, recusa de fornecedor, publicidade enganosa)
- Relação de consumo confirmada (CDC arts. 2º + 3º)
- Documentação: nota fiscal, contrato, recibo, faturas, prints de mensagens
- Tentativa anterior de resolução (reclamação no Procon? Carta? Telefonema?)
- Inscrição em cadastro de inadimplentes (SPC/Serasa)?
- Urgência: prazo de prescrição (CDC 26 — 30/90 dias para vício; 27 — 5 anos para fato do produto)

**Previdenciário (BPC/LOAS — Justiça Federal):**
- Idade ≥ 65 anos OU deficiência (impedimento de longo prazo)
- Composição familiar e renda per capita (≤ 1/4 SM regra; Tema 27 STF permite outros critérios)
- Cadastro CadÚnico atualizado? Quando?
- Prévio requerimento administrativo INSS (Tema 350 STF — condição da ação)
- Documentação: RG, CPF, CadÚnico, laudo médico se deficiência, comprovante de renda da família
- Encaminhamento à DPU se DPE estadual não tem competência

**Locação (Lei 8.245/91) — defesa em despejo:**
- Tipo de contrato (residencial / não-residencial / temporada)
- Causa do despejo (falta de pagamento? Denúncia vazia? Infração contratual? Outras hipóteses art. 9º)
- Notificação recebida (data, conteúdo)
- Valores em discussão (alugueres em atraso, encargos)
- Possibilidade de purgação da mora (Lei 8.245 art. 62 II — depósito de aluguel + multa + custas + honorários)
- Vícios do imóvel (CDC se relação de consumo + Lei 8.245)
- Urgência: prazo para purgação ou para contestar; data da audiência

**Possessória:**
- Tipo: reintegração (já perdeu posse), manutenção (turbação parcial), interdito proibitório (ameaça)
- Data do esbulho/turbação (força nova até 1 ano CPC 558; força velha após — rito comum)
- Prova de posse anterior (documentos, contas em nome, testemunhas)
- Atual ocupação por terceiro
- Risco de desocupação iminente

**Defesa em ação de cobrança:**
- Origem da dívida alegada
- Já houve pagamento (parcial ou integral)? Documentação
- Tese de defesa cabível (prescrição CC 206; inexigibilidade; cobrança indevida CDC 42)
- Prazo: 15 dias úteis para contestar (CPC 335) + dobro para Defensor (CPC 186 = 30 dias úteis)

### Passo 3: Identificação cruzada de pretensões

Enquanto roda o template por área, escute pretensões fora dessa área:

| Assistido(a) diz | Também flag |
|---|---|
| "Tenho medo do meu marido / ex" | Lei Maria da Penha — mesmo em intake de Família ou Consumidor; flag para medida protetiva urgente |
| "Meu filho não quer voltar do pai dele" | ECA — possível conflito de guarda + envolvimento do Conselho Tutelar |
| "Estou sem o medicamento há semanas" | Saúde pública — mesmo em intake de outra área |
| "Não consigo trabalhar por causa do problema de saúde" | Possível BPC/LOAS + pretensão previdenciária |
| "Me cortaram a água / luz / gás" | Consumidor + possível concessionária + dignidade da pessoa humana CF 1º III |
| "O patrão me pôs na rua sem pagar" | Trabalhista — encaminhar ao Núcleo Trabalhista da DP se houver; ou Justiça do Trabalho |
| "Meu vizinho está construindo no meu terreno" | Possessória + possível arbitramento de honorários periciais (se Justiça Comum) |
| "Foram me cobrar uma dívida de 5 anos atrás" | Consumidor + prescrição CC 206 + dano moral CDC 42 |

Note toda pretensão cruzada no sumário. A unidade pode tratar, encaminhar (a outra DP, ao Núcleo Especializado, ao Conselho Tutelar, ao CRAS/CREAS), ou ambos — chamada do(a) supervisor(a). O(A) estagiário(a) deve ver.

### Passo 4: Flags de impedimento institucional

Per o processo de checagem que `~/.claude/plugins/config/claude-for-legal/legal-clinic/CLAUDE.md` descreve. No mínimo:

- Nome(s) da contraparte — a unidade representa ou já representou?
- Partes relacionadas — alguém mais com quem estagiário(a) ou unidade pode ter conflito?
- Impedimento posicional — este caso pede algo que prejudicaria outro(a) assistido(a) da unidade?
- **Vedações institucionais (Defensor — LC 80/94 art. 46):** o(a) Defensor(a) responsável tem relação privada com a contraparte? Está em hipótese de impedimento (LC 80/94 art. 134 + CPC 144-148)?
- **Para NPJ:** algum(a) estagiário(a) ou professor(a) tem relação com a contraparte?

Flag para revisão do(a) supervisor(a). Não resolva o impedimento — surface.

### Passo 5: Classificação de triagem

Não é decisão de aceitação — input para triagem:

| Classificação | Significa |
|---|---|
| **Urgente** | Prazo em dias úteis, urgência humanitária (vida/saúde/violência), dano irreversível iminente |
| **Tempo-sensível** | Prazo em semanas, dano em curso mas não imediatamente irreversível |
| **Padrão** | Sem prazo imediato, pode entrar na fila normal |
| **Pode estar fora do escopo** | Pretensão fora das áreas da unidade — flag para encaminhamento a outra DP ou núcleo |

### Passo 6: Checagem de flag de supervisão

Per `~/.claude/plugins/config/claude-for-legal/legal-clinic/CLAUDE.md` modelo de supervisão e gatilhos. Se fila formal ou flags configuráveis habilitados, e gatilho presente (prazo mencionado, indicador Lei Maria da Penha, criança em risco, saúde mental, criminal por escala, etc.), note a flag.

### Passo 7: Handoff de prazo — entregável obrigatório

Se o intake surface qualquer prazo (contestação, audiência, prescrição, decadência, purgação, intimação para responder, etc.), **emita um bloco `/legal-clinic:deadlines --add ...` pronto para copiar-colar como parte do output**. Isto é entregável obrigatório, não sugestão — o intake identifica prazos, e o(a) estagiário(a) não deve ter que re-transcrever para a skill de prazos.

Formate cada prazo como bloco de código cercado para o(a) estagiário(a) copiar, com cada campo pré-populado:

```
/legal-clinic:deadlines --add
  case=[slug do caso ou palavra-chave-sobrenome]
  type=[contestacao|audiencia|prescricao|decadencia|purgacao|notificacao|intimacao|cumprimento|outro]
  description="[descrição em uma linha do que vence]"
  due=[VERIFICAR — estagiário(a) + supervisor(a) calculam do evento gatilho com CPC 219 dias úteis + dobro Defensor CPC 186]
  source="[evento gatilho + cite de lei/dispositivo, ex.: 'Ação de despejo Lei 8.245 art. 62, II, citado em 2026-05-04, prazo 15 dias úteis para purgação']"
  owner=[nome do(a) estagiário(a)]
  warnings=[14,7,3,1]
  prazo_em_dobro_defensor=true  # se aplicável
```

Regras:
- Um bloco por prazo identificado. Não combine. Cada um passa pela checagem de duplicação pré-add da skill de prazos.
- Deixe `due=` como `[VERIFICAR — estagiário(a) + supervisor(a) calculam]` quando o prazo é jurisdicional (contestação, prescrição, prazo de janela específica). A skill de prazos não calcula por você; estagiário(a) + supervisor(a) fazem a conta e atualizam a entrada.
- Quando uma data é dada no documento gatilho (data de audiência em intimação), coloque no `due=`. Quando a data é calculada (contar N dias úteis do evento), deixe o marcador `[VERIFICAR]`.
- Se nenhum prazo é identificado no intake, omita esta seção — não fabrique.

## Output

```markdown
# Sumário de Intake: [Nome do(a) Assistido(a) ou ID]

---
[MINUTA ASSISTIDA POR IA — exige análise do(a) estagiário(a) e revisão do(a) supervisor(a)]

**Sigilo do(a) assistido(a) (LC 80/94 art. 4º-A V) + sigilo profissional (Lei 8.906/94 art. 7º XIX).** Este sumário deriva de comunicações com o(a) assistido(a) sob sigilo. Distribuir fora do círculo de sigilo (incluindo fora da unidade) pode constituir infração ético-disciplinar. Armazene em local com controle de acesso (pasta privada, sistema interno Sapiens-DPGU ou similar), marque adequadamente, e decisões de distribuição em consulta com supervisor(a).
---

**Data:** [data] | **Intake por:** [estagiário(a)] | **Área de atuação:** [primária + qualquer cruzada]

## Bottom line

[Atender o caso / Recusar porque X / Precisar de mais informação sobre Y — próximo passo é Z]

## Situação do(a) assistido(a) (nas palavras dele/dela)

[A narrativa que o(a) assistido(a) deu, antes da categorização jurídica. Esta é a história humana.]

## Pretensões jurídicas identificadas

*Cada citação de dispositivo, súmula, Tema ou julgado nesta seção carrega tag de proveniência (vide guardrails compartilhados do CLAUDE.md para o vocabulário de tag). `[usuário forneceu]` se o(a) supervisor(a) subiu o texto, `[lei / planalto.gov.br]` se você puxou nesta sessão de fonte oficial, tag de MCP de pesquisa (`[BNP]`, `[TJAM]`, etc.) se veio de resultado de ferramenta nesta conversa, `[conhecimento do modelo — verificar]` caso contrário. Default é `[conhecimento do modelo — verificar]`. Defensor(a)-Supervisor(a) que não pode verificar contra conector precisa ver a tag para saber o que conferir primeiro.*

### Primária ([área])
- [Pretensão 1]: [uma linha com qualquer cite taggada, ex.: "CC art. 1.694 + Súmula 358 STJ `[conhecimento do modelo — verificar]`"]
- [Pretensão 2]: [uma linha]

### Pretensões cruzadas
- [Outra área]: [o que o(a) assistido(a) disse que levantou]
  [INCERTO: se a unidade trata ou encaminha — chamada do(a) supervisor(a)]

## Fatos-chave

| Fato | Fonte | Documentação |
|---|---|---|
| [fato] | [declaração do(a) assistido(a) / documento juntado] | [tenho / preciso obter] |

## Hipossuficiência (Defensor — Súmula 481 STJ)

**Presumida:** [sim / não — Defensor presume; coleta documental é confirmatória, não constitutiva]
**Comprovação documental disponível:** [CadÚnico / comprovante renda / outros]
**Renda familiar per capita declarada:** [R$]

## Checagem de impedimento

**Contraparte:** [nome(s)]
**Partes relacionadas:** [se houver]
**Vedações institucionais (Defensor LC 80/94 art. 46):** [confirmar — relação privada? parecer remunerado anterior?]
**Flag:** [clear / precisa checagem contra base interna ou consulta ao(à) supervisor(a)]

## Triagem

**Classificação:** [Urgente / Tempo-sensível / Padrão / Pode estar fora do escopo]
**Prazo direcionador:** [se houver — data e o que é]
**Urgência humanitária:** [se aplicável — vida, saúde, despejo iminente, violência]

## Prazos a logar

[Um bloco `/legal-clinic:deadlines --add ...` por prazo identificado — Passo 7. Se nenhum, omita esta seção.]

## Notas jurisdicionais

*Cada dispositivo, súmula, regra ou julgado nesta seção carrega tag de proveniência — mesmo vocabulário de `## Pretensões jurídicas identificadas`. Default `[conhecimento do modelo — verificar]`. Quando nenhum MCP de pesquisa está conectado nesta sessão, registre na linha **Fontes:** da nota do revisor — não emita banner separado.*

[Questões específicas do estado, da comarca, ou da vara relevantes a este tipo de caso, per CLAUDE.md jurisdição, com cada cite taggado. Para DPEAM: vara da atribuição da unidade — 1ª/12ª JEC ou 19ª/20ª Cível Comum, etc.]

## Flags de supervisão

[Se modelo de supervisão inclui flags: quais dispararam e por quê. Se fila formal: "EM FILA para [supervisor(a)]."]

---

## Pedidos de verificação para o(a) estagiário(a)

Antes da análise, verifique:
- [ ] [Fato específico em que o intake se baseia — confirme com assistido(a) ou documentos]
- [ ] [Data de prazo — confirme do documento real, não da memória do(a) assistido(a)]
- [ ] [Qualquer conclusão jurídica acima é hipótese de partida — pesquise antes de confiar]
- [ ] Hipossuficiência: declaração assinada pelo(a) assistido(a) presente na pasta

## O que este sumário NÃO faz

Este sumário não decide se a unidade atende este caso. Isto é sua análise e juízo do(a) Defensor(a)-Supervisor(a). Estrutura o que o(a) assistido(a) te contou para você gastar seu tempo na análise em vez da redação.
```

## Referências de templates por área

Armazene conjuntos específicos de pergunta por área em `references/intake-templates/[area].md`. Cold-start populou destes do formulário do(a) supervisor(a); se nenhum, use defaults acima.

## O que esta skill NÃO faz

- **Decide aceitação.** Estagiário(a) analisa, Defensor(a)/Professor(a)-Supervisor(a) decide.
- **Resolve impedimentos.** Flag para o(a) supervisor(a).
- **Dá orientação durante intake.** Intake é coleta; orientação vem após análise e revisão do(a) supervisor(a).
- **Produz documento final.** O sumário é ponto de partida — o(a) estagiário(a) lê, corrige qualquer caracterização errada, e constrói a análise a partir dele.

## Feche com árvore de decisão de próximos passos

Termine com a árvore per CLAUDE.md `## Outputs`. Customize ao que esta skill acabou de produzir.
