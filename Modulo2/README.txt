# Desafio Power BI - Análise de Vendas e Lucratividade (DIO)

Este repositório apresenta a resolução do desafio prático do curso de Analista de Power BI da DIO. O objetivo consistiu em replicar duas páginas de um relatório base e desenvolver uma terceira página autoral, aplicando conceitos de modelagem, DAX e visualização de dados.

## Sobre os Dados

A base de dados utilizada é a "Financial Sample", disponibilizada pela Microsoft no repositório da instrutora Juliana Zanelatto. O conjunto contém informações de vendas, lucro, descontos, produtos, segmentos e países.

Nota sobre a qualidade dos dados:
Durante a exploração, identificou-se que o ano de 2013 possui registros apenas de setembro a dezembro (4 meses), enquanto 2014 possui o ano completo (12 meses). Esse detalhe é fundamental para a interpretação correta das métricas de correlação apresentadas na análise.

## Estrutura do Relatório

### Página 1: Visão Geral de Vendas e Descontos (Replicada)
- Pizza: Soma de Sales por Product.
- Dispersão (Scatter Plot): Impacto do Desconto na Margem de Lucro, com linha de tendência e análise de correlação de Pearson.
- Área: Média de Sale Price e Média de Profit por Product.
- Barras Empilhadas: Soma de Sales por Ano, Mês e Segmento.

### Página 2: Análise de Lucro por País (Replicada)
- Cartões: Soma de Units Sold e Soma de Sales.
- Pizza: Soma de Profit por Country.
- Barras: Soma de Sales por Country.
- Barras Verticais: Soma de Profit por Ano e Mês.

### Página 3: Distribuição Geográfica e por Segmento (Autoral)
- Mapa 1: Soma de Sales e Soma de Units Sold por Country.
- Mapa 2: Soma de Profit por Country e análise de margem.
- Pizza: Soma de Profit por Segment.
- Treemap: Média de Margem Lucro % por Segment.
- Barras: Soma de Profit por Country.

## Principais Insights da Análise

1. O Desconto Corrói a Margem? (Análise de Correlação)
A análise do coeficiente de correlação de Pearson revelou que:
- Por País: A correlação é de -0,97, indicando que, em média, os países que concedem mais descontos são os que apresentam as piores margens de lucro.
- Por Produto (Período Total): A correlação é de 0,09, praticamente nula.
- O viés estatístico e o filtro de 2014: O valor 0,09 era influenciado pelos dados incompletos de 2013. Ao filtrar apenas o ano de 2014 (ano completo), a correlação por Produto altera-se para -0,68, evidenciando que, naquele ano, a política de descontos impactou negativamente a margem de forma consistente.

2. Lucratividade por Segmento:
O segmento Government apresenta a maior lucratividade, representando mais de 65% do lucro total, seguido por Small Business.

## Sobre os Mapas e suas Diferenças

A construção da Página 3 evidenciou uma limitação técnica importante do Power BI que merece ser documentada.

O visual "Mapa" padrão do Power BI, utilizado na versão Desktop sem conta corporativa, não oferece controle granular sobre a escala de tamanho das bolhas, como as opções de Tamanho Mínimo e Máximo. Em determinados contextos, quando há muitos pontos de dados ou valores muito díspares, o visual tende a padronizar o tamanho das bolhas, falhando em representar a proporção exata das vendas ou lucros entre os países.

Foi avaliada a migração para o Azure Maps, que é o visual recomendado pela Microsoft e possui exatamente as opções necessárias para escalar as bolhas corretamente. No entanto, o Azure Maps exige uma autenticação com conta corporativa ou escolar para renderizar os visuais.

Como não havia acesso a uma conta corporativa no momento da execução, o Mapa Padrão foi mantido com ajustes de zoom e transparência, entregando a melhor representação possível dentro das limitações da ferramenta. O aprendizado sobre as diferenças entre os visuais de mapa e suas exigências de autenticação configura um dos maiores ganhos técnicos deste desafio.

## Tecnologias e Habilidades Utilizadas

- Power BI Desktop: Criação de relatórios e dashboards.
- Power Query: Limpeza, transformação e criação de coluna de índice (ID_Linha) para garantir a granularidade correta nos visuais de dispersão.
- DAX (Data Analysis Expressions): Criação de medidas e colunas calculadas (% Desconto, Margem Lucro %, Coeficiente de Correlação).
- Estatística: Cálculo e interpretação do Coeficiente de Correlação de Pearson e análise de viés de dados.
- Design de Dashboards: Preocupação com layout limpo, títulos claros, dicas de ferramentas (tooltips) e usabilidade.

## Arquivos do Repositório

O repositório está organizado da seguinte forma:
- README.md (este arquivo)
- /Desafio-Financial (contém o arquivo .pbix e o .pdf do relatório financeiro)
- /Desafio-Diabetes (contém o arquivo .pbix e o .pdf do relatório de diabetes)

---
Projeto desenvolvido como parte do Bootcamp de Analista de Power BI da DIO.
