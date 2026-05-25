---
name: matter-workspace
description: Gerencia workspaces de caso para advocacia multi-cliente — cria, lista, troca, fecha ou desliga o caso ativo. Use quando o usuário quer criar novo workspace de caso, trocar o caso ativo, listar casos, arquivar um caso, ou trabalhar em nível-prática sem caso ativo.
argument-hint: "<new | list | switch | close | none> [slug]"
---

# /matter-workspace

Profissionais atuam em vários clientes/assistidos e casos. Um workspace de caso mantém o contexto de um(a) cliente/assistido(a) ou patrocínio separado de todos os outros. Este comando gerencia esses workspaces.

## Subcomandos

- `/litigation-legal:matter-workspace new <slug>` — cria novo workspace de caso, roda intake curto, grava `matter.md`
- `/litigation-legal:matter-workspace list` — lista casos com status e flag de ativo
- `/litigation-legal:matter-workspace switch <slug>` — define o caso ativo
- `/litigation-legal:matter-workspace close <slug>` — arquiva um caso (move para `~/.claude/plugins/config/claude-for-legal/litigation-legal/matters/_archived/`, nunca apaga)
- `/litigation-legal:matter-workspace none` — desacopla de qualquer caso ativo, trabalha só em nível-prática

Nota: `/litigation-legal:matter-briefing [slug]` (sem subcomando) é um comando separado que produz um briefing de caso específico — útil para revisão de portfólio em DJ. A gestão de workspace de caso vive aqui.

## Instruções

1. Leia `~/.claude/plugins/config/claude-for-legal/litigation-legal/CLAUDE.md` — confirme que a seção `## Workspaces de caso` está populada. Se `Habilitado` é `✗`, diga ao usuário: "Workspaces de caso estão desligados — você está configurado como prática com um cliente, então o plugin trabalha a partir do contexto de nível-prática automaticamente. Se você efetivamente atende múltiplos clientes/assistidos, rerode `/litigation-legal:cold-start-interview --redo` e selecione uma configuração de advocacia privada ou Defensoria. Caso contrário, não precisa de `/matter-workspace`." Não dê erro — o estado desligado é o esperado para usuários de DJ.
2. Siga o workflow e a referência abaixo.
3. Despache no primeiro token de `$ARGUMENTS`:
   - `new` → rode a entrevista de intake, grave `~/.claude/plugins/config/claude-for-legal/litigation-legal/matters/<slug>/matter.md`, semeie `history.md` e `notes.md`.
   - `list` → enumere `~/.claude/plugins/config/claude-for-legal/litigation-legal/matters/*/matter.md`, imprima tabela, marque o caso ativo.
   - `switch` → atualize a linha `Caso ativo:` no CLAUDE.md de nível-prática.
   - `close` → mova `~/.claude/plugins/config/claude-for-legal/litigation-legal/matters/<slug>/` para `~/.claude/plugins/config/claude-for-legal/litigation-legal/matters/_archived/<slug>/`, registre a data de fechamento em `history.md`.
   - `none` → defina `Caso ativo:` como `nenhum — só contexto de nível-prática`.
4. Mostre ao usuário o que mudou e confirme antes de gravar.

## Notas

- A skill nunca lê entre casos a menos que `Contexto cruzado entre casos` esteja `on` no CLAUDE.md de nível-prática.
- Arquivar não é apagar — casos fechados permanecem legíveis para fins de retenção / checagem de impedimentos.
- Slugs são minúsculos com hífens. Se um slug é reutilizado entre arquivados e ativos, o arquivado é preservado sob `_archived/<slug>/`.

---

# Matter Workspace

Profissionais multi-cliente (advocacia privada — autônomo, banca pequena, banca grande; Defensoria Pública atendendo múltiplos(as) assistidos(as)) atuam em vários casos. Contexto de um não pode vazar no outro. Esta skill é a camada fina de gestão de arquivos que torna isso verdadeiro.

**Estado default é desligado.** Usuários de DJ nunca veem isto — rodam só em nível-prática. Workspaces de caso ligam no cold-start para usuários de advocacia privada ou Defensoria, ou editando `## Workspaces de caso` no CLAUDE.md de nível-prática. Se `Habilitado` é `✗`, esta skill não roda; a skill `/matter-workspace` explica o estado desligado e sugere `/cold-start-interview --redo` para usuários que efetivamente precisam de isolamento de caso.

## Layout de armazenamento

Todo dado de caso vive sob:

```
~/.claude/plugins/config/claude-for-legal/litigation-legal/
├── CLAUDE.md                       # perfil de atuação nível-prática
└── matters/
    ├── <slug>/
    │   ├── matter.md               # cliente/assistido, contraparte, tipo de caso, fatos-chave, overrides
    │   ├── history.md              # log datado de eventos, decisões, minutas, revisões
    │   ├── notes.md                # notas de trabalho em formato livre
    │   └── outputs/                # outputs de skills para este caso (subpasta opcional)
    └── _archived/
        └── <slug>/                 # casos fechados — legíveis mas não ativos
```

Slugs são minúsculos com hífens. Exemplos: `silva-vs-cemig-2026`, `bpc-loas-maria-souza`, `obriga-fazer-amil-2026`.

## Caso ativo está no CLAUDE.md de nível-prática

A linha `Caso ativo:` sob `## Workspaces de caso` no CLAUDE.md de nível-prática é a fonte única da verdade. Trocar de caso edita aquela linha. Sem arquivo de estado separado.

## Lógica dos subcomandos

### `new <slug>`

1. Confirme que o slug já não está presente em `matters/<slug>/` ou `matters/_archived/<slug>/`. Se reutilizado, peça ao usuário para escolher outro slug.
2. Rode a entrevista de intake:
   - **Cliente / Assistido(a)** (a parte que representamos, ou a unidade interna de negócio se DJ)
   - **Contraparte** (o outro lado — pode ser múltiplas)
   - **Tipo de caso** (leia o perfil de atuação do plugin para categorias típicas; para litigation-legal: cível contratual | trabalhista | consumidor | PI | regulatório / investigação | responsabilidade civil | ação coletiva | obrigação de fazer/saúde | benefício previdenciário (BPC, aposentadoria) | família / sucessões | outro)
   - **Nível de confidencialidade** (padrão | reforçado | segredo de justiça (CPC art. 189) — reforçado solicita cuidado extra em settings cross-matter)
   - **Fatos-chave** (2–5 frases: do que se trata o caso, quem são os stakeholders, o que está em jogo)
   - **Overrides específicos do caso ao playbook da prática** (ex.: "cliente exige limite de responsabilidade de 24 meses, não 12", "contraparte é parceiro estratégico — tom preservador da relação", "assistida em situação de violência doméstica — articular com rede de proteção")
   - **Casos relacionados** (slugs de quaisquer casos conexos)
3. Grave `matters/<slug>/matter.md` usando o template abaixo.
4. Semeie `matters/<slug>/history.md` com uma única entrada "Aberto".
5. Crie um `matters/<slug>/notes.md` vazio.
6. **Não** troque automaticamente para o novo caso. Pergunte: "Quer trocar para `<slug>` agora? (`/litigation-legal:matter-workspace switch <slug>`)"

### `list`

Enumere `matters/*/matter.md`. Leia frontmatter ou primeiras linhas de cada arquivo para extrair status. Imprima uma tabela:

| Slug | Cliente / Assistido(a) | Tipo de caso | Status | Aberto | Ativo |
|---|---|---|---|---|---|

Marque o caso atualmente ativo com `*`. Inclua `_archived/*` sob heading separado "Arquivados" se existir algum.

### `switch <slug>`

1. Confirme que `matters/<slug>/matter.md` existe. Se não, ofereça `/litigation-legal:matter-workspace new <slug>`.
2. Edite a linha `Caso ativo:` no CLAUDE.md de nível-prática para `Caso ativo: <slug>`.
3. Mostre ao usuário o sumário de matter.md para confirmar que está no caso certo.

### `close <slug>`

1. Confirme que `matters/<slug>/` existe.
2. Anexe entrada "Fechado" a `matters/<slug>/history.md` com a data de hoje.
3. Mova `matters/<slug>/` → `matters/_archived/<slug>/`.
4. Se o caso fechado era o ativo, defina `Caso ativo:` como `nenhum — só contexto de nível-prática`.

### `none`

Defina `Caso ativo:` no CLAUDE.md de nível-prática como `nenhum — só contexto de nível-prática`. Confirme com o usuário.

## Template `matter.md`

```markdown
[CABEÇALHO DE SIGILO — por config do plugin ## Outputs — varia por papel; vide `## Quem está usando` no CLAUDE.md de nível-prática]

# Caso: [Cliente / Assistido(a)] — [descrição curta]

**Slug:** [slug]
**Aberto:** [YYYY-MM-DD]
**Status:** ativo
**Confidencialidade:** [padrão / reforçado / segredo de justiça]

---

## Partes

**Cliente / Assistido(a):** [nome]
**Contraparte:** [nome(s)]

## Tipo de caso

[obrigação de fazer / cobrança / indenização / ação revisional / BPC / família / outro — com racional de uma linha]

## Fatos-chave

[2–5 frases. Do que se trata o caso. Quem são os stakeholders. O que está em jogo. O que torna diferente do playbook default.]

## Overrides específicos do caso

*Qualquer desvio do playbook nível-prática que se aplique a este caso e só a ele.*

- [ex.: "Tom: preservador da relação — contraparte é parceiro estratégico."]
- [ex.: "Lei de regência: deve ser direito inglês, não brasileiro."]
- [ex.: "Assistida em risco de violência — articular com CREAS e CRAM antes de cada audiência."]

## Casos relacionados

- [slug — uma linha do porquê estão relacionados]

## Notas de confidencialidade

[Se reforçado ou segredo de justiça (CPC art. 189), descreva o porquê. Quem pode ver os arquivos do caso. Se contexto cross-matter é admissível mesmo se globalmente ligado.]
```

## Seed de `history.md`

```markdown
# Histórico: [Cliente / Assistido(a)] — [descrição curta]

Log de eventos somente-anexar. Mais recente no topo.

---

## [YYYY-MM-DD] — Caso aberto

Intake completo. Slug: `[slug]`. Status: ativo.
[Qualquer contexto inicial que valha preservar além de matter.md — ex.: "Aberto em resposta a notificação extrajudicial recebida da [contraparte]."]
```

## Contexto cruzado entre casos

O CLAUDE.md de nível-prática tem uma flag `Contexto cruzado entre casos:`. Quando está `off` (o default), uma skill trabalhando no caso A **nunca lê** arquivos em `matters/B/` para nenhum outro `B`. Ponto final. Esta é a garantia de confidencialidade pela qual a configuração existe.

Quando está `on`, uma skill pode ler arquivos entre pastas de caso só quando o usuário explicitamente pede (ex.: "compare nossa posição sobre limitação de responsabilidade nos últimos cinco casos com fornecedores"). Mesmo quando `on`, o default é carregar só o caso ativo a menos que o usuário peça visão cross-matter.

## O que esta skill não faz

- **Roda checagem de impedimentos.** Impedimentos são responsabilidade do(a) profissional / banca / unidade da DP; o intake captura o que o usuário declara.
- **Aplica retenção.** Fechar arquiva um caso; não apaga. Política de retenção está fora de escopo.
- **Roteia outputs automaticamente.** A skill substantiva decide onde gravar; esta skill diz *qual pasta* está ativa, não o que pôr nela.
- **Decide se cross-matter é apropriado.** Lê a flag e obedece.
