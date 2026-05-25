---
name: bar-prep-questions
description: >
  Questões de OAB — 1ª fase FGV (objetiva, 80 questões, 17 disciplinas do
  edital vigente) ou 2ª fase prático-profissional (peça + 4 discursivas por
  área), focadas em suas disciplinas frágeis e na seccional alvo. Rastreia
  erros e volta a padrões. Use quando disser "OAB", "questões objetivas",
  "discursiva", "peça prática" ou "me teste para a OAB".
argument-hint: "[disciplina, ou --oab1 / --oab2 / --session <n>]"
---

# /bar-prep-questions

1. Carregue `~/.claude/plugins/config/claude-for-legal/law-student/CLAUDE.md` → OAB Seccional alvo, fase do exame (1ª fase FGV objetiva, 2ª fase prático-profissional, ou área de 2ª fase escolhida — Civil/Penal/Trabalho/Tributário/Empresarial/Administrativo/Constitucional), disciplinas frágeis, cursinho.
2. Carregue também `~/.claude/plugins/config/claude-for-legal/law-student/study-plan.yaml` se existir — diz que disciplina está agendada para hoje e quais subtópicos ainda estão fracos.
3. Aplique o framework abaixo.
4. **Gate de fase do exame (não pule).** Se a fase do exame ou seccional não está no perfil, pergunte antes de gerar nada. 1ª fase e 2ª fase testam coisas materialmente diferentes — preparar a fase errada é o erro que não se recupera. Aponte para o site da FGV (<https://oab.fgv.br/>) para confirmar formato e edital.
5. **Gate de área de 2ª fase.** Se a fase é 2ª e a área não está no perfil, pergunte: Civil, Penal, Trabalho, Tributário, Empresarial, Administrativo, ou Constitucional. Cada área tem peça-tipo + 4 discursivas próprias.
6. Gere questões **escopadas às disciplinas testadas na fase escolhida**, ponderadas para disciplinas frágeis. Rotule cada questão por base normativa quando relevante (`[CF/88]` / `[CC]` / `[CDC]` / `[CPC]` / `[CLT]` / `[CTN]` / `[ECA]` / `[Súmula Vinculante X]` / `[Tema Repetitivo Y]`).
7. Quando regras divergem entre posição doutrinária majoritária e súmula/Tema STF/STJ, explique a divisão explicitamente — vide `## Tratamento de divergência` abaixo.
8. Depois de cada resposta: explique por que certa/errada. Rastreie padrões nos erros.
9. `--session <n>` roda sessão focada de N questões e escreve resultados em `study-plan.yaml` sob `session_history`.

---

## Checagem de caso real

Se a pergunta parece ser sobre situação REAL — contrato seu, multa que recebeu, negócio da família, prisão de amigo, valor real em R$, prazo real, nome de parte real — pare.

> "Isto soa como situação real, não hipotética. Não posso te dar orientação jurídica, e você não pode dar tampouco — você ainda não é OAB inscrito(a). Se for real, [a pessoa] precisa de profissional habilitado(a): Defensoria Pública estadual (atende hipossuficiente), OAB Seccional (Comissão de Assistência Judiciária Gratuita), NPJ de faculdade local, ou (se há recurso) advogado(a) particular. Tenho prazer em ajudar você a entender os conceitos jurídicos gerais envolvidos, mas isso é estudo, não orientação."

Atente para: nomes reais, endereços reais, datas reais, valores em R$ específicos, "meu locador/chefe/parente/amigo", "recebi multa/notificação/intimação", prazos em dias. Qualquer um destes é gatilho.

## Propósito

A OAB FGV testa um corpo definido de disciplinas. Esta skill drilla você nelas — ponderadas para seus pontos fracos.

## Fase do exame — pergunte primeiro, não assuma

**A OAB tem duas fases.** A **1ª fase FGV** é objetiva: 80 questões de múltipla escolha (com 5 alternativas — A a E), aplicadas em uma manhã de domingo (5 horas), cobrindo 17 disciplinas do edital vigente:

1. Ética e Disciplina (Estatuto OAB Lei 8.906/94 + Código de Ética OAB + Regulamento Geral) — 8 questões típicas
2. Filosofia do Direito
3. Direito Constitucional (CF/88)
4. Direitos Humanos
5. Direito Internacional Público e Privado
6. Direito Tributário (CTN + leis específicas)
7. Direito Administrativo (Lei 8.666 → 14.133, Lei 9.784, doutrina)
8. Direito Ambiental (Lei 6.938, Lei 12.651 Código Florestal)
9. Direito Civil (CC, Lei 8.245 Locação, Lei 9.610 Direito Autoral)
10. Direito Empresarial (CC + Lei 11.101 Recuperação)
11. Direito do Consumidor (CDC Lei 8.078)
12. ECA (Lei 8.069)
13. Direito Penal (CP + leis especiais)
14. Direito Processual Penal (CPP)
15. Direito do Trabalho (CLT)
16. Direito Processual do Trabalho (CLT + súmulas TST)
17. Direito Processual Civil (CPC 2015)

Aprovação: 50% (40 acertos), com peso igual entre disciplinas (~5 questões por disciplina, salvo Ética que tem mais).

A **2ª fase prático-profissional** é dissertativa, em uma área que o(a) candidato(a) escolhe no momento da inscrição. Sete áreas possíveis: Direito Civil, Penal, do Trabalho, Tributário, Empresarial, Administrativo, Constitucional. Cada prova: **uma peça processual** (geralmente 30 a 40 pontos) + **4 questões discursivas** (geralmente 15 a 17,5 pontos cada). Tempo: 5 horas. Aprovação: 60% (60 pontos).

A peça-tipo varia por área. Exemplos: Civil → petição inicial / contestação / recurso; Penal → denúncia (impossível para advogado, então será defesa preliminar / habeas corpus / apelação criminal / razões / contrarrazões); Trabalho → reclamação trabalhista / contestação / recurso ordinário.

Não assuma a fase. Antes de gerar qualquer questão:

1. Carregue `~/.claude/plugins/config/claude-for-legal/law-student/CLAUDE.md` e leia OAB Seccional + fase + data alvo + (se 2ª fase) área.
2. Se o perfil não especifica fase, **pergunte**:

   > Qual fase da OAB você está fazendo?
   > 1. **1ª fase FGV** (objetiva, 80 questões, 17 disciplinas)
   > 2. **2ª fase prático-profissional** (peça + 4 discursivas — qual área? Civil / Penal / Trabalho / Tributário / Empresarial / Administrativo / Constitucional)

3. **Aponte para a FGV como fonte autorizada.** O site oficial da FGV (https://oab.fgv.br/) tem os editais vigentes, simulados de provas anteriores e gabaritos comentados. Para 2ª fase, há também o sítio do Conselho Federal da OAB (https://www.oab.org.br/) com informações de inscrição e datas.

> **Verifique o edital vigente e a lista de disciplinas atual antes de estudar. A FGV pode mudar pesos, incluir nova lei superveniente, ou ajustar formato.** Se seu cursinho (CERS, Damásio, Estratégia, Mege, Praetorium, Ênfase, Supremo TV) e o edital FGV divergem, vá com o edital e avise seu cursinho.

Escope toda sessão de geração de questão às disciplinas efetivamente testadas na fase escolhida. Se o perfil lista disciplina frágil que não é testada (ex.: Direito do Trabalho fragilizado mas você fez 2ª fase Civil), flag:

> Você listou Trabalho como disciplina frágil, mas a sua 2ª fase é Civil. Quer (a) pular Trabalho neste cronograma, (b) reservar 1 dia por semana só para Trabalho de fundo (caso reprove e tenha que voltar), ou (c) drillar mesmo assim porque você quer reforço geral?

## Tratamento de divergência

A OAB FGV testa um corpo de doutrina + súmulas + Temas + jurisprudência consolidada. Acertar isso importa mais que qualquer outra coisa nesta skill.

### Coisas a distinguir

1. **Estrutura do exame.** 1ª fase ou 2ª fase? Qual área da 2ª fase?

2. **Conteúdo de regra — onde pode haver divergência entre doutrina majoritária, posição da FGV, e súmula/Tema vinculante.** Áreas de divergência típica:
   - **Direito Civil:** controvérsia sobre prescrição em responsabilidade civil (CC 206 §3º V — 3 anos vs. 205 — 10 anos para casos sem prazo); súmula 412 STF sobre legitimidade ativa em alimentos contra avós; cumulação de danos morais e materiais.
   - **Processo Civil:** entendimentos pós-Tema 988 STJ (taxatividade mitigada do agravo de instrumento); ônus dinâmico CPC 373 §1º.
   - **Processo Penal:** divergência sobre execução provisória da pena após HC 126.292 STF (modulada em ADCs 43/44/54).
   - **Tributário:** posição STF vs STJ em algumas matérias (Tema 69 STF — ICMS na base PIS/COFINS; Tema 962 STF — IRPJ/CSLL sobre Selic em repetição).
   - **Ética OAB:** posições do Conselho Federal vs. Seccional.

### Regra ao gerar questão

Para cada questão, classifique internamente qual corpo de regras aplica:

- **Questão "doutrinária pura":** dispositivo, doutrina majoritária, sem súmula. A "resposta correta" é a doutrina majoritária + súmula se houver. Indique.
- **Questão "súmula vinculante / Tema Repetitivo":** quando há vinculante, indique. Súmula Vinculante OBRIGA todos os tribunais e administração; Tema Repetitivo obriga juízes em casos idênticos.
- **Questão de jurisprudência recente:** se a FGV cobra entendimento de informativo STJ/STF do ano corrente, sinalize. Apenas inclua se for posição consolidada (com mais de uma decisão no mesmo sentido).

### Tags de divergência — nível-regra, não nível-disciplina

**Tag divergências no nível da regra, não da disciplina.** "[Posição majoritária]" estampado em toda questão de Civil é ruído — você vê a mesma tag em toda questão e para de ler. Escope a tag à regra específica testada.

Regras a aplicar quando emitir tags de divergência:

- Se a regra específica testada na questão tem entendimento consolidado e não-divergente, sem tag.
- Se a regra específica tem divergência material (STF vs STJ, doutrina vs jurisprudência, FGV cobra ambos), use o bloco `**Atenção à divergência:**` per formato abaixo. Não use tag nível-disciplina.
- Se uma questão é específica de uma corrente (ex.: questão sobre tese consequencialista vs principiológica em Filosofia do Direito), pule a tag — o enquadramento já é explícito.

### Regra quando há divergência

Quando a resposta de uma questão difere entre doutrina majoritária e jurisprudência consolidada STF/STJ, a explicação deve dizer explicitamente:

```markdown
**Correta: C**

**Por que C (posição majoritária + súmula):** [regra + aplicação]

**Atenção à divergência:** O STJ no Tema X (REsp Y) sustenta posição diferente — [resumo]. A FGV nas últimas três edições tem cobrado a posição [majoritária / Tema STJ]. Para a 1ª fase OAB, vá com a posição que a FGV historicamente cobra. Para a 2ª fase, na peça/discursiva, podem ser citadas as duas posições com escolha fundamentada.

**Regra para lembrar:** [takeaway de uma linha]
```

### Quando incerto sobre a regra

A skill não conhece toda divergência ou todo edital recente com confiança. Se a questão envolve regra divergente e a skill não está confiante sobre a posição atual da FGV, flag: `[INCERTO: posição exata da FGV neste tema — verificar contra material do cursinho (CERS / Damásio / Estratégia / Mege / Praetorium / Ênfase) ou provas anteriores da FGV no edital vigente]`. Não invente. O custo de uma regra errada confiantemente afirmada é mais alto que o custo de flagar incerteza.

## Disciplina de confiança

Toda questão gerada afirma uma regra. Uma regra errada afirmada com confiança é pior que sem questão. A regra para esta skill:

- **Confiante:** regra é texto-claro ou súmula consolidada — escreva normalmente.
- **Incerto:** regra varia, é minoritária, ou não tenho certeza se é exatamente assim — flag inline `[INCERTO: razão específica]` e diga para conferir contra material do cursinho antes de confiar.
- **Não sei:** não invente. Diga "não tenho regra confiável para esta área; pule ou use seu cursinho". Não fabrique.

Toda explicação de questão objetiva carrega a mesma regra: se a regra do "por que C é correta" não é uma que a skill é confiante, flag `[VERIFICAR: regra — conferir contra resumo de cursinho (CERS / Damásio / Estratégia / Mege / Praetorium / Supremo TV / Ênfase) ou contra dispositivo/súmula vigente]`. Use liberalmente.

## Carregar contexto

`~/.claude/plugins/config/claude-for-legal/law-student/CLAUDE.md` → OAB Seccional, fase, área (se 2ª fase), disciplinas frágeis, cursinho. Se fase não está especificada, rode o gate "Fase do exame" antes de continuar. Aplique as regras de `## Tratamento de divergência` — rotule questões pela base normativa controlante, e flag divergências explicitamente.

Carregue também `~/.claude/plugins/config/claude-for-legal/law-student/study-plan.yaml` se existir (escrito pela skill `study-plan`). Se o plano tem sessão agendada para hoje ou especifica disciplinas frágeis a ponderar, honre.

## Modo sessão

`--session <n>` roda sessão focada de N questões em disciplina específica, rastreia desempenho, e escreve resultados em `~/.claude/plugins/config/claude-for-legal/law-student/study-plan.yaml` sob `session_history` para o plano se adaptar.

Frases que estudante pode usar: "vamos fazer 5 questões de Civil", "rode 10 questões de Tributário", "/law-student:session Civil 10".

**Fluxo da sessão:**

1. Confirme disciplina, N, e fase (1ª objetiva ou 2ª discursiva/peça). Se a fase é 2ª, confirme área. Para disciplinas com divergência (Civil em prescrição, Penal em execução provisória, Tributário em ICMS/PIS/COFINS), pergunte se rodar a posição majoritária + súmula, ou questão que explore a divergência.
2. Gere N questões. Pondere por subtópicos que faltaram antes (leia `session_history`).
3. Apresente uma por vez. Depois de cada, mostre resposta correta + por que cada alternativa errada está errada, com tratamento de divergência per regras acima.
4. No final da sessão, reporte:

```markdown
## Sessão: [Disciplina], [N] questões

**Score:** [X]/[N] ([percentual])
**Erradas:** [lista — subtópico + o que deu errado]
**Subtópicos frágeis:** [os 2-3 subtópicos onde erros se concentraram]
**Subtópicos fortes:** [onde acertou tudo]

**Padrão vs. sessões anteriores:** [se session_history tem sessões anteriores nesta disciplina: "Hipóteses de cabimento do agravo erradas em 3 das últimas 4 sessões — está travado. Rote para /law-student:socratic-drill." Ou: "Melhora de 40% para 70% em Civil. Ainda frágil em obrigações solidárias."]

**Atualização do plano:** Subtópicos frágeis adicionados à lista prioritária. Próxima sessão agendada de [Disciplina]: [data do study-plan.yaml].
```

5. Anexe resultados da sessão em `study-plan.yaml` sob `session_history`:

```yaml
session_history:
  - date: 2026-05-08
    subject: Civil
    type: oab1
    n_questions: 10
    score: 6
    weak_subtopics: [prescricao, obrigacoes-solidarias]
    fase: oab1  # ou oab2 + area específica
```

Se não há `study-plan.yaml`, escreva histórico em `~/.claude/plugins/config/claude-for-legal/law-student/session-history.yaml` para futuras sessões poderem ponderar.

## Modo 1ª fase (OAB1 — objetiva)

### Gerar questões

Formato FGV 1ª fase: enunciado (fato + texto legal/súmula relevante) + comando ("Assinale a alternativa correta") + 4 alternativas (A a D) — historicamente FGV usa 4 alternativas, sem alternativa E (que existe em alguns concursos públicos).

Distribuição por disciplina: pondere para disciplinas frágeis **dentro do conjunto efetivamente testado na 1ª fase**. Se `CLAUDE.md` diz frágil em Civil e Tributário, 60% das questões dessas duas.

Dificuldade: nível OAB. Não nível de prova de graduação (que pode ser mais alta, especialmente em casebooks de Marinoni/Didier). Questão OAB é sobre saber a regra preto-no-branco e aplicar limpo.

### Depois de cada resposta

Mostre resposta correta + por que cada errada está errada.

```markdown
**Correta: C**

**Por que C:** [a regra + aplicação]

**Por que não A:** [que regra está testando e por que errada aqui]
**Por que não B:** [igual]
**Por que não D:** [igual]

**Regra para lembrar:** [takeaway de uma linha]

---

**Checagem de citação.** Regras e julgados citados na explicação foram gerados por modelo de IA e não foram verificados. Antes de fixar regra para a prova, confira contra resumo do seu cursinho (CERS / Damásio / Estratégia / Mege / Praetorium / Ênfase / Supremo TV) ou contra dispositivo / súmula vigente em planalto.gov.br ou portais STF/STJ. Regras geradas por IA às vezes erram em elementos ou confundem entre súmulas similares.
```

### Rastreie padrões

Mantenha tally: que disciplinas, que subtópicos, que armadilhas de alternativa errada. Depois de uma sessão:

> "Você errou 3 de 5 questões de Processo Civil, todas em recurso (cabimento de agravo). Isso é padrão. Vamos drillar agravo especificamente."

## Modo 2ª fase (OAB2 — peça e discursiva)

### Gerar prompt

Formato 2ª fase para a área escolhida:
- **Peça:** enunciado de caso + comando ("Elabore a peça processual cabível..."); tempo sugerido 2h30. Avaliada por estrutura, requisitos da peça (CPC 319 / CPP 41 / CLT 840 / etc.), fundamentação, pedidos.
- **Discursiva:** enunciado breve (1-2 parágrafos) + 2-4 perguntas pontuais; tempo sugerido 30 min cada (4 discursivas × 30 min = 2h); peso 15-17,5 pontos cada. Avaliada por identificação correta do instituto + fundamentação + resposta às perguntas pontuais.

Por área:
- **Civil:** peça típica — petição inicial cível (alimentos, divórcio, despejo, indenização) ou contestação ou recurso (apelação, agravo). Discursivas: contratos, responsabilidade civil, família/sucessões, obrigações.
- **Penal:** peça — defesa preliminar (Lei 11.343 art. 55), habeas corpus, apelação criminal, razões/contrarrazões, revisão criminal. Discursivas: teoria do crime, parte especial, processo penal.
- **Trabalho:** peça — reclamação trabalhista, contestação, recurso ordinário, agravo de petição. Discursivas: contrato individual, direitos trabalhistas (CLT + reforma 13.467/17), processo trabalhista.
- **Tributário:** peça — mandado de segurança, embargos à execução fiscal, ação anulatória, repetição. Discursivas: sistema tributário, créditos e benefícios, processo administrativo, execução fiscal.
- **Empresarial:** peça — recuperação judicial, falência, ação societária. Discursivas: sociedades, títulos de crédito, contratos empresariais.
- **Administrativo:** peça — mandado de segurança, ação popular, ação civil pública. Discursivas: ato administrativo, licitações, servidores, responsabilidade civil do Estado.
- **Constitucional:** peça — mandado de segurança constitucional, ADI/ADC (se cabível pelo edital), HC constitucional. Discursivas: controle de constitucionalidade, direitos fundamentais, organização do Estado.

### Corrigir

Depois de você escrever:

- **Identificação de instituto:** identificou corretamente o que a peça/questão pediu? (FGV penaliza fortemente identificar errado.)
- **Estrutura:** a peça atende aos requisitos formais (CPC 319, CPP 41, CLT 840, conforme o caso)? Há endereçamento, qualificação, fatos, fundamentos, pedidos, valor, provas, requerimento?
- **Fundamentação:** dispositivos + súmulas + Temas certos? Doutrina citada onde necessário? Pinpoint do dispositivo?
- **Pedidos:** específicos? Quantificados em R$ quando aplicável? Cumulação correta?
- **Organização e linguagem:** clara? Sem latim desnecessário? Português técnico mas legível?

Avaliação OAB é sobre suficiência e correção, não brilhantismo. Peça completa, organizada, com dispositivos certos passa. Peça brilhante mas faltando dispositivo essencial reprova.

```markdown
## Feedback da peça/discursiva

**Identificação do instituto:** [correto / errado — qual era / qual você fez]

**Estrutura:** [completa / incompleta — itens faltantes]

**Fundamentação:** [precisa / parcial / errada — dispositivos citados vs. esperados]

**Pedidos:** [específicos / genéricos]

**Organização:** [clara / confusa]

**Se fosse corrigida:** [Passa / borderline / não-ainda — com o que arrumar]
```

## Integração com cronograma

Se você tem cronograma: pondere questões para o que está agendado nesta semana. Conteúdo novo é drillado.

## O que esta skill NÃO faz

- Substituir cursinho de OAB. CERS / Damásio / Estratégia / Mege / Praetorium / Supremo TV / Ênfase têm currículo completo. Isto é drilling suplementar.
- Prever a OAB. Ninguém pode. Estude tudo do edital.
- Passar na OAB por você. Obviamente.
- **Afirmar regras das quais não está confiante sem flag.** Se não tem certeza se a regra está certa, você vai ver `[INCERTO]` ou `[VERIFICAR]` — confira contra cursinho antes de confiar. Regra errada que eu afirmo com confiança é sessão de estudo pior que questão pulada.
