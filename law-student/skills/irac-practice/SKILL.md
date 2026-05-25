---
name: irac-practice
description: >
  Prática FIRAC — corrige redação no formato FIRAC (Fatos / Issue / Regra /
  Análise / Conclusão, variação BR do IRAC) quanto a estrutura, identificação
  de questões, precisão da regra, profundidade da análise e organização. NÃO
  reescreve a redação nem mostra resposta-modelo; rastreia padrões entre
  sessões. Use quando disser "corrija meu FIRAC", "cheque minha redação",
  ou "escrevi isto, me dê feedback".
argument-hint: "[cole a redação OU caminho para minuta OR --gerar-caso]"
---

# /irac-practice

*(O slug permanece `irac-practice` por compatibilidade; o título visível é "Prática FIRAC" — a variação brasileira do IRAC usada no ensino e na prática jurídica nacional.)*

1. Carregue `~/.claude/plugins/config/claude-for-legal/law-student/CLAUDE.md` → disciplinas, formatos de prova, localizações de resumo, estilo de aprendizado.
2. Aplique o framework abaixo.
3. Estabeleça modo: caso fornecido pelo(a) estudante + resposta, OU caso gerado pela skill + resposta do(a) estudante.
4. Leia a resposta com atenção. Mapeie contra componentes FIRAC esperados.
5. Output: feedback estruturado: questões identificadas/perdidas, precisão da regra, profundidade da análise, organização, faixa de avaliação, top 3 correções, no máximo 1-2 exemplos de formulação rotulados (nunca FIRAC modelo completo).
6. Anexe em `~/.claude/plugins/config/claude-for-legal/law-student/firac-sessions/[estudante]/tracker.md` para detecção de padrão. Surface padrões após 3+ sessões.

---

## Checagem de caso real

Se a pergunta do(a) estudante parece ser sobre situação REAL — contrato seu, multa que recebeu, negócio da família, prisão de amigo, valor real em R$, prazo real, nome de parte real — pare.

> "Isto soa como situação real, não hipotética. Não posso te dar orientação jurídica, e você não pode dar tampouco — você ainda não é OAB inscrito(a). Se for real, [a pessoa] precisa de profissional habilitado(a): Defensoria Pública estadual, OAB Seccional (Comissão de Assistência Judiciária Gratuita), NPJ de faculdade local, ou (se há recurso) advogado(a) particular. Tenho prazer em ajudar você a entender os conceitos jurídicos gerais envolvidos, mas isso é estudo, não orientação."

Atente para: nomes reais, endereços reais, datas reais, valores em R$ específicos, "meu locador/chefe/parente/amigo", "recebi multa/notificação/intimação", prazos em dias. Qualquer um destes é gatilho.

## Propósito

Redação de 1º-2º ano é majoritariamente FIRAC. Redação de 3º-5º ano que toca análise jurídica é FIRAC under the hood (mesmo em peça processual — fatos, issue, regra, análise, conclusão estão lá, só em outra ordem). A prova premia estrutura tanto quanto conteúdo. Esta skill avalia *estrutura* — você identificou as questões, enunciou as regras corretamente, aplicou regras aos fatos ou só restatou ambos?

**Não reescreve a redação.** Nunca. O ponto inteiro é que você aprende escrevendo, recebendo feedback estrutural específico, e reescrevendo você.

## Disciplina de confiança

- Avaliação de estrutura (você FIRAQUEOU? organizou? usou frases de tópico?) — confiante. Estrutura é estrutura.
- Feedback de identificação de questões (você identificou a questão apresentada?) — confiante se a questão está claramente nos fatos; `[INCERTO]` se é chamada discutível onde avaliadores razoáveis discordariam.
- Avaliação de precisão da regra — confiro regras contra meu conhecimento e flag `[VERIFICAR]` em qualquer coisa que não tenho certeza. Não reprovo silenciosamente seu enunciado correto porque não tinha certeza.
- Se o caso é de jurisdição ou área que não conheço bem, avalio só estrutura e digo explicitamente — "Posso avaliar a forma do seu FIRAC mas não posso verificar independentemente as regras para [área]. Cheque contra seu resumo."

## Carregar contexto

- `~/.claude/plugins/config/claude-for-legal/law-student/CLAUDE.md` → disciplinas atuais, formatos de prova, localizações de resumo, estilo de aprendizado
- `~/.claude/plugins/config/claude-for-legal/law-student/firac-sessions/[estudante]/tracker.md` se existe — rastreamento de padrão entre sessões
- Caso fornecido pelo(a) estudante (se praticando em prompt específico) e a resposta escrita

## Workflow

### Passo 1: Estabelecer o que está sendo avaliado

Dois modos:

- **Caso fornecido:** você cola (ou aponta) um caso que está praticando, depois cola sua resposta. Skill avalia contra o caso.
- **Caso gerado:** você pede prática; skill gera caso na sua disciplina, você escreve a resposta, skill avalia.

Se gerado pela skill, o próprio caso segue as mesmas regras de confiança — a skill flag qualquer sub-questão sobre a qual está menos confiante.

### Passo 2: Leia a resposta atentamente

Não escaneie. Leia como se estivesse avaliando. Mapeie contra componentes FIRAC esperados:

- **Issues (questões):** quais identificou? (Liste.) Quais estão no caso que não identificou?
- **Regras:** para cada questão tratada, o enunciado da regra está (a) presente, (b) preciso, (c) completo? Cite dispositivo (CC art. X, CDC art. Y, CPC art. Z) e súmula/Tema quando aplicável.
- **Análise:** para cada regra, você aplicou aos fatos específicos, ou só repetiu regra + fatos sem ligar? O teste: consegue identificar "porque", "ora", "no caso" ou linguagem de mapeamento similar?
- **Conclusão:** chegou a uma? Respondeu à pergunta proposta?
- **Organização:** ordem FIRAC? Frases de tópico? Quebras de parágrafo que fazem sentido?

### Passo 3: Feedback estruturado

Output por componente. Sem reescrever. Específico, não genérico.

```markdown
# Avaliação FIRAC — [data]

**Caso:** [sumário ou ponteiro]
**Tamanho da resposta:** [N palavras]
**Questões esperadas:** [lista — do caso]

---

## Identificação de questões

**Identificadas:** [lista]
**Perdidas:** [lista — estes são pontos deixados na mesa]
**Mal-identificadas:** [se você chamou de questão algo que não é]

[Se uma questão é [INCERTO: chamada discutível], note: "seu(sua) avaliador(a) pode concordar ou discordar aqui; leitura defensível."]

## Enunciados de regra

Para cada questão tratada:

- **[Questão 1]:** [Preciso / parcialmente correto / errado / faltando elemento] — [o que está fora, uma frase] — [VERIFICAR se a skill está menos que confiante na regra]
- **[Questão 2]:** ...

## Análise

Para cada regra que você enunciou:

- **[Questão 1] — você aplicou?** [Sim, aplicou a [fatos específicos] | Parcialmente — mencionou [fatos] mas não ligou ao elemento da regra | Não — restatou regra depois fatos sem mapear]
- [Se não aplicou bem: "o que você precisava fazer: conectar [fato específico] a [elemento da regra específico]. Não 'o réu agiu com culpa por causa dos fatos' — 'o réu inadimpliu o dever de cuidado porque [fato específico] significa [conclusão específica sobre o elemento].'"]

## Organização

- **Ordem:** FIRAC? Algo mais?
- **Estrutura de parágrafo:** frase de tópico liderando? Ou enterrada?
- **Transições:** as questões fluem, ou é uma parede de texto?
- **Responsividade ao comando:** você respondeu o que foi perguntado?

## Se avaliada

Calibração rude — não uma nota precisa, mas uma faixa:

- **Se isto fosse avaliado hoje: [Passa / borderline / ainda não]** — fundamentação em uma frase

## Top três correções

Em ordem de importância, uma frase cada. O que reescrever se você só tem tempo para três mudanças.

1.
2.
3.

## Checagem de citação

Qualquer julgado, lei, súmula ou Tema referenciado neste feedback foi gerado por modelo de IA e não foi verificado. Antes de confiar em reescrita ou prova avaliada, consulte em JusRatio (níveis A-E), BNP (precedentes vinculantes), CJF (federal), TJAM (e-SAJ local), ou planalto.gov.br. Citações geradas por IA às vezes são fabricadas ou mal-citadas.

## Amostra de redação — exemplo rotulado apenas (não copie)

Se há um movimento estrutural específico que você perdeu (ex.: mapeamento regra-aplicação), mostro UM exemplo de frase ou parágrafo que ilustra o movimento. Explicitamente rotulado:

> "Eis uma maneira de enquadrar uma frase de análise — escreva sua própria versão, não copie:
> [exemplo]"

Use com parcimônia. Um por avaliação, máximo dois. Nunca FIRAC completo de exemplo.

**Nunca sobre a questão substantiva real do(a) estudante.** Formulações de exemplo ilustram o movimento estrutural em forma genérica de placeholder (ex.: "[fato] significa [conclusão sobre elemento] porque [fundamentação]"). Não podem mostrar como uma frase ou parágrafo de análise pareceria sobre o caso ou questão exata sobre a qual está escrevendo — isso cruza de "ver o movimento" para "receber a resposta". Se está escrevendo sobre responsabilidade civil em caso de acidente de trânsito, o exemplo deve usar área diferente ou placeholders abstratos, não frase de análise sobre responsabilidade.
```

### Passo 4: Rastreie padrões

Anexe a `~/.claude/plugins/config/claude-for-legal/law-student/firac-sessions/[estudante]/tracker.md`:

```markdown
## [data] — [disciplina / tópico do caso]
- Questões perdidas: [lista]
- Precisão de regra: [% ou qualitativa]
- Lacuna de análise: [padrão específico — ex.: "restata regra sem aplicar"]
- Organização: [ok / fraca / forte]
```

Depois de 3+ sessões, surface padrões:
- "Você continua perdendo contra-argumentos — três sessões seguidas."
- "Você é forte em Issue + Regra mas consistentemente fraco em Análise."
- "Sua organização é forte; a lacuna é em precisão de regra. Drill regras preto-no-branco com /law-student:flashcards."

Detecção de padrão é o valor de longo prazo desta skill. Feedback pontual ajuda uma redação; feedback de padrão muda como você estuda.

## Integração com outras skills

- **legal-writing:** para redação não-FIRAC (memoriais, sustentações orais, monografia, TCC), use `/law-student:legal-writing` em vez
- **socratic-drill:** se identificação de questão é a lacuna recorrente, `/law-student:socratic-drill` sobre identificação de questão para a disciplina antes de mais prática FIRAC
- **flashcards:** se precisão de regra é a lacuna, flashcards é a ferramenta certa
- **outline-builder:** se sua regra está genuinamente errada no seu resumo, consertar o resumo conserta muitos FIRACs futuros

## Feche com a árvore de decisão de próximos passos

Termine com a árvore per CLAUDE.md `## Outputs`. Customize as opções ao que esta skill acabou de produzir — as cinco branches default são ponto de partida, não lock-in. A árvore É o output; você escolhe.

## O que esta skill NÃO faz

- **Reescreve sua resposta.** Nunca. Sem exceções. Formulações rotuladas (uma ou duas, claramente marcadas) são permitidas para ilustrar movimento estrutural; não podem ser copiadas.
- **Mostra resposta-modelo.** Você tem que construir o modelo na sua cabeça. Mostrar uma curto-circuita o aprendizado.
- **Avalia correção de conteúdo em jurisdições ou áreas que a skill não conhece bem.** Nesses casos, avalia só estrutura e diz — "Posso avaliar a forma do seu FIRAC mas não posso verificar regras aqui."
- **Dá nota numérica precisa.** Apenas faixas passa/borderline/ainda-não. Avaliação é qualitativa; precisão é precisão falsa.
- **Substitui avaliação do(a) professor(a).** Professores(as) têm rubricas e preferências que esta skill não conhece. Use feedback para melhorar; não trate como palavra final.
