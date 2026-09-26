# Desafio de Projeto: Relatório Financeiro com Foco na Experiência do Usuário (DIO)

Este repositório apresenta a resolução do segundo desafio de projeto do módulo 6 do bootcamp de Analista de Power BI da DIO. O objetivo consistiu em evoluir o relatório financeiro criativo previamente desenvolvido, aplicando princípios de experiência do usuário (UX), design de interface e técnicas avançadas de interatividade no Power BI.

## 1. Objetivo do Desafio

Modificar o relatório criativo original, focando na experiência do usuário. Os pontos considerados foram:

- Posicionamento dos visuais
- Contraste visual
- Proporção áurea
- Segmentação dos dados
- Botões de navegabilidade com destaque de foco e seleção
- Menus de navegação em cada página
- Estilo livre dos botões
- Relatório composto por 3 páginas

## 2. Estrutura Final do Relatório

O relatório foi consolidado em uma única página principal chamada "Lucro e Eficiência", com três modos de visualização alternados por botões (Bookmarks). Essa abordagem mantém a tela limpa e evita a fadiga visual que ocorreria com 8 visuais expostos simultaneamente.

### View 1: Overview (Visão Geral de Lucro)
- 4 cartões de KPI: Lucro Total, Margem de Lucro, Lucro por Unidade, Desconto Médio
- Gráfico de Barras Horizontais: Lucro Total por Segmento, com formatação condicional por ranking
- Gráfico de Cascata (Waterfall): Lucro Total por Trimestre, com cores semânticas (Aumentar, Diminuir, Total)

### View 2: Discounts (Análise de Descontos)
- Gráfico de Barras Horizontais: Top 5 Produtos por Lucro, usando filtro N Superior
- Gráfico de Dispersão: Impacto do Desconto na Margem de Lucro, com linha de tendência única

### View 3: Drill Down (Detalhamento)
- Árvore de Decomposição (Decomposition Tree): Country → Segment → Product
- Matriz de Lucro por Produto e Trimestre, configurada como dica de ferramenta (Tooltip) do Waterfall

## 3. Decisões de Design

### Paleta de Cores
A paleta foi construída sobre um tema escuro com destaques em verde, garantindo contraste e legibilidade.

- Fundo do relatório: `#0A0E0F` (quase preto, com um toque de roxo escuro no layout final)
- Fundo dos cards: `#12181A`
- Texto primário: `#E8F5E9`
- Texto secundário: `#8FA89C`
- Verde de destaque (KPIs e totais): `#00E676`
- Vermelho técnico (valores negativos): `#FF5252`
- Roxo médio (barras e elementos neutros): `#7E57C2`

### Tipografia
- Títulos: Segoe UI Semibold
- Números de KPI: Segoe UI com formatação de milhar/milhão

### Proporção Áurea Aplicada
- Faixa dos botões de View + Título: ~10% da altura
- Faixa dos KPIs: ~20% da altura
- Faixa dos gráficos principais: ~70% da altura

## 4. Técnicas Avançadas Utilizadas

### 4.1 Marcadores (Bookmarks) com Botões
O relatório usa três botões no topo ("Overview", "Discounts", "Drill Down") que alternam qual conjunto de visuais está visível. A implementação seguiu estes passos:

1. Os visuais foram agrupados por view usando o painel Seleção.
2. Cada estado de visibilidade foi salvo como um Marcador.
3. A opção "Dados" foi desmarcada nos marcadores, garantindo que a alternância entre views não resetasse os filtros escolhidos pelo usuário.
4. Cada botão foi configurado com a Ação "Marcador", apontando para o marcador correspondente.
5. O botão ativo recebe contorno verde neon, mantendo a consistência visual do menu lateral.

### 4.2 Matriz como Dica de Ferramenta (Tooltip)
A Matriz de Lucro por Produto e Trimestre foi movida para uma página separada configurada como "Dica de ferramenta". Ela foi vinculada ao Waterfall, de modo que ao passar o mouse sobre uma barra, a Matriz detalhada aparece filtrada pelo trimestre específico.

### 4.3 Formatação Condicional por Regras
No gráfico de barras de Lucro por Segmento, foi aplicada a formatação condicional por Regras para garantir 4 tons sempre distintos, independentemente da discrepância dos valores. A medida utilizada foi:

Rank Segmento = 
RANKX(
    ALLSELECTED('financials'[Segment]),
    CALCULATE(SUM('financials'[Profit])),
    ,
    DESC,
    Dense
)

A regra foi configurada para que o rank 5 (Enterprise, que possui lucro negativo) fosse destacado em vermelho `#FF5252`, enquanto os ranks 1 a 4 seguissem a escala de verdes.

### 4.4 Chiclet Slicer para Segmentação
Foi utilizado o visual customizado Chiclet Slicer para filtro de Ano (2013 e 2014), substituindo o slicer padrão do Power BI por uma interface mais limpa e alinhada ao tema escuro.

### 4.5 Cores Semânticas no Waterfall
- Aumentar: `#00E676` (verde neon)
- Diminuir: `#FF5252` (vermelho)
- Total: `#B39DDB` (roxo claro)

## 5. Insights da Análise

- O segmento Government concentra o maior lucro absoluto, sendo o principal motor financeiro da empresa.
- O segmento Enterprise opera no prejuízo (lucro negativo), o que justifica o destaque em vermelho na View 1.
- Existe uma correlação negativa clara entre o percentual de desconto aplicado e a margem de lucro resultante: quanto maior o desconto, menor a margem.
- Os produtos Paseo e VTT lideram o ranking de lucro, enquanto Carretera e Montana apresentam lucros menores.
- O faturamento cresce ao longo do ano, com o quarto trimestre sendo o mais forte em vendas.

## 6. Sobre a Exportação em PDF

O relatório foi exportado para PDF para fins de documentação do portfólio. É importante destacar que:

- O PDF preserva o layout, as cores e as escolhas visuais de cada uma das 3 views.
- Recursos interativos não são transferidos para o PDF, pois o formato é estático. Isso inclui:
  - Os botões de alternância entre views (marcadores)
  - A dica de ferramenta da Matriz vinculada ao Waterfall
  - A árvore de decomposição expandível
  - O Chiclet Slicer funcional
- Para explorar a interatividade completa, é necessário abrir o arquivo `.pbix` no Power BI Desktop ou publicar o relatório no Power BI Service.

## 7. Aprendizados e Solução de Problemas

Durante o desenvolvimento, alguns obstáculos técnicos foram encontrados e solucionados:

1. **Erro de medida DAX com espaço no nome da coluna:** A coluna `Sales` no arquivo original possuía um espaço invisível antes do nome (` Sales`), o que impedia a criação de medidas. Solução: referenciar corretamente com o espaço ou renomear a coluna na origem.

2. **Treemap com compressão visual por outlier:** O Treemap nativo do Power BI comprime a escala de cores quando existe um outlier (Government). Solução: substituir o Treemap por um Gráfico de Barras Horizontais com formatação condicional por ranking, eliminando o problema e melhorando a leitura.

3. **Múltiplas linhas de tendência no Scatter Plot:** Ao usar uma legenda com várias categorias, o Power BI gera uma linha de tendência por categoria. Solução: remover temporariamente a legenda, adicionar a linha geral e restaurar a legenda.

4. **Cardinalidade Muitos para Muitos em relacionamentos:** A tabela `D_Detalhes` não possuía chave estrangeira na `F_Vendas`, gerando um relacionamento pontilhado. Isso foi aceito por ser o comportamento esperado para este contexto, sem comprometer a análise.

## 8. Arquivos do Repositório

- `relatorio_financeiro.pbix`: Arquivo original do Power BI com toda a interatividade
- `relatorio_financeiro.pdf`: Exportação estática das 3 views
- `README.md`: Este arquivo

---
Projeto desenvolvido como parte do Bootcamp de Analista de Power BI da DIO.
