# Brasileirão (2003–2025) — Impacto de um bom início (Top 5 após 5 rodadas)

## 📌 Objetivo do projeto
Este projeto analisa o Brasileirão Série A por pontos corridos (2003–2025) para responder:
**o que acontece, em média, com clubes que começam bem o campeonato**, ficando entre os **5 melhores após as 5 primeiras rodadas**.

A ideia é transformar dados em um relatório interativo (Looker Studio) com KPIs e visualizações que ajudam a entender:
- Chances de título
- Chances de G4/G6 (Libertadores)
- Risco de rebaixamento mesmo com bom início
- Quais clubes mais aparecem entre os melhores no início
- Casos raros (ex.: 100% nas 5 primeiras rodadas)

---

## 🔗 Links do projeto
- **Relatório (Looker Studio):** https://lookerstudio.google.com/s/oRLQk_6-3Wg  
- **Base (Google Sheets):** https://docs.google.com/spreadsheets/d/1GyfOwbeJVwNj5rHUcu4TCscy1OgOnusY9ESu5pJkLTI/edit?usp=sharing

---

## 🧰 Ferramentas utilizadas
- **BigQuery (SQL):** extração e transformação inicial dos dados
- **Google Sheets:** limpeza/validação e padronização final
- **Looker Studio:** construção do dashboard e visualizações

---

## 📂 Fonte de dados
Os dados foram extraídos via BigQuery a partir de datasets públicos (Base dos Dados / Transfermarkt),
com informações de partidas e classificação do Brasileirão Série A.

O dataset final utilizado no relatório contém:
- Temporada (`season`)
- Clube (`team`)
- Pontos e jogos nas 5 primeiras rodadas (`pts_5`, `jogos_5`)
- Desfecho da temporada (`pos_final`, `pts_final`)
- Flags de resultado (`is_champion`, `is_top4`, `is_top6`, `is_top8`, `is_top10`)
- (Criado no Looker/Sheets) `is_rebaixado`
- Faixas de classificação (`faixa_posicao_final`) e faixas de pontuação inicial (quando aplicável)

### Nota metodológica (muito importante)
A base final contém **apenas os clubes que estavam no Top 5 após 5 rodadas**.
Portanto, as taxas e probabilidades apresentadas no relatório representam:
**P(resultado | começou bem nas 5 primeiras rodadas)**.

### Nota histórica (formato do campeonato)
O Brasileirão Série A em pontos corridos teve mudanças no número de participantes no início do recorte:
- **2003 e 2004:** 24 clubes (**46 rodadas**)
- **2005:** 22 clubes (**42 rodadas**)
- **2006 em diante:** 20 clubes (**38 rodadas**)

Este projeto compara apenas as **5 primeiras rodadas**, mantendo a análise do “início do campeonato” consistente entre os anos.

---

## 🔄 Pipeline do projeto (passo a passo)

### 1) Extração no BigQuery
1. Consulta SQL no BigQuery para obter jogos/tabelas necessárias.
2. Cálculo de pontos nas primeiras 5 rodadas por clube e temporada.
3. Seleção do **Top 5** por temporada após 5 rodadas.
4. Enriquecimento com dados de desfecho (posição e pontos finais).
5. Exportação dos resultados para `.CSV`.

### 2) Limpeza e validação no Google Sheets
No Google Sheets:
- Padronização de nomes de clubes (maiúsculo e consistência)
- Verificação de tipos (número vs texto) e percentuais
- Conferência de consistência (ex.: `pts_5` máximo 15; `jogos_5` = 5)
- Ajustes finais antes de conectar ao Looker Studio

### 3) Construção do relatório no Looker Studio
No Looker Studio:
- Conexão com a planilha no Google Sheets
- Criação de campos calculados (ex.: `is_rebaixado`, `aproveitamento_5`, `faixa_pts_5`)
- Montagem do dashboard em **4 páginas**:
  1. **Resumo executivo** (KPIs e visão por temporada)
  2. **Ranking e consistência por clube** (quem começa bem e quem converte em título)
  3. **Casos raros e riscos** (100% e rebaixamentos dentro do Top 5 inicial)
  4. **Influência do início no desfecho** (dispersão e segmentações)

---

## 🧠 Perguntas respondidas no relatório (e principais resultados)

### ✅ 1) Qual a probabilidade de ser campeão começando bem (Top 5 após 5 rodadas)?
- **10,4%** (12 campeões em 115 casos)

### ✅ 2) Ainda existe risco de rebaixamento mesmo começando bem?
- **9,6%** (11 rebaixamentos em 115 casos)

### ✅ Insight curioso: campeão vs rebaixado quase empatam
Dentro do recorte “Top 5 após 5 rodadas”, os eventos extremos ficaram muito próximos:
- **campeão = 10,4%** vs **rebaixado = 9,6%**
- Diferença de apenas **0,8 ponto percentual** (10,4 − 9,6)

Isso reforça que um início excelente não elimina risco e que o Brasileirão é um campeonato com grande variabilidade ao longo da temporada.

### ✅ 3) Qual a probabilidade de ir para Libertadores começando bem?
Como o número de vagas varia por época, o relatório traz duas leituras:
- **G4 (direto):** **33,9%**
- **G6:** **46,1%**

### ✅ 4) Qual clube mais vezes começou bem desde 2003?
- **Atlético-MG** (10 aparições no Top 5 após 5 rodadas)

### ✅ 5) Quais clubes fizeram 100% (15/15) nas 5 primeiras rodadas?
- **São Paulo (2011)**
- **Botafogo (2023)**

### ✅ 6) Houve ano com mais de um clube do Top 5 inicial rebaixado?
- Sim: **2007** e **2024**

### ✅ 7) Qual clube começa bem muitas vezes, mas nunca foi campeão?
- **Internacional** (alta frequência no Top 5 inicial e 0 títulos no período)

### ✅ 8) Qual a influência de começar bem e ser campeão?
- Em **52%** das temporadas (12/23), o campeão já estava no Top 5 após 5 rodadas.
- Ainda assim, começar bem **não garante título**, pois o campeonato é longo e exige consistência.

---

## 📊 Estrutura do dashboard (Looker Studio)
- **Página 1 — Resumo executivo:** KPIs + visão por temporada
- **Página 2 — Ranking de clubes:** quem começa bem mais vezes e quem converte em título
- **Página 3 — Casos raros e riscos:** 100% e rebaixamentos dentro do Top 5 inicial
- **Página 4 — Relação início x resultado:** scatter (pts_5 vs pts_final) e análises por faixas

---

## 📝 Conclusão
Começar o Brasileirão entre os **5 melhores após 5 rodadas** é um sinal positivo, mas está longe de ser determinístico.
No período analisado, a chance de título foi **10,4%**, enquanto o risco de rebaixamento ainda foi **9,6%**.
O gap entre os dois extremos é de apenas **0,8 p.p.**, reforçando que um início forte não protege contra queda de desempenho — a conversão em título depende de consistência ao longo da temporada.

---

## 🚀 Próximos passos (ideias de evolução)
- Incluir **todos os clubes após 5 rodadas** (não apenas o Top 5) para medir o efeito com mais fidelidade
- Modelar previsões (ex.: regressão/árvore) para estimar probabilidade de G4/G6/rebaixamento com base no início
- Adicionar variáveis de contexto (ex.: elenco, investimento, técnico)

---

## 👤 Autor
**Giovanni Vitor Habermann**  
LinkedIn: https://www.linkedin.com/in/giovannivitorsilva/  
GitHub: https://github.com/GVSilva95
