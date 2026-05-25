---
name: memo
description: >
  Memo de análise de caso com scaffold FIRAC e lacunas de pesquisa sinalizadas
  — o scaffold, não a análise. Blocos de Regra são PESQUISA NECESSÁRIA,
  Aplicação é prompt de ANÁLISE DO(A) ESTAGIÁRIO(A), Conclusão fica em branco.
  Use quando estagiário(a) precisa escafoldar memo de análise de caso, escrever
  sua análise, ou construir memo FIRAC para um caso.
argument-hint: "[optional: specific issue to focus]"
---

# /memo

1. Carregue `~/.claude/plugins/config/claude-for-legal/legal-clinic/CLAUDE.md` → áreas de atuação, jurisdição.
2. Use o workflow abaixo. Leia sumário de intake / notas do caso.
3. Enquadre questões como perguntas. Escafolde FIRAC para cada — blocos de Regra são PESQUISA NECESSÁRIA, Aplicação é prompt de ANÁLISE DO(A) ESTAGIÁRIO(A), Conclusão fica em branco.
4. Pontos fortes/fracos/questões abertas. Sumário de lacunas de pesquisa.
5. Output com rótulo proeminente "a análise é sua".

```
/legal-clinic:memo
```

---

# Memo: Análise Interna de Caso

## Propósito

O memo de análise de caso é onde mora o pensamento do(a) estagiário(a). Esta skill fornece o scaffolding FIRAC e sinaliza as lacunas de pesquisa — o(a) estagiário(a) preenche a análise.

**A análise é do(a) estagiário(a).** Esta skill estrutura; não conclui.

## Carregue contexto

`~/.claude/plugins/config/claude-for-legal/legal-clinic/CLAUDE.md` → áreas de atuação, jurisdição, estilo de supervisão.
Sumário de intake e notas do caso para fatos.

## Checagem pedagógica

Leia o guia do(a) supervisor(a) para esta área de atuação em `~/.claude/plugins/config/claude-for-legal/legal-clinic/guides/<area-de-atuacao>.md`. Cheque o setting `pedagogy_posture`:

- **`guide` (default):** Produza a estrutura FIRAC e a lista de lacunas de pesquisa. Peça ao(à) estagiário(a) para redigir cada enunciado de regra a partir de pesquisa, em vez de dar framework. Dê feedback no que escreveu. Ofereça preencher a regra-framework de uma seção só depois que o(a) estagiário(a) tentou uma vez.
- **`assist`:** Produza o scaffold do memo e preencha o que pode ser preenchido. Sinalize itens para revisão do(a) estagiário(a). Estagiário(a) edita e aprende revisando. (Nota: esta skill de memo sempre deixa os blocos `[ANÁLISE DO(A) ESTAGIÁRIO(A)]` e `[CONCLUSÃO DO(A) ESTAGIÁRIO(A)]` em branco por design — `assist` significa que a skill produz o scaffold FIRAC e o enunciado-framework da regra; não produz a aplicação nem a conclusão.)
- **`teach`:** Não produza o framework nem o conteúdo do scaffold. Peça ao(à) estagiário(a) para enquadrar as questões, enunciar as regras a partir da pesquisa dele(a), e fazer a aplicação. Dê feedback. Faça perguntas leading quando travam. Só mostre um enunciado-modelo de regra ou um parágrafo-modelo de aplicação depois de duas tentativas, e só para a seção em que está travado(a). Acompanhe o que acertou e errou para que o(a) supervisor(a) veja progresso.

Se nenhum guia existe, use `guide`. Se o guia existe mas não seta postura, use `guide`.

Qualquer que seja a postura, o output sempre inclui: "**Modo pedagógico: [assist/guide/teach]** — setado pelo guia do(a) seu(sua) supervisor(a). Isso significa que eu [descrição do que o(a) estagiário(a) fez vs. o que a skill fez]."

## Workflow

### Passo 1: Enquadre as questões

Do sumário de intake e das notas do caso: quais são as questões jurídicas que este caso apresenta?

Enuncie cada uma como pergunta. Não "habitabilidade do imóvel locado" — "O(a) assistido(a) pode opor exceção de habitabilidade prejudicada à ação de despejo por falta de pagamento, com base na falha estrutural não reparada pelo(a) locador(a), e isso compensa o aluguel devido?"

Se há múltiplas questões, cada uma ganha seu próprio bloco FIRAC.

### Passo 2: Escafolde o FIRAC

Para cada questão:

**Fatos:** Os fatos relevantes do caso (do intake), enxutos para o necessário à análise dessa questão.

**Issue:** Enunciada como pergunta (do Passo 1).

**Regra:** Esta é lacuna de pesquisa, não conclusão. Enuncie o que o(a) estagiário(a) precisa achar:

> `[PESQUISA NECESSÁRIA: doutrina e jurisprudência sobre exceção de
> habitabilidade prejudicada em ação de despejo na Lei 8.245/91 — fundamentos
> nos arts. 22 e 23 (obrigações do(a) locador(a)), elementos, remédios
> incluindo compensação de aluguel. Comece em: Lei 8.245/91 arts. 22-23,
> depois jurisprudência STJ sobre exceção de contrato não cumprido em
> locação (CC art. 476). Veja /research-start para roadmap.]`

Se a skill tem confiança alta no framework geral da regra (ex.: "a maioria das relações locatícias civis admite exceção de contrato não cumprido"), enuncie como ponto de partida framework — **mas explicitamente marque como não-verificada**:

> *Framework (não-verificado — confirmar para [jurisdição]):* O Código Civil
> reconhece a exceptio non adimpleti contractus (CC art. 476), aplicável a
> relações sinalagmáticas, incluindo locação (Lei 8.245/91 que disciplina
> deveres do(a) locador(a) nos arts. 22-23). A jurisprudência admite
> compensação ou redução proporcional do aluguel quando a falha do(a)
> locador(a) afeta o uso pacífico do imóvel.
> `[VERIFICAR: elementos específicos e remédios admitidos pelo STJ e TJAM]`

**Análise (Aplicação):** Aqui é onde vai a análise do(a) estagiário(a). Escafolde a estrutura, não preencha:

> `[ANÁLISE DO(A) ESTAGIÁRIO(A): Aplique a regra aos fatos. Fatos-chave a
> endereçar:
> - Sistema hidráulico com vazamento desde novembro — "quanto tempo é
>   irrazoável"?
> - Assistido(a) notificou o(a) locador(a) [quando? como? documentado?]
> - Resposta do(a) locador(a) ou falta dela
> - Específico de Lei 8.245/91 / TJAM: assistido(a) precisava de
>   notificação extrajudicial prévia? consignar aluguel em juízo? outros
>   pré-requisitos procedimentais?]`

Liste os fatos que importam. Deixe o(a) estagiário(a) fazer a aplicação.

**Conclusão:** Explicitamente em branco:

> `[CONCLUSÃO DO(A) ESTAGIÁRIO(A): Com base na sua pesquisa e análise acima,
> qual o resultado provável? Quão forte é esta defesa? Quais as fraquezas?]`

### Passo 3: Identifique pontos fortes, fracos, questões abertas

Seção separada, depois dos blocos FIRAC:

**Pontos fortes (aparentes dos fatos — estagiário(a) deve testar):**
- [Fato que parece útil e por quê]

**Pontos fracos (aparentes dos fatos — estagiário(a) deve avaliar quão sérios):**
- [Fato que parece danoso e por quê]
- `[INCERTO: se [X] é de fato ponto fraco — depende da regra de [jurisdição] sobre [Y]]`

**Questões abertas (coisas que o memo não responde sem mais informação):**
- Fatuais: [o que não sabemos do(a) assistido(a)]
- Jurídicas: [o que precisa de pesquisa]
- Estratégicas: [juízos para o(a) estagiário(a)/supervisor(a)]

## Output

```markdown
═══════════════════════════════════════════════════════════════════════
  SCAFFOLD ASSISTIDO POR IA — A ANÁLISE É SUA PARA ESCREVER
  Todo bloco [PESQUISA NECESSÁRIA] e [ANÁLISE DO(A) ESTAGIÁRIO(A)] é
  prompt, não placeholder para deletar. O pensamento acontece quando
  você preenche.
═══════════════════════════════════════════════════════════════════════

# Memo de Análise de Caso: [Assistido(a)] — [Matéria]

**Data:** [data] | **Por:** [estagiário(a)] | **Para:** [Supervisor(a)]

---

## Bottom line

[Pegar o caso / Declinar porque X / Precisa de mais informação sobre Y — próximo passo é Z]

---

## Questões Apresentadas

1. [Issue como pergunta]
2. [Issue como pergunta]

---

## Questão 1: [Issue]

### Regra

[Framework inicial com flags VERIFICAR, e blocos PESQUISA NECESSÁRIA]

### Aplicação

[Scaffold de ANÁLISE DO(A) ESTAGIÁRIO(A) com os fatos que importam]

### Conclusão

[CONCLUSÃO DO(A) ESTAGIÁRIO(A) — em branco]

---

[repita para cada questão]

---

## Pontos Fortes

[lista com ressalvas]

## Pontos Fracos

[lista com flags INCERTO onde aplicável]

## Questões Abertas

**Fatuais:** [lista]
**Jurídicas:** [lista — essas alimentam /research-start]
**Estratégicas:** [lista — essas são para discussão com Supervisor(a)]

---

## Sumário de lacunas de pesquisa

[Todo bloco PESQUISA NECESSÁRIA puxado para uma lista, para que o(a)
estagiário(a) trabalhe sistematicamente — e possa rodar /research-start em
cada]

═══════════════════════════════════════════════════════════════════════

## O que este memo NÃO é

Isto é scaffold, não análise. Os blocos [ANÁLISE DO(A) ESTAGIÁRIO(A)] são onde
vive o valor educacional — preenchê-los é o trabalho. Memo com esses blocos
ainda em branco é memo que ainda não foi escrito.

---

**Verificação de citação — exigida antes de uso.** Quaisquer regras-framework, julgados ou dispositivos sugeridos acima foram gerados por modelo de IA e não foram verificados. Antes de confiar em qualquer citação — ou de incluir em trabalho voltado ao(à) assistido(a) — rode pelos MCPs do plugin (JusRatio, BNP, CJF, TJAM, DataJud) ou pelo sítio oficial (planalto.gov.br, sítios dos tribunais) para acurácia e status atual de vigência/overruling. Sinalize citações não-verificadas ao(à) seu(sua) supervisor(a).

**Atribuição de fonte.** Tagueie toda citação sugerida no scaffold com de onde veio: `[JusRatio]`, `[BNP]`, `[CJF]`, `[TJAM]`, `[DataJud]` para citações recuperadas de MCP de pesquisa jurídica nesta sessão; `[lei / planalto.gov.br]` ou `[CNJ]` ou `[CSDPGE]` para texto puxado de sítio oficial nesta sessão; `[busca web — verificar]` para citações de busca web; `[conhecimento do modelo — verificar]` para citações recordadas de dados de treino; `[usuário forneceu]` para citações que supervisor(a) ou pasta do caso forneceram. Citações tagueadas `verificar` carregam risco maior de fabricação que citações ferramentaorecuperadas e devem ser checadas primeiro. Nunca retire ou colapse as tags — elas são o sinal mais rápido para o(a) supervisor(a) sobre quais citações verificar.

**Sem suplementação silenciosa.** Se uma consulta ao MCP de pesquisa configurado retorna poucos ou nenhum resultado para regra que o memo precisa, diga e pare. NÃO preencha a lacuna a partir de busca web ou conhecimento do modelo sem perguntar. Diga: "A busca retornou [N] resultados de [ferramenta]. Cobertura aparenta fina para [regra / questão]. Opções: (1) ampliar a query, (2) tentar outra ferramenta de pesquisa, (3) buscar na web — resultados serão tagueados `[busca web — verificar]` e devem ser checados contra fonte primária antes de confiar, ou (4) deixar `[REGRA A VERIFICAR]` e parar. Qual você prefere?" O(a) Defensor(a)-Supervisor(a) decide se aceita fontes de menor confiança.
```

## O que esta skill NÃO faz

- **Escrever a análise.** Ela escafolda o FIRAC e sinaliza as lacunas. O(a) estagiário(a) raciocina sobre a aplicação.
- **Fornecer regras verificadas.** Todo enunciado de regra é explicitamente não-verificado até o(a) estagiário(a) pesquisar.
- **Chegar a conclusões.** O C em FIRAC fica em branco de propósito.
- **Substituir a conversa com o(a) supervisor(a).** A seção Questões Abertas / Estratégicas é a pauta dessa conversa, não substituto.

## Encerre com a árvore de próximos passos

Encerre com a árvore de próximos passos conforme CLAUDE.md `## Outputs`. Customize as opções para o que esta skill acabou de produzir — os cinco branches default (redigir o X, escalonar, pegar mais fatos, observar e esperar, outra coisa) são ponto de partida, não lock-in. A árvore é o output; o(a) supervisor(a) escolhe.

