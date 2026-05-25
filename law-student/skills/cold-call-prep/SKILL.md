---
name: cold-call-prep
description: >
  Prepara você para uma chamada de classe — prevê as perguntas prováveis do(a)
  professor(a) e drila socraticamente, sinalizando onde você está shaky para
  você saber o que reler antes da aula. Use quando o(a) usuário(a) disser
  "prepara aula de amanhã", "chamada em [julgado]", "o que o(a) professor(a)
  pode perguntar sobre", ou apontar para leitura designada.
argument-hint: "[nome do julgado, ou cole o texto, ou caminho da leitura]"
---

# /cold-call-prep

1. Carregue `~/.claude/plugins/config/claude-for-legal/law-student/CLAUDE.md` → lista de disciplinas, professores(as), estilo de aprendizado.
2. Aplique o workflow abaixo.
3. Identifique a leitura (nome do julgado + citação, professor(a), disciplina, contexto no plano de aula).
4. Preveja 6-10 perguntas prováveis em categorias (Fatos / Tese fixada / Fundamentação / Aplicação / Política), ponderadas pelas tendências conhecidas do(a) professor(a).
5. Drile usando padrão socrático — pergunta, espera, pressiona, restringe quando emperra. Não dá respostas.
6. Sumário pós-drill: forte/shaky/perdido; o que rechecar antes da aula.

---

## Checagem de caso real

Se a pergunta do(a) estudante soa como sendo sobre situação REAL — contrato de aluguel dele(a), multa de trânsito, negócio da família, prisão de amigo, valor real, prazo real, parte identificada — pare.

> "Isto soa como situação real, não hipótese de estudo. Não posso dar parecer jurídico, e você também não pode — você ainda não é advogado(a) inscrito(a) na OAB. Se for real, [a pessoa] precisa de orientação concreta: se você é estagiário(a) sob supervisão na DP/MP/NPJ, use o fluxo institucional via plugin `legal-clinic`. Se for problema próprio ou de pessoa identificável, procure a OAB Seccional, a Defensoria Pública do seu estado, ou o serviço de assistência judiciária da sua IES. Posso te ajudar a entender os conceitos jurídicos em abstrato — isso é estudo, não orientação concreta."

Atenção para: nomes reais, endereços reais, datas reais, valores específicos, "meu(minha) locador(a)/chefe/pai/mãe/amigo(a)", "recebi multa/notificação/intimação", prazos em dias. Qualquer um destes é gatilho.

## Propósito

Chamada de classe vive ou morre na preparação. O(A) professor(a) leu o julgado dezenas de vezes e conhece as perguntas; o(a) estudante leu uma vez. Esta skill estreita o gap — prevê os padrões de pergunta para o julgado, drila o(a) estudante neles, e surface o que não está fixado.

Não substitui ler o julgado. É um teste de que você leu mesmo.

## Disciplina de confiança

- Quando o(a) estudante fornece texto do acórdão ou trecho do manual: prevejo perguntas com base no texto efetivo. Confiante.
- Quando o(a) estudante fornece só o nome do julgado: prevejo com base no que sei sobre o caso. Marco `[INCERTO]` em qualquer pergunta que dependa de detalhes que não tenho certeza. Recomendo fortemente que cole o acórdão ou o tratamento do manual primeiro.
- Se não conheço bem o caso: digo. "Não tenho leitura confiável deste julgado — cole o texto ou o tratamento do manual e trabalho a partir disso. Caso contrário, minhas perguntas são chute educado."

## Carregar contexto

- `~/.claude/plugins/config/claude-for-legal/law-student/CLAUDE.md` → disciplinas atuais, professores(as), estilo de aprendizado
- Fornecido pelo(a) usuário(a): nome do julgado / texto do acórdão / páginas do manual / lista de leitura

## Workflow

### Passo 1: Identifique a leitura + professor(a)

- Nome do julgado e citação (ex.: RE 567.985/MT, HC 82.424/RS, ADI 4.277/DF)
- Professor(a) (da lista de disciplinas em ~/.claude/plugins/config/claude-for-legal/law-student/CLAUDE.md — tom e foco variam por professor(a))
- Disciplina / área
- Onde este julgado cai no plano de aula (para contexto — é o primeiro julgado sobre o tema, um caso que restringe, um contraexemplo?)

### Passo 2: Preveja as perguntas

Professores(as) fazem chamada de classe em padrões recorrentes. Preveja nestas categorias:

**Fatos (aquecimento):**
- Quem são as partes? O que aconteceu? Qual a postura processual?
- O que decidiu o juízo de 1º grau? E o tribunal recorrido?
- Por que este julgado está no manual? Que instituto ilustra?

**Tese fixada / regra:**
- Qual a ratio decidendi? Uma frase.
- Qual a regra portátil que sai do caso — o takeaway que vai para o seu resumo?
- Como você fraseia a tese se ela fosse para o seu esquema?

**Fundamentação:**
- Por que o tribunal decidiu assim?
- Quais argumentos o tribunal rejeitou?
- Houve voto vencido? O que ele argumentou?
- Houve voto convergente com fundamentos diversos?

**Aplicação / hipóteses:**
- E se [fato X] fosse diferente — mesmo resultado?
- Como este caso se compara a [precedente anterior do plano de aula]?
- Qual o limite da tese? Onde a regra para?

**Política / teoria (princípios constitucionais, função do instituto):**
- Que valor/princípio o tribunal está protegendo (dignidade da pessoa humana, segurança jurídica, livre iniciativa)?
- A tese faz sentido? Abordagens alternativas na doutrina?

**Sabor específico do(a) professor(a) (das notas em ~/.claude/plugins/config/claude-for-legal/law-student/CLAUDE.md):**
- Se o(a) professor(a) é conhecido(a) por chamada pesada em hipóteses, pondere Aplicação/Hipótese
- Se pesado(a) em política/princípio, pondere Política/Teoria
- Se socrático(a) fato-pesado (estilo tradicional), pondere Fatos + Tese

Escolha 6-10 perguntas pelas categorias. Ranqueie pela probabilidade de ser perguntada primeiro (Fatos geralmente vêm primeiro, depois Tese, depois as categorias mais duras).

### Passo 3: Drile

Use o padrão da `socratic-drill`:

1. Faça a Pergunta 1. Espere a resposta.
2. Se certa + bem fundamentada: reconheça, vá para a Pergunta 2.
3. Se certa mas relaxada: não deixe passar. "Você chegou lá, mas explica — por que a fundamentação do tribunal sustenta isso?"
4. Se errada: não dê a resposta. Faça uma pergunta restritiva. "Que fatos o tribunal apoia?" Conduza até.
5. Se emperrou: restrinja mais. "Antes da tese — qual a postura processual?"
6. Se genuinamente perdido(a): diga para reler o julgado. "Isto é releitura, não chute. Volta quando tiver lido de novo."

### Passo 4: Sumário pós-drill

No fim:

```markdown
# Preparação para chamada de classe — [julgado] — [data]

**Perguntas driladas:** [N]
**Forte:** [perguntas onde estava confiante + certo(a)]
**Shaky:** [perguntas onde chutou ou hesitou]
**Perdido:** [perguntas onde não sabia]

## Antes da aula amanhã:
- [coisa específica para rechecar — fatos que errou, regra que não conseguiu enunciar]
- [se shaky em política/princípio: "releia o voto vencido — geralmente é dali que saem as perguntas de princípio"]

## Perguntas com maior probabilidade na aula:
- [top 3 das 10 — as que o(a) professor(a) tem maior chance de abrir]
```

## Integração

- **case-brief:** se o(a) estudante ainda não fichou o julgado, ofereça rodar `/law-student:case-brief` antes da preparação de chamada. Fichamento também é ferramenta de preparação para chamada.
- **socratic-drill:** se a preparação surface ponto fraco na matéria (não só neste julgado), siga com `/law-student:socratic-drill [matéria]`.
- **flashcards:** se a regra do caso é algo que o(a) estudante deve memorizar, ofereça adicionar ao deck de flashcards.

## O que esta skill não faz

- **Ser o(a) professor(a).** A chamada de classe real pode ir para qualquer lugar. Esta skill prevê padrões; professores(as) surpreendem.
- **Substituir leitura do julgado.** Se você não leu, a skill não ajuda — perguntas exigem texto que você absorveu.
- **Dar a tese fixada sem você tentar primeiro.** Padrão drill-me: eu pergunto, você responde.
- **Prever perguntas de nicho jurisdicional.** Se o(a) professor(a) tem temas favoritos conhecidos, capture-os nas notas de disciplina em ~/.claude/plugins/config/claude-for-legal/law-student/CLAUDE.md e a skill pondera; caso contrário, trabalha de padrões gerais.
