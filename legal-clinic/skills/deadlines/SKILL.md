---
name: deadlines
description: >
  Acompanha prazos de casos — adicionar, relatório cross-case, atualizar,
  cumprir, encerrar. Alerta em limiares configuráveis (default 14/7/3/1 dias);
  itens vencidos ficam sinalizados até resolvidos. O registro operacional da
  carga de trabalho da unidade. Use quando estagiário(a) ou supervisor(a)
  precisa adicionar prazo, perguntar o que vence esta semana, pegar relatório
  de prazos, ou atualizar prazo de caso.
argument-hint: "[--add | --report (default) | --update [id] | --complete [id] | --close [id] | --horizon=N]"
---

# /deadlines

1. Carregue `~/.claude/plugins/config/claude-for-legal/legal-clinic/CLAUDE.md` → jurisdição, áreas de atuação, cadência de dias de alerta.
2. Use o workflow abaixo.
3. Rote por flag:
   - `--add`: capture caso, tipo, descrição, data devida, fonte, owner. Escreva em `~/.claude/plugins/config/claude-for-legal/legal-clinic/deadlines.yaml`. Cheque duplicatas primeiro.
   - `--report` (default): rollup cross-case — vencidos, próximos 3d, próximos 7d, próximos 14d; por owner; por área de atuação; flags de não-atribuídos.
   - `--update [id]`: modifique campos; logue nota com data.
   - `--complete [id]`: marque cumprido; confirme com o(a) estagiário(a) que o trabalho está de fato protocolado/submetido.
   - `--close [id]`: encerre-sem-cumprir; exija razão nas notas.
4. Confirme qualquer escrita antes de gravar.

---

# Prazos

## Propósito

O maior risco operacional da unidade ou NPJ é perder prazo. Estagiários(as) carregam múltiplos casos, fazem outras atividades em paralelo, e rotacionam por termo/semestre. Prazos que vivem só na cabeça de estagiários(as) individuais caem no handoff, são esquecidos na semana de provas, e se perdem quando estagiário(a) inesperadamente deixa a unidade. Esta skill é o registro operacional central.

O(a) Defensor(a)-Supervisor(a) ou Professor(a)-Orientador(a) é quem responde se prazo é perdido. A skill é calibrada para esse nível de risco — alertas disparam cedo, itens vencidos ficam visíveis até explicitamente resolvidos, handoffs (via `/semester-handoff`) puxam a lista de prazos adiante para o(a) próximo(a) estagiário(a).

## Carregue contexto

- `~/.claude/plugins/config/claude-for-legal/legal-clinic/CLAUDE.md` → jurisdição, áreas de atuação, dias de alerta de prazo (default 14/7/3/1), supervisores(as)
- `~/.claude/plugins/config/claude-for-legal/legal-clinic/deadlines.yaml` — o livro-razão

**Suposição jurisdicional.** Cálculos de prazo e limiares de alerta assumem a jurisdição setada no CLAUDE.md. Cálculo de prazos depende do procedimento: **CPC art. 219 contagem em dias úteis** (regra geral cível) com **suspensão CPC art. 220** (20/12 a 20/1 — recesso forense); **Lei 9.099/95 art. 12-A** trata o JEC com contagem em dias corridos por jurisprudência STJ consolidada; **CPC art. 186 — prazo em dobro para Defensoria Pública** (e art. 183 para Fazenda, art. 229 para litisconsortes com procuradores distintos). Penal segue regra própria (CPP art. 798 — dias corridos com vacatio entre meio-dia ato/protocolo). Se a matéria envolve outro tribunal, regimento interno específico, ou questão federal vs. estadual, confirme o prazo contra a regra de regência com o(a) seu(sua) supervisor(a) antes de confiar.

## Modos

Flag: `--add | --report | --update | --complete | --close` (default: report)

### `--add` — logar novo prazo

**Inputs:**
- Case ID + nome (qual caso)
- Área de atuação
- Tipo (peça / audiência / prazo prescricional / decadencial / instrução / impugnação / contestação / réplica / recurso / cumprimento / outro)
- Descrição — uma linha do que vence
- Data devida (e hora + fuso se aplicável)
- Fonte — de onde o prazo veio (despacho citado em 2026-04-20 com prazo CPC art. 335, prazo decadencial CDC art. 26, cláusula contratual §7)
- Owner estagiário(a) — quem é responsável

A skill gera um `id` slug automaticamente: `[case]-[short-desc]-[AAAA-MM]`.

**Extração de outras skills:** quando `/client-intake`, `/draft` ou `/status` sinalizam prazo em seu output, devem fazer handoff para esta skill com campos pré-populados. Estagiário(a) confirma e adiciona.

**Checagem pré-add:** se prazo com mesmo case_id + tipo + data devida já existe, sinalize como provável duplicata e pergunte antes de adicionar.

**Banda de plausibilidade.** Depois que o(a) estagiário(a) entra com data devida, NÃO calcule nem verifique — mas aplique checagem rude de plausibilidade contra ranges típicos para o tipo de peça, e sinalize ao(à) estagiário(a) se a data cair muito fora. Isto é andaime para apanhar erros grosseiros na conta do(a) próprio(a) estagiário(a), não alternativa para calcular contra a regra.

**Bandas são jurisdiction-keyed.** Carregue o arquivo de banda para a jurisdição desta unidade de `references/plausibility-bands/{uf}.md` onde `{uf}` é o código de duas letras de `~/.claude/plugins/config/claude-for-legal/legal-clinic/CLAUDE.md` → jurisdição da unidade (e federal sempre carrega junto). O plugin legal-clinic distribui `references/plausibility-bands/AM.md` (a popular) e `references/plausibility-bands/SP.md` (estrutura placeholder) como pontos de partida.

**Parada dura no cold-start se o arquivo de banda está faltando.** Se `references/plausibility-bands/{uf}.md` não existe para a jurisdição da unidade, NÃO rode silenciosamente sem checagens de plausibilidade. No cold-start, diga ao(à) supervisor(a):

> "Não tenho checagens de plausibilidade de prazo para [UF] — a banda para a jurisdição desta unidade não está nos arquivos de referência distribuídos. Posso ainda acompanhar prazos (adicionar, reportar, atualizar, cumprir, encerrar), mas não consigo sanity-checar contra ranges típicos. Como construir o arquivo de banda a partir das regras da sua UF: copie `references/plausibility-bands/SP.md` como template, preencha uma linha por tipo de prazo que sua unidade vê mais (range típico, manejo do evento desencadeador, regra de contagem CPC/CPP/JEC, citação curta da fonte), salve em `references/plausibility-bands/{uf}.md`, e re-rode `/legal-clinic:deadlines`. Até lá, todo prazo que eu aceitar vai carregar `warnings: no-plausibility-band` e sua revisão deve tratar datas como não-checadas."

Não faça fallback para a tabela AM em unidade de não-AM. O caso de degradação silenciosa — distribuir checagem do Amazonas para unidade de São Paulo — é a falha que este fix existe para fechar.

**Lógica da checagem de plausibilidade:**

1. Carregue a tabela de bandas para a jurisdição desta unidade de `references/plausibility-bands/{uf}.md` (mais federal-sempre).
2. Depois que o(a) estagiário(a) entra com `due:`, compare com data do evento desencadeador + range típico para esse `type:` (se range típico existe no arquivo de banda carregado para o tipo de peça).
3. Se dentro do range, escreva a entrada. Não diga nada — a banda existe para apanhar erros, não parabenizar matemática certa.
4. Se fora do range por margem material, pare antes de escrever e diga:
   > A data que você entrou cai fora do range típico para [tipo] em [jurisdição]. Prazos de [tipo] para [tipo de peça] tipicamente caem ~[range] após [evento desencadeador]. Sua entrada: [data], que está [N] dias a partir de [evento desencadeador]. Re-cheque sua conta contra [regra citada do arquivo de banda] e a regra de contagem da jurisdição (CPC art. 219 dias úteis + suspensão art. 220, ou JEC dias corridos, ou prazo em dobro CPC art. 186). Se sua conta está certa (exceção de regimento local, evento desencadeador atípico, suspensão, renúncia), confirme e eu adiciono a entrada como está. Senão, recompute e re-rode `/deadlines --add`.
5. Se nenhuma banda é conhecida para esse `type:` (peça incomum, prazo não-padrão), não faça sanity-check — escreva a entrada e anote no campo `warnings:` que nenhuma banda de plausibilidade se aplica.
6. Se o arquivo de banda está faltando inteiramente para esta jurisdição, a parada dura acima se aplica no cold-start; em steady-state (supervisor[a] reconheceu a lacuna e prosseguiu), toda entrada é escrita com `warnings: no-plausibility-band`.

**A skill não calcula.** Se o(a) estagiário(a) entra com `[VERIFICAR]` no campo `due:` porque ainda não fez a conta, escreva a entrada com `due: [VERIFICAR]` — a banda de plausibilidade roda só quando o(a) estagiário(a) fornece data concreta. O cálculo fica com o(a) estagiário(a) e supervisor(a).

### `--report` (default) — rollup cross-case

Leia `~/.claude/plugins/config/claude-for-legal/legal-clinic/deadlines.yaml`. Produza:

```markdown
# Relatório de Prazos — [hoje]

**Prazos ativos:** [N]
**Vencidos:** [N] ⚠️
**Vencem esta semana (próximos 7 dias):** [N]

---

## ⚠️ Vencidos (sinalizados para atenção imediata)

| ID | Caso | Tipo | Devido | Owner | Dias vencidos |
|---|---|---|---|---|---|

## 🔴 Vencem hoje / próximos 3 dias

| ID | Caso | Tipo | Devido | Owner |
|---|---|---|---|---|

## 🟡 Vencem em 4-7 dias

| ID | Caso | Tipo | Devido | Owner |
|---|---|---|---|---|

## 🟢 Vencem em 8-14 dias

[lista]

## Além de 14 dias

[só contagem — expanda com `/deadlines --report --horizon=30` para detalhes]

---

## Por owner estagiário(a) (distribuição de carga)

| Estagiário(a) | Vencidos | Próximos 7d | Próximos 14d | Total ativos |
|---|---|---|---|---|

## Por área de atuação

[mesma tabela, agrupada por área]

## Prazos não-atribuídos

[lista — sinalize se algum prazo ativo não tem owner_student]
```

### `--update` — modificar prazo existente

Updates comuns: data devida mudou (suspensão decretada, adiamento de audiência), owner mudou (reatribuição), notas adicionadas.

Todo update escreve nota datada inline; histórico fica visível na entrada.

### `--complete` — marcar cumprido

- Seta `status: completed`, `completed_date: [hoje]`.
- Confirma com o(a) estagiário(a) que o trabalho efetivo foi feito e protocolado/submetido.
- Remove dos relatórios ativos mas fica no yaml.

### `--close` — encerrar sem cumprir

Para prazos que não mais se aplicam — acordo homologado, pedido desistência aceito, assistido(a) descontinuou a representação. Exige entrada de `notes:` explicando por quê.

## Cadência de alertas

Conforme dias de alerta de prazo em `~/.claude/plugins/config/claude-for-legal/legal-clinic/CLAUDE.md`. Default 14, 7, 3, 1.

Alertas não auto-surface — este plugin não tem comportamento agendado/de agente. Mas toda vez que `/deadlines` é invocado (ou `/status`, que rota para esta skill em checagens de prazo), o relatório puxa adiante qualquer coisa atingindo limiar de alerta.

Se prazo passa data devida sem ser marcado completo, move para `status: overdue` e fica lá em todo relatório até explicitamente resolvido. Prazos vencidos não auto-encerram.

## Integração

- **`/client-intake`:** quando intake sinaliza urgência de timeline (data de notificação extrajudicial, prazo decadencial CDC, data de audiência), ofereça `/deadlines --add` com campos pré-populados.
- **`/draft`:** quando minuta de peça referencia prazo (contestação em 15 dias úteis, impugnação à contestação, prazo de recurso), ofereça adicionar.
- **`/status`:** a skill status lê `~/.claude/plugins/config/claude-for-legal/legal-clinic/deadlines.yaml` para o caso relevante e inclui prazos próximos no output.
- **`/semester-handoff`:** lê deadlines.yaml para identificar todos os prazos ativos nos casos do(a) estagiário(a) que sai; cada memo de handoff carrega os prazos adiante.
- **`/supervisor-review-queue` (se revisão formal habilitada):** prazos perto do corte ganham prioridade na fila de revisão.

## O que esta skill não faz

- **Calcular prazos a partir de eventos desencadeadores.** Se a citação foi efetivada hoje e a contestação vence em 15 dias úteis pela regra do CPC art. 335 + art. 219 + art. 186 (dobro), a skill não faz essa conta — o(a) estagiário(a) faz, usando a regra, e loga a data resultante. (Fazer a conta autonomamente cria risco que a skill não deveria assumir; regras variam por procedimento e por tribunal.)
- **Protocolar ou intimar nada.** A skill rastreia datas; protocolização acontece fora do plugin.
- **Auto-notificar.** Sem notificações agendadas. O relatório surface alertas quando invocado; não empurra. Um cron agendado poderia ser adicionado depois mas precisaria opt-in explícito do(a) supervisor(a) por unidade.
- **Override regras locais.** Se o(a) estagiário(a) loga data devida que contradiz regimento local, a skill não apanha. Outra razão para calendarizar com `[VERIFICAR: confirmar contra regra local TJAM/CSDPGE]` para qualquer prazo não-rotineiro.
