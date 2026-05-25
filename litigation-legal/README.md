# Plugin Contencioso (Brasil)

Apoio ao advogado de contencioso para gerenciar um portfólio de casos. O cold-start captura sua calibração de risco, panorama de litigiosidade e estilo da casa — o frame contra o qual cada caso é triado. O intake padronizado transforma novos casos em entradas estruturadas no log e arquivos de histórico por caso. Os rollups de status e briefings aprofundados leem do log.

Construído para quem controla vários casos simultaneamente: **Defensor(a) Público(a) responsável por unidade com múltiplas varas**, **advogado(a) de DJ corporativo** coordenando escritórios externos, **sócio(a) de banca** ou **advogado(a) autônomo(a)**. Este plugin é um parceiro de raciocínio, não um sistema de gestão de processos. Se você usa Sapiens-DPGU, LegalDesk, Themis, Projuris, Astrea, ADVBOX, LawDesk ou Tikal Tech — isto não substitui. Fica ao lado, como sua camada estruturada de raciocínio.

**Cada saída é uma minuta para revisão do advogado responsável — citada, sinalizada e com travas — não é parecer jurídico.** O plugin executa o trabalho: lê os documentos, aplica seu playbook, identifica os pontos, redige o memorando. Um advogado habilitado revisa, verifica e decide. As citações vêm marcadas por fonte para você saber quais vieram de ferramenta de pesquisa e quais precisam ser checadas. Marcas de sigilo são aplicadas de forma conservadora para nada vazar por acidente. Ações consequenciais — protocolar, enviar, executar — exigem confirmação explícita.

## Pré-requisitos

Vários recursos referenciam integrações de Gmail e tarefas agendadas. Essas exigem servidores MCP configurados no seu ambiente — não vêm embutidos. Sem eles, as saídas são gravadas em arquivos para envio manual:

- **Gmail MCP** — `/oc-status` cria rascunhos no Gmail se autenticado; caso contrário, escreve minutas markdown em `oc-status/[YYYY-MM-DD]/[slug].md`.
- **Tarefas agendadas MCP** — nada de agendamento automático vem embutido. Configure um lembrete recorrente de calendário para rodar os comandos semanais.

O plugin funciona ponta a ponta sem qualquer integração — elas são aditivas.

## Para quem é

| Papel | Uso principal |
|---|---|
| **Defensor(a) Público(a) (membro de unidade)** | Portfólio por vara da atribuição (JEC e/ou Vara Comum), intake do(a) assistido(a), teses repetitivas, prazos do CPC em dias úteis, comunicação ao Defensor Público-Geral em casos específicos |
| **Defensor(a)-Supervisor(a) de estágio** | Tudo do anterior + supervisão de estagiários(as) (ver também o plugin `legal-clinic`) |
| **Advogado(a) de DJ (contencioso interno)** | Tudo — intake, triagem, status, histórico, briefings |
| **Coordenador / Head Jurídico** | Visão de portfólio, rollups para diretoria/conselho |
| **Diretor Jurídico** | Status rápido do portfólio, deep dive em qualquer caso |
| **Sócio / advogado em sociedade** | Carteira de casos por cliente, status para sócio sênior |
| **Advogado autônomo / banca pequena** | Caseload pessoal, contrato de honorários, comunicação com cliente |

## Primeira execução: cold-start

A entrevista de cold-start escreve o perfil-casa de atuação — persistente em todos os casos. Três pilares:

- **Calibração de risco** — apetite, limiares de materialidade, gatilhos de provisão CPC 25 (in-house) / valoração de causa (autônomo) / priorização de urgência humanitária e risco de prescrição (Defensor), alçada de transação, perfil de seguros (quando aplicável), matriz de severidade × probabilidade
- **Panorama** — unidade/empresa/cliente típico, áreas geográficas, varas atendidas (Defensor), status regulatório, padrões de demandas, contrapartes frequentes, bancas externas / núcleos especializados / DPs colaboradoras de referência, stakeholders internos
- **Estilo da casa** — formato de memo para diretoria/conselho / comunicação ao Defensor Público-Geral, formato de memo de provisão, estilo de instrução para escritório externo / DP colaboradora, convenções de sigilo, normas de escalonamento

Oferece padrões sensatos em cada passo (ex.: matriz 3×3 de severidade-probabilidade) e mantém tudo editável livremente. Se você não tem um framework escrito ainda, esta é a etapa que força a articulação.

```
/litigation-legal:cold-start-interview
```

Sua configuração fica em `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md` e sobrevive às atualizações do plugin.

## Comandos

| Comando | Função |
|---|---|
| `/litigation-legal:cold-start-interview` | Cold-start → escreve perfil-casa em `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md` |
| `/litigation-legal:matter-intake` | Intake padronizado → escreve `matters/[slug]/` + acrescenta ao `_log.yaml` |
| `/litigation-legal:portfolio-status` | Rollup de portfólio — distribuição de risco, prazos próximos, casos parados |
| `/litigation-legal:matter-briefing [slug]` | Briefing aprofundado de um caso — pronto para leitura antes de reunião com cliente / sócio / escritório externo |
| `/litigation-legal:matter-update [slug]` | Acrescenta evento datado ao histórico do caso; atualiza `last_updated` no log |
| `/litigation-legal:matter-close [slug]` | Arquiva o caso fora do portfólio ativo (mantido, não excluído) |
| `/litigation-legal:demand-intake [título]` | Coleta de contexto pré-redação de notificação extrajudicial (pagamento / inadimplemento / cessar-e-desistir / rescisão de emprego / preservação documental) |
| `/litigation-legal:demand-draft [slug]` | Redige a notificação a partir do intake — roda gate de confidencialidade negocial (Lei 13.140/2015) / sigilo, gera `.docx`, escreve checklist pós-envio |
| `/litigation-legal:demand-received [path]` | Triagem de notificação extrajudicial recebida — análise de opções, cruzamento com portfólio, encaminhamento para criação de caso |
| `/litigation-legal:subpoena-triage [path]` | Triagem de intimação / ofício / requisição — classifica, analisa escopo/ônus/sigilo, framework de objeções, plano de cumprimento |
| `/litigation-legal:legal-hold [slug] [--issue/--refresh/--release/--status]` | Emite, renova, libera ou reporta dever de guarda documental — escreve `.docx` + atualiza log |
| `/litigation-legal:chronology [slug]` | Constrói ou atualiza cronologia a partir das fontes documentais declaradas + uploads — marcada por relevância conforme a tese do caso |
| `/litigation-legal:oc-status` | Redige e-mails semanais de status pedido ao escritório externo, em todo o portfólio; rascunhos no Gmail se MCP disponível |
| `/litigation-legal:claim-chart` | Constrói ou revisa matriz de elementos — matriz de patente (infração / nulidade / revisão, sob LPI 9.279/96) ou matriz cível (qualquer causa de pedir ou defesa) com detecção de lacunas |

## Skills

| Skill | Propósito |
|---|---|
| **cold-start-interview** | Perfil-casa — calibração de risco, panorama, estilo |
| **matter-intake** | Perguntas padronizadas de intake; escreve arquivo do caso + linha no log |
| **portfolio-status** | Rollup no log — risco, prazos, casos parados |
| **matter-briefing** | Leitura aprofundada de um caso a partir do arquivo + histórico |
| **matter-update** | Acréscimo estruturado de evento; atualiza `last_updated` no log |
| **matter-close** | Arquivamento; captura desfecho |
| **demand-intake** | Coleta adaptativa de contexto para notificação extrajudicial — partes, fatos, alavanca, filtros de sigilo |
| **demand-draft** | Gate de confidencialidade negocial / sigilo, depois redige `.docx` com placeholders `[CITE:___]`; escreve checklist pós-envio; oferece criação do caso |
| **demand-received** | Triagem de notificação recebida — mérito, opções, cruzamento com portfólio |
| **subpoena-triage** | Classifica intimação/ofício/requisição, analisa escopo/ônus/sigilo, produz framework de objeções + plano de cumprimento |
| **legal-hold** | Emite / renova / libera / relata dever de guarda; escreve `.docx` de comunicação; atualiza campos `legal_hold` do log |
| **chronology** | Extrai eventos datados das fontes declaradas + uploads; deduplica; marca relevância conforme tese |
| **oc-status** | Redator semanal de e-mails de status pedido a escritórios externos, em todo o portfólio; markdown + Gmail drafts |
| **claim-chart** | Matriz de patente (infração / nulidade / revisão, sob LPI) ou matriz cível (qualquer causa de pedir ou defesa). Mapeamento elemento por elemento, cada célula com citação pinpoint, detecção de lacunas. Vem com biblioteca-modelo de causas de pedir brasileiras. |

## Comandos interativos vs. agentes agendados

Os comandos acima rodam quando você os invoca — para quando você está trabalhando um caso. Os agentes abaixo rodam em cadência — para o que se move enquanto você não está olhando:

| Agente | O que observa | Cadência padrão |
|---|---|---|
| **docket-watcher** | Modo jurisprudência: monitora mudanças em precedentes (STF/STJ/tribunais) relevantes às suas teses via JusRatio (overruling, novas súmulas, repetitivos); informa quando uma tese muda. Modo andamentos: placeholder — Brasil não tem MCP nativo de PJe / eproc / ESAJ / Projudi; integração com plataformas comerciais (Escavador, Jusbrasil PRO, Astrea, Projuris) é manual. | Semanal |

## Como os dados são organizados

```
litigation-legal/
├── CLAUDE.md                          # Perfil-CASA — risco, panorama, estilo
├── matters/
│   ├── _log.yaml                      # Ledger do portfólio (uma entrada por caso)
│   └── [slug-do-caso]/
│       ├── matter.md                  # Intake específico do caso + tese + posição
│       ├── history.md                 # Log append-only de eventos
│       ├── chronology.md              # Cronologia advocatícia (sob demanda)
│       └── legal-hold-v[N].docx       # Comunicações de dever de guarda (emissão, renovação, liberação)
├── demand-letters/                    # Notificações extrajudiciais expedidas
│   └── [slug]/
│       ├── intake.md
│       ├── draft-v1.docx
│       └── checklist.md
├── inbound/                           # Notificações recebidas, intimações/ofícios, ofícios de órgãos
│   └── [slug]/
│       ├── incoming.[ext]
│       ├── triage.md
│       └── response-v1.docx           # Se respondermos
└── oc-status/                         # Rascunhos semanais de pedido de status ao escritório externo
    └── [YYYY-MM-DD]/
        ├── _summary.md
        └── [slug].md                  # Um e-mail por caso
```

Pastas separadas porque cada uma tem fluxo distinto. Casos entram no portfólio; notificações e itens recebidos podem ou não virar caso; pedidos de status ao escritório externo são artefatos periódicos. Quando se relacionam, o campo `related_matters` e cross-links em `matter.md` os amarram.

O log é YAML porque é parseável pelas skills de rollup. Arquivos por caso são markdown porque é onde você lê e edita. Ambos são versionáveis em texto puro — nada proprietário.

## Conectores e verificação de citações

**Conecte uma ferramenta de pesquisa primeiro — os guardrails de citação dependem dela.** Sem ela, cada citação é marcada `[verificar]` e a nota do revisor acima de cada entregável registra que as fontes não foram verificadas. O plugin funciona de qualquer jeito; ele só faz mais da verificação por você quando há ferramenta de pesquisa conectada.

Os conectores de pesquisa neste plugin não são apenas fontes de dados — eles fazem a diferença entre uma citação verificada e uma que você precisa checar. Uma citação obtida via **JusRatio** (jurisprudência brasileira — STF, STJ, tribunais estaduais; níveis de autoridade A/B/C/D/E, timeline de decisões, overruling por tema, busca de legislação, informativos) é marcada com a fonte e pode ser rastreada. Uma citação do conhecimento do modelo ou de busca web é marcada `[verificar]` ou `[verificar-pinpoint]` e deve ser conferida contra a fonte primária antes que alguém confie nela. O plugin estratifica as citações para que seu tempo de verificação vá onde importa.

**Priorize níveis A e B do JusRatio** ao fundamentar: A = vinculante forte (Súmula Vinculante, ADI/ADC/ADPF); B = precedente qualificado (Tema Repetitivo STJ, Repercussão Geral STF).

## Integrações

Vem com o conjunto geral de conectores em `.mcp.json`:

- **Slack** — busca mensagens, lê canais, encontra discussões
- **Google Drive** — busca, lê e recupera documentos
- **JusRatio** — jurisprudência brasileira ranqueada por relevância semântica e autoridade

Projetado para ser útil sem nada conectado. Quando/se quiser puxar do PJe / eproc / ESAJ / Projudi / Escavador / Jusbrasil PRO / Astrea / DMS interno / e-mail, skills de integração podem ser adicionadas sem mudar a arquitetura central. **Não há MCP nativo para sistemas processuais brasileiros (PJe, eproc, ESAJ, Projudi) no momento** — a leitura de andamentos é manual ou via terceiros comerciais.

## Como o plugin aprende

Seu perfil de atuação em `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md` não é estático — melhora conforme você usa. As skills avisam quando um output usou um default que você deveria afinar. Você pode re-rodar o setup, editar o arquivo direto ou pedir para uma skill registrar uma nova posição.

## Notas

- Toda skill lê de `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md` primeiro. Se seu apetite ao risco muda ou um novo escritório externo entra na carteira, atualize lá — não maquile no caso individual.
- `## Company profile` (ou `## Perfil do cliente` para autônomos) é a primeira seção por convenção. Se você roda outros plugins `-legal`, copie para evitar redigitar o mesmo contexto.
- `_log.yaml` é a fonte da verdade para estado do portfólio. Mantenha limpo.
- Histórico do caso é append-only. Se algo estava errado, registre a correção como nova entrada — não edite o passado.
- Casos encerrados ficam em `_log.yaml` (histórico pesquisável). `/portfolio-status` os filtra dos rollups ativos por padrão.

## Convenções de marcas inline

Três marcas aparecem em outputs de skill e em minutas. Não são ressalvas — são itens de ação:

- `[CITE: citação específica necessária]` — placeholder para autoridade legal. O advogado preenche ou confirma antes de enviar.
- `[VERIFICAR: fato específico]` — alegação factual ainda não confirmada à fonte. O advogado verifica antes de confiar.
- `[SME VERIFICAR: juízo específico]` — juízo (leitura de mérito, marcação de relevância, força de objeção, status de sigilo) que requer revisão de especialista. SME = advogado habilitado na área/jurisdição. Usada liberalmente — qualquer coisa carregada de juízo deve carregar essa marca.

Uma minuta ou triagem com marcas não resolvidas não é final, por mais polida que pareça.

## Provimento OAB 205/2021 — Uso de IA na advocacia

Este plugin é ferramenta de apoio. O Provimento OAB 205/2021 e a Resolução CNJ 332/2020 impõem ao advogado:

- **Dever de revisão**: toda peça gerada com auxílio de IA deve ser revista pelo advogado antes de protocolar/enviar
- **Transparência com o cliente** sobre uso de IA em peças (quando aplicável conforme contrato)
- **Vedação de delegar juízo profissional** à máquina — a decisão é sua

Saídas deste plugin são minutas. A responsabilidade profissional, ética e disciplinar permanece integral do advogado habilitado.

## Testing & QA
