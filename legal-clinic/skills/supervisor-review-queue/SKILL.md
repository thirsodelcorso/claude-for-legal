---
name: supervisor-review-queue
description: >
  Fila de revisão do(a) supervisor(a) — output de estagiário(a) espera aqui
  pela aprovação do(a) supervisor(a) antes de ir ao(à) assistido(a) ou ao
  juízo. Só ativa se "fila de revisão formal" foi escolhida como estilo de
  supervisão no setup; senão dormente. Use quando o(a) supervisor(a) quer ver
  o que está aguardando revisão, aprovar, editar-e-aprovar, ou devolver um
  item.
argument-hint: "[--approve ID | --return ID 'note' | --edit ID]"
---

# /supervisor-review-queue

1. Cheque `~/.claude/plugins/config/claude-for-legal/legal-clinic/CLAUDE.md` → estilo de supervisão. Se NÃO "fila de revisão formal": explique que a unidade está setada para [flags/toque mais leve], sem fila formal, e como mudar.
2. Use o workflow abaixo.
3. Default: mostre o que está aguardando, por urgência, por estagiário(a).
4. Ações: aprovar / editar-e-aprovar / devolver com nota. Tudo logado.

```
/legal-clinic:supervisor-review-queue
```

```
/legal-clinic:supervisor-review-queue --approve Q-003
```

```
/legal-clinic:supervisor-review-queue --return Q-004 "Cheque o requisito de intimação — regimento local mudou"
```

---

# Fila de Revisão do(a) Supervisor(a) (Opcional)

## Propósito

Algumas unidades querem gate formal: estagiário(a) redige, supervisor(a) revisa, output libera. Outras acham prescritivo demais — supervisionam via reunião de equipe e atendimentos conjuntos, não via fila.

**Esta skill só está ativa se `~/.claude/plugins/config/claude-for-legal/legal-clinic/CLAUDE.md` → Estilo de supervisão é "fila de revisão formal."** Senão está dormente — a entrevista de cold-start pergunta ao(à) supervisor(a) qual modelo quer, e esta é uma de três opções.

Se usar workflow formal de revisão é genuinamente questão aberta para adoção da unidade. Depende do nível de experiência dos(as) estagiários(as), do caseload, e de como o(a) supervisor(a) já roda supervisão. Supervisor(a) decide no setup e pode mudar depois.

## Carregue contexto

`~/.claude/plugins/config/claude-for-legal/legal-clinic/CLAUDE.md` → estilo de supervisão. Se NÃO "fila de revisão formal": responda com "A unidade está setada para supervisão [flags/toque mais leve] — não há fila formal. [Supervisor(a)] revisa via [estrutura existente da unidade]. Para mudar para fila formal, edite CLAUDE.md → Estilo de supervisão."

Se fila formal ESTÁ habilitada → leia gatilhos de flag e prossiga.

## A fila

Vive em `references/review-queue.yaml`. Cada entrada:

```yaml
- id: Q-001
  type: "draft"  # intake | draft | memo | status | client-letter
  client: "[nome ou ID do(a) assistido(a)]"
  student: "[nome do(a) estagiário(a)]"
  submitted: [timestamp]
  flags:
    - rule: "Peça a protocolar"
      detail: "Contestação em ação de despejo — sempre na fila"
  content_path: "[caminho do documento]"
  status: "pending"  # pending | approved | edited-approved | returned
```

## Modos

### O que está aguardando

```markdown
## Fila de Revisão — [data]

**Pendentes:** [N] | **Mais antigo:** [N] horas

### 🔴 Sensível a prazo
| ID | Tipo | Assistido(a) | Estagiário(a) | Por que sinalizado | Aguardando |
|---|---|---|---|---|---|

### Padrão
[mesma tabela]

### Por estagiário(a)
[Breakdown — spot padrões: quem está enfileirando muito, quem pode precisar de check-in]
```

### Revisar um item

Mostre conteúdo completo + por que foi sinalizado + notas do(a) estagiário(a).

### Aprovar / editar-e-aprovar / devolver

- **Aprovar:** Status → aprovado, estagiário(a) notificado(a), logado.
- **Editar e aprovar:** Supervisor(a) edita inline, versão aprovada é a editada, original preservado no log para que o(a) estagiário(a) veja o diff (momento de ensino).
- **Devolver:** Com uma nota. Estagiário(a) revisa e reapresenta.

## Logging

Toda ação logada. Logs de aprovação são registros da unidade — documentam que um(a) Defensor(a) habilitado(a) (inscrito(a) na OAB ou regularmente investido(a) no cargo) ou advogado(a) orientador(a) regularmente inscrito(a) na OAB revisou trabalho de estagiário(a) antes de ir ao(à) assistido(a) ou juízo. Isso importa para a compliance da unidade (corregedoria DPE / coordenação NPJ) e para avaliação de estagiário(a).

## Sinal pedagógico

A fila é também dado. Padrão em devoluções ("Estagiário(a) X continua perdendo o requisito de intimação") é conversa de coaching. Padrão em edições ("Notificações extrajudiciais de todo mundo estão longas demais") é update de `/ramp` para o próximo termo.

## O que esta skill NÃO faz

- **Rodar a não ser que o(a) supervisor(a) tenha escolhido.** É um de três modelos de supervisão, não o único.
- **Auto-aprovar.** O(a) supervisor(a) aprova.
- **Substituir a estrutura existente de supervisão da unidade.** É um gate para trabalho-produto, não substituto para reunião de equipe, atendimento conjunto, ou ver estagiários(as) em ação.
