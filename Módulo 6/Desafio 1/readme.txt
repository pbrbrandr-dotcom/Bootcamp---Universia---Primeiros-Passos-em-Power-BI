# Desafio de Projeto: Relatório Financeiro com Foco na Experiência do Usuário (DIO)

Este repositório apresenta a resolução do desafio de projeto do módulo de Power BI Analyst da DIO. O objetivo consistiu em modificar um relatório financeiro criativo previamente desenvolvido, aplicando princípios de experiência do usuário (UX), design de interface e modelagem de dados avançada.

## 1. Princípios de UX e Design Aplicados

O relatório foi reestruturado para melhorar a navegação e a legibilidade, seguindo as diretrizes propostas no desafio:

- **Posicionamento:** Adoção de um menu lateral fixo à esquerda. Isso garante que a navegação esteja sempre acessível em todas as páginas, sem competir visualmente com os dados.
- **Contraste:** Utilização de um tema escuro (dark mode) com cores de destaque pontuais. O alto contraste entre o fundo e os elementos principais reduz a fadiga visual e direciona a atenção do usuário para as métricas mais importantes.
- **Proporção Áurea:** Distribuição dos visuais na tela de forma hierárquica. Os KPIs principais ocupam a parte superior, seguidos por gráficos de análise de tendência e, por fim, tabelas detalhadas. O espaçamento entre os cards segue uma lógica de respiro visual.
- **Segmentação dos Dados:** Inclusão de slicers de data e de ano/país, permitindo que o usuário filtre os dados e crie seu próprio contexto de análise em tempo real.

## 2. Paleta de Cores e Tipografia

A identidade visual foi construída com base em um tema escuro com destaques em verde neon, criando uma estética moderna e corporativa.

### Paleta de Cores
- Fundo do Relatório: `#0A0E0F`
- Fundo de Cards/Visuais: `#12181A`
- Bordas/Divisores: `#2A3638`
- Texto Primário: `#E8F5E9`
- Texto Secundário: `#8FA89C`
- Verde Neon (Destaques e KPIs): `#00E676` / `#39FF88`
- Vermelho Técnico (Valores Negativos): `#FF5252`

### Tipografia
- **Títulos:** Rajdhani / Chakra Petch / Sora (fontes sem serifa com apelo tecnológico).
- **Corpo:** Segoe UI (legibilidade e padrão do sistema).
- **Números de KPI:** Consolas (efeito "ticker" ou HUD, garantindo alinhamento e leitura rápida).

## 3. Estrutura do Relatório

O relatório é composto por 3 páginas interconectadas por um menu de navegação lateral:

### Página 1: Sales Report
- Cartões de KPI (Total de Vendas e Unidades Vendidas).
- Gráfico de linhas para evolução de vendas por período.
- Gráfico de barras para vendas por segmento.
- Matriz de resumo de vendas por ano e segmento.

### Página 2: Report de Lucro Detalhado
- Treemap para análise de lucro por segmento.
- Gráfico de Cascata (Waterfall) para lucro por trimestre.
- Árvore de Decomposição (Decomposition Tree) para detalhamento hierárquico do lucro por país e ano.

### Página 3: Report de Vendas Detalhado
- Matriz de detalhamento de vendas por trimestre.
- Gráfico combinado (Colunas e Linhas) para comparar Vendas e Gross Sales ao longo do período.
- Gráfico de linhas para tendência de vendas.

## 4. Técnicas Avançadas e Decisões de Projeto

### 4.1 Treemap: Evolução e Solução de Problemas
O desenvolvimento do Treemap "Soma de Profit por Segment" passou por uma evolução técnica para resolver problemas de escala e legibilidade:

- **Desafio:** A cor categórica manual ou a escala monocromática por valor absoluto comprimia visualmente os segmentos menores, pois o segmento "Government" é um outlier que estica a escala de magnitude.
- **Solução (Ranking):** Foi criada uma medida DAX para classificar os segmentos por posição relativa, em vez de valor absoluto:
  
  Rank Segmento = 
  RANKX(
      ALLSELECTED('financials'[Segment]),
      CALCULATE(SUM('financials'[Profit])),
      ,
      DESC,
      Dense
  )

- **Formatação Condicional por Regras:** A cor passou a ser definida pela posição no ranking (1 a 4), garantindo que os quatro tons de verde sejam sempre distintos entre si, independentemente da discrepância dos valores.
- **Redundância de Informação:** A área do bloco já comunica o lucro absoluto. Para evitar redundância visual, a cor foi realocada para uma segunda variável: a margem.
- **Cálculo da Margem:** 
  
  Margem = DIVIDE(SUM(financials[Profit]), [total sales])

- **Gradiente de Cor:** Aplicação de um gradiente baseado na Margem, partindo de `#1A2C26` (quase fundido ao fundo) até `#3E8E68` (presença moderada), simulando uma "emergência" do fundo através da luminância.
- **Ajuste de Fundo:** O fundo do visual foi alterado para `#12181A` para eliminar o contorno claro padrão entre os tiles, uma vez que o Treemap não possui controle nativo de borda.

### 4.2 Waterfall: Cores Semânticas
No gráfico de Cascata (Soma de Profit por Trimestre), as cores foram mapeadas de acordo com a semântica da categoria, e não por magnitude:
- Aumentar: `#1DB954`
- Diminuir: `#FF5252`
- Total: `#00E676` (Destaque)

### 4.3 Botões de Navegação
O menu lateral foi construído para proporcionar uma navegação fluida. Foram aplicadas configurações de estado (foco e seleção) para destacar visualmente em qual página o usuário se encontra no momento. O estilo dos botões seguiu o raio fixo de 0 a 2px em todos os cards, conforme definido no design system do projeto.

## 5. Considerações Finais

O projeto demonstra a aplicação prática de conceitos de UX/UI, modelagem de dados e DAX no Power BI. As decisões de design, como o uso de cores semânticas no Waterfall e a lógica de ranking no Treemap, evidenciam a preocupação em entregar um dashboard não apenas esteticamente agradável, mas também funcional e de fácil interpretação.

O arquivo final foi exportado em PDF para documentação do portfólio, preservando o layout e as escolhas visuais. Recursos interativos, como o clique simples nos botões de navegação (que dispensam o uso de Ctrl no Power BI Service), são características nativas da ferramenta de visualização e não são transferidos para o formato estático.

---
Projeto desenvolvido como parte do Bootcamp de Analista de Power BI da DIO.
