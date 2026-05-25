---
name: customize
description: >
  Customização guiada do seu perfil de atuação em contencioso — mude uma
  coisa sem rerodar o cold-start inteiro. Ajuste papel na advocacia,
  posição processual (autor / réu / ambos), calibração de risco, panorama,
  estilo da casa, contatos de escalonamento, vocabulário de severidade ou
  paths de workspace de caso. Use quando o usuário disser "mudar meu [x]",
  "atualizar meu perfil", "editar meu config", ou "customizar".
argument-hint: "[section name, or describe what you want to change]"
---

# /customize

## Quando isto roda

O usuário digitou `/litigation-legal:customize`. Quer mudar algo no perfil
de contencioso — uma calibração de risco, uma regra de estilo da casa, um
contato de escalonamento, uma nota de panorama — sem rerodar a entrevista
inteira de cold-start e sem editar YAML na mão.

## O que fazer

1. **Ler o config.** Leia
   `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md`
   (e `~/.claude/plugins/config/claude-for-legal/company-profile.md` um
   nível acima). Se o config do plugin não existe ou ainda contém
   `[PLACEHOLDER]`, diga:

   > Você ainda não rodou o setup. Rode primeiro
   > `/litigation-legal:cold-start-interview` — customize é para ajustar
   > um perfil que você já tem.

2. **Mostre o mapa customizável.** Liste o que está no perfil, agrupado,
   com sumário de uma linha do valor atual:

   - **Pessoa jurídica / quem você é** — razão social, setor, jurisdições,
     porte, contexto de atuação *(compartilhado entre os 12 plugins —
     mudanças propagam via `company-profile.md`)*
   - **Papel na advocacia** — Defensor(a) Público(a) / departamento
     jurídico / advogado(a) em sociedade / advogado(a) autônomo(a)
   - **Posição processual** — autor / réu / ambos, e nuances de postura
     (defesa em ação coletiva, defesa em ação regulatória, autor
     comercial, etc.)
   - **Calibração de risco** — o que conta como alto / médio / baixo risco
     em notificação extrajudicial recebida, ofício requisitório, ou caso
     novo; gatilhos de escalonamento
   - **Panorama** — contrapartes frequentes, foros amigáveis e desfavoráveis,
     magistrados conhecidos, relações estáveis com escritórios externos /
     núcleos especializados
   - **Estilo da casa** — estilo de peças, formato de declaração, template
     de notificação extrajudicial, estrutura de outline de oitiva, template
     de comunicação de dever de guarda documental
   - **Mapa de vocabulário de severidade** — como você traduz rótulos de
     severidade entre outputs ao cliente / internos / endereçados ao juízo
   - **Pessoas** — responsáveis por caso, equipe interna, escritórios
     externos por tipo de matéria, cadeia de escalonamento
   - **Workflow** — workspaces de caso, log de portfólio, cadência de
     status com escritórios externos, cadência de renovação de dever de
     guarda
   - **Integrações** — armazenamento documental / protocolo eletrônico
     (PJe / eproc / e-SAJ / Projudi) / agenda / status, fallbacks

3. **Pergunte o que quer mudar.**

   > O que você gostaria de ajustar? Escolha uma seção, ou descreva a
   > mudança com suas próprias palavras.

4. **Faça a mudança.** Mostre o valor atual, peça o novo valor, explique
   o que muda downstream, confirme, escreva no config.

   Exemplos:
   - *Posição ambos → réu apenas:* "`/matter-intake` vai parar de
     perguntar as questões de polo ativo. `/demand-draft` continua
     funcionando para notificações pré-litigiosas defensivas mas o frame
     inicial será diferente."
   - *Calibração de risco apertando o limiar de alto risco:* "Mais
     notificações recebidas e ofícios passarão por `/matter-briefing` e
     `/oc-status`."
   - *Novo escritório externo padrão para matéria de PI:* "`/oc-status`
     incluirá este escritório nas varreduras semanais para casos com tag
     de PI."

5. **Para mudanças de perfil compartilhado** (razão social, setor,
   jurisdições, contexto de atuação, porte): escreva em
   `~/.claude/plugins/config/claude-for-legal/company-profile.md` e note:

   > Esta mudança afeta todos os 12 plugins — qualquer plugin que lê seu
   > footprint jurisdicional agora vê [novo valor].

6. **Fechamento.**

   > Pronto. Seu próximo output vai refletir a mudança. Mais alguma coisa?
   > Pode rodar `/litigation-legal:customize` a qualquer momento.

## Guardrails

- **Nunca apague uma seção.** Se o usuário quer "remover" um tipo de
  matéria do escopo, ofereça marcar como `[Não tratado atualmente]` e
  explique o que muda no roteamento do intake.
- **Sinalize inconsistência interna.** Se a mudança tornar o perfil
  inconsistente (ex.: posição autor-apenas + roster de escritórios
  externos só defensivo; ou "alto volume" de portfólio + sem workspaces
  de caso configurados), sinalize a tensão.
- **Sinalize degradação de guardrail.** O gate de confidencialidade
  negocial (Lei 13.140/2015 art. 30) / sigilo profissional em
  `/demand-draft`, o cabeçalho de sigilo em outputs de caso, tags de
  atribuição de fonte, e tags `[verificar]` em autoridades citadas são
  load-bearing — não remova. A flag `[review]` e o framing "não
  protocolar sem revisão do(a) advogado(a)" são load-bearing.
- **Uma mudança por vez.** Não rerode a entrevista inteira.
