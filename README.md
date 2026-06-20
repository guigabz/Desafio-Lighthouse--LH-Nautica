# ⚓ Pipeline de Ciência de Dados End-to-End: LH Náutica (Desafio Lighthouse)

Este projeto consiste em uma solução completa de inteligência de dados desenvolvida para a **LH Náutica**, cobrindo todas as etapas de um projeto de Ciência de Dados: desde a ingestão e tratamento de dados brutos e corrompidos de diferentes fontes (CRM e ERP), passando por uma Análise Exploratória de Dados (EDA) profunda, até a criação de **modelos preditivos de regressão** e um **sistema de recomendação de produtos** baseado em filtragem colaborativa/similaridade.

---

## 🏗️ Arquitetura da Solução e Tecnologias

O pipeline foi projetado para unificar dados isolados (*data silos*) e transformá-los em motores de decisão preditiva.

* **Manipulação e Limpeza:** `Python 3`, `Pandas`, `NumPy`, `JSON`, `Regular Expressions (Regex)`
* **Visualização e Insights:** `Matplotlib`, `Seaborn`, `Matplotlib.ticker`
* **Machine Learning Preditivo:** `Scikit-Learn` (Modelos de Regressão, `RandomForestRegressor`, `LinearRegression`)
* **Engenharia de Features & Pré-processamento:** `StandardScaler`, `LabelEncoder`, `train_test_split`
* **Sistema de Recomendação:** `Cosine Similarity` (Similaridade de Cosseno)

---

## 🔄 1. Ingestão e Engenharia de Data Cleaning

O ecossistema inicial era composto por dados altamente heterogêneos e inconsistentes que passavam por validação dinâmica:
1.  **Dicionários Aninhados (`custos_importacao.json`):** Estruturas complexas em JSON contendo o histórico cambial e taxas em USD.
2.  **Inconsistência de Delimitadores (`vendas_2023_2024.csv`):** Tratamento de separadores mistos (`;` e `,`) que quebravam o parseamento inicial.
3.  **Limpeza de Strings e Moeda (`produtos_raw.csv`):** Extração de caracteres monetários (`R$`) via padrões regex para conversão rigorosa em tipos numéricos (`float`).
4.  **Tratamento de Datas Híbridas:** Conversão de strings temporais registradas em múltiplos padrões internacionais (`YYYY-MM-DD` e `DD-MM-YYYY`) utilizando parsing com tolerância a formatos mistos.
5.  **Padronização Textual (Categorias):** Correção de mais de 30 variações geradas por erros de input manual (ex: `E L E T R Ô N I C O S`, `Eletronicoz`) reduzidas a 3 categorias core: `PROPULSAO`, `ANCORAGEM` e `ELETRONICOS`.

---

## 📊 2. Análise Exploratória de Dados (EDA) & Insights

Após a higienização dos dados, análises estatísticas e visualizações gráficas foram geradas para extrair inteligência de negócio:

### Distribuição e Volume do Portfólio
O portfólio da LH Náutica apresentou um equilíbrio milimétrico após a padronização das categorias, mitigando o viés de dados desbalanceados para o modelo preditivo.


![Lucro por Categorias](imagem)


### Análise de Preços e Outliers
A categoria de **Eletrônicos** apresentou a maior dispersão de preços e os maiores tickets unitários (com destaque para equipamentos de navegação avançados como o *Transponder AIS Maré Magnum* a mais de R$ 33.000,00). 

![Boxplot de Preços por Categoria](caminho_da_sua_imagem/boxplot_precos.png)

---

## 🤖 3. Modelagem Preditiva (Machine Learning)

Com a base consolidada, o objetivo foi prever variáveis contínuas de interesse comercial (ex: volume de vendas ou precificação otimizada).

### Estratégia de Treinamento
* **Divisão dos Dados:** Holdout 80/20 (`train_test_split`) com semente aleatória para reprodutibilidade.
* **Feature Engineering:** Aplicação de `LabelEncoder` para variáveis categóricas estruturais e `StandardScaler` para normalizar escalas e acelerar a convergência dos algoritmos.
* **Modelos Comparados:** * *Baseline:* Regressão Linear (para mensurar relações lineares simples).
    * *Modelo Campeão:* Random Forest Regressor (capaz de capturar interações não-lineares complexas entre os componentes da embarcação e custos cambiais).

### Métricas de Avaliação
O modelo foi avaliado utilizando as métricas padrão de mercado para problemas de regressão (`MAE`, `MSE`, `RMSE` e `R²`).

---

## 🛍️ 4. Sistema de Recomendação Integrado

Para maximizar o *Customer Lifetime Value (LTV)* e criar estratégias de *Cross-Selling*, foi desenvolvido um algoritmo de recomendação baseado em **Matriz de Similaridade por Cosseno**.

O sistema calcula a proximidade vetorial entre os perfis de compras dos itens. Se um cliente adquire um produto específico, o script busca no espaço vetorial quais itens possuem o maior cosseno de proximidade.

### Exemplo Prático de Execução:
Ao instanciar a função `recomendar_proximo_passo("Motor de Popa Honda Vector Kinetic 174HP")`, o motor de similaridade retorna os 3 principais produtos que estatisticamente complementam essa compra.

---
