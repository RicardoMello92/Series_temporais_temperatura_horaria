# Projeto Series Temporais / Machine Learning: Previsão de Temperaturas

Este repositório contém um projeto de Machine Learning focado em Séries Temporais, com o objetivo de prever temperaturas horárias futuras.

## 1. Objetivo

O objetivo principal deste projeto é aplicar e comparar diferentes abordagens de Machine Learning (Regressão e Forecasting) para prever a temperatura horária (em graus Celsius) com base em dados meteorológicos históricos. Foram utilizadas técnicas de Regressão e Forecasting

## 2. Metodologia

A metodologia utilizada pode ser dividida nas seguintes etapas:

### 2.1. Fonte e Tratamento dos Dados

* **Fonte:** Os dados foram obtidos do Kaggle: [Timeseries Weather Dataset](https://www.kaggle.com/datasets/parthdande/timeseries-weather-dataset).
* **Limpeza:** O dataset não apresentava valores nulos.
* **Filtragem:** Para focar em dados mais recentes, o conjunto de dados foi filtrado para incluir apenas registros a partir do ano de 2018.
* **Engenharia de Features (Janela Deslizante):** Foi utilizada uma abordagem de regressão de série temporal. O modelo foi treinado usando uma janela deslizante (sliding window) onde 168 registros passados (7 dias de dados horários) foram usados como *features* (entrada) para prever o próximo passo (horizonte de 1 hora) como *target* (saída).
* **Variáveis:** A variável alvo (target) foi a `temperature`. As colunas `is_Day`, `precipitation (mm)` e `snowfall (cm)` foram removidas durante a fase de EDA.

### 2.2. Modelos Testados

Três modelos distintos foram treinados e avaliados:

1.  **TST Plus (Time Series Transformer):** Uma arquitetura baseada em Transformers da biblioteca `TSAI` (Timeseries Ai). O modelo foi utilizado para Regressão e Forecasting
2.  **Inception Time Plus:** Uma arquitetura baseada em Redes Neurais Convolucionais, também da biblioteca `TSAI`. O modelo foi utilizado para Regressão e Forecasting
3.  **Prophet:** A biblioteca open-source de forecasting desenvolvida pelo Facebook.

### 2.3. Validação e Métrica

* **Divisão dos Dados:** Para os modelos da `TSAI`, foi utilizada uma divisão temporal de 80% para treino e 20% para validação, sem embaralhamento (`shuffle=False`), para respeitar a cronologia dos dados. O Prophet utilizou uma divisão similar.
* **Métrica de Avaliação:** O **MAE** (Mean Absolute Error) foi a métrica escolhida para comparar o desempenho dos modelos.

## 3. Resultados

Os modelos apresentaram os seguintes resultados no conjunto de validação:

| Modelo | MAE (Mean Absolute Error) |
| :--- | :--- |
| TST Plus (Regressão) | 1.45 |
| Inception Time Plus (Regress]ao)| 1.23 |
| TST Plus (Forecasting) | 0.71 |
| **Inception Time Plus (Forecasting)**| **0.59** |
| Prophet | 7.18 |

**IMAGEM DO COMPARATIVO TSAI**

*Obs: Os modelos da TSAI (InceptionTimePlus e TSTPlus) utilizaram *callbacks* de `EarlyStoppingCallback` e `SaveModelCallback` para otimizar o treinamento e salvar o melhor resultado.*

## 4. Conclusão

A análise dos resultados demonstra que o aprendizado por Forecasting se mostrou mais eficiente do que a Regressão Multivariada.

O modelo Inception Time Plus para Forecasting obteve o desempenho superior, com um MAE de 0.59 graus Celsius. Com base na amplitudo dos dados de temperatura observados (min: 7.1°C e máx:41.7°C), esse MAE se mostra como um bom resultado.

O **Prophet**, apesar de ser muito bom para dados com forte sazonalidade e tendências, teve seu desempenho ofuscado pelos modelos Deep Learning, o que pode ser atribuído à **complexidade da periodicidade horária e alta volatilidade dos dados**.

## 5. Próximos Passos

Para aprimorar o modelo, os seguintes passos podem ser explorados:

1.  **Otimização de Hiperparâmetros:** Realizar uma busca exaustiva (ex: *Grid Search* ou *Bayesian Optimization*) nos hiperparâmetros do modelo vencedor (Inception Time Plus), como tamanho da janela de entrada, *learning rate* e número de camadas.
2.  **Aumento do Horizonte de Previsão:** Explorar previsões não apenas para o próximo passo (horizonte 1), mas também para 24, 48 ou 168 horas (7 dias) à frente.
3.  **Análise de Outliers e Eventos Extremos:** Investigar o desempenho dos modelos durante eventos climáticos atípicos para garantir robustez em cenários extremos.
