---
name: flashcards
description: >
  Gera ou drila flashcards para memorização de letra-fria — buckets estilo
  Leitner, armazenamento markdown por disciplina, modo drill com auto-
  avaliação. Use quando o(a) usuário(a) disser "drila flashcards", "faz
  flashcards a partir de", "me testa nos cards", ou quer memorizar regras.
argument-hint: "[disciplina] [--generate | --drill | --review | --stats | --session <n>]"
---

# /flashcards

1. Carregue `~/.claude/plugins/config/claude-for-legal/law-student/CLAUDE.md` → disciplinas atuais, matérias frágeis, localização dos resumos.
2. Aplique o framework abaixo.
3. Roteie pela flag:
   - `--generate`: monta cards a partir da fonte (caminho do resumo, notas, manual) conforme regras de redação de card. Escreva em `~/.claude/plugins/config/claude-for-legal/law-student/flashcards/[disciplina]/cards.md`.
   - `--drill` (default): prioriza cards vencidos + novos; mostra Q, espera resposta, mostra A, toma auto-avaliação, atualiza buckets + próxima revisão.
   - `--review`: navega deck por bucket.
   - `--stats`: snapshot de progresso; sinaliza cards travados para drill verbal.
   - `--session <n>`: sessão focada de N cards, priorizada por erros prévios + cards vencidos; acrescenta resultados a `study-plan.yaml` → `session_history`.
4. Aplique disciplina de confiança: marque todo card gerado de conhecimento-sem-fonte com `[VERIFICAR]`.

---

## Checagem de caso real

Se a pergunta do(a) estudante soa como sendo sobre situação REAL — contrato de aluguel dele(a), multa de trânsito, negócio da família, prisão de amigo, valor real, prazo real, parte identificada — pare.

> "Isto soa como situação real, não hipótese de estudo. Não posso dar parecer jurídico, e você também não pode — você ainda não é advogado(a) inscrito(a) na OAB. Se for real, [a pessoa] precisa de orientação concreta: se você é estagiário(a) sob supervisão na DP/MP/NPJ, use o fluxo institucional via plugin `legal-clinic`. Se for problema próprio ou de pessoa identificável, procure a OAB Seccional, a Defensoria Pública do seu estado, ou o serviço de assistência judiciária da sua IES. Posso te ajudar a entender os conceitos jurídicos em abstrato — isso é estudo, não orientação concreta."

Atenção para: nomes reais, endereços reais, datas reais, valores específicos, "meu(minha) locador(a)/chefe/pai/mãe/amigo(a)", "recebi multa/notificação/intimação", prazos em dias. Qualquer um destes é gatilho.

## Propósito

Resumos são para síntese; flashcards são para memorização. A OAB e a maioria das provas de faculdade premiam recall rápido de regra. Esta skill gera cards do seu resumo (ou notas ou trechos de manual), drila com espaçamento leve, e rastreia o que fixou e o que não.

**Não é sistema SRS completo.** Buckets simples estilo Leitner. Bom o suficiente para estudar, leve o suficiente para manter. Se você quer Anki, use Anki; isto é para quando você está no chat e quer drill rápido.

## Disciplina de confiança

Mesma regra das outras skills geradoras de conteúdo:

- Se gerando cards de fonte que você fornece (resumo, notas, trecho de manual), Q e A vêm dali. Confiante.
- Se gerando cards de conhecimento sem fonte, marco todo card que enuncia regra na qual não estou totalmente confiante com `[VERIFICAR: regra — confirmar contra fonte]`. Você deve checar antes de fixar o card como alvo de aprendizado.
- Se não conheço bem a área, gero menos cards em vez de inventar. Melhor ter 8 cards bons que 20 onde 5 estão errados.

## Carregar contexto

- `~/.claude/plugins/config/claude-for-legal/law-student/CLAUDE.md` → disciplinas atuais, matérias frágeis, resumos existentes
- `~/.claude/plugins/config/claude-for-legal/law-student/flashcards/[disciplina]/cards.md` se existir (build incremental)
- Fonte fornecida pelo(a) usuário(a) (caminho do resumo, notas, trecho de manual) se dada

## Modos

Flag: `--generate | --drill | --review | --stats | --session <n>` (default: pergunta)

### `--session <n>` — sessão focada de N cards

Para quando o(a) estudante diz "vamos fazer 5 cards de Civil" ou roda `/law-student:session Civil 5 --flashcards`.

- Carregue `~/.claude/plugins/config/claude-for-legal/law-student/study-plan.yaml` se existir e leia `session_history` para esta disciplina.
- Priorize: cards anteriormente marcados errado > cards vencidos > cards novos.
- Rode N cards um por vez conforme o fluxo `--drill`.
- No fim da sessão, acrescente resultados a `study-plan.yaml` → `session_history`:

```yaml
session_history:
  - date: 2026-05-08
    subject: Civil
    type: flashcards
    n_cards: 5
    right: 3
    partial: 1
    wrong: 1
    stuck_topics: [prescricao-extracontratual]
```

- Se não houver `study-plan.yaml`, escreva em `~/.claude/plugins/config/claude-for-legal/law-student/session-history.yaml` em vez.

### `--generate` — cria cards

**Inputs:**
- Disciplina (nome da matéria ou tópico)
- Fonte (caminho do resumo, notas, ou "usa meu resumo existente em ~/.claude/plugins/config/claude-for-legal/law-student/CLAUDE.md")
- Opcional: meta de quantidade de cards (default 10-20 por sessão)

**Estrutura do card:**

```markdown
### Card [N]
**Q:** [pergunta — um conceito, um card]
**A:** [resposta — a regra, uma ou duas frases]
**Fonte:** [seção do resumo, página do manual, data da nota de aula]
**Bucket:** novo
**Última revisão:** —
**Próxima revisão:** [data de hoje]
**Notas:** [opcional — distinções, exceções, armadilhas]
```

**Regras de redação de card:**
1. **Um conceito por card.** "Elementos da responsabilidade civil" vira 4 cards, não 1.
2. **Frente é pergunta, não tópico.** "Dever de cuidado na responsabilidade" ruim. "Quais os elementos da responsabilidade civil subjetiva (CC art. 186)?" bom.
3. **Verso é regra, não parágrafo.** Se a resposta precisa de parágrafo, divide em múltiplos cards.
4. **Cite a fonte** para você poder rechecar durante o drill.

**Checagem de citação.** Quando cards são gerados do meu conhecimento em vez de fonte que você colou, a regra e qualquer julgado/dispositivo citado no verso foram gerados por modelo de IA e não foram verificados. Antes de memorizar um card, confirme contra seu resumo, manual, ou ferramenta de pesquisa (JusRatio, BNP, CJF, planalto.gov.br). Card errado drilado até maestria é pior que card nenhum.

### `--drill` — sessão de estudo

**Priorização:**
1. Cards onde `next_review <= hoje` E bucket != fixado
2. Cards novos ainda não tentados
3. Se nenhum vencido nem novo: pergunte se quer revisar cards fixados (para prevenir decaimento)

**Fluxo de drill por card:**
1. Mostre Q. Espere a resposta.
2. Usuário(a) responde (ou digita "pulo" / "não sei")
3. Mostre A.
4. Usuário(a) se auto-avalia: `certo` / `parcial` / `errado` / `não sei`
5. Atualize bucket + próxima revisão conforme tabela:

| Auto-avaliação | Mudança de bucket | Próxima revisão |
|---|---|---|
| certo | sobe um (novo → aprendendo → revisão → fixado) | +1d novo, +3d aprendendo, +7d revisão, +21d fixado |
| parcial | mesmo bucket | +1d |
| errado | desce um (revisão → aprendendo; aprendendo → novo; novo fica novo) | hoje +4h |
| não sei | desce um | hoje +4h |

### `--review` — navega deck

Mostre todos os cards de uma disciplina. Agrupados por bucket. Útil para escanear o que está no deck e ajustar manualmente conteúdo de card.

### `--stats` — snapshot de progresso

Por disciplina: total de cards, distribuição por bucket, vencidos hoje, revisados na semana. Destaque qualquer card que tenha caído para `novo` mais de duas vezes — esses são os conceitos travados que valem drill verbal via `/law-student:socratic-drill`.

## Integração com outras skills

- **outline-builder:** depois de montar ou estender um resumo, ofereça gerar flashcards do novo material
- **socratic-drill:** se um card foi errado 2+ vezes, roteie para `/law-student:socratic-drill` para trabalhar verbalmente — flashcards não bastam para conceitos que você não entende
- **bar-prep-questions:** disciplinas OAB com stats ruins de flashcard pesam mais na rodagem de 1ª fase FGV

## Armazenamento

```
flashcards/
└── [disciplina]/
    └── cards.md
```

Um arquivo por disciplina. Cards em markdown. Metadados de bucket/revisão inline por card. Não ideal para decks muito grandes (>500) mas bom para tamanhos típicos.

## O que esta skill não faz

- **Substituir Anki.** Se você já tem hábito de flashcard, mantenha. Isto é para quando está no chat e quer drilar sem trocar de app.
- **Inventar cards para bater meta de quantidade.** Se só consigo gerar 8 cards confiantes da sua fonte, você recebe 8. Encher com chute marcado `[VERIFICAR]` é pior que deck menor.
- **Forçar disciplina de estudo.** Dias perdidos de revisão se acumulam; a skill só mostra o que está vencido. Você decide se drila.
- **Te ensinar a regra.** Cards são para drilar o que você já estudou. Se um card está consistentemente errado, o problema é a montante — use `/law-student:socratic-drill` ou releia a fonte.
