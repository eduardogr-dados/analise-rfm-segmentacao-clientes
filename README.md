![Python](https://img.shields.io/badge/Python-3.12-blue?logo=python)
![Scikit-learn](https://img.shields.io/badge/SciKit--Learn-orange?logo=scikit-learn)
![License](https://img.shields.io/badge/License-MIT-yellow.svg)

# **Segmentação de clientes com K-means**

## Visão Geral do Projeto
- **Objetivo principal**: Segmentar a base de clientes de uma loja de varejo online para identificar diferentes perfis de comportamento de compra, permitindo a criação de estratégias de marketing e de negócio personalizadas.
- **Escopo**: O projeto abrange desde a limpeza e o pré-processamento de um ano de dados transacionais até a engenharia de features (Análise RFM), a aplicação de modelagem não supervisionada (K-Means) e a interpretação e recomendação de ações com base nos segmentos encontrados.
- **Principais perguntas de negócio**:
  * Quais são os principais perfis de clientes da nossa base de dados?
  * Existem clientes com alto risco de perda (churn) que necessitam de atenção imediata?
  * Como podemos personalizar as ações de marketing para aumentar o engajamento e a retenção?

## Tecnologias Utilizadas
- **Linguagem de Programação**: Python 3.12.7
- **Bibliotecas de Análise de Dados**: Pandas, NumPy
- **Bibliotecas de Visualização**: Matplotlib, Seaborn
- **Bibliotecas de Machine Learning**: Scikit-learn
- **Ambiente de Desenvolvimento**: Jupyter Notebook

## Como Executar o Projeto
1. Clone o repositório: `git clone https://github.com/seu-usuario/nome-do-repositorio.git`
2. Navegue até o diretório do projeto: `cd nome-do-repositorio`
3. Instale as dependências: `pip install -r requirements.txt`
4. Abra o notebook principal: `jupyter notebook notebooks/segmentacao_clientes.ipynb`

## Estrutura do Repositório
```
/
├── data/        # Contém o dataset bruto (Online Retail.xlsx)
├── assets/      # Gráficos para o arquivo README
├── notebook/    # Notebook Jupyter com a análise completa
└── README.md
```


## Compreensão dos Dados
### Origem
- Os dados foram obtidos do **Online Retail Dataset**, disponível no [Repositório de Machine Learning da UCI](https://archive.ics.uci.edu/dataset/352/online+retail). O conjunto de dados contém transações ocorridas entre 01/12/2010 e 09/12/2011.

### Estrutura e Limpeza
- **Colunas relevantes**: `InvoiceNo`, `StockCode`, `Description`, `Quantity`, `InvoiceDate`, `UnitPrice`, `CustomerID`, `Country`.
- **Processo de Limpeza**: Um rigoroso processo de limpeza foi aplicado para garantir a qualidade dos dados para a análise, incluindo:
```
  • Remoção de 25% das transações que não possuíam `CustomerID`, pois eram inutilizáveis para a segmentação de clientes.
  • Exclusão de transações com quantidade negativa, que correspondiam a cancelamentos.
  • Correção de tipos de dados, como a conversão de datas e preços para formatos numéricos adequados.
  • Remoção de entradas que não eram produtos, mas sim taxas de serviço (ex: 'POST', 'M').
  • Padronização de nomes de países para ter melhor consistência.
```
- **Engenharia de Features**: A análise RFM foi a principal técnica de engenharia de features. Para cada cliente, foram criadas as seguintes métricas:
```
  • `Recency`: Dias desde a última compra.
  • `Frequency`: Número total de transações (faturas) distintas.
  • `MonetaryValue`: Soma total do valor gasto.
```

### Limitações
- Os dados se referem a um período de apenas um ano, o que limita análises de tendências de longo prazo.
- A perda de 25% dos dados devido a `CustomerID`s ausentes pode introduzir um viés se esses clientes tiverem um perfil de compra diferente dos clientes registrados.

## Análise

### Tópico 1: Segmentação do Varejo com K-Means

Após a separação estratégica do segmento de atacado, o algoritmo K-Means foi aplicado na base de varejo. A combinação do Método do Cotovelo e da Análise de Silhueta indicou K=4 como o número ideal de clusters. Principais descobertas:

| Segmento            | Clientes | R        | F   | M      | Perfil                                                                   |
| ------------------- | -------- | -------- | --- | ------ | ------------------------------------------------------------------------ |
| **Campeões**        | 827      | 15 dias  | 11x | £4.154 | Elite do varejo, com altíssima frequência e valor.                       |
| **Clientes Ativos** | 1.165    | 81 dias  | 4x  | £1.479 | Base sólida do negócio, com bom valor, mas precisando de estímulos.      |
| **Promissores**     | 819      | 19 dias  | 2x  | £512   | Clientes novos ou recentes com alto potencial de crescimento.            |
| **Hibernando**      | 1.478    | 188 dias | 1x  | £308   | O maior grupo, composto por clientes inativos e com alto risco de perda. |

Os resultados foram consolidados em um relatório no Power BI. A página **Perfil de Clientes** apresenta uma visão executiva dos segmentos, com KPIs globais, distribuição de clientes, receita por segmento, tempo de inatividade e a relação entre valor monetário e engajamento por cliente.

![Perfil de Clientes](assets/Perfil_Clientes.png)

### Tópico 2: Análise do Segmento de Atacado

Uma análise descritiva foi realizada nos 44 clientes de atacado. Destaques:

- **Impacto Estratégico:** Este 1% da base de clientes é responsável por **32% do faturamento total** da empresa.
- **Modelo de Distribuição:** O padrão de 1-2 clientes por país internacional sugere um modelo de negócio B2B com parceiros de distribuição exclusivos.
- **Padrões de Compra:** Não há um padrão único; existem clientes **"Especialistas"** (alto volume de poucos produtos) e **"Generalistas"** (ampla variedade de itens).
- **Risco Financeiro:** Foi identificado que **9,26%** da receita do atacado está em risco devido à inatividade de apenas 5 clientes, representando **£263.764** em receita comprometida.

A página **Parceiros Estratégicos no Atacado em Risco** detalha os clientes inativos, seu valor monetário individual, tempo de inatividade e localização geográfica.

![Parceiros Estratégicos no Atacado em Risco](Atacado_Risco.png)

## Conclusão
- **Resultados-chave**: O projeto validou que a base de clientes é composta por dois mundos distintos, Varejo e Atacado, cada um com seus próprios padrões e necessidades. No varejo, foram identificados 4 segmentos acionáveis. No atacado, foi revelado um pequeno grupo de clientes de altíssimo impacto, operando como parceiros estratégicos.
- **Fatores de influência**: As métricas RFM foram drivers eficazes para a segmentação. A decisão metodológica de separar os dados de varejo e atacado foi crucial para a obtenção de insights claros e não distorcidos.
- **Lições aprendidas**:

  * A qualidade dos dados é fundamental; a ausência de IDs de cliente pode levar a perdas significativas de informação.
  * A interpretação de negócio deve sobrepor a otimização puramente matemática (ex: escolher K=4 em vez de K=2 para maior granularidade).
  * Outliers nem sempre são erros; podem representar um segmento de negócio inteiramente novo e valioso.

<blank>

- **Recomendações para análises futuras**:

  * **Curto Prazo:** Implementar um dashboard interativo para monitorar a evolução dos segmentos de varejo em tempo real.
  * **Médio Prazo:** Aprofundar a análise da cesta de compras para entender os perfis "Especialista vs. Generalista" e personalizar as recomendações.
  * **Longo Prazo:** Evoluir da análise descritiva para a preditiva, construindo modelos de previsão de churn e de Valor do Cliente (CLV).

## Autor
- **Eduardo garcia Reis**
- **LinkedIn**: [linkedin.com/in/eduardoreis](http://www.linkedin.com/in/eduardo-reis-a4609b278)
- **Portfólio**: [github.com/eduardogr-dados](https://github.com/eduardogr-dados?tab=repositories)

## Licença
Este projeto está sob a licença MIT.
