---
name: customize
description: >
  Customização guiada do perfil da unidade ou NPJ — mudar uma coisa sem
  re-rodar a entrevista inteira de cold-start. Ajusta perfil da unidade,
  jurisdição, estilo de supervisão, templates por área de atuação, configuração
  de termo/semestre ou salvaguardas de output. Use quando o(a) usuário(a)
  disser "muda meu [item]", "novo semestre", "adiciona área de atuação",
  "atualiza minha config" ou "customizar".
argument-hint: "[section name, or describe what you want to change]"
---

# /customize

## Quando isto roda

O(a) usuário(a) digitou `/legal-clinic:customize`. Ele(a) (geralmente o(a)
Defensor(a)-Supervisor(a) ou Professor(a)-Orientador(a), às vezes um(a)
estagiário(a)) quer mudar algo no perfil da unidade — uma jurisdição, um estilo
de supervisão, um template de área de atuação, uma virada de termo — sem
re-rodar a entrevista inteira de cold-start e sem editar YAML na mão.

## O que fazer

1. **Leia a config.** Leia
   `~/.claude/plugins/config/claude-for-legal/legal-clinic/CLAUDE.md`.
   Se a config do plugin não existir ou ainda contiver valores `[PLACEHOLDER]`,
   diga:

   > Você ainda não rodou o setup. Rode `/legal-clinic:cold-start-interview`
   > primeiro — customize é para ajustar um perfil que você já tem.

2. **Mostre o mapa do que dá para customizar.** Liste o que está no perfil,
   agrupado, com sumário em uma linha do valor atual:

   - **Perfil da unidade / NPJ** — nome da unidade ou NPJ, IES sede (se NPJ),
     Defensor(a)-Supervisor(a) ou Professor(a)-Orientador(a), áreas de atuação
     ativas, limites de tipo de caso
   - **Jurisdição** — UF primária, comarca, varas, órgãos administrativos,
     caminho das resoluções e regimentos locais
   - **Estilo de supervisão** — informal vs. fila de revisão formal; se
     formal, quem revisa o quê antes de sair
   - **Templates por área de atuação** — quais templates estão ativos
     (Família/Sucessões, Consumidor, Saúde Pública, Previdenciário/BPC-LOAS,
     Locação, Possessória, etc.) e quaisquer overrides locais
   - **Termo / semestre** — termo atual, estagiários(as) ativos(as), regras de
     rollover, formato do memo de handoff
   - **Salvaguardas de output** — padrões de linguagem simples para outputs ao
     (à) assistido(a), regras de alerta de prazo, rotulagem de sigilo
   - **Documentos-semente** — manual/regimento da unidade, resoluções CSDPGE,
     regras locais (TJAM), cartas-modelo, memos de exemplo, biblioteca de
     formulários
   - **Outputs** — formato do guia do(a) supervisor(a), templates de carta ao
     (à) assistido(a), scaffolds de memo
   - **Workflow** — diretórios de caso, localização do tracker de prazos,
     canal da fila de revisão
   - **Integrações** — sistema interno (Sapiens-DPGU / sistema próprio AM) /
     armazenamento documental / acompanhamento processual e-SAJ via DataJud —
     status e fallbacks

3. **Pergunte o que quer mudar.**

   > O que você gostaria de ajustar? Escolha uma seção, ou descreva a mudança
   > com suas palavras.

4. **Faça a mudança.** Mostre o valor atual, peça o valor novo, explique o que
   muda downstream, confirme, escreva na config.

   Exemplos:
   - *Adicionando nova área de atuação:* "`/client-intake` vai rotear matérias
     desse tipo pelo novo template. `/draft`, `/memo` e `/client-letter` vão
     usar os prompts da área. `/research-start` vai adicionar os termos de
     busca correspondentes nos MCPs (JusRatio / BNP / TJAM)."
   - *Estilo de supervisão informal → fila de revisão formal:*
     "`/supervisor-review-queue` fica ativa — output de estagiário(a) vai cair
     lá para sign-off do(a) supervisor(a) antes de ir ao(à) assistido(a)."
   - *Rollover de novo termo:* "Vou arquivar os casos ativos do termo
     anterior, carregar adiante as matérias que você sinalizar como
     continuando, e fazer os(as) estagiários(as) entrantes passarem pelo
     `/ramp`."

5. **Encerre.**

   > Pronto. Seu próximo output vai refletir a mudança. Mais alguma coisa?
   > Você pode rodar `/legal-clinic:customize` a qualquer momento.

## Guardrails

- **Nunca apague uma seção.** Se o(a) usuário(a) quiser "soltar" uma área de
  atuação, ofereça marcar como `[Arquivada]` e explique que arquivar mantém
  o histórico de caso acessível mas esconde o template do roteamento do
  `/client-intake`.
- **Sinalize inconsistência interna.** Se a mudança deixar o perfil
  inconsistente (ex.: fila de revisão formal ligada + nota de supervisão
  informal; ou área de atuação ligada + sem resoluções de jurisdição
  configuradas), sinalize a tensão.
- **Sinalize degradação de guardrail.** Estes são load-bearing e não devem ser
  removidos: o enquadramento "NÃO é trabalho final" no `/draft`, padrões de
  linguagem simples nos outputs ao(à) assistido(a), "NÃO decide aceitação do
  caso" no `/client-intake`, "NÃO é parecer substantivo" no `/client-letter`,
  e o enquadramento scaffold-não-análise no `/memo`. Eles existem porque
  estagiários(as) entregam produto — se as salvaguardas caem, o risco de
  trabalho de estagiário(a) chegar ao(à) assistido(a) sem revisão do(a)
  supervisor(a) sobe. Confirme o trade-off com o(a) usuário(a), e se for
  estagiário(a) e não o(a) supervisor(a), sugira que converse com o(a)
  supervisor(a) primeiro.
- **Uma mudança por vez.** Não re-pergunte a entrevista inteira.
