# Desafio Power BI - Análise de Vendas e Lucratividade (DIO)

Este repositório contém a resolução do primeiro desafio prático do curso de Analista de Power BI da [DIO (Digital Innovation One)](https://web.dio.me/). O objetivo foi replicar duas páginas de um relatório base e desenvolver uma terceira página autoral, explorando conceitos de modelagem, DAX e visualização de dados.

## 📊 Sobre os Dados

O conjunto de dados utilizado é o **Financial Sample** (disponibilizado pela Microsoft no repositório da instrutora Juliana Zanelatto). A base contém informações de vendas, lucro, descontos, produtos, segmentos e países.

**Nota sobre a Qualidade dos Dados:** 
Durante a análise, identificou-se que o ano de **2013 possui dados apenas de Setembro a Dezembro** (4 meses), enquanto 2014 possui o ano completo (12 meses). Esse detalhe foi crucial para a interpretação correta das métricas de correlação, como explicado na seção de Insights.

## 📑 Estrutura do Relatório

### Página 1: Visão Geral de Vendas e Descontos (Replicada)
- **Cartões/Pizza:** Soma de Sales por Product e participação no total.
- **Dispersão (Scatter Plot):** Impacto do Desconto na Margem de Lucro. Inclui linha de tendência e análise de correlação de Pearson.
- **Área:** Média de Sale Price e Média de Profit por Product.
- **Barras Empilhadas:** Soma de Sales por Ano, Mês e Segmento.

### Página 2: Análise de Lucro por País (Replicada)
- **Cartões:** Soma de Units Sold e Soma de Sales.
- **Pizza:** Soma de Profit por Country.
- **Barras:** Soma de Sales por Country.
- **Barras Verticais:** Soma de Profit por Ano e Mês.

### Página 3: Distribuição Geográfica e por Segmento (Autoral)
- **Mapa 1:** Soma de Sales e Soma de Units Sold por Country.
- **Mapa 2:** Soma de Profit por Country (e análise de margem).
- **Pizza:** Soma de Profit por Segment.
- **Treemap:** Média de Margem Lucro % por Segment.
- **Barras:** Soma de Profit por Country.

## 💡 Principais Insights da Análise

1. **O Desconto Corrói a Margem? (Análise de Correlação)**
   Utilizando o coeficiente de correlação de Pearson, descobrimos que:
   - **Por País:** A correlação é de **-0,97** (quase perfeita). Isso significa que, em média, os países que dão mais desconto são exatamente os que têm as piores margens de lucro.
   - **Por Produto (Período Total):** A correlação é de **0,09** (praticamente nula). 
   - **O "Pulo do Gato" (Filtro 2014):** O valor 0,09 era um viés estatístico causado pelos dados incompletos de 2013. Ao filtrar apenas o ano de **2014 (ano completo)**, a correlação por Produto salta para **-0,68**, provando que, naquele ano, a política de descontos impactou negativamente a margem de forma consistente.

2. **Lucratividade por Segmento:**
   O segmento **Government** é o mais lucrativo, representando mais de 65% do lucro total, seguido por Small Business.

## 🗺️ Por que os mapas estão diferentes do exemplo original?

Durante a construção da Página 3, nos deparamos com uma limitação técnica importante do Power BI que vale ser documentada:

O visual **"Mapa" padrão** do Power BI, utilizado na versão Desktop sem conta corporativa, possui uma limitação conhecida: **ele não oferece controle granular sobre a escala de tamanho das bolhas** (Tamanho Mínimo e Máximo). Em determinados contextos, quando há muitos pontos de dados ou valores muito díspares, o visual tende a padronizar o tamanho das bolhas, falhando em representar a proporção exata das vendas/lucros entre os países.

**Tentativa de Solução:**
Tentamos migrar para o **Azure Maps**, que é o visual recomendado pela Microsoft e possui exatamente as opções de "Tamanho Mínimo" e "Tamanho Máximo" necessárias para escalar as bolhas corretamente. No entanto, o Azure Maps exige uma **autenticação com conta corporativa ou escolar** para renderizar os visuais.

**Conclusão:**
Como não tínhamos acesso a uma conta corporativa no momento da execução, mantivemos o **Mapa Padrão** ajustando o zoom e a transparência para entregar a melhor representação possível dentro das limitações da ferramenta. O aprendizado sobre as diferenças entre os visuais de mapa (Padrão vs. Azure Maps) e suas exigências de autenticação foi um dos maiores ganhos técnicos deste desafio.

## 🛠️ Tecnologias e Habilidades Utilizadas

- **Power BI Desktop:** Criação de relatórios e dashboards.
- **Power Query:** Limpeza, transformação e criação de coluna de índice (`ID_Linha`) para garantir a granularidade correta nos visuais de dispersão.
- **DAX (Data Analysis Expressions):** Criação de medidas e colunas calculadas (% Desconto, Margem Lucro %, Coeficiente de Correlação).
- **Estatística:** Cálculo e interpretação do Coeficiente de Correlação de Pearson e análise de viés de dados.
- **Design de Dashboards:** Preocupação com layout limpo, títulos claros, dicas de ferramentas (tooltips) e usabilidade.

## 📂 Como acessar os arquivos

1. **Relatório em PDF:** O arquivo `financial.pdf` contém a exportação de todas as páginas do relatório para visualização rápida.
2. **Projeto Power BI:** O arquivo `.pbix` (caso disponível) pode ser aberto no Power BI Desktop para explorar as medidas e a modelagem.

---
*Projeto desenvolvido como parte do Bootcamp de Analista de Power BI da DIO.*
