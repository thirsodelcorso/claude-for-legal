---
name: plain-language-letters
description: >
  Referência: OBSOLETA — use `/client-letter` para correspondência de rotina
  ou `/status client` para atualizações substantivas. Dividida em duas skills
  mais focadas durante o rebuild v2. Mantida como redirecionamento para
  migração.
user-invocable: false
---

# [OBSOLETA] Plain-Language Letters → ver `/client-letter` e `/status client`

Esta skill foi dividida durante o rebuild v2:

- **Correspondência de rotina** (confirmação de atendimento, pedido de
  documentos, breve atualização do tipo "protocolei") → `skills/client-letter/`
  — use `/client-letter [tipo]`

- **Atualizações substantivas de status ao(à) assistido(a)** →
  `skills/status/` em modo voltado ao(à) assistido(a) — use `/status client`

Ambas aplicam os padrões de linguagem simples (nível de leitura, sem jargão) do
CLAUDE.md.

Ver os arquivos SKILL.md respectivos para o workflow completo.
