---
name: client-comms-log
description: >
  Loga uma comunicação com o(a) assistido(a) — ligação, e-mail, mensagem,
  carta, atendimento presencial, recado. Registro por caso append-only, com
  entradas datadas, direção, meio, sumário, itens de ação. Roda junto com
  /client-letter e /status client. Use ao logar uma ligação ou e-mail do(a)
  assistido(a), ao revisar o log de comunicações, ou ao perguntar "o que a
  gente disse para [assistido(a)] da última vez".
argument-hint: "[case-id] [--add (default) | --read | --summary | --patterns]"
---

# /client-comms-log

1. Use o workflow abaixo.
2. Exija case-id (peça se não fornecido).
3. Rote por flag:
   - `--add` (default): capture direção, meio, estagiário(a), sumário, itens de ação, follow-up devido. Confirme com o(a) usuário(a). Append (prepend mais-recente-primeiro) em `~/.claude/plugins/config/claude-for-legal/legal-clinic/client-comms/[case-id]/log.md`.
   - `--read`: mostre as N entradas mais recentes.
   - `--summary`: leitura condensada de um parágrafo.
   - `--patterns`: varra por comms não-respondidas, follow-ups perdidos, lacunas de idioma, mudanças de tom, lacunas de contato. Voltado à supervisão.
4. Integração: ofereça `/legal-clinic:deadlines --add` se o log estabelecer prazo; rote para `/legal-clinic:semester-handoff` via `--summary` quando relevante.

---

# Log de Comunicações com o(a) Assistido(a)

## Propósito

Quatro razões para manter este log:

1. **Defesa contra responsabilização.** Se o(a) assistido(a) alega "ninguém nunca me disse [X]," uma entrada datada mostrando o contrário é a resposta. Defensores(as)-Supervisores(as) e Professores(as)-Orientadores(as) carregam responsabilidade institucional pelo trabalho do(a) estagiário(a); registros contemporâneos protegem.
2. **Continuidade no handoff.** O(a) estagiário(a) do termo seguinte assume e lê o log; não re-pergunta ao(à) assistido(a) coisas já respondidas.
3. **Visibilidade de supervisão.** Cinco recados não-retornados em seis semanas é padrão. O log torna visíveis padrões que estagiários(as) individuais talvez não sinalizassem por conta própria.
4. **Retenção de arquivo.** Defensorias e NPJs têm obrigações de manter pastas de caso completas (LC 80/94 + regulamento institucional). Histórico de comunicação é parte disso.

Leve. Append-only. Trabalho do(a) estagiário(a) é escrever entrada de duas frases depois de cada contato; a skill formata e faz append.

## Carregue contexto

- `~/.claude/plugins/config/claude-for-legal/legal-clinic/client-comms/[case-id]/log.md` (se existir) — alvo do append
- `~/.claude/plugins/config/claude-for-legal/legal-clinic/CLAUDE.md` → não lido em profundidade; esta skill é case-scoped

## Modos

Flag: `--add | --read | --summary | --patterns` (default: add)

### `--add` (default) — logar nova entrada

**Inputs:**
- Case ID (obrigatório — qual caso)
- Data + hora (default: agora)
- Direção: `in` (assistido[a] → unidade) | `out` (unidade → assistido[a])
- Meio: `ligação | email | mensagem | carta | presencial | vídeo | recado-deixado | recado-recebido`
- Quem (estagiário[a]): nome
- Quem (lado do[a] assistido[a]): nome do(a) assistido(a), ou "terceiro: [descrição]" se for de advogado(a) contrário(a), familiar, etc.
- Duração / tamanho (ex.: "ligação de 10 min", "e-mail de 3 parágrafos", "atendimento presencial de 45 min")
- Sumário: 2-4 frases. O que aconteceu, o que foi substantivo.
- Itens de ação:
  - O que o(a) estagiário(a) deve ao(à) assistido(a) (com prazo)
  - O que o(a) assistido(a) deve ao(à) estagiário(a) (com timing esperado)
- Follow-up devido: data se aplicável
- Notas: qualquer coisa que importa mas não cabe acima — idioma usado, tom emocional, dinâmica familiar observada

**Antes de escrever:** mostre ao(à) usuário(a) a entrada formatada e peça confirmação. Registros da unidade devem ser revisados antes de escritos, não depois.

**Append** em `~/.claude/plugins/config/claude-for-legal/legal-clinic/client-comms/[case-id]/log.md`. Se o log não existir, crie com cabeçalho:

```markdown
# Log de Comunicações — [nome do caso]

**Case ID:** [case-id]
**Assistido(a):** [nome]
**Aberto em:** [AAAA-MM-DD]

Append-only. Mais recente no topo.

---
```

Então faça prepend de novas entradas no topo (mais recente primeiro).

### `--read` — mostre entradas recentes

Imprima as N entradas mais recentes (default 5). Útil ao pegar um caso no meio do termo ou antes de uma ligação ao(à) assistido(a).

### `--summary` — leitura condensada

Produza sumário de um parágrafo do log — contato mais recente, total de entradas, meio comum, qualquer item de ação aberto do lado do(a) estagiário(a), qualquer comunicação não-respondida. Alimenta `/semester-handoff` e `/status`.

### `--patterns` — sinalize preocupações ao longo do log

Varra por:

- **Comunicações não-respondidas do(a) assistido(a).** Assistido(a) ligou ou e-mailou N vezes sem entrada de resposta.
- **Follow-up perdido.** Item de ação com data de follow-up, e nenhuma entrada posterior resolvendo.
- **Questões de idioma / acessibilidade.** Idioma do(a) assistido(a) anotado como não-português (ex.: língua indígena nativa); cheque se comunicações de saída foram nesse idioma ou via tradução.
- **Padrões de escalonamento.** Tom do(a) assistido(a) mudando (frustrado / angustiado) ao longo das entradas.
- **Lacunas.** Trechos longos sem contato em caso ativo.

Isto é ferramenta de supervisão. Defensores(as)-Supervisores(as) e Professores(as)-Orientadores(as) rodando `--patterns` em seus casos veem quais estagiários(as) podem precisar de apoio.

## Integração

- **`/client-letter`:** depois de gerar e enviar carta, ofereça logar como comm de saída.
- **`/status client`:** ao produzir sumário de status voltado ao(à) assistido(a), ofereça logar (esses sumários frequentemente vão para o(a) assistido(a)).
- **`/client-intake`:** primeira entrada no log de todo caso novo é o contato de intake.
- **`/semester-handoff`:** memos de handoff leem `--summary` por caso para popular a seção de histórico de comunicações.
- **`/deadlines`:** se uma comunicação estabeleceu prazo ("assistido(a) disse que precisa responder até sexta"), ofereça `/deadlines --add`.

## O que esta skill não faz

- **Armazenar análise jurídica substantiva.** Isso vive em intake, memo e arquivos de status. O log é registro de comunicação — fatos de contato, não estratégia jurídica.
- **Auto-logar de sistemas externos.** Se a unidade usa sistema interno (Sapiens-DPGU / sistema próprio AM), uma integração futura poderia puxar logs de ligação e e-mail automaticamente. Isso é roadmap; não v1.
- **Editar entradas passadas.** Append-only. Se uma entrada está errada, escreva nova entrada referenciando e corrigindo. A integridade do log depende de não reescrever história.
- **Forçar disciplina de log.** Se um(a) estagiário(a) não loga uma ligação, a skill não tem como saber. Higiene de log é problema de cultura da unidade; a skill só torna logar fácil.
- **Lidar com notas reservadas ou só-para-Defensor(a).** Se o(a) estagiário(a) precisa registrar pensamento estratégico, isso vai no arquivo de análise interna do caso, não no log de comms.
