# Template de Dashboard

*Referenciado pelo guardrail de Oferta de Dashboard. Mantenha dashboards simples e consistentes — o valor é velocidade de compreensão, não polidez visual.*

## Estrutura (topo a base)

1. **Título e metadados.** O que é, quando foi gerado, o que cobre. Uma linha.
2. **Estatísticas-sumário.** As contagens que importam, color-coded. "40 achados: 🔴 3 bloqueantes · 🟠 8 altos · 🟡 15 médios · 🟢 14 baixos — 6 com prazo nesta semana." É a linha mais valiosa. Faça-a escaneável.
3. **A nota do revisor.** Mesmo formato de um bloco como qualquer output. Fontes, escopo, flags, antes-de-confiar. Dashboards não pulam a metadata de segurança.
4. **Gráfico(s).** Um ou dois no máximo. Escolha o que mostra o formato:
   - **Distribuição de risco** (barra): contagens por severidade. Use para achados, issues, flags.
   - **Breakdown por categoria** (pizza ou barra empilhada): contagens por tipo. Use para licenças OSS, tipos de contrato, áreas de atuação, varas atendidas.
   - **Timeline** (Gantt-lite ou tabela ordenada): datas em ordem. Use para controle de prazos do CPC, registros de renovação, checklist de fechamento, agenda de audiências.
   - Nunca mais de dois. Um dashboard com cinco gráficos é um relatório, e relatórios são mais difíceis de ler que a tabela.
5. **A tabela.** Ordenável, filtrável, color-coded por severidade/status. Colunas: as que estavam no output original, recortadas para o que cabe em uma tela. Coloque "detalhes" ou "notas" como última coluna — é a que é truncada.
6. **A árvore de decisão.** Mesmas opções do output em texto. "Próximo passo?"

## Rendering by surface

- **Cowork / Claude Desktop:** HTML artifact. Self-contained, single file, inline CSS. No external dependencies, no CDN, no npm. Tables: HTML `<table>` with `data-sort` attributes and a small inline JS sorter. Charts: inline SVG or Unicode block chars for bar charts. Keep the JS minimal — sorting and filtering, nothing else.
- **Claude Code:** Write the same HTML file to the plugin's outputs folder (`~/.claude/plugins/config/claude-for-legal/<plugin>/outputs/dashboard-<topic>-<date>.html`) and tell the user to open it: `open <path>` on macOS, or "open in your browser." Also produce a markdown version with Unicode block charts for the summary stats so the user can see the shape without leaving the terminal.
- **Excel (optional, where it fits):** For `tabular-review`, `renewal-tracker`, `entity-compliance`, and anything the user will take into a meeting or share with a non-technical stakeholder. Use the existing Excel output spec. Apply the formula-injection defense.
- **Escape untrusted input (apply every dashboard, every time).** Every value that came from outside this session — OSS package/license fields from third-party manifests, counterparty contract text, diligence findings, vendor names, matter descriptions, any user- or VDR-supplied string — must be HTML-escaped before it lands in the document. Escape `&`, `<`, `>`, `"`, `'` into entities when writing into table cells, summary lines, chart labels, and tooltip text. In the inline JS sorter/filter, set cell text via `textContent`, never `innerHTML`. Do not emit `<script>` blocks whose contents interpolate untrusted strings. Do not render untrusted URLs into `href` or `src` without scheme-checking (`http:` / `https:` / `mailto:` only). This is the HTML-surface equivalent of the formula-injection defense on the Excel side — same threat (attacker-controlled cell content), different execution surface (browser JS instead of spreadsheet formula). A dashboard the reviewer opens in a browser is a trust boundary; treat it like one.

## Keep it boring

- **Color palette:** Red / orange / yellow / green for severity. Gray for neutral. Blue for status. Nothing else.
- **No animations, no frameworks, no external fonts.** A dashboard that breaks offline is a dashboard that breaks.
- **No clever layouts.** Summary, reviewer note, chart, table, decision tree. Top to bottom. Every dashboard looks the same so the reader knows where to look.
- **The markdown version matters.** Some users are in a terminal and won't open a browser. The summary stat line with Unicode bars (e.g., `🔴 ███ 3  🟠 ████████ 8  🟡 ███████████████ 15  🟢 ██████████████ 14`) gives them the shape.
