# Detecção de Fraudes em Cartões de Crédito

## Sobre o Projeto

Este projeto explora o uso de modelos de Machine Learning para identificar transações potencialmente fraudulentas em cartões de crédito.

O principal desafio é o desbalanceamento da base: aproximadamente 99,83% das transações são normais e apenas 0,17% são fraudulentas. Nesse cenário, a acurácia isolada pode ser enganosa, por isso a avaliação dá atenção especial ao recall, à precisão e ao F1-score da classe fraude.

## Dataset

Os dados são carregados diretamente de:

https://storage.googleapis.com/download.tensorflow.org/data/creditcard.csv

O conjunto inclui as variáveis `Time`, `Amount`, `V1` a `V28` e a coluna `Class`, que indica se uma transação é normal (`0`) ou fraudulenta (`1`).

O arquivo de dados não é incluído no repositório.

## Etapas do projeto

1. Exploração dos dados e análise do desbalanceamento.
2. Preparação das variáveis, incluindo transformação logarítmica do valor da transação.
3. Separação estratificada dos conjuntos de treino e teste.
4. Padronização das variáveis numéricas.
5. Treinamento e comparação de Regressão Logística e Random Forest.
6. Avaliação com matriz de confusão, precisão, recall, F1-score e curvas de avaliação.
7. Ajuste do limiar de decisão.
8. Análise de importância das variáveis e explicações com SHAP.

## Resultados

Os resultados abaixo foram obtidos no conjunto de teste.

| Modelo              | Limiar | Precisão da fraude | Recall da fraude | F1-score |
| ------------------- | -----: | -----------------: | ---------------: | -------: |
| Regressão Logística |   0,50 |              5,86% |           91,84% |   11,02% |
| Regressão Logística |   0,80 |             16,60% |           89,80% |   28,03% |
| Random Forest       |   0,50 |             96,15% |           76,53% |   85,23% |
| Random Forest       |   0,20 |             84,54% |           83,67% |   84,10% |

A Regressão Logística, no limiar padrão, apresentou recall elevado, mas baixa precisão. A Random Forest apresentou melhor equilíbrio entre precisão e recall nos cenários testados.

Ao reduzir o limiar da Random Forest de 0,50 para 0,20, o número de fraudes identificadas aumentou de 75 para 82, enquanto os falsos positivos passaram de 3 para 15.

O limiar adequado depende do equilíbrio desejado entre fraudes não detectadas e alertas falsos.

## Explicabilidade com SHAP

A análise global foi realizada em uma amostra de 200 transações. Nessa amostra, `V12`, `V14` e `V4` apresentaram as maiores importâncias médias absolutas segundo o SHAP.

Também foi analisada uma transação fraudulenta identificada corretamente pelo modelo, com probabilidade estimada de fraude de 97,5%. Para essa transação, `V14`, `V10` e `V17` foram as maiores contribuições positivas para a saída explicada da classe fraude.

As variáveis `V1` a `V28` foram transformadas por PCA. Portanto, suas contribuições não devem ser interpretadas como relações causais ou como características financeiras diretamente identificáveis.

## Tecnologias utilizadas

* Python
* pandas e NumPy
* scikit-learn
* Matplotlib
* SHAP
* Jupyter Notebook

## Como executar

1. Clone este repositório.
2. Instale as dependências descritas em `requirements.txt`.
3. Abra `deteccao_fraudes.ipynb` no Jupyter Notebook ou no Google Colab.
4. Execute as células na ordem apresentada.

O notebook carrega o dataset diretamente da URL indicada acima.

## Limitações e próximos passos

* Testar XGBoost e estratégias de undersampling e oversampling.
* Investigar a escolha do limiar com validação separada do conjunto de teste.
* Ampliar a busca de hiperparâmetros.
* Avaliar a estabilidade das métricas em diferentes divisões dos dados.
* Investigar variáveis comportamentais temporais, quando disponíveis.

Este é um projeto de estudo. Os resultados obtidos não devem ser interpretados como garantia de desempenho em transações reais futuras.

