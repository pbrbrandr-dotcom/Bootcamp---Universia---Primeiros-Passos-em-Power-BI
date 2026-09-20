# Desafio de Modelagem Dimensional: Star Schema com Financial Sample

Este repositório apresenta a resolução do desafio de modelagem dimensional do bootcamp de Analista de Power BI da DIO. O objetivo consistiu em transformar uma tabela única (Financial Sample) em um modelo Star Schema, separando os dados em tabelas Dimensão e Fato.

## 1. Fundamentação Teórica

Antes de iniciar a construção, é importante compreender os conceitos aplicados neste projeto.

### O que é um Star Schema?
O Star Schema (Esquema em Estrela) é uma estrutura de modelagem de dados otimizada para consultas analíticas e ferramentas de BI. Ele é composto por uma tabela central (Fato) cercada por tabelas descritivas (Dimensões).

### Tabela Fato vs. Tabela Dimensão
- Tabela Fato (F_Vendas): Armazena os eventos quantitativos (métricas) e as chaves estrangeiras que conectam as dimensões. Exemplos: Sales, Profit, Units Sold, SK_ID.
- Tabelas Dimensão (D_...): Armazenam o contexto qualitativo e descritivo dos dados. Respondem a perguntas como "Quem?", "Onde?", "Quando?" e "O quê?". Exemplos: D_Produtos, D_Calendário, D_Detalhes.

### O que é Granularidade?
Granularidade refere-se ao nível de detalhe armazenado em uma tabela. 
- Alta granularidade: Cada linha representa um evento único e específico (ex: uma venda individual com data, produto e valor exatos).
- Baixa granularidade: Cada linha representa um resumo ou agrupamento (ex: total de vendas por ano).
A tabela F_Vendas deste projeto possui alta granularidade, permitindo que os dados sejam fatiados em qualquer nível de detalhe através das dimensões.

## 2. Processo de Construção (ETL no Power Query)

A construção do modelo foi realizada inteiramente no Power Query, utilizando a tabela original como base (staging).

### Financials_origem
Tabela original carregada do arquivo Excel. Permanece como um backup dos dados brutos. No modelo final, foi configurada para ficar oculta no modo de exibição de relatório.

### D_Produtos
Criada a partir de uma referência à financials_origem. Utilizou-se a função Agrupar Por na coluna Product para calcular as seguintes métricas:
- Média de Unidades Vendidas
- Média do valor de vendas
- Mediana do valor de vendas
- Valor máximo de venda
- Valor mínimo de venda
Posteriormente, foi adicionada uma Coluna de Índice (ID_produto) para servir como chave primária.

### D_Produtos_Detalhes
Criada por referência. Foram selecionadas as colunas Product, Discount Band, Sale Price, Units Sold e Manufacturing Price. Utilizou-se a função Remover Duplicatas com base na combinação de todas essas colunas, garantindo que cada variação de preço e desconto por produto fosse preservada. Uma Coluna de Índice (ID_produtos) foi adicionada.

### D_Descontos
Criada por referência. Para garantir a unicidade da chave primária, foi necessário selecionar apenas a coluna Discount Band e aplicar Remover Duplicatas. Esta etapa é crucial: se a remoção de duplicatas for aplicada a múltiplas colunas (como faixa de desconto e valor do desconto), a tabela resultante terá valores repetidos na coluna de faixa, impossibilitando a criação do relacionamento "Um para Muitos". Uma Coluna de Índice (ID_produto) foi adicionada.

### D_Detalhes
Criada por referência. Foram selecionadas as colunas Segment, Country, Month Name e Year. Aplicou-se Remover Duplicatas com base nessas quatro colunas para criar uma tabela de contexto única. Uma Coluna de Índice (ID_Detalhes) foi adicionada.

### F_Vendas
Criada por referência à financials_origem. Uma Coluna de Índice (SK_ID) foi adicionada para atuar como chave substituta (Surrogate Key). Foram mantidas apenas as colunas necessárias para o contexto e as métricas: SK_ID, Product, Units Sold, Sale Price, Discount Band, Segment, Country, Sales, Profit e Date.

## 3. Criação da Tabela Calendário (DAX)

A tabela D_Calendário não foi criada no Power Query, mas sim diretamente no modelo do Power BI utilizando a linguagem DAX. Isso garante maior controle sobre a inteligência temporal.

A fórmula utilizada para criar a tabela foi:
D_Calendário = 
VAR DataMinima = MIN('financials_origem'[Date])
VAR DataMaxima = MAX('financials_origem'[Date])
RETURN
CALENDAR(DataMinima, DataMaxima)

Posteriormente, foram criadas colunas calculadas para aumentar a granularidade da dimensão de tempo:
- Ano = YEAR('D_Calendário'[Date])
- Mês = MONTH('D_Calendário'[Date])
- Trimestre = QUARTER('D_Calendário'[Date])

## 4. Modelagem de Dados (Star Schema)

O modelo final foi estruturado com a tabela F_Vendas no centro, conectada às tabelas dimensão através de relacionamentos "Um para Muitos (1:*)", onde o lado "Um" é sempre a dimensão (chave única) e o lado "Muitos" é a tabela Fato.

- F_Vendas[Product] -> D_Produtos[Product]
- F_Vendas[Product] -> D_Produtos_Detalhes[Product]
- F_Vendas[Discount Band] -> D_Descontos[Discount Band]
- F_Vendas[Date] -> D_Calendário[Date]
- F_Vendas[Segment] e [Country] -> D_Detalhes[Segment] e [Country] (Relacionamento Muitos para Muitos, aceitável neste contexto pela ausência de uma chave única na Fato para esta dimensão específica).

## 5. Aprendizados e Solução de Problemas

Durante a construção, alguns obstáculos técnicos foram encontrados e solucionados:
1. Erro de "Valores Duplicados" no Relacionamento: Ocorreu ao tentar ligar a D_Descontos à F_Vendas. A causa foi a remoção de duplicatas aplicada a múltiplas colunas, o que deixou a coluna Discount Band com valores repetidos. A solução foi refazer a etapa no Power Query removendo duplicatas apenas da coluna Discount Band.
2. Diferença entre Duplicar e Referenciar: A função "Duplicar" cria uma cópia independente da tabela. A função "Referenciar" cria uma tabela dependente da original. O uso de "Referenciar" é a prática recomendada em ETL, pois se a tabela original for atualizada, todas as referências se atualizam automaticamente.
3. Formatação vs. Tipo de Dados: No Power BI, o tipo de dados (Data, Texto, Número) é diferente da formatação visual (como a data é exibida). Uma data pode ser exibida como "segunda-feira" ou "01/09/2013", mas continuará sendo do tipo Data, permitindo o relacionamento.

---
Projeto desenvolvido como parte do Bootcamp de Analista de Power BI da DIO.
